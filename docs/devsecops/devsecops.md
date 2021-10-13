# Configure SonarQube code analysis in your Pipeline

As a bonus lab, you will now configure an extra task in your existing Pipeline to conduct code scanning on your petclinic source code. This exercise is to show you one way of incorporating security code scanning as part of your automated CI/CD pipeline.

We will use the popular open source package SonarQube to do the code scanning. According to Wikipedia, "SonarQube is an open-source platform developed by SonarSource for continuous inspection of code quality to perform automatic reviews with static analysis of code to detect bugs, code smells, and security vulnerabilities on 20+ programming languages."

For Petclinic, we will be using SonarScanner for Maven. The ability to execute the SonarQube analysis via a regular Maven goal makes it available anywhere Maven is available (developer build, CI server, etc.), without the need to manually download, setup, and maintain a SonarQube Runner installation. For more information on SonarScanner for Maven, please see [here](https://docs.sonarqube.org/latest/analysis/scan/sonarscanner-for-maven/){target="_blank" rel="noopener noreferrer"}.

## Accessing the SonarQube server with your assigned credentials

The lab instructors have already setup a SonarQube server within the OpenShift cluster for you to access for code scanning. Credentials have also been setup for you. Please use your assigned credentials to test access to the SonarQube Server.

Access the SonarQube server [here](https://sonarqube-1633551248720.apps.cloudnative.marist.edu){target="_blank" rel="noopener noreferrer"}

Select `Log in` in the upper right hand corner. And log in with your assigned credentials.

If you are not successful with this step, please let the instructor know.

## Generate a security token for your SonarQube account

You'll need either your credentials, or an access token associated with your account, in order to access the server for code scanning. 

Let's use the access token method.

Now that you've logged in, select your account in the upper right hand corner of the SonarQube server page.

![sonarqubeaccount](../images/DevSecOps/sonarqubeaccount.png) 

In the account panel, go to the security tab, and type in the name `petclinic` to help identify your token, and then select `Generate`. Now copy and save this token to be used in the next step.

![sonarqubetoken](../images/DevSecOps/sonarqubetoken.png) 

## Configuring maven-settings with the Sonar scanner plugin

We need to configure maven with the Sonar scanner plugin prefix. We will do that by including the sonar scanner plugin in the maven settings file.

We will create a Kubernetes ConfigMap for the mavens settings file.

Click on the import button at the top of the OpenShift console.

Copy and paste the entirety of the following into the editor and then hit "Save" (copy by clicking on the copy icon in the top right of the box below).

```bash
kind: ConfigMap
apiVersion: v1
metadata:
  name: maven-settings
data:
  settings.xml: |
    <?xml version="1.0" encoding="UTF-8"?>
    <settings>
      <pluginGroups>
        <pluginGroup>io.spring.javaformat</pluginGroup>
        <pluginGroup>org.sonarsource.scanner.maven</pluginGroup>
      </pluginGroups>
      <profiles>
        <profile>
          <id>sonar</id>
          <activation>
            <activeByDefault>true</activeByDefault>
          </activation>
          <properties>
            <!-- Wait until the quality check is complete in SonarQube  -->
            <sonar.qualitygate.wait>
              true
            </sonar.qualitygate.wait>
            <!-- Exclude DTO Files from SonarQube duplication check as these should have duplications -->
            <sonar.cpd.exclusions>
              **/*DTO*
            </sonar.cpd.exclusions>
          </properties>
        </profile>
      </profiles>
    </settings>
```

## Configuring maven task into Pipeline to do code analysis

Go back to your OpenShift console and go to your pipeline. Your pipeline should look like the picture below, at this point of the workshop.

![presqpipeline](../images/DevSecOps/presqpipeline.png) 

1. We will insert the code analysis task before the build task. The idea being we want to scan the source code for bugs and vulnerabilities, before we build a container image out of it.

    a. From your pipeline screen, Go to Actions -> Edit Pipeline.

    b. Select the plus sign before the build task, as in the picture below.

      ![addsqtask](../images/DevSecOps/addsqtask.png)

    c. Then select the task `maven` from the drop down list.

      ![maventask](../images/DevSecOps/maventask.png)

    !!! tip
        Once you add a specific task (i.e. `maven`), clicking on the oval of the task will enable you to edit its default values for your needs.

2. Give the task the following parameters to do the code analysis with the proper maven goals set to do code scanning against our SonarQube server, <b>be careful to substitute the `-Dsonar.login` goal with the token that you generated in the previous step. Also be mindful to put your name in the value of the `Dsonar.projectName` goal.</b> 

    ![codeanalysistask](../images/DevSecOps/codeanalysistask.png)

    **Display Name**

    ``` bash
    code-analysis
    ```

    **MAVEN_IMAGE**
    ``` bash
    maven:3.8.1-jdk-11-openj9
    ```

    **GOALS**

    ``` bash
    install
    ```
    ``` bash
    sonar:sonar
    ```
    ``` bash
    -Dsonar.login=<use-your-token-from-previous-step>
    ```
    ``` bash
    -Dsonar.host.url=https://sonarqube-1633551248720.apps.cloudnative.marist.edu
    ``` 
    ``` bash
    -Dsonar.projectName=petclinic-<your-name>
    ```

    !!! caution
        Remember to replace `<your-name>` with your name such as `petclinic-garrett`.

3. Now you can click away to get back to the main pipeline edit panel.

4. Save the `pipeline`.

5. Now we will need to add our pipeline workspaces to this task.

    a. Switch to `YAML` from pipeline menu.

      ![Switch to yaml](../images/Part1/SwitchYaml.png)

    !!! question "Why are you editing yaml directly?"
        `Workspaces` are more versatile than traditional `PipelineResources` which is why you are using them. However, as the transition to workspaces continues, the OpenShift Pipeline Builder doesn't support editing the `Workspace` mapping from a pipeline to a task via the Builder UI so you have to do it directly in the yaml for now.



    b. Find the `code-analysis` task and add the following workspace definition:

      ```
            workspaces:
              - name: source
                workspace: workspace
              - name: maven-settings
                workspace: maven-settings
      ```

    !!! question "How can you easily find the `code-analysis` task and add the workspace definition?"

        1. You can click on the black yaml box and then use your find keyboard shortcut (`ctrl+f` for Windows / `command+f` for mac) to bring up the find textbox (labeled 1 in the image below). Then, you can search the following term by pasting it into the find textbox:
          ``` bash
          name: code-analysis
          ```
        2. Paste the workspace definition under the highlighted line as shown in the image below.

          ![Code Analysis Task Add Workspace](../images/DevSecOps/AddWorkspaceCodeTask.png)

    c. Add `maven-settings` to the list of pipeline workspaces

      1. Scroll down to the very bottom of the pipeline yaml file where you can find the workspaces for the pipeline defined.

      2. Add the `maven-settings` workspace to the pipeline with the following:

          ```
              - name: maven-settings
          ```

      3. Save the pipeline

      ![Add Maven Settings Workspace](../images/DevSecOps/AddMavenSettingsWorkspace.png)

    !!! note
        After the save message appears you can then proceed to `Cancel` back to the pipeline menu.

## Run the pipeline

Go to the Actions menu of your pipeline and select Start.

![startpipeline](../images/DevSecOps/startpipelinerun.png)

Hit Start after reviewing the settings panel and making sure to set the options for the `maven-settings` workspace (select `configmap` as the resource choice and `maven-settings` as the specific configmap to use as in the image below).

![Choose maven-settings when starting pipeline](../images/DevSecOps/ChooseMavenSettings.png)

You can go to your pipeline logs and see the output for each of the tasks. <TO DO .. add screen shot of pipeline logs>

It will take 15-20 minutes for the code analysis to run completely through. This task will wait until the quality check is complete in SonarQube and if the quality gate fails, this task will fail and the pipeline will not continue to run. If the quality gate succeeds, this task will succeed and progress onto the next task in the pipeline.

Let's see if our code passes the code analysis...

![codeanalysisfail](../images/DevSecOps/codeanalysisfail.png)

It fails :disappointed:. Next, we are going to see why it failed.

## Analyzing the Failure in SonarQube

### View your project

At this point please return to the SonarQube server [here](https://sonarqube-1633551248720.apps.cloudnative.marist.edu){target="_blank" rel="noopener noreferrer"}, and view the code scan report to see what caused the quality check to fail. After logging in, please do the following:

![SonarProject View Fail](../images/DevSecOps/FindPetclinicSonar.png)

1. Type your name in the project search bar to bring up your project

2. Click on your project (which should have a `Failed` label)

### Check what caused the failure

![Failure Check](../images/DevSecOps/CheckFailureReason.png)

You can see that the overall code check failed due to a security rating worse than A. You should see 9 vulnerabilities that caused this failure. In order to check what these are, please click on the vulnerabilities link as shown in the image.

![Vulnerabilities](../images/DevSecOps/VulnerabilityOverview.png)

1. See individual vulnerabilities and click on `Why is this an issue?`

2. Read the vulnerability descriptions to see why they are a problem and get insights into fixing them in the code.

## Update PetClinic to fix the issues that came up in the SonarQube scan

In the scan, there were various security issues related to the use of entity objects for data transfer instead of data transfer objects (DTOs) when using @RequestMapping and similar methods. In order to fix these, you will have to make changes to the java code for the application. Luckily for you, the changes have already been made on the `security-fixes` branch of the project. In order to bring these changes to the main branch you will need to make a pul request and merge the `security-fixes` branch into the main branch. 

You can do this with the following actions:

1. Go to your fork of the petclinic repository in GitHub and choose to create a new pull request

      ![Create new pull request](../images/DevSecOps/OpenNewPullRequest.png)

      1. Click on the `Pull Requests` tab

      2. Click on `New pull request`


2. Change your base repository from the main repository to your fork.

    ![Choose your repository](../images/DevSecOps/ChooseYourBranch.png)

    1. Click on base repository default of `ibm-wsc/spring-petclinic`

    2. Change to your petclinic fork (in my case this is `siler23/petclinic` but yours will be different)

3. Choose the `security-fixes` branch to merge into the `main` branch and create your pull request

    ![Choose security-fixes branch](../images/DevSecOps/CreatePullRequestFromSecurityFixes.png)

    1. Choose `security-fixes` branch to compare to `main`
    2. Click `Create pull request`

4. Write a justification for your pull request and confirm again that you want to create it

    ![Create pull request for real](../images/DevSecOps/CreatePullRequestForReal.png)

    1. Write a justification such as 

      ```
      Create fixes for all of the security vulnerabilities that showed up in the SonarQube scan.
      ```
    
    2. Click `Create pull request`

5. Merge your pull request, merging the `security-fixes` branch with all of the security fixes into the `main` branch.

    ![Merge pull request](../images/DevSecOps/MergePullRequest.png)

6. Confirm the merge 

    ![Confirm merge](../images/DevSecOps/ConfirmMergeRequest.png)

7. Delete the `security-fixes` branch now that it's been successfully merged into the `main` branch of your petclinic repository fork.

    ![Delete security-fixes branch](../images/DevSecOps/SuccessfullyMerged.png)

    1. See that the `security-fixes` branch was successfully merged!

    2. Click `Delete branch` to delete the now superfluous `security-fixes` branch.

## Verify that vulnerabilities in petclinic have been patched 

1. See a new pipeline triggered back in the `Pipelines` view of your OpenShift namespace.

    ![New pipeline running](../images/DevSecOps/PipelineRunning.png)

2. View the pipeline run and watch it successfully complete the `code-analysis` task.

    ![Security Success](../images/DevSecOps/CodeAnalysisPasses.png)

    !!! note
        You can also wait to see the other tasks pass but since the main goal of this section was to focus on integrating security into DevOps and you have already gone through the pipeline without the `code-analysis` task, there is really no need to do so.

3. View the [SonarQube server](https://sonarqube-1633551248720.apps.cloudnative.marist.edu){target="_blank" rel="noopener noreferrer"} again to see the updated results for your project (based on the latest scan)

    1. See your project passes and click on it for full results

        !!! tip
            Search for your project with your name like before.

        ![See your project passes](../images/DevSecOps/SeeYourProjectPasses.png)

    2. View the final results of the scan.

        ![See Passed Full Results](../images/DevSecOps/PassedFullPage.png)

        Those pesky vulnerabilities have been squashed! :tada:

## Summary :telescope:

In this section, you started on your DevSecOps journey by integrating SonarQube security scanning into your DevOps pipeline. Initially, the scan flagged several security vulnerabilities, causing the pipeline to fail before the vulnerable code could get packaged into a container. Next, you were able to dig into the vulnerabilities and figure out what needed to be changed with the SonarQube report. Then, you applied a security patch, eliminating the flagged security vulnerabilities in the PetClinic application. Finally, your pipeline succeeded and secure code was packaged into a container and deployed and the pipeline set up to catch any new security vulnerabilities as soon as they appear. Congratulations!
