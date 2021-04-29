# Time to put it all in action for take 2

## Make a change in GitHub

1. Navigate to your GitHub fork

2. Choose to go to file

    ![Go to file](../images/Part2/GoToFile.png)

3. Choose a file such as the following:

    ``` bash
    src/main/resources/db/mysql/data.sql
    ```

4. Change the file

    ![Change File](../images/Part2/ChangeFile.png)

5. Commit the change to your GitHub fork

    ![Github Fork commit](../images/Part2/CommitChange.png)

6. See the commit hash and message

    ![Github Hash and Message](../images/Part2/SeeCommitHash.png)

## Continuous Integration via OpenShift Pipelines

1. View the newly triggered PipelineRun

    ![PipelineRun Triggered](../images/Part2/PipelineTriggered.png)

    We can see the event listener has triggered the `PipelineRun` instead of a user this time.

2. We can see the variables populated with the correct values from Github

    ![Git Variables Populated](../images/Part2/GitVariablesExist.png)

3. The PipelineRun successfully completes

    ![Successful PipelineRun](../images/Part2/PipelineSucceeded.png)

:smile: