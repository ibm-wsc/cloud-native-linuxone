*[OCP]: OpenShift Container Platform
*[Tekton]: a cloud native (Kubernetes based) open-source framework for creating CI/CD systems. It serves as the base of OpenShift Pipelines, allowing developers to build, test, and deploy across cloud providers and on-premise systems
*[OpenShift]: (OpenShift Container Platform / OCP): an open source container application platform based on the Kubernetes container orchestrator for enterprise application development and deployment
*[Kubernetes]: (k8s): an open-source system for automating deployment, scaling, and management of containerized applications.
*[JVM]: (Java Virtual Machine): the runtime engine of the Java Platform. It enables Java programs and programs compiled into Java bytecode to run on any computer that has a native JVM
*[OCI]: (Open Container Initiative): an open governance structure for the express purpose of creating open industry standards around container formats and runtimes
*[repo]: repository
*[UI]: user interface
*[container image]: application and all of its software dependencies packaged into a tarball that runs as a container using a runtime engine such as Docker or Podman.
*[container]: a standard unit of software that packages up code and all its dependencies so the application runs quickly and reliably from one computing environment to another
*[kubelet]: an agent that runs on each node of a Kubernetes cluster. It "manages" its node by reading the container manifests, and ensuring the defined containers are started and running.
*[event listener]: (EventListener) is a Kubernetes custom resource that allows users a declarative way to process incoming HTTP based events with JSON payloads.
*[git push]: updates the remote branch with local commits. You can also think of git push as update or publish.
*[git clone]: clones a repository into a newly created directory, creates remote-tracking branches for each branch in the cloned repository (visible using git branch --remotes), and creates and checks out an initial branch that is forked from the cloned repository’s currently active branch.
*[CI/CD]: (Continuous Integration / Continuous Deployment): CI = Automate the building and testing of new code to integrate it into the codebase. CD = Automate releasing (deploying the build) into different environments. Together, your application is seamlessly updated, tested and deployed into your environments when new code is published (via Source Code Management software such as Git Hub).
*[SCM]: (Source Code Management): Software (i.e. git) used to track the changes to a code base over time and merge contributions from different collaborators, resolving conflicts as they arise.
*[OpenShift Pipelines]: Kubernetes-native CI/CD based on the Tekton open-source project and integrated with OpenShift via the OpenShift UI.
*[LinuxONE]: IBM LinuxONE is a family of servers built to host enterprise Linux applications using the s390x architecture that it shares with the IBM Z family of servers.
*[Spring Boot]: Java open-source framework for microservices
*[microservices]: loosely coupled architecture where the different pieces (services) of an application can be maintained, updated and scaled independently.
*[Spring MVC]: (Spring Model View Controllers): 
*[Spring Data JPA]: (Spring Data Java Persistent API): 
*[cloud native]: an application that takes full advantage of cloud capabilities due to a microservices based architecture that can be scaled up and down independently.
*[s390x optimizations]: optimizations for the s390x architecture that the LinuxONE family of servers uses. They enable software to run better on these servers.
*[ClusterTask]: a Task scoped to the entire cluster instead of a single project. 
*[Task]: collection of Steps that you define and arrange in a specific order of execution to build a Pipeline. It executes as a Pod on your Kubernetes cluster.
*[Trigger]: captures and processes events from an external source (webhook) and maps the incoming data to a set of predefined parameters via a TriggerTemplate to create a new PipelineRun. 
*[trigger]: captures and processes events from an external source (webhook) and maps the incoming data to a set of predefined parameters via a TriggerTemplate to create a new PipelineRun. 
*[Webhook]: sends a payload to the specified URL when a specific event occurs (such as a GitHub push)
*[webhook]: sends a payload to the specified URL when a specific event occurs (such as a GitHub push)
*[DevOps]: methodology that accelerates delivery and improves quality of software through automation (CI/CD) that eliminates the friction between software development and IT operations