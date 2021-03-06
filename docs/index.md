# Cloud Native LinuxONE Workshop 

Welcome to our Cloud Native LinuxONE workshop. Developers can leverage OpenShift to create continuous integration pipelines for Linux® workloads on IBM Z® and LinuxONE. You can quickly get up and running on OpenShift with a continuous workflow.

## Agenda

### Introductory Presentations

* 12:00 - 12:30pm ET - [Introduction to Kubernetes and OpenShift](presentations/Kubernetes_Openshift_Introduction.pdf)
* 12:30 - 01:00pm - [Introduction to Cloud Native DevOps](presentations/cloud_native_devops_introduction.pdf)

### Lab: Build and Deploy a Cloud Native DevOps Pipeline in OpenShift on IBM Z and LinuxONE

* 1pm - 1:15pm - [Introduction to Cloud Native DevOps Pipeline Lab](introduction.md)
* 1:15pm - 4pm
    * [Deploy PetClinic via OpenShift Pipelines](build-and-deploy/build_overview.md)
    * [Configure PetClinic's Integration and Deployment via OpenShift Pipelines to Meet Your Organization's Needs](full-dev-pipeline/configure_overview.md)[^1]
    * [Extend Pipeline to Upgrade from Development to Staging](application-promotion/promote_overview.md)

[^1]: For the purposes of this lab, you are fulfilling the requirements of a fictional organization. These requirements could change for your specific organization but would follow a similar pattern with different specifics.

### Introduction to RBAC and Container Registry, Q&A

* 4pm - 4:30pm - Introduction to RBAC and Hands-On with RBAC

### BONUS lab: Adding code scanning with SonarQube to your Pipeline

* 4:30pm - 5pm - [Configure SonarQube code analysis in your Pipeline](devsecops/devsecops.md)

## Acknowledgements
* Thanks to the following contributors:

    - Chee Yee for setting up the LinuxONE Community Cloud for the OpenShift Pipelines workflow

    - The `spring` developers for creating the [petclinic demo](https://projects.spring.io/spring-petclinic/){target="_blank" rel="noopener noreferrer"} and the `redhat-developer-demos` for sharing the `spring-petclinic` version of sample application we started with

## Key Contributors
* Barry Silliman
* Faraz Khan
* Rishi Patel
* William Doyle

## Workshop authors
* [Garrett Woodworth](mailto:garrett.lee.woodworth@ibm.com)
* [Jin VanStee](mailto:jinxiong@us.ibm.com)

--8<-- "includes/glossary.md"
