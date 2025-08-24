## Basic Kubernetes Architecture

![Basic Kubernetes Architecture](kubernetes-cluster-architecture.svg)

*Figure: Basic Kubernetes Architecture*

# CLUSTER ARCHITECTURE:

- A Kubernetes cluster consists of control plane plus a set of worker machines, called nodes, that run containerized applications
- Every cluster needs atleast  one worker node in order to run Pods.

- The worker nodes host the pods that are the components of the application workload 
- The control plane manages the worker nodes and the pods in the cluster
- In production environents, the control plane usually runs across multile computers and cluster usually runs multiple nodes,   providing fault-tolerance and high availability

# CONTROL PLANE COMPONENTS:

- The control planes components make global decisions about the Cluster (for example scheduling) as well as detecting and responding to cluster events (for example starting up a new pod when a Deployments replicas field is unsatisfied)

1. KUBE-APISERVER:

- The API server is a component of the kubernetes control plane that exposes the kubernetes API. The API server is the front end of the kubernetes control plane

- The main implementation of kubernetes API server is a kube-apiserver 
- Kube-apiserver is designed to scale horizontally thats is, it scales by deploying more instances
- You can run several instances of kube-apiserver and balance traffic between those instances

2. ETCD:

- Consistent and high available key value store used as Kubernetes backing store for all cluster data
- If your kubernetes cluster uses etcd as its backing store, make sure you have a back plan for the data
- You can find in depth information about etcd in the official 

3. KUBE SCHEDULER:

- Control plane component that watches for newly created pods with no assigned node and selects a node for them to run on.
- Fators taken in to account for sheduling decisions inlude
    - individual and collective resoure requirement 
    - hardware, software, policy costraints
    - affiity and anti-affinity specifications
    - data locality
    - inter workload interferene
    - deadlines

4. KUBE CONTROLLER MANAGER:

- Control plane component that runs controller processes
- Logically each controller is a seperate process but to reduce complexity, they are all compiled in to a single binary and run in a single process

- There are many different types of controllers. Some of them are 

1. NODE CONTROLLER:
    - Responsible for noticing and responding when nodes go down
2. JOB CONTROLLER:
    - Watches for job objects that represent one-off tasks, then creates pods to run those tasks, then creates pods to run those tasks to completion
3. ENSPOINT SLICE CONTROLLER
    - Populates EndpointSlice objects (to provide a link between Services and Pods)
4. SERVICEACCOUNT CONTROLLER:
    - Creates default ServiceAccounts for new namespaces

5. CLOUD CONTROLLER MANAGER:

- A kubernetes control plane component that embeds cloud-specific control logic 
- The cloud controller manager lets you link your cluster in to your providers API and seperates out the components that interact with that cloud platform form components that only interact with your cluster 
- The cloud controller manager only runs controllers that are specific to your cloud provider 
- If you are running Kubernetes on your own premises or in a learning environment inside your own PC the cluster does not have Cloud controller manager

- THE FOLLOWING CONTROLLERS CAN HAVE CLOUD PROVIDER DEPENDENCIES:

- Node Controller:
    - For checking the cloud provider to determine if a node has been deleted in the cloud after it stops responding
- Route controller:
    - For setting up routes in the underlying cloud infrastructure
- Service controller: 
    - For creating, updating and deleting cloud provider load balancers


# NODE COMPONENTS:

1. KUBELET:
    - An agent that runs on each node in the luster
    - It makes sure that containers are running in a pod
    - The kubelet takes a set of PodSpecs that are provided through various mechanisms and ensures that the ontainers desribed in those PodSpes are running and healthy 
    - The kubelet doesnt manage containers that are not created by kubernetes

2. KUBE-PROY:
    - Kube-proxy is a network proxy that runs on each node in your cluster implementing part of the kubernetes Service concept
    - Kube-proxy maintains network rules on nodes. These network rules allow network communication to your pods fro network sessions inside or outside of cluster

3. Container runtime:
    - A fundamental component that empowers kubernetes to run containers effectively 
    - It is responsible for managing the execution and life cycle of containers within kubernetes environment

    - Kubernetes supports container runtime such as containerd, CRI-O and any other implementation of Kubernetes CRI