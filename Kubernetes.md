# Kubernetes 

-   **Kubernetes** is a **greek** word for **Helmsman** or **Captain of ship**
-   **Kubernetes** is also referred as **K8s** because **Kubernetes** has 8 letters between first and last character

#   What is Kubernetes ?
-   Kubernetes is a **Container management/orchestration tool**
-   Developed by Google lab (and later donated to **CNCF - Cloud Native Computing Foundation**)
-   open source
-   Written in **GoLang**

#   What is Container Orchestration Engine ?
-   Container orchestration tool or engine automates deploying, scaling and managing containerized application on a group of server.
-   Examples for such tools are:
    -   Kubernetes
    -   Docker Swarm
    -   Apache Mesos Marathon

#   What are Containerized applications ?
-   Container allow a developer to package up an application with all the parts it needs, such as libraries and other dependencies, and ship it all out as one package.
-   Docker is a tool designed to make it easier to deploy and run applications by using containers

#   Why do we need Kubernetes ?
-   Organization have to use multiple containers for each application(not just one container) to ensure
    -   Availability
    -   Load balancing
    -   Scale Up and Down based on user load
-   when there are so many containers, organization have to take care of 
    -   Deploying
    -   Scheduling
    -   scaling
    -   load balancing
    -   batch execution
    -   rollbacks
    -   monitoring
-   Container Orchestration Tools helps organization to automate these processes
-   One of such most widely used and popular tool is **Kubernetes**
-   We have other tools such as Docker Swarm, Apache Mesos Marathon etc

#   Features of Kubernetes
-   **Automatic Bin packing**
    -   Kubernetes will take care of packaging these jobs(containers) in **bins(servers)** in most effective way.
    -   Kubernetes automatically packages your application and schedules the container based on the requirements and resources available.
    -   When you specify a **Pod** you can optionally specify how much CPU and Memory(RAM) each container needs. When containers have resource requests specified, the scheduler can make better decisions about which **Node**s to place the PodS on. (Pod's and Node's are kubernetes terms, check kubernetes architecture)    
-   **Service discovery and Load balancing**
    -   Every **Pod** contains
        -   Unique IP
        -   container's
        -   Volume(storage)
    -   **Pods** that have the same set of functions are abstracted into sets, called **services**
    -   A kubernetes **Service** is a set of **Pod** that work together
    -   Kubernetes gives **Pods** their own IP addresses, and a single DNS name for a set of **Pods**, and can load balance across them
    -   With this system, kubernetes has control over network and communication between pods and can load balance between them. 
-   **Storage orchestration**
    -   Containers running inside a Pod may need to store data
    -   Pods can have a storage volumes
    -   Usually a single volume is shared within all containers in a pod
    -   Kubernetes allows to mount the storage system of your choice (Local, Cloud(AWS),Network(NFS))
-   **Self Healing**
    -   If container fails ->   restart container
    -   If node dies    ->  replaces and reschedule containers on other nodes
    -   If container does not respond to user defined health check ->   Kills container
    -   The process which take cares of all this functionality is called **ReplicationController**
-   **Automated rollouts and rollbacks**
    -   Rollout
        -   deploy changes to the application or its configuration
    -   Rollback
        -   Revert the changes and restore to previous state
    -   Kubernetes ensures there is no downtime during this process
-   **Secret and certification management**
    -   Secret
        -   Kubernetes sensitive data like passwords,keys, tokens are handled using Secrets
        -   Secret is a Kubernetes object created outside pods & containers
        -   Makes sensitive data portable and easy to manage
        -   Secret is a Kubernetes object that separates sensitive data from pods and containers
    -   Config maps
        -   Kubernetes' configurations are handled using Config Maps
        -   Config Map is a Kubernetes object created outside pods & containers
        -   Makes configuration portable and easy to manage
        -   Config Map is a Kubernetes object that separates configuration from pods and containers
    -   Kubernetes manages secrets and configuration details for an application separately from the container image
    -   Deploy and update secrets and application configuration without rebuilding your image and without exposing secrets in your stack configuration.
    -   Secrets and Configurations are stored separately in ETCD -> ETCD is a key-value datastore.
    -   The max size limit for a secret is 1 MB.
-   **Batch execution**
    -   Batch job requires an executable/process to be **run to completion**
    -   In Kubernetes **run to completion** jobs are primarily used for batch processing
    -   Each job creates one or more pods
    -   During job execution if any container or pod fails, **Job controller** will reschedule the container or pod in another node
    -   Can run multiple pods in parallel and can scale up if required
    -   As the job is completed, the pods will move from running state to shut down state.
    -   Kubernetes supports batch execution, long running jobs and replaces failed containers. 
-   **Horizontal scaling**
    -   In kubernetes we can scale up or down the containers, we can do this in following ways
        -   Using commands
        -   from the dashboard(Kubernetes UI)
        -   Automatically based on CPU usage
    -   Replication controller(rc or rcs)
        -   Replication controller is a structure that enables to create multiple pods, then make sure that the number of pods always exist.
        -   If a pod crashes, the replication controller replaces it.
    -   Manifest
        -   Replication controller gets the number of pods to run and make available at any time from the information provided in manifest file.
    -   Horizontal Pod autoscaler
        -   automatically scales the number of pods in a replication controller based on observed CPU Utilization or with custom matrix
    
# Kubernetes Architecture
-   When you deploy Kubernetes, you get a **cluster**
-   A cluster is a set of machines, called **nodes**
-   A cluster has atleast one **worker node(minions)** and atleast one **master node**
-   There can be more than one **master node** in a cluster to provide a cluster with failover and high availability
-   There can be multiple **clusters** in a Kubernetes architecture
-   A node can be a 
    -   Physical machine
    -   Virtual machine
    -   VM on cloud
-   The worker nodes hosts the pods that are the components of the application
-   The master node manages the worker nodes and pods in the cluster
-   When a node fails, moves the workload of the failed node to another worker node.    
-   4 Components of master node
    -   **API Server**  -   for all communication
        -   Exposes API for almost every operation
        -   Users interact with the API using a tool called **kubectl**
        -   **kubectl** is a command line utility to interact with Kubernetes API
        -   **kubectl** is a GO Language binary
    -   **Scheduler**   -   schedules pods on nodes
        -   schedules pods across multiple nodes
        -   Components on the master node that watches newly created pods that have no node assigned, and selects a node for them to run on
        -   Scheduler gets the information for hardware configuration from configuration file and schedules the pods on nodes accordingly. 
    -   **Control Manager** -   runs controller
        -   **Kube-controller-manager**
            -   runs controllers responsible to act when nodes become unavailable, to ensure pod counts are as expected, to create endpoints, service accounts, and API access tokens.
                -   Node controller -   responsible for noticing and responding when nodes go down
                -   Replication controller  -   responsible for maintaining the correct number of pods for every replication controller object in the system.
                -   Endpoints controller    -   Populates the endpoint objects(i.e join services and pods)
                -   Service account and token controller - Creates default accounts and API access tokens for new namespaces.
            -   Responsible for overall health of the cluster, ensures nodes are running all the time, correct number of pods are running as per spec file
            -   Logically, each controller is a separate process,but to reduce complexity, they are all compiled into single binary and run in a single process.
            -   Controller run monitoring continuously to compare cluster's desired state to its current state. In case of mismatch corrective action is taken in the cluster until its current state matches the desired state.
        -   **Cloud-controller-manager** -  runs controllers responsible to interact with the underlying infrastructure of a cloud provider when node becomes unavailable, to manage storage volumes when provided by a cloud service, and to manage load balancing and routing.
            -   **Node controller** - for checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
            -   **Route controller** -  for setting up routes in the underlying cloud infrastructure
            -   **Service controller**  -   for creating, updating and deleting cloud provider load balancers
            -   **Volume controller**   -   for creating, attaching and mounting volumes, and interacting with the cloud provider to orchestrate volumes
        -   you can disable the controller loops by setting the **--cloud-provider** flag to **external** when starting the **kube-controller-manager**
    -   **ETCD** - Open Source, Distributed key-value database from CoreOS
        -   Single source of truth for all components of the Kubernetes cluster
        -   Out of all master components only the API Server is able to communicate with the ETCD datastore
        -   ETCD can be part of Kubernetes Master OR it can be configured externally
-   Every node in Kubernetes cluster must run a container runtime like docker
-   3 Components of worker node
    -   **kubelet**
        -   Kubelet is an agent that runs on each node
        -   communicates with components from the master node
        -   Make sures that the containers are running in a pod
        -   Kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the containers described in those PodSpecs are running and healthy
        -   in case any Pod has any issue, kubelet tries to restart the pods on the same node or a different node
        -   Kubelet doesnot manage the containers which are not created by Kubernetes
    -   **kube-proxy**
        -   A network agent which runs on each node responsible for maintaining network configuration and rules
        -   Core networking components in Kubernetes
        -   Exposes services to the out-side world
        -   All worker node runs on a deamon called kube-proxy, which watches the **API server on the master node** for the addition and removal of services and endpoints. 
    -   **Container runtime**
        -   Container runtime is the software that is responsible for running the containers
        -   Kubernetes supports several container runtimes
            -   Docker
            -   Containerd
            -   Cri-o
            -   Rktlet
            -   Kubernets CRI(Container runtime interface)
        -   Kubernetes doesnot have the capability to directly handle containers
        -   In order to run and manage a container's lifecycle, kubernets requires a container runtime on the node where a Pod and its containers are to be scheduled
-   Addons
    -   Add-ons are the extended functionality of kubernetes
        -   **Dashboard** - a general purpose web based user interface for cluster management
        -   **monitoring** -    collect cluster level container metrics and saves them to a central data store
        -   **Logging** -   collect cluster level container logs and saves them to a central log store for analysis
        -   **DNS** -   cluster DNS is a DNS server required to assign DNS records to Kubernetes objects and resources.

# Disadvantages
-   Kubernetes supports not more than 5000 nodes
-   Kubernetes supports not more than 1,50,000 total pods
-   Kubernetes supports not more than 3,00,000 total containers
-   Kubernetes supports not more than 100 pods per node


# Options to use Kubernetes
-   Online Kubernetes Labs
    -   https://www.katacoda.com/courses/kubernetes/playground
    -   https://labs.play-with-k8s.com/
    -   https://training.play-with-kubernetes.com/
    -   https://cloudacademy.com/lab/introduction-kubernetes-playground/
-   Kubernetes installation tools
    -   Minikube
    -   Kubeadm
-   Cloud based Kubernetes Services
    -   GKE -   Google Kubernetes Engine
    -   AKS -   Azure Kubernetes Service
    -   Amazon EKS
    
# Kubernetes Commands
-   To launch kubernetes cluster
    ```launch.sh```

-   To get the Kubernetes cluster-info (Health status)
    ```kubectl cluster-info```
    
-   To get the complete cluster info
    ```kubectl cluster-info dump```
    
-   To list all the nodes
    ```kubectl get nodes```
