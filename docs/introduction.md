# Cloud Native Workshop Introduction
You will build and deploy an application (the cloud native way) using OpenShift Container Platform and its CI/CD workflow OpenShift Pipelines.

## Lab Overview
You will set up a virtual pet clinic (based on the classic spring boot demo referenced in the main [documentation](https://projects.spring.io/spring-petclinic/){target="_blank" rel="noopener noreferrer"} running on LinuxONE using source code on GitHub and OpenShift Pipelines to seamlessly update, test, and deploy your pet clinic.

This lab is broken into three parts:

1. Building and deploying the PetClinic Java application with OpenShift Pipelines

2. Configuring PetClinic's integration and deployment pipeline to meet your organization's needs[^1]

3. Promoting PetClinic from development to staging with testing and GitHub integration (adding the C [continuous] in CI/CD)

Throughout your journey across the three parts listed above, you will slowly construct the following architecture:

![Cloud Native Workshop Architecture Overview](images/IntroSection/CloudNativeArchitectureDiagram.png)

[^1]: For the purposes of this lab you are fulfilling the requirements of a fictional organization. These requirements could change for your specific organization but would follow a similar pattern with different specifics.

--8<-- "includes/glossary.md"