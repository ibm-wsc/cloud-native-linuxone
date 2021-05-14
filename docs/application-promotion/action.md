# Time to put it all in action for take 2

## Make a change in GitHub

1. Navigate to your GitHub fork

2. Choose to `Go to file`

    ![Go to file](../images/Part2/GoToFile.png)

3. Choose the following file (copy and paste box below image):

    ![Choose File](../images/Part2/ChooseFile.png)

    ``` bash
    src/main/resources/db/mysql/data.sql
    ```

4. Select to edit the file

    ![Edit File](../images/Part2/EditGitFile.png)

5. Change the file to add the pet field of your choice and commit it to your GitHub fork (description and copy and paste box are below image)

    ![Change File](../images/Part2/ChangeFile.png)

    1. Make a change using the pet type you want to add (example is a turtle) 

        !!! note
            The copy and paste box below can be entered on **line 24** with `enter` pressed after it to match the image above.

        !!! example "Turtle"
            ``` mysql
            INSERT IGNORE INTO types VALUES (7, 'turtle');
            ```

        !!! Tip "I want to add Willow, an awesome armadillo, not Yertle the turtle!"
            If you want to add something other than a turtle as an option, please change `turtle` to that animal (i.e. `armadillo`) in the mysql statement above. For the armadillo example, the statement becomes:

            ```mysql
            INSERT IGNORE INTO types VALUES (7, 'armadillo');
            ```

    2. Type in a commit message (you can make this whatever you want) and commit the change (example from image below)

        !!! example "Yertle the turtle"

            **Title**
            ``` bash
            Turtle Time
            ```

            **Extended Description**
            ``` bash
            I want to be able to add Yertle the turtle.
            ```

6. Take note of the git commit message and hash

    ![Github Hash and Message](../images/Part2/SeeCommitHash.png)

## Continuous Integration via OpenShift Pipelines

## Successfully Run Pipeline via GitHub
1. Visit the newly triggered pipeline run in the `Pipelines` menu in the UI

    ![Newly Triggered PipelineRun](../images/Part2/ViewGitPipelineRun.png)

2. View the pipeline run from the `Details` view

    ![PipelineRun Triggered](../images/Part2/PipelineTriggered.png)

    You can see the event listener has triggered the `PipelineRun` instead of a user this time.

3. You can see the variables populated with the correct values from Github in the `YAML` view of the pipeline run.

    ![Git Variables Populated](../images/Part2/GitVariablesExist.png)

4. Watch the results of your build pipeline run. It should complete successfully as in the pictures below.

    **Pipeline Run Success View Perspective:**

    ![Successful PipelineRun Details View](../images/Part2/PipelineSucceeded.png)

    !!! Success "Pipeline Run Details View"
        In the pipeline run `Details` view, you can see the pipeline run succeeded with all tasks having a green check mark. Additionally, observe that the event listener has triggered the `PipelineRun` instead of a user this time.

    **Pipeline Run Success Logs Perspective:**

    ![Successful PipelineRun Logs View 1](../images/Part2/PipelineRunLogsSucceeded.png)

    !!! Success "Pipeline Run Logs View 1"
        In the pipeline run `Logs` view, you can also see that the pipeline run tasks all have green check marks. Looking at the last task, you can see that the that the external connection check worked and the PetClinic application is available at the route printed in the logs. Additionally, you can see via the series of tasks marked with green checks that the dev deployment ran successfully and the system cleaned it up and ran the staging deployment successfully to complete the pipeline.
        
    ![Succeesful PipelineRun Logs View 2](../images/Part2/PipelineRunCommitLogs.png)

    !!! Success "Pipeline Run Logs View 2"
        When you switch to the `deploy-staging` task logs, by clicking on the `task` on the left hand side of the `Logs` view of the pipeline run, you see this was an automated build from git since the task prints out the `GIT_MESSAGE` that you typed in your commit word for word. (_Note: If you chose a different commit message that will show instead of the one displayed in the image above._).

### See Changes in Application

1. Navigate to the `Topology` view and open a new tab with your recently deployed `staging` version of the PetClinic application by clicking `Open URL`.

    ![PetClinic Visit Time](../images/Part2/PetclinicTimeAgain.png)

2. Navigate to the `Find Owners` tab

     ![PetClinic Initial View](../images/Part2/InitialViewPetClinic.png)

3. Choose to add a new owner

    ![Add Owner](../images/Part2/AddOwner.png)

4. Add the owner with details of your choice

    ![Dr. Seuss Owner Add](../images/Part2/AddSeus.png)

5. Choose to add one of the owner's pets

    ![Add Pet](../images/Part2/AddNewPet.png)

6. Fill in the pet's details and select the new type of pet you added (turtle for the example)

    ![Add Yertle the Turtle](../images/Part2/AddYertleTheTurtle.png)

7. View the newly created pet of the new type (Yertle the turtle for the example)

    ![Long Live Yertle](../images/Part2/LongLiveYertle.png)

## Summary :fireworks:

In this section, you made a change to your PetClinic application to add a new pet type of your choice and pushed the change to GitHub. This triggered a new pipeline run which built a new image for the application tagged with the git commit hash and displayed the commit message explaining the change the build was implementing. Next, your pipeline deployed this change to OpenShift in development, tested it internally and externally and then rolled it out to staging (where it was also tested automatically). Finally, you visited the application and used the new feature (new type of pet) by adding a pet of that type to a new owner successfully. In other words, you are off the ground and running with "cloud-native" CI/CD for your PetClinic application on IBM Z/LinuxONE! Congratulations!!!