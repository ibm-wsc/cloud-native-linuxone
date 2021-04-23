# It's time to open up your pet clinic for testing

In this section, we will build, test and deploy the Java web application for our pet clinic (PetClinic) and then automate the process using OpenShift Pipelines. This involves the following tasks:

1. [Get Your Pet Clinic Up and Running on OpenShift](upandrunning.md)
    - Deploying MySQL database
    - Building PetClinic with automated testing
    - Accessing PetClinic and adding your pet

2. [PetClinic Continuous Integration using OpenShift Pipelines](pipeline.md)
    - Automate PetClinic and MySQL deployment
    - Adding GitHub Integration and tag images
    - Make clean image from S2I build
    - Trigger pipeline with empty commit