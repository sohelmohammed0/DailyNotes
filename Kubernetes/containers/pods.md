# Name Spaces:
- A virtual cluster inside a Kubernetes cluster
-  Think of it like folders on your computer — they keep resources grouped and separate.

## Default Name Spaces:
- Default: Where a pod lands when user dont specify a namespace
- kube-system: system components (Kube-dns, kube-proxy etc)
- Kube-Public: Readable by everyone (even unauthenticated) rarely used by apps
- Kube-node-release: stores node heartbeat releases

**Why Use namesapces:**
- Multi-team isolation (dev,prod enviroments)
- Resource quotas: (limit dev namespaces to 2 CPUs)
- RBAC: (Role Bases Access Control)

# POD:
- A pod is a smallest deployable unit
- It wraps one or more containers and give
    1. One IP adress per Pod (all containers inside share it)
    2. Shared Localhost (containers can talk over localhost)
    3. Shared Volumes (Like sahred folders)

**Pod LifeCycle :**
- Pending -> Running -> Succeeded/Failed -> Unknown

**Restart Polices :**
- Always (Default for Deployemnts)
- OnFailure (Restart only if exit code /= 0)
- Never

**Probes :**
- Liveness Probes: Is container alive? If not restart
- Readiness Probe: Is container ready to serve traffic? If not remove from service endpoints
- Startup Probe: Delay other probes until startup fineshes

-> Kubectl get ns
-> kubectl create ns dev


``` yml
apiVersion: v1
Kind: Pod
metadata:
  name: echo-dev
  namespace: dev
spec:
  containers:
    - name: echo
      image: hashicorp/http-echo:0.2.3
      agrs: ["-text=hello-from-dev-namespace"]
      ports:
        - containerPort: 5678

```

-> kubectl config set-context --current --namespace=dev











































| Level | Focus                         | You’ll Be Able To                                                                   |
| ----- | ----------------------------- | ----------------------------------------------------------------------------------- |
| 0     | Foundations                   | Explain Pod vs Container, namespaces, volumes, lifecycle                            |
| 1     | Single-container Pod          | Run, inspect, exec, logs, port-forward                                              |
| 2     | Multi-container Pod & Sidecar | Share volumes/IPC; implement a log-collection sidecar                               |
| 3     | Init Containers               | Gate startup; run sequential init steps with fail/timeout behavior                  |
| 4     | Ambassador Pattern            | Proxy service access locally inside a pod                                           |
| 5     | Static & Mirror Pods          | Create/manage kubelet-managed pods; recognize mirror pods                           |
| 6     | Multi-Pod Workloads           | Choose between multi-container pod vs multiple pods; apply Deployments/StatefulSets |
| 7     | Capstone                      | Build a realistic app using init + sidecar + ambassador and justify choices         |
