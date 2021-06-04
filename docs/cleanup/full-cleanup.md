# Environment Cleanup

In this section you will clean up the different things you made during the lab in order to free up resources for other projects you intend to embark on in the community cloud as well as for other users of the environment.

## Pipelines Section Cleanup

From the `Pipelines` section of the OpenShift UI, please complete the following cleanup tasks:

1. Delete the trigger for your pipeline

    1. Choose to remove the trigger

        ![Remove Trigger](../images/Cleanup/RemoveTrigger.png)

        1. Click the 3 dots
        2. Choose `Remove Trigger`

    2. Confirm the trigger removal

        ![Confirm Remove Trigger](../images/Cleanup/ConfirmRemoveTrigger.png)

        1. Choose your trigger from the dropdown menu
        2. Click `Remove`

2. Delete the pipeline

    ![Delete Pipeline](../images/Cleanup/DeletePipeline.png)

    1. Click the 3 dots
    2. Choose `Delete Pipeline`

## Topology Section Cleanup

From the `Topology` section of the OpenShift UI, please complete the following cleanup tasks:

1. Delete the `spring-petclinic-staging` deployment and its associated resources

    1. Right-click on the icon and choose `Delete Deployment`

        ![Delete Spring Petclinic Staging Deployment](../images/Cleanup/DeletePetClinicStaging.png)

    2. Click `Delete` (keep box checked to also delete dependent objects of this resource)

        ![Confirm Delete of Spring PetClinic Staging Deployment](../images/Cleanup/ConfirmDeletePetClinicStaging.png)

2. Delete `mysql` deployment config and its associated resources

    1. Right-click on the icon and choose `Delete DeploymentConfig`

        ![Delete MySQL Deployment Config](../images/Cleanup/DeleteDeploymentConfigMySQL.png)

    2. Click `Delete` (keep box checked to also delete dependent objects of this resource)

        ![Confirm Delete of MySQL DeploymentConfig](../images/Cleanup/ConfirmDeleteDeploymentConfigMySQL.png)

## :tada: Thank You for Cleaning Up! :tada:

--8<-- "includes/glossary.md"