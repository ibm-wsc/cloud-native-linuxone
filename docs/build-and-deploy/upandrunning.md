# Getting your PetClinic Open with a Shift (Title Placeholder for something less cheesy)

For this workshop we will be using the iconic Spring PetClinic application. The Spring PetClinic is a sample application designed to show how the Spring stack can be used to build simple, but powerful database-oriented applications. [The official version of PetClinic](https://github.com/spring-projects/spring-petclinic) demonstrates the use of Spring Boot with Spring MVC and Spring Data JPA. 

For this workshop, we will not be focusing on the ins and outs of the PetClinic application itself, but rather on leveraging OpenShift tooling to build a PetClinic cloud-native application and a DevOps pipeline for the application.

We will start by building our PetClinic application from the source code and connecting it to a mysql database.

## Deploying MySQL database

**1.** First, we need to setup our mysql database. Luckily, this is very easy on OpenShift with the mysql template available from the main developer topology window. Follow the steps in the diagram below to bring up the available database options.

![Add DB](upandrunningimages/devselectdatabase.png)

**2.** Next, select the `MySQL (Ephemeral)` tile. Note we are choosing the ephemeral option because at this point we do not care to persist the database beyond the life of the container.

![Select MySQL](upandrunningimages/selectmqephemeral.png)

**3.** Click on instantiate template.

![Select DB](upandrunningimages/instantiatetemplate.png)

**4.** Fill the wizard with the parameters as shown in the image below:

![DB parameters](upandrunningimages/mysqlparameters.png)

Click the `Create` button. 

Again, we are using the **Ephemeral** implementation because this a short-lived demo and we do not need to retain the data.  

In a production system, you will most likely be using a permanent MySQL instance. This stores the data in a Persistent Volume (basically a virtual hard drive), meaning the MySQL pod can be destroyed and replaced with the data remaining intact.

A minute or two later, in the `Topology` view of your OpenShift Console, you should see `mysql` in running state.

![DB running](upandrunningimages/mysqlrunning.png)

## Building and Deploying PetClinic Application

There are multiple ways OpenShift enables cloud-native application developers to package up their applications and deploy them. For PetClinic, we will be building our application image from source, leveraging OpenShift's S2I (Source to Image) capability. This allows us to quickly test the building, packaging, and deployment of our application, and gives us the option to create a DevOps pipeline from this workflow. It's a good way to start to  understand how OpenShift Pipelines work.

**1.** Start with choosing Add From Git:

![from git](upandrunningimages/fromgit.png)

**2.** Enter `https://github.com/ibm-wsc/spring-petclinic` in the `Git Repo URL` field. Expand the `Show Advanced Git Options` section, and type in `main` for the `Git Reference`. This tells OpenShift which GitHub repo and branch to pull the source code from.

![git url](upandrunningimages/giturl.png)

**3.** Scroll down to the Builder section. Select the `OpenJ9` tile and select `openj9-11-el8` as the builder image version. As you can see OpenShift offers many different builder images to help you build images from a variety of programming languages. For Java on Z, the recommended JVM is `OpenJ9` because it has built-in s390x optimizations as well as container optimizations.

![builder image](upandrunningimages/builderimage.png)

**4.** In the General section, put in the following entries for Application Name and Name. 

![app name](upandrunningimages/applicationname.png)

**5.** Scroll down to the  Pipelines section, and check off the box next to `Add pipeline`. You can also expand the `Show pipeline visualization` section to see a visual of the build pipeline.

![add pipe](upandrunningimages/addpipelines.png)

**6.** We are almost there! We will need to configure a couple of Advanced Options. First, click on `Routing` in the Advanced Options section to expand the Routing options.

![open routing panel](upandrunningimages/routinglink.png)

**7.** In the Routing options section, only fill out the Security options as follows. You can leave the rest alone. These options will enable only TLS access to your PetClinic application.

![routing options](upandrunningimages/routingpanel.png)

**8.** 


