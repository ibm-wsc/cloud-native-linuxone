# PetClinic + OpenShift Pipelines

Now that PetClinic is up and running on your OpenShift cluster, it's time to add functionality to your pipeline to achieve basic continuous integration. The OpenShift pipeline you created in the [PetClinic Up and Running](upandrunning.md) uses [Tekton](https://tekton.dev){target="_blank" rel="noopener noreferrer"} to run a series of tasks (each with one or more steps) to accomplish a workflow (pipeline). You will use the Pipeline Builder UI built into OpenShift to quickly and easily craft a pipeline for your project.

!!! info "Why OpenShift Pipelines?"
    - Portable: OpenShift resources defined via yaml files -> portable across OpenShift clusters

    - Low Resource Usage: Containers spin up when triggered -> resources only used when needed

    - Configurable: Can tailor overall pipeline and individual tasks to needs of your enterprise/organization 
    
    - Ease of Use: Pipeline Builder UI and built-in cluster resources (i.e. `ClusterTasks`, ClusterTriggerBindings`, etc.) enable you to easily create a pipeline and export the yaml files with minimal knowledge

## PetClinic Pipeline

When you deployed the PetClinic application using the `From Git` option in the [PetClinic Up and Running](upandrunning.md) section, you chose to create a basic pipeline. You'll start with this pipeline and edit it to add new functionality for your use case. 

Navigate to the `Pipelines` tab in the `Developer` perspective on the left and then click the three dots to the right of the pipeline name (`spring-petclinic`) and choose `Edit Pipeline`. ![Pipeline Image](../images/Part1/EditNewPipeline.png) 

## Ensure MySQL Database Deployed for each Run

This will bring you to the Pipeline Builder UI where you can edit your pipeline. Here you will make sure the MySQL database is configured according to your specification before the `build` task.

1. Add a `mysql-deploy` task in parallel to the `git-fetch` task. 
    ![Add Parallel MySQL](../images/Part1/mySQL_ParallelTask.png) 

    !!! info "Why is `mysql-deploy` in Parallel?"
        This ensures MySQL is in place for each `PetClinic` application build (which would fail without it).  

    Click `Select Task` in the middle of the rectangle of the new task and choose the `openshift-client` task from the dropdown menu. 

    ![ChooseOpenShift Client](../images/Part1/Choose_OpenShiftClientTask.png)

    Click on the middle of the oval of the `openshift-client` task to enter values for it (copy and paste boxes below image).

    ![Click OpenShift Client](../images/Part1/OpenShiftClientOval.png)

    !!! Tip
        Once you add a specific task (i.e. `openshift-client`), clicking on the oval of the task will enable you to edit its default values for your needs.

    Give the task the following parameters to ensure the MySQL database is available with the necessary configuration:

    ![MySQL Task](../images/Part1/MySQLTemplateTask.png)

    **Display Name**

    ``` bash
    mysql-deploy
    ```

    **SCRIPT**

    ``` bash
    oc process openshift//mysql-ephemeral -p MYSQL_USER=petclinic -p MYSQL_PASSWORD=petclinic -p MYSQL_ROOT_PASSWORD=petclinic -p MYSQL_DATABASE=petclinic | oc apply -f -
    ```

    !!! note "Simply Click Away"
        Once you have entered the string into the `SCRIPT` section, just click away (i.e. on a regular section of the page) to get the configuration menu to go away and keep the new value(s) you just entered for the task.

    !!! Tip "What is `oc process` doing?"
        `oc process` is processing the [OpenShift template](https://docs.openshift.com/container-platform/4.7/openshift_images/using-templates.html#templates-overview_using-templates){target="_blank" rel="noopener noreferrer"} for the `mysql-ephemeral` database with the parameters given via a series of `-p` arguments and finally `oc apply -f -` ensures that any missing components will be recreated.

    !!! warning "No help please!"
        Make sure `help` is deleted from the `ARGS` section (it will be greyed out once deleted) or bad things will happen (i.e. the help screen will come up instead of the proper command running). 

2. Add a `mysql-rollout-wait` task

    You need to make sure that `mysql` is fully deployed before your `build` task begins. In order to achieve this, you will use the OpenShift Client again and wait for the `rollout` of the `mysql` `deploymentConfig` to complete after the `mysql-deploy` task. Add a sequential task after `mysql-deploy`:

    ![mysql sequential task](../images/Part1/mySQL_SequentialTask.png)

    `Select Task` as `openshift-client` like before and then fill out the task with the following parameters (copy and paste boxes below image for changes):

    ![mysql-check task](../images/Part1/MySQLRolloutWait.png)

    **Display Name**

    ``` bash
    mysql-rollout-wait
    ```

    **ARGS**

    ``` bash
    rollout
    ```
    
    ``` bash
    status
    ```

    ``` bash
    dc/mysql
    ```

    !!! warning "No help please!"
        Make sure `help` is deleted from the `ARGS` section (it will be greyed out once deleted) or bad things will happen (i.e. the help screen will come up instead of the proper command running).

    !!! Tip "What the ARGS?"
        You may be wondering why you used the `SCRIPT` section in the `mysql-deploy` task for the entire command, but now are using the `ARGS` to individually list each argument of the command? Both work and so you are going through both methods here. On the one hand, the `SCRIPT` method is easier to copy and paste and looks the same as it would entered on the command line. On the other hand, the `ARGS` method adds readability to the task. Choose whichever method you prefer, though beware of input errors  with the `ARGS` method for long commands. _FYI: The equivalent `SCRIPT` command for the `mysql-rollout-wait` task is_:

        ``` bash
        oc rollout status dc/mysql
        ```

:tada: Now your `mysql-deploy` and `mysql-rollout` tasks will have `MySQL` alive and well for the `build` task!

## Make Clean Image from S2I build

The `s2i-java-11` image is very convenient for making an image from source code. However, the simplicity that gives it value can make it fail at meeting the needs of many organizations by itself. In your case, you will take the artifacts from the s2i image and copy them to a new Docker image that can meet all your needs to get the best of both worlds. You'll create an optimized image starting from a compact `openj9` java 11 base and employing [the advanced layers feature in spring](https://spring.io/blog/2020/01/27/creating-docker-images-with-spring-boot-2-3-0-m1#layered-jars){target="_blank" rel="noopener noreferrer"} that optimizes Docker image caching with the [final-Dockerfile](https://raw.githubusercontent.com/ibm-wsc/spring-petclinic/main/final-Dockerfile){target="_blank" rel="noopener noreferrer"} in the [ibm-wsc/spring-petclinic](https://github.com/ibm-wsc/spring-petclinic){target="_blank" rel="noopener noreferrer"} git repository you forked. 

1. Add `Buildah` task

    Add the `buildah` task as a sequential task after the `build` task.

    ![Add Buildah Task](../images/Part1/AddBuildahTask.png)

2. Configure `buildah` task

    !!! Tip
        Each value that you need to configure is listed below with the value in a click-to-copy window (other values can be left alone to match the image)

    ![Buildah Task Values](../images/Part1/ProducingCleanImageBuildah2.png)

    DISPLAY NAME:
    ```
    clean-image
    ```

    IMAGE:
    ```
    $(params.IMAGE_NAME)-minimal:$(params.COMMIT_SHA)
    ```

    DOCKERFILE:
    ```
    ./final-Dockerfile
    ```

    TLSVERIFY:
    ```
    false
    ```

    BUILD_EXTRA_ARGS:
    ```
    --build-arg PETCLINIC_S2I_IMAGE=$(params.IMAGE_NAME)
    ```

3. Add `GIT_MESSAGE`, and `COMMIT_SHA` parameters to the pipeline

    Click `Add Parameter` twice ...

    ![Add Parameter](../images/Part1/AddParameter.png)

    and then fill in the parameter details for `GIT_MESSAGE` and `COMMIT_SHA` (copy and paste boxes below image)

    ![Add Image and Git Parameters](../images/Part1/AddParameters.png)

    **GIT_MESSAGE**

    `GIT_MESSAGE` Parameter Name:
    ```
    GIT_MESSAGE
    ```
    `GIT_MESSAGE` Parameter Description:
    ```
    Git commit message if triggered by Git, otherwise it's a manual build
    ```
    `GIT_MESSAGE` Parameter Default Value
    ```
    This is a manual build (not triggered by Git)
    ```

    **COMMIT_SHA**

    `COMMIT_SHA` Parameter Name:
    ```
    COMMIT_SHA
    ```
    `COMMIT_SHA` Parameter Description:
    ```
    SHA of Git commit if triggered by Git, otherwise just update manual tag
    ```
    `COMMIT_SHA` Parameter Default Value:
    ```
    manual
    ```

    !!! Tip
        Save parameters when done with entry by clicking on blue `SAVE` box before moving onto step 4. If blue `SAVE` box doesn't appear (is greyed out) delete extra blank parameters you may have accidentally added with the `-`.

4. Add workspace to `clean-image` task 

    1. Save current pipeline edit and switch to `YAML` from pipeline menu.

        ![Switch to yaml](../images/Part1/SwitchYaml.png)

        !!! info "Why are you editing yaml directly?"
            `Workspaces` are more versatile than traditional `PipelineResources` which is why you are using them. However, as the transition to workspaces continues, the OpenShift Pipeline Builder doesn't support editing the `Workspace` mapping from a pipeline to a task via the Builder UI so you have to do it directly in the yaml for now.

    2. Find the `clean-image-task` and add the following workspace definition:

        ``` yaml
              workspaces:
              - name: source
                workspace: workspace
        ```

        ![Clean Image Workspace](../images/Part1/AddWorspaceProducingCleanImage.png)

    3. Save the update

        ![Save Pipeline Edit Yaml](../images/Part1/PipelineUpdatedYaml.png)

        !!! note
            After the save message above appears you can then proceed to `Cancel` back to the pipeline menu.

## Manage resource across environments with Kustomize

[Kustomize](https://kustomize.io){target="_blank" rel="noopener noreferrer"} is a tool for customizing Kubernetes resource configuration.

!!! note "From the documentation overview"
    Kustomize traverses a Kubernetes manifest to add, remove or update configuration options without forking. It is available both as a standalone binary and as a native feature of kubectl. See the [Introducing Kustomize Kubernetes Blog Post](https://kubernetes.io/blog/2018/05/29/introducing-kustomize-template-free-configuration-customization-for-kubernetes/){target="_blank" rel="noopener noreferrer"} for a more in-depth overview of Kustomize and its purpose.

As part of doing things the "cloud-native" way you will be using Kustomize to manage resource changes across your `dev` and `staging` environments as well as injecting information from your pipeline (such as newly created image information with git commits) into your Kubernetes (OpenShift) resources. 

To see how you use Kustomize, see the Kustomize configuration in your GitHub code in the subdirectories of the [ocp-files directory](https://github.com/ibm-wsc/spring-petclinic/tree/main/ocp-files){target="_blank" rel="noopener noreferrer"}.

For more information on how kubectl (and oc through kubectl) integrates Kustomize, see the [kubectl documentation](https://kubectl.docs.kubernetes.io/guides/introduction/kustomize/){target="_blank" rel="noopener noreferrer"}.

### Creating Custom Task for Kustomize

Since there is no `ClusterTask` defined for Kustomize, you will create a custom task for this purpose. It will change into the Kustomize directory, run a Kustomize command on the directory, and then apply the files from the directory using the built-in Kustomize functionality of the oc command line tool (via kubectl's Kustomize support)

1. Copy the `kustomize` task using the following definition (copy by clicking on the copy icon in the top right of the box below):

    ``` yaml
    apiVersion: tekton.dev/v1beta1
    kind: Task
    metadata:
      name: kustomize
    spec:
      description: >-
        This task runs commands against the cluster where the task run is being
        executed.

        Kustomize is a tool for Kubernetes native configuration management.  It
        introduces a template-free way to customize application configuration that
        simplifies the use of off-the-shelf applications.  Now, built into kubectl
        as apply -k and oc as oc apply -k.
      params:
      - default: ocp-files
        description: The directory where the kustomization yaml file(s) reside in the git directory
        name: KUSTOMIZE_DIR
        type: string
      - default: base
        description: subdirectory of KUSTOMIZE_DIR used for extra configuration of current resources
        name: EDIT_SUDBDIR
        type: string
      - default: overlay/dev
        description: subdirectory of KUSTOMIZE_DIR used for specifying resources for a specific release such as dev or staging
        name: RELEASE_SUBDIR
        type: string
      - default: kustomize --help
        description: The Kustomize CLI command to run
        name: SCRIPT
        type: string
      steps:
      - image: 'docker.io/gmoney23/kustomize-s390x:v4.1.2'
        name: kustomize
        resources:
          limits:
            cpu: 200m
            memory: 200Mi
          requests:
            cpu: 200m
            memory: 200Mi
        workingDir: "$(workspaces.source.path)/$(params.KUSTOMIZE_DIR)/$(params.EDIT_SUDBDIR)"
        script: $(params.SCRIPT)
      - image: 'image-registry.openshift-image-registry.svc:5000/openshift/cli:latest'
        name: apply-oc-files
        resources:
          limits:
            cpu: 200m
            memory: 200Mi
          requests:
            cpu: 200m
            memory: 200Mi
        script: oc apply -k "$(workspaces.source.path)/$(params.KUSTOMIZE_DIR)/$(params.RELEASE_SUBDIR)"
      workspaces:
      - name: source
        description: The git source code
    ```

2. Create the `kustomize` Task
    
    a. Click `Import YAML` to bring up the box where you can create Kubernetes resource definitions from yaml

    b. Paste the `kustomize` Task into the box
    
    c. Scroll down and click `Create` to create the `kustomize` Task 

    ![Create Kustomize Task](../images/Part1/CreateKustomizeTask.png)

You should now see the created `kustomize` Task. Navigate back to the `Pipelines` section of the OpenShift UI and go back to editing your pipeline.

![Back to Pipelines](../images/Part1/BackToPipelines.png)

### Add Kustomize Task to Pipeline

1. Add a sequential task after `clean-image` and when you `Select Task` choose the `kustomize` task. 

    ![Add Kustomize task to Pipeline Dev](../images/Part1/AddKustomizeTaskPipeline.png)

2. Configure `kustomize` task

    Since your initial deploy will be for the `dev` environment, the only values you need to change are the `Display Name` and the `SCRIPT` (copy and paste boxes below image):

    ![Kustomize Configure 1](../images/Part1/KustomizeDevChoices.png)

    **Display Name**
    
    ``` bash
    kustomize-dev
    ```

    **SCRIPT**

    ``` bash
    kustomize edit set image spring-petclinic=$(params.IMAGE_NAME)-minimal:$(params.COMMIT_SHA)
    ```
    
3. Save pipeline

4. Add workspace to `kustomize-dev` task 

    1. Save current pipeline edit and switch to `YAML` from pipeline menu.

    ![Switch to yaml](../images/Part1/SwitchYaml.png)

    !!! Info "Why are you editing yaml directly?"
        `Workspaces` are more versatile than traditional `PipelineResources` which is why you are using them. However, as the transition to workspaces continues, the OpenShift Pipeline Builder doesn't support editing the `Workspace` mapping from a pipeline to a task via the Builder UI so you have to do it directly in the yaml for now.

    2. Find the `kustomize-dev` and add the following workspace definition:

        ``` yaml
              workspaces:
              - name: source
                workspace: workspace
        ```

    ![Kustomize Dev Add Workspace](../images/Part1/AddWorkspaceKustomizeDev.png)

    3. Save the update

    ![Save Pipeline Edit Yaml](../images/Part1/PipelineUpdatedYaml.png)

    !!! note
        After the save message above appears you can then proceed to `Cancel` back to the pipeline menu.

## Clean Old PetClinic Instances at the Beginning of a Run

1. Go back to editing your pipeline via `Actions -> Edit Pipeline`

    ![Actions Edit Pipeline](../images/Part1/ActionsEditPipeline.png)

2. Add a `Task` named `cleanup-resources` sequentially at the beginning of the pipeline before `fetch-repository` (using the `openshift-client` ClusterTask).

    ![add cleanup sequential](../images/Part1/AddSequentialTask.png)

3. Configure the task with the following parameters (copy and paste boxes below image for changes):

    ![cleanup resources](../images/Part1/CleanupResourcesTask.png)

    **Display Name**

    ``` bash
    cleanup-resources
    ```

    **SCRIPT**

    ``` bash
    oc delete deployment,cm,svc,route -l app=$(params.APP_NAME) --ignore-not-found
    ```

    and an empty `ARGS` value.

    !!! warning "No help please!"
        Make sure `help` is deleted from the `ARGS` section (it will be greyed out once deleted) or bad things will happen (i.e. the help screen will come up instead of the proper command running). 

## Update Deploy Task to deploy-dev

1. Click on the `deploy` Task at the end of the pipeline and change the following parameters to the corresponding values (copy and paste boxes below image):

    ![Deploy Dev Task Completed](../images/Part1/DeployDevCompleted.png)

    **Display Name**
    ``` bash
    deploy-dev
    ```
    
    **Script**
    ``` bash
    echo "$(params.GIT_MESSAGE)" && oc $@
    ```

    **Last Arg**

    From `deploy/$(params.APP_NAME)` to:

    ``` bash
    deploy/spring-petclinic-dev
    ```

2. `Save` your pipeline!

## Run the Updated Pipeline

1. Go to `Actions` -> `Start` in the right hand corner of the pipeline menu

    ![Actions Start Pipeline](../images/Part1/StartPipelineManually.png)

2. Manually trigger a `PipelineRun` by accepting the default values and clicking on `Start`.

    !!! Note "Persistent Volume Claim Note"
        Please select a `PersistentVolumeClaim` if it is not already filled out for you to complete your pipeline. If it is already filled out for you then jump right to starting the pipeline.

    ![Trigger Pipeline Manual](../images/Part1/PipelineExampleManualParameters.png)

3. Watch the results of your build pipeline run. It should complete successfully as in the pictures below.

    **Pipeline Run Success View Perspective:**

    ![Pipeline Run View](../images/Part1/PipelineRolloutRunView.png)

    !!! Success "Pipeline Run Details View"
        In the pipeline run `Details` view, you can see the pipeline run succeeded with all tasks having a green check mark. Additionally, the pipeline run in the image was `Triggered By` a user versus an automated source such as an event listener watching for a GitHub push...

    **Pipeline Run Success Logs Perspective:**

    ![Pipeline Run Logs](../images/Part1/PipelineRolloutLogsView.png)

    !!! Success "Pipeline Run Logs View"
        From the pipeline run `Logs` view you can see that the pipeline run tasks all have green check marks and that this was a manual build (from the message in the log output of the final [`deploy-dev`] task).

:thumbsup: