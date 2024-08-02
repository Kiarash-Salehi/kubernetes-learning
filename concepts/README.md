# kubernetes-learning

## What is Kubernetes

[Kubernetes](https://kubernetes.io/docs/concepts/overview/), also known as K8s, is an open source system for automating deployment, scaling, and management of containerized applications.

Kubernetes is a portable, extensible, open source platform for managing containerized workloads and services, that facilitates both declarative configuration and automation. It has a large, rapidly growing ecosystem. Kubernetes services, support, and tools are widely available.

The name Kubernetes originates from Greek, meaning helmsman or pilot. K8s as an abbreviation results from counting the eight letters between the "K" and the "s". Google open-sourced the Kubernetes project in 2014. Kubernetes combines over 15 years of Google's experience running production workloads at scale with best-of-breed ideas and practices from the community.

## How application deployments used to be done

Let's take a look at why Kubernetes is so useful by going back in time.

![Deployment evolution](./img/1.svg)
Deployment evolution

**Traditional deployment era**: Early on, organizations ran applications on physical servers. There was no way to define resource boundaries for applications in a physical server, and this caused resource allocation issues. For example, if multiple applications run on a physical server, there can be instances where one application would take up most of the resources, and as a result, the other applications would underperform. A solution for this would be to run each application on a different physical server. But this did not scale as resources were underutilized, and it was expensive for organizations to maintain many physical servers.

**Virtualized deployment era**: As a solution, virtualization was introduced. It allows you to run multiple Virtual Machines (VMs) on a single physical server's CPU. Virtualization allows applications to be isolated between VMs and provides a level of security as the information of one application cannot be freely accessed by another application.

Virtualization allows better utilization of resources in a physical server and allows better scalability because an application can be added or updated easily, reduces hardware costs, and much more. With virtualization you can present a set of physical resources as a cluster of disposable virtual machines.

Each VM is a full machine running all the components, including its own operating system, on top of the virtualized hardware.

**Container deployment era**: Containers are similar to VMs, but they have relaxed isolation properties to share the Operating System (OS) among the applications. Therefore, containers are considered lightweight. Similar to a VM, a container has its own filesystem, share of CPU, memory, process space, and more. As they are decoupled from the underlying infrastructure, they are portable across clouds and OS distributions.

Containers have become popular because they provide extra benefits, such as:

- Agile application creation and deployment: increased ease and efficiency of container image creation compared to VM image use.
- Continuous development, integration, and deployment: provides for reliable and frequent container image build and deployment with quick and efficient rollbacks (due to image immutability).
- Dev and Ops separation of concerns: create application container images at build/release time rather than deployment time, thereby decoupling applications from infrastructure.
- Observability: not only surfaces OS-level information and metrics, but also application health and other signals.
- Environmental consistency across development, testing, and production: runs the same on a laptop as it does in the cloud.
- Cloud and OS distribution portability: runs on Ubuntu, RHEL, CoreOS, on-premises, on major public clouds, and anywhere else.
- Application-centric management: raises the level of abstraction from running an OS on virtual hardware to running an application on an OS using logical resources.
- Loosely coupled, distributed, elastic, liberated micro-services: applications are broken into smaller, independent pieces and can be deployed and managed dynamically – not a monolithic stack running on one big single-purpose machine.
- Resource isolation: predictable application performance.
- Resource utilization: high efficiency and density.

## Why you need Kubernetes and what it can do

Containers are a good way to bundle and run your applications. In a production environment, you need to manage the containers that run the applications and ensure that there is no downtime. For example, if a container goes down, another container needs to start. Wouldn't it be easier if this behavior was handled by a system?

That's how Kubernetes comes to the rescue! Kubernetes provides you with a framework to run distributed systems resiliently. It takes care of scaling and failover for your application, provides deployment patterns, and more. For example: Kubernetes can easily manage a canary deployment for your system.

Kubernetes provides you with:

- **Service discovery and load balancing** Kubernetes can expose a container using the DNS name or using their own IP address. If traffic to a container is high, Kubernetes is able to load balance and distribute the network traffic so that the deployment is stable.
- **Storage orchestration** Kubernetes allows you to automatically mount a storage system of your choice, such as local storages, public cloud providers, and more.
- **Automated rollouts and rollbacks** You can describe the desired state for your deployed containers using Kubernetes, and it can change the actual state to the desired state at a controlled rate. For example, you can automate Kubernetes to create new containers for your deployment, remove existing containers and adopt all their resources to the new container.
- **Automatic bin packing** You provide Kubernetes with a cluster of nodes that it can use to run containerized tasks. You tell Kubernetes how much CPU and memory (RAM) each container needs. Kubernetes can fit containers onto your nodes to make the best use of your resources.
- **Self-healing** Kubernetes restarts containers that fail, replaces containers, kills containers that don't respond to your user-defined health check, and doesn't advertise them to clients until they are ready to serve.
- **Secret and configuration management** Kubernetes lets you store and manage sensitive information, such as passwords, OAuth tokens, and SSH keys. You can deploy and update secrets and application configuration without rebuilding your container images, and without exposing secrets in your stack configuration.
- **Batch execution** In addition to services, Kubernetes can manage your batch and CI workloads, replacing containers that fail, if desired.
- **Horizontal scaling** Scale your application up and down with a simple command, with a UI, or automatically based on CPU usage.
- **IPv4/IPv6 dual-stack** Allocation of IPv4 and IPv6 addresses to Pods and Services
- **Designed for extensibility** Add features to your Kubernetes cluster without changing upstream source code.

## What Kubernetes is not

Kubernetes is not a traditional, all-inclusive PaaS (Platform as a Service) system. Since Kubernetes operates at the container level rather than at the hardware level, it provides some generally applicable features common to PaaS offerings, such as deployment, scaling, load balancing, and lets users integrate their logging, monitoring, and alerting solutions. However, Kubernetes is not monolithic, and these default solutions are optional and pluggable. Kubernetes provides the building blocks for building developer platforms, but preserves user choice and flexibility where it is important.

Kubernetes:

- Does not limit the types of applications supported. Kubernetes aims to support an extremely diverse variety of workloads, including stateless, stateful, and data-processing workloads. If an application can run in a container, it should run great on Kubernetes.
- Does not deploy source code and does not build your application. Continuous Integration, Delivery, and Deployment (CI/CD) workflows are determined by organization cultures and preferences as well as technical requirements.
- Does not provide application-level services, such as middleware (for example, message buses), data-processing frameworks (for example, Spark), databases (for example, MySQL), caches, nor cluster storage systems (for example, Ceph) as built-in services. Such components can run on Kubernetes, and/or can be accessed by applications running on Kubernetes through portable mechanisms, such as the Open Service Broker.
- Does not dictate logging, monitoring, or alerting solutions. It provides some integrations as proof of concept, and mechanisms to collect and export metrics.
- Does not provide nor mandate a configuration language/system (for example, Jsonnet). It provides a declarative API that may be targeted by arbitrary forms of declarative specifications.
- Does not provide nor adopt any comprehensive machine configuration, maintenance, management, or self-healing systems.
- Additionally, Kubernetes is not a mere orchestration system. In fact, it eliminates the need for orchestration. The technical definition of orchestration is execution of a defined workflow: first do A, then B, then C. In contrast, Kubernetes comprises a set of independent, composable control processes that continuously drive the current state towards the provided desired state. It shouldn't matter how you get from A to C. Centralized control is also not required. This results in a system that is easier to use and more powerful, robust, resilient, and extensible.

## Kubernetes Components

When you deploy Kubernetes, you get a cluster.

A Kubernetes cluster consists of a set of worker machines, called [nodes](https://kubernetes.io/docs/concepts/architecture/nodes/), that run containerized applications. Every cluster has at least one worker node.

The worker node(s) host the [Pods](https://kubernetes.io/docs/concepts/workloads/pods/) that are the components of the application workload. The [control plane](https://kubernetes.io/docs/reference/glossary/?all=true#term-control-plane) manages the worker nodes and the Pods in the cluster. In production environments, the control plane usually runs across multiple computers and a cluster usually runs multiple nodes, providing fault-tolerance and high availability.

This document outlines the various components you need to have for a complete and working Kubernetes cluster.

![The components of a Kubernetes cluster](./img/2.svg)
The components of a Kubernetes cluster

## Control Plane Components

The control plane's components make global decisions about the cluster (for example, scheduling), as well as detecting and responding to cluster events (for example, starting up a new pod when a Deployment's replicas field is unsatisfied).

Control plane components can be run on any machine in the cluster. However, for simplicity, setup scripts typically start all control plane components on the same machine, and do not run user containers on this machine. See Creating Highly Available clusters with kubeadm for an example control plane setup that runs across multiple machines.

### kube-apiserver

The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane.

The main implementation of a Kubernetes API server is kube-apiserver. kube-apiserver is designed to scale horizontally—that is, it scales by deploying more instances. You can run several instances of kube-apiserver and balance traffic between those instances.

### etcd

Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

If your Kubernetes cluster uses etcd as its backing store, make sure you have a back up plan for the data.

You can find in-depth information about etcd in the official documentation.

### kube-scheduler

Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on.

Factors taken into account for scheduling decisions include: individual and collective resource requirements, hardware/software/policy constraints, affinity and anti-affinity specifications, data locality, inter-workload interference, and deadlines.

### kube-controller-manager

Control plane component that runs controller processes.

Logically, each controller is a separate process, but to reduce complexity, they are all compiled into a single binary and run in a single process.

There are many different types of controllers. Some examples of them are:

- Node controller: Responsible for noticing and responding when nodes go down.
- Job controller: Watches for Job objects that represent one-off tasks, then creates Pods to run those tasks to completion.
- EndpointSlice controller: Populates EndpointSlice objects (to provide a link between Services and Pods).
- ServiceAccount controller: Create default ServiceAccounts for new namespaces.
  The above is not an exhaustive list.

### cloud-controller-manager

A Kubernetes control plane component that embeds cloud-specific control logic. The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster.
The cloud-controller-manager only runs controllers that are specific to your cloud provider. If you are running Kubernetes on your own premises, or in a learning environment inside your own PC, the cluster does not have a cloud controller manager.

As with the kube-controller-manager, the cloud-controller-manager combines several logically independent control loops into a single binary that you run as a single process. You can scale horizontally (run more than one copy) to improve performance or to help tolerate failures.

The following controllers can have cloud provider dependencies:

- Node controller: For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
- Route controller: For setting up routes in the underlying cloud infrastructure
- Service controller: For creating, updating and deleting cloud provider load balancers

## Node Components

Node components run on every node, maintaining running pods and providing the Kubernetes runtime environment.

### kubelet

An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.

The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy. The kubelet doesn't manage containers which were not created by Kubernetes.

### kube-proxy

kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.

kube-proxy maintains network rules on nodes. These network rules allow network communication to your Pods from network sessions inside or outside of your cluster.

kube-proxy uses the operating system packet filtering layer if there is one and it's available. Otherwise, kube-proxy forwards the traffic itself.

### Container runtime

A fundamental component that empowers Kubernetes to run containers effectively. It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment.

Kubernetes supports container runtimes such as containerd, CRI-O, and any other implementation of the Kubernetes CRI (Container Runtime Interface).

## Addons

Addons use Kubernetes resources (DaemonSet, Deployment, etc) to implement cluster features. Because these are providing cluster-level features, namespaced resources for addons belong within the kube-system namespace.

Selected addons are described below; for an extended list of available addons, please see Addons.

### DNS

While the other addons are not strictly required, all Kubernetes clusters should have cluster DNS, as many examples rely on it.

Cluster DNS is a DNS server, in addition to the other DNS server(s) in your environment, which serves DNS records for Kubernetes services.

Containers started by Kubernetes automatically include this DNS server in their DNS searches.

### Web UI (Dashboard)

Dashboard is a general purpose, web-based UI for Kubernetes clusters. It allows users to manage and troubleshoot applications running in the cluster, as well as the cluster itself.

Container Resource Monitoring
Container Resource Monitoring records generic time-series metrics about containers in a central database, and provides a UI for browsing that data.

### Cluster-level Logging

A cluster-level logging mechanism is responsible for saving container logs to a central log store with search/browsing interface.

### Network Plugins

Network plugins are software components that implement the container network interface (CNI) specification. They are responsible for allocating IP addresses to pods and enabling them to communicate with each other within the cluster.

## Cluster Architecture

![Kubernetes cluster architecture](./img/3.svg)
Kubernetes cluster architecture

### Pods

Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.

A Pod (as in a pod of whales or pea pod) is a group of one or more containers, with shared storage and network resources, and a specification for how to run the containers. A Pod's contents are always co-located and co-scheduled, and run in a shared context. A Pod models an application-specific "logical host": it contains one or more application containers which are relatively tightly coupled. In non-cloud contexts, applications executed on the same physical or virtual machine are analogous to cloud applications executed on the same logical host.

As well as application containers, a Pod can contain init containers that run during Pod startup. You can also inject ephemeral containers for debugging a running Pod.

#### What is a Pod?

```md
**Note**:
You need to install a [container runtime](https://kubernetes.io/docs/setup/production-environment/container-runtimes/) into each node in the cluster so that Pods can run there.
```

The shared context of a Pod is a set of Linux namespaces, cgroups, and potentially other facets of isolation - the same things that isolate a container. Within a Pod's context, the individual applications may have further sub-isolations applied.

A Pod is similar to a set of containers with shared namespaces and shared filesystem volumes.

Pods in a Kubernetes cluster are used in two main ways:

- **Pods that run a single container**. The "one-container-per-Pod" model is the most common Kubernetes use case; in this case, you can think of a Pod as a wrapper around a single container; Kubernetes manages Pods rather than managing the containers directly.

- **Pods that run multiple containers that need to work together**. A Pod can encapsulate an application composed of multiple co-located containers that are tightly coupled and need to share resources. These co-located containers form a single cohesive unit.

Grouping multiple co-located and co-managed containers in a single Pod is a relatively advanced use case. You should use this pattern only in specific instances in which your containers are tightly coupled.

You don't need to run multiple containers to provide replication (for resilience or capacity); if you need multiple replicas, see Workload management.

### Nodes

Kubernetes runs your workload by placing containers into Pods to run on Nodes. A node may be a virtual or physical machine, depending on the cluster. Each node is managed by the control plane and contains the services necessary to run Pods.

Typically you have several nodes in a cluster; in a learning or resource-limited environment, you might have only one node.

The components on a node include the kubelet, a container runtime, and the kube-proxy.

#### Node name uniqueness

The name identifies a Node. Two Nodes cannot have the same name at the same time. Kubernetes also assumes that a resource with the same name is the same object. In case of a Node, it is implicitly assumed that an instance using the same name will have the same state (e.g. network settings, root disk contents) and attributes like node labels. This may lead to inconsistencies if an instance was modified without changing its name. If the Node needs to be replaced or updated significantly, the existing Node object needs to be removed from API server first and re-added after the update.

## Glossary

**Add-ons**
Resources that extend the functionality of Kubernetes.

**Admission Controller**
A piece of code that intercepts requests to the Kubernetes API server prior to persistence of the object.

**Affinity**
In Kubernetes, affinity is a set of rules that give hints to the scheduler about where to place pods.

**Aggregation Layer**
The aggregation layer lets you install additional Kubernetes-style APIs in your cluster.

**Annotation**
A key-value pair that is used to attach arbitrary non-identifying metadata to objects.

**API Group**
A set of related paths in Kubernetes API.

**API server**
The API server is a component of the Kubernetes control plane that exposes the Kubernetes API. The API server is the front end for the Kubernetes control plane.

**API-initiated eviction**
API-initiated eviction is the process by which you use the Eviction API to create an Eviction object that triggers graceful pod termination.

**App Container**
Application containers (or app containers) are the containers in a pod that are started after any init containers have completed.

**Application Architect**
A person responsible for the high-level design of an application.

**Application Developer**
A person who writes an application that runs in a Kubernetes cluster.

**Applications**
undefined

**Approver**
A person who can review and approve Kubernetes code contributions.

**cAdvisor**
cAdvisor (Container Advisor) provides container users an understanding of the resource usage and performance characteristics of their running containers.

**Certificate**
A cryptographically secure file used to validate access to the Kubernetes cluster.

**cgroup (control group)**
A group of Linux processes with optional resource isolation, accounting and limits.

**CIDR**
CIDR (Classless Inter-Domain Routing) is a notation for describing blocks of IP addresses and is used heavily in various networking configurations.

**CLA (Contributor License Agreement)**
Terms under which a contributor grants a license to an open source project for their contributions.

**Cloud Controller Manager**
A Kubernetes control plane component that embeds cloud-specific control logic. The cloud controller manager lets you link your cluster into your cloud provider's API, and separates out the components that interact with that cloud platform from components that only interact with your cluster.

**Cloud Native Computing Foundation (CNCF)**
The Cloud Native Computing Foundation (CNCF) builds sustainable ecosystems and fosters a community around projects that orchestrate containers as part of a microservices architecture.

**Cloud Provider**
A business or other organization that offers a cloud computing platform.

**Cluster**
A set of worker machines, called nodes, that run containerized applications. Every cluster has at least one worker node.

**Cluster Architect**
A person who designs infrastructure that involves one or more Kubernetes clusters.

**Cluster Infrastructure**
undefined

**Cluster Operations**
The work involved in managing a Kubernetes cluster: managing day-to-day operations, and co-ordinating upgrades.

**Cluster Operator**
A person who configures, controls, and monitors clusters.

**Code Contributor**
A person who develops and contributes code to the Kubernetes open source codebase.

**ConfigMap**
An API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables, command-line arguments, or as configuration files in a volume.

**Container**
A lightweight and portable executable image that contains software and all of its dependencies.

**Container Environment Variables**
Container environment variables are name=value pairs that provide useful information into containers running in a pod

**Container Lifecycle Hooks**
The lifecycle hooks expose events in the Container management lifecycle and let the user run code when the events occur.

**Container network interface (CNI)**
Container network interface (CNI) plugins are a type of Network plugin that adheres to the appc/CNI specification.

**Container Runtime**
A fundamental component that empowers Kubernetes to run containers effectively. It is responsible for managing the execution and lifecycle of containers within the Kubernetes environment.

**Container Runtime Interface**
The main protocol for the communication between the kubelet and Container Runtime.

**Container runtime interface (CRI)**
The container runtime interface (CRI) is an API for container runtimes to integrate with the kubelet on a node.

**Container Storage Interface (CSI)**
The Container Storage Interface (CSI) defines a standard interface to expose storage systems to containers.

**containerd**
A container runtime with an emphasis on simplicity, robustness and portability

**Contributor**
Someone who donates code, documentation, or their time to help the Kubernetes project or community.

**Control Plane**
The container orchestration layer that exposes the API and interfaces to define, deploy, and manage the lifecycle of containers.

**Controller**
In Kubernetes, controllers are control loops that watch the state of your cluster, then make or request changes where needed. Each controller tries to move the current cluster state closer to the desired state.

**CRI-O**
A tool that lets you use OCI container runtimes with Kubernetes CRI.

**CronJob**
Manages a Job that runs on a periodic schedule.

**CustomResourceDefinition**
Custom code that defines a resource to add to your Kubernetes API server without building a complete custom server.

**DaemonSet**
Ensures a copy of a Pod is running across a set of nodes in a cluster.

**Data Plane**
undefined

**Deployment**
An API object that manages a replicated application, typically by running Pods with no local state.

**Developer (disambiguation)**
May refer to: Application Developer, Code Contributor, or Platform Developer.

**Device Plugin**
Device plugins run on worker Nodes and provide Podswith access to resources, such as local hardware, that require vendor-specific initialization or setup steps.

**Disruption**
Disruptions are events that lead to one or more Pods going out of service. A disruption has consequences for workload resources, such as Deployment, that rely on the affected Pods.

**Docker**
Docker (specifically, Docker Engine) is a software technology providing operating-system-level virtualization also known as containers.

**Dockershim**
The dockershim is a component of Kubernetes version 1.23 and earlier. It allows the kubelet to communicate with Docker Engine.

**Downstream (disambiguation)**
May refer to: code in the Kubernetes ecosystem that depends upon the core Kubernetes codebase or a forked repo.

**Downward API**
Kubernetes' mechanism to expose Pod and container field values to code running in a container.

**Dynamic Volume Provisioning**
Allows users to request automatic creation of storage Volumes.

**Endpoints**
Endpoints track the IP addresses of Pods with matching selectors.

**EndpointSlice**
A way to group network endpoints together with Kubernetes resources.

**Ephemeral Container**
A Container type that you can temporarily run inside a Pod.

**etcd**
Consistent and highly-available key value store used as Kubernetes' backing store for all cluster data.

**Event**
Event is a Kubernetes object that describes state change/notable occurrences in the system.

**Eviction**
Eviction is the process of terminating one or more Pods on Nodes.

**Extensions**
Extensions are software components that extend and deeply integrate with Kubernetes to support new types of hardware.

**Feature gate**
Feature gates are a set of keys (opaque string values) that you can use to control which Kubernetes features are enabled in your cluster.

**Finalizer**
Finalizers are namespaced keys that tell Kubernetes to wait until specific conditions are met before it fully deletes resources marked for deletion. Finalizers alert controllers to clean up resources the deleted object owned.

**FlexVolume**
FlexVolume is a deprecated interface for creating out-of-tree volume plugins. The Container Storage Interface is a newer interface that addresses several problems with FlexVolume.

**Garbage Collection**
Garbage collection is a collective term for the various mechanisms Kubernetes uses to clean up cluster resources.

**Gateway API**
A family of API kinds for modeling service networking in Kubernetes.

**Group Version Resource**
Means of representing unique Kubernetes API resource.

**Helm Chart**
A package of pre-configured Kubernetes resources that can be managed with the Helm tool.

**Horizontal Pod Autoscaler**
An API resource that automatically scales the number of Pod replicas based on targeted CPU utilization or custom metric targets.

**HostAliases**
A HostAliases is a mapping between the IP address and hostname to be injected into a Pod's hosts file.

**Image**
Stored instance of a Container that holds a set of software needed to run an application.

**Immutable Infrastructure**
Immutable Infrastructure refers to computer infrastructure (virtual machines, containers, network appliances) that cannot be changed once deployed.

**Ingress**
An API object that manages external access to the services in a cluster, typically HTTP.

**Init Container**
One or more initialization containers that must run to completion before any app containers run.

**Istio**
An open platform (not Kubernetes-specific) that provides a uniform way to integrate microservices, manage traffic flow, enforce policies, and aggregate telemetry data.

**Job**
A finite or batch task that runs to completion.

**JSON Web Token (JWT)**
A means of representing claims to be transferred between two parties.

**kOps (Kubernetes Operations)**
kOps will not only help you create, destroy, upgrade and maintain production-grade, highly available, Kubernetes cluster, but it will also provision the necessary cloud infrastructure.

**kube-controller-manager**
Control plane component that runs controller processes.

**kube-proxy**
kube-proxy is a network proxy that runs on each node in your cluster, implementing part of the Kubernetes Service concept.

**kube-scheduler**
Control plane component that watches for newly created Pods with no assigned node, and selects a node for them to run on.

**Kubeadm**
A tool for quickly installing Kubernetes and setting up a secure cluster.

**Kubectl**
Command line tool for communicating with a Kubernetes cluster's control plane, using the Kubernetes API.

**Kubelet**
An agent that runs on each node in the cluster. It makes sure that containers are running in a Pod.

**Kubernetes API**
The application that serves Kubernetes functionality through a RESTful interface and stores the state of the cluster.

**Label**
Tags objects with identifying attributes that are meaningful and relevant to users.

**LimitRange**
Provides constraints to limit resource consumption per Containers or Pods in a namespace.

**Logging**
Logs are the list of events that are logged by cluster or application.

**Managed Service**
A software offering maintained by a third-party provider.

**Manifest**
Specification of a Kubernetes API object in JSON or YAML format.

**Master**
Legacy term, used as synonym for nodes hosting the control plane.

**Member**
A continuously active contributor in the K8s community.

**Minikube**
A tool for running Kubernetes locally.

**Mirror Pod**
A pod object that a kubelet uses to represent a static pod

**Mixed Version Proxy (MVP)**
Feature to let a kube-apiserver proxy a resource request to a different peer API server.

**Name**
A client-provided string that refers to an object in a resource URL, such as /api/v1/pods/some-name.

**Namespace**
An abstraction used by Kubernetes to support isolation of groups of resources within a single cluster.

**Network Policy**
A specification of how groups of Pods are allowed to communicate with each other and with other network endpoints.

**Node**
A node is a worker machine in Kubernetes.

**Node-pressure eviction**
Node-pressure eviction is the process by which the kubelet proactively terminates pods to reclaim resources on nodes.

**Object**
An entity in the Kubernetes system. The Kubernetes API uses these entities to represent the state of your cluster.

**Operator pattern**
The operator pattern is a system design that links a Controller to one or more custom resources.

**Persistent Volume**
An API object that represents a piece of storage in the cluster. Available as a general, pluggable resource that persists beyond the lifecycle of any individual Pod.

**Persistent Volume Claim**
Claims storage resources defined in a PersistentVolume so that it can be mounted as a volume in a container.

**Platform Developer**
A person who customizes the Kubernetes platform to fit the needs of their project.

**Pod**
The smallest and simplest Kubernetes object. A Pod represents a set of running containers on your cluster.

**Pod Disruption**
Pod disruption is the process by which Pods on Nodes are terminated either voluntarily or involuntarily.

**Pod Disruption Budget**
A Pod Disruption Budget allows an application owner to create an object for a replicated application, that ensures a certain number or percentage of Pods with an assigned label will not be voluntarily evicted at any point in time.

**Pod Lifecycle**
The sequence of states through which a Pod passes during its lifetime.

**Pod Priority**
Pod Priority indicates the importance of a Pod relative to other Pods.

**Pod Security Policy**
Enables fine-grained authorization of Pod creation and updates.

**Preemption**
Preemption logic in Kubernetes helps a pending Pod to find a suitable Node by evicting low priority Pods existing on that Node.

**PriorityClass**
A PriorityClass is a named class for the scheduling priority that should be assigned to a Pod in that class.

**Probe**
A check that the kubelet periodically performs against a container that is running in a pod, that will define container's state and health and informing container's lifecycle.

**Proxy**
In computing, a proxy is a server that acts as an intermediary for a remote service.

**QoS Class**
QoS Class (Quality of Service Class) provides a way for Kubernetes to classify Pods within the cluster into several classes and make decisions about scheduling and eviction.

**Quantity**
A whole-number representation of small or large numbers using SI suffixes.

**RBAC (Role-Based Access Control)**
Manages authorization decisions, allowing admins to dynamically configure access policies through the Kubernetes API.

**Replica**
A copy or duplicate of a Pod or a set of pods. Replicas ensure high availability, scalability, and fault tolerance by maintaining multiple identical instances of a pod.

**ReplicaSet**
A ReplicaSet (aims to) maintain a set of replica Pods running at any given time.

**ReplicationController**
A workload resource that manages a replicated application, ensuring that a specific number of instances of a Pod are running.

**Resource Quotas**
Provides constraints that limit aggregate resource consumption per Namespace.

**Reviewer**
A person who reviews code for quality and correctness on some part of the project.

**Secret**
Stores sensitive information, such as passwords, OAuth tokens, and SSH keys.

**Security Context**
The securityContext field defines privilege and access control settings for a Pod or container.

**Selector**
Allows users to filter a list of resources based on labels.

**Service**
A method for exposing a network application that is running as one or more Pods in your cluster.

**Service Catalog**
A former extension API that enabled applications running in Kubernetes clusters to easily use external managed software offerings, such as a datastore service offered by a cloud provider.

**ServiceAccount**
Provides an identity for processes that run in a Pod.

**Shuffle-sharding**
A technique for assigning requests to queues that provides better isolation than hashing modulo the number of queues.

**Sidecar Container**
One or more containers that are typically started before any app containers run.

**SIG (special interest group)**
Community members who collectively manage an ongoing piece or aspect of the larger Kubernetes open source project.

**Spec**
Defines how each object, like Pods or Services, should be configured and its desired state.

**StatefulSet**
Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

**Static Pod**
A pod managed directly by the kubelet daemon on a specific node,

**Storage Class**
A StorageClass provides a way for administrators to describe different available storage types.

**sysctl**
sysctl is a semi-standardized interface for reading or changing the attributes of the running Unix kernel.

**Taint**
A core object consisting of three required properties: key, value, and effect. Taints prevent the scheduling of Pods on nodes or node groups.

**Toleration**
A core object consisting of three required properties: key, value, and effect. Tolerations enable the scheduling of pods on nodes or node groups that have matching taints.

**UID**
A Kubernetes systems-generated string to uniquely identify objects.

**Upstream (disambiguation)**
May refer to: core Kubernetes or the source repo from which a repo was forked.

**user namespace**
A kernel feature to emulate root. Used for "rootless containers".

**Volume**
A directory containing data, accessible to the containers in a Pod.

**Volume Plugin**
A Volume Plugin enables integration of storage within a Pod.

**WG (working group)**
Facilitates the discussion and/or implementation of a short-lived, narrow, or decoupled project for a committee, SIG, or cross-SIG effort.

**Workload**
A workload is an application running on Kubernetes.

**Orchestration**
In system administration, orchestration is the automated configuring, coordinating, and managing of computer systems and software. Many tools exist to automate server configuration and management, including Airflow, Kubernetes, Ansible, Puppet, Salt, Terraform, and AWS CloudFormation.
