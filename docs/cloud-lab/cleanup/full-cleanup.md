# Environment Cleanup

In this section you will clean up the different things you made during the lab in order to free up resources for other projects you intend to embark on in the community cloud as well as for other users of the environment.

## Pipelines Section Cleanup

From the `Pipelines` section of the OpenShift UI, please complete the following cleanup tasks:

1. Delete the trigger for your pipeline

    1. Choose to remove the trigger

        ![Remove Trigger](../../images/Cleanup/RemoveTrigger.png)

        1. Click the 3 dots
        2. Choose `Remove Trigger`

    2. Confirm the trigger removal

        ![Confirm Remove Trigger](../../images/Cleanup/ConfirmRemoveTrigger.png)

        1. Choose your trigger from the dropdown menu
        2. Click `Remove`

2. Delete the pipeline

    ![Delete Pipeline](../../images/Cleanup/DeletePipeline.png)

    1. Click the 3 dots
    2. Choose `Delete Pipeline`

## Topology Section Cleanup

From the `Topology` section of the OpenShift UI, please complete the following cleanup tasks:

1. Delete the `spring-petclinic-staging` deployment and its associated resources

    1. Right-click on the icon and choose `Delete Deployment`

        ![Delete Spring Petclinic Staging Deployment](../../images/Cleanup/DeletePetClinicStaging.png)

    2. Click `Delete` (keep box checked to also delete dependent objects of this resource)

        ![Confirm Delete of Spring PetClinic Staging Deployment](../../images/Cleanup/ConfirmDeletePetClinicStaging.png)

2. Delete `mysql` deployment config and its associated resources

    1. Right-click on the icon and choose `Delete DeploymentConfig`

        ![Delete MySQL Deployment Config](../../images/Cleanup/DeleteDeploymentConfigMySQL.png)

    2. Click `Delete` (keep box checked to also delete dependent objects of this resource)

        ![Confirm Delete of MySQL DeploymentConfig](../../images/Cleanup/ConfirmDeleteDeploymentConfigMySQL.png)

## Delete Leftover Resources

1. Click on the `Search` tab from the OpenShift menu

    ![Search Tab](../../images/Cleanup/ClickSearchTab.png)

2. Click on the `Resources` drop down menu

    ![Resources DropDown](../../images/Cleanup/ClickResourceDropdown.png)

3. Check (click the checkbox) the following resources (you can search for them individually) 

    1. Secret

        ```
        Secret
        ```

    2. Route (route.openshift.io/v1)

        ```
        Route
        ```

    3. Service (core/v1)

        ```
        Service
        ```

    4. ImageStream

        ```
        ImageStream
        ```

    5. ConfigMap

        ```
        ConfigMap
        ```

    6. PersistentVolumeClaim

        ```
        PersistentVolumeClaim
        ```

    ![Select Resource](../../images/Cleanup/SelectResources.png)
    
4. Select `Name` for the filter

    ![Select Name Filter](../../images/Cleanup/SelectNameFilter.png)

5. Delete the resources for `mysql`

    1. Search for the *Name* `mysql`

        ```
        mysql
        ```

        ![SearchMySQL](../../images/Cleanup/SearchMySQL.png)

    2. Click on the 3 dots to the right of the first individual resource

        ![Delete Resource](../../images/Cleanup/DeleteResource.png)
    
    3. Confirm the deletion in the following window

        ![Confirm Delete Resource](../../images/Cleanup/ConfirmDeleteResource.png)
    
    4. Repeat this for all of the other resources that appear for `mysql`

        !!! Tip
                This should include 2 secrets (`mysql` and one starting with `mysql-ephemeral-parameters-`) and 1 `mysql` service.
        
6. Delete the resources for the *Name* `spring-petclinic`

    ```
    spring-petclinic
    ```

    ![SearchSpringPetclinic](../../images/Cleanup/SearchSpringPetclinic.png)

    !!! Tip
            This should include 2 secrets, 1 route, 1 service, 2 imageStreams, and 2 configMaps.

7. Delete the resources for the *Name* `event-listener`

    ```
    event-listener
    ```

    ![SearchEventListener](../../images/Cleanup/SearchEventListener.png)

    !!! Tip
            This should include 1 route.

8. Delete the `persistentVolumeClaim` associated with your pipeline

    1. Leave the *Name* field blank and go to the *PersistentVolumeClaim* section of the page

    2. Delete the `persistentVolumeClaim` (if there are more than 1, delete the 1 created for this lab [you can look at the creation time to double check this])

    !!! Tip
            This should include 1 `persistentVolumeClaim`.

## GitHub Section Cleanup

Finally, you will cleanup the GitHub fork you made on your GitHub account with the following steps:

1. Navigate to the settings for your forked GitHub repository

    ![Pick GitHub Settings](../../images/Cleanup/GitHubSettings.png)

2. Scroll to the bottom of the settings page (to the `danger zone`) and choose to delete your repository

    ![Choose to Delete Repository](../../images/Cleanup/ChooseDeleteRepository.png)

3. Confirm repository delete (retyping your forked repository's name)

    ![Confirm Repository Delete](../../images/Cleanup/ConfirmDeleteRepository.png)
    
## :tada: Thank You for Cleaning Up! :tada: