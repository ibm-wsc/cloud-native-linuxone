# It's time to open up your pet clinic for testing

In this section, we will build, test and deploy the Java web application for your pet clinic (PetClinic) and then automate the process using OpenShift Pipelines. This involves the following tasks:

1. [Get Your Pet Clinic Up and Running on OpenShift](upandrunning.md)
    - Deploying MySQL database
    - Building PetClinic with automated testing
    - Accessing PetClinic and adding an owner

2. [Create Your PetClinic Continuous Integration/Continuous Deployment Flow using OpenShift Pipelines](pipeline.md)
    - Automate MySQL deployment using OpenShift template
    - Make clean image from S2I build
    - Manage PetClinic deployment resources across dev and staging using KUSTOMIZE
    - Setup PetClinic image deployment automation with tagging based on source