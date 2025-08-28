1. What is a pod?
    - Pod is the smallest deployable unit in K8s
    - Its a wrapper one or more cotainer which share same the same network (IP, port space) and storage volumes
    - Pods are ephemeral - if one dies, Kubernetes can recreate it through controllers like replicaset/Deployment

2. Why Pods instead of containers directly?
    - Kubernetes doesnt manage containers directly -> It manages pods
    - Easier scaling 
    - Networking between apps
    - Storage Sharing
    - Example: If you run Nginx + sidecar container both can live in the same pod and share resources

3. Multiple containers in a Pod?
    - Yes Pod can have multiple containers
    - They are tightly coupled and usually follow the sidecar pattern (helper container)

Communication between Pods and control plane