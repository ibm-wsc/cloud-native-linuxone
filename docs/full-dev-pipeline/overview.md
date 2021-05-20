# Configure PetClinic's Integration and Deployment Pipeline to Meet Your Organization's Needs[^1]

It's time to expand your pipeline to automate the integration and deployment process for your application (with specific configuration for your organization) using OpenShift Pipelines. This involves the following tasks:

1. [Automate PetClinic Build and Test for your Organization's Needs](pipeline.md)

    - Automate MySQL deployment using OpenShift template
    - Make clean image from S2I build to meet the security needs of your organization

2. [Automate PetClinic development deployment to meet your Organization's Needs](runpipeline.md)[^1]

    - Manage PetClinic deployment resources using KUSTOMIZE
    - Setup PetClinic image deployment automation with tagging based on source

[^1]: For the purposes of this lab, you are fulfilling the requirements of a fictional organization. These requirements could change for your specific organization but would follow a similar pattern with different specifics.