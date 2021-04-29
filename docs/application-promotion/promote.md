# From dev to staging

## Deploy Staging 

1. We will use our existing `kustomize-deploy-resources` task to deploy the `staging` version of our 
OpenShift files in a new task.

    ![StagingAdd](../images/Part2/AddStaging.png) 
    
2. The only changes to make are

    Display Name:
    ``` bash
    kustomize-deploy-resources-staging
    ```

    and

    RELEASE_SUBDIR
    ``` bash
    overlay/staging
    ```

    to make the following configuration:

    ![Kustomize Staging Task](../images/Part2/KustomizeStaging.png)

2. Add workspace to `kustomize-deploy-resources` task 

    Save current pipeline edit and switch to `yaml` from pipeline menu.

    ![Switch to yaml](../images/Part1/SwitchYaml.png)

    !!! Info "Why are we editing yaml directly?"
        `Workspaces` are more versatile than traditional `PipelineResources` which is why we are using them. However, as the transition to workspaces continues, the OpenShift Pipeline Builder doesn't support editing the `Workspace` mapping from a pipeline to a task via the Builder UI so we have to do it directly in the yaml for now.

    Find the `kustomize-deploy-resources` and add the following workspace definition:

    ```
          workspaces:
          - name: source
            workspace: workspace
    ```

    ![Kustomize Dev Add Workspace](../images/Part2/KustomizeStagingWorkspace.png)

    Save the update

    ![Save Pipeline Edit Yaml](../images/Part1/PipelineUpdatedYaml.png)

    !!! note
        After the save message above appears you can then proceed to `Cancel` back to the pipeline menu.

## Rollout Staging

1. Edit the pipeline again and add a `deploy-staging` task with the openshift-client `ClusterTask`

    ![Deploy Staging Task](../images/Part2/DeployStagingTask.png)

2. Give the task the following parameters to mirror that of the dev-deploy task which waits for the dev release to rollout to complete:

    ![Staging Rollout](../images/Part2/DeployStagingParameters.png)

3. Save task