# Kubernetes Classroom Notes - Beginner Friendly Guide

This document explains the basics of Kubernetes concepts covered in
today's class, with examples, tasks, and references.\
It is designed so that even a **complete beginner** can understand.

------------------------------------------------------------------------

## 1. Ways of Communicating with the Control Plane

The **Control Plane** manages the Kubernetes cluster. You can
communicate with it in different ways:

1.  **Code-based**
    -   Using SDKs or client libraries in languages like **Python**,
        **Go**, **Java**, etc.\
    -   Example: `client-go` library for Go, `kubernetes` Python client.
2.  **CLI (Command Line Interface)**
    -   Using `kubectl` (the Kubernetes command-line tool).\
    -   Most common method for admins and developers.

------------------------------------------------------------------------

## 2. What is a Pod?

-   A **Pod** is the smallest deployable unit in Kubernetes.\
-   It wraps **one or more containers** together with storage and
    network resources.\
-   Containers inside a Pod share the same **IP address** and
    **namespace**.

### Difference Between Pod and Container

  -----------------------------------------------------------------------
  **Container**                                **Pod**
  -------------------------------------------- --------------------------
  Runs a single application or process.        Can hold one or multiple
                                               containers.

  Managed by Docker or container runtime.      Managed by Kubernetes.

  Limited in networking and scaling.           Provides abstraction,
                                               scaling, networking, and
                                               orchestration.
  -----------------------------------------------------------------------

------------------------------------------------------------------------

## 3. Communication Between Pods and Control Plane

When you create a Pod, the **control plane** schedules it on a Node.\
Communication happens through the **Kubernetes API server**.

### Pod Creation Methods

1.  **Using kubectl (Imperative)**
    -   Directly using commands.\

    -   Example:

        ``` bash
        kubectl run mypod --image=nginx
        ```

    -   **Imperative** means you tell Kubernetes *exactly what to do,
        step by step*.
2.  **Using Manifest File (Declarative)**
    -   You describe the desired state in a YAML file.\

    -   Example `pod.yaml`:

        ``` yaml
        apiVersion: v1
        kind: Pod
        metadata:
          name: mypod
        spec:
          containers:
          - name: nginx-container
            image: nginx
        ```

    -   Run:

        ``` bash
        kubectl apply -f pod.yaml
        ```

    -   **Declarative** means you define *what you want*, and Kubernetes
        figures out how to achieve it.
3.  **Using Programming Language**
    -   Example with Python client:

        ``` python
        from kubernetes import client, config
        config.load_kube_config()

        pod = client.V1Pod(
            metadata=client.V1ObjectMeta(name="mypod"),
            spec=client.V1PodSpec(containers=[client.V1Container(name="nginx", image="nginx")])
        )

        v1 = client.CoreV1Api()
        v1.create_namespaced_pod(namespace="default", body=pod)
        ```

------------------------------------------------------------------------

## 4. Kubernetes API Versions

Kubernetes resources evolve. API versions define stability:

1.  **Alpha**
    -   Early stage, may be unstable.\
    -   Disabled by default.
2.  **Beta**
    -   Tested more, enabled by default.\
    -   Still subject to change.
3.  **Stable (GA)**
    -   Fully tested, safe for production.\
    -   Example: `v1`.

------------------------------------------------------------------------

## 5. API Groups

-   Organizes APIs into categories.\
-   Example groups:
    -   `core` (pods, services, configmaps)\
    -   `apps` (deployments, statefulsets)\
    -   `batch` (jobs, cronjobs)

------------------------------------------------------------------------

## 6. Namespaces and Scope

-   A **Namespace** is a way to divide cluster resources between
    multiple users or projects.\
-   Example:
    -   `default` (default namespace)\
    -   `kube-system` (system components)\
    -   `dev`, `prod` (custom environments)

**Scope:**\
- **Namespaced resources:** Pods, Services, ConfigMaps\
- **Cluster-wide resources:** Nodes, PersistentVolumes

------------------------------------------------------------------------

## 7. What is "Kind"?

-   `kind` defines the type of Kubernetes object in a YAML manifest.\
-   Example:
    -   Pod\
    -   Deployment\
    -   Service

------------------------------------------------------------------------

## 8. YAML Data Types in Kubernetes Manifests

Kubernetes manifests use YAML. Basic data types:

1.  **String**

    ``` yaml
    name: "nginx"
    ```

2.  **Number**

    ``` yaml
    replicas: 3
    ```

3.  **Boolean**

    ``` yaml
    enabled: true
    ```

4.  **List / Array**

    ``` yaml
    ports:
      - 80
      - 443
    ```

5.  **Map / Object**

    ``` yaml
    resources:
      limits:
        cpu: "500m"
        memory: "256Mi"
    ```

------------------------------------------------------------------------

## 9. Task for Practice

1.  Create a Pod declaratively with the following specs:

    -   Name: `mytaskpod`
    -   Image: `nginx`
    -   Namespace: `default`
    -   Expose port `80`

2.  Verify the pod:

    ``` bash
    kubectl get pods
    kubectl describe pod mytaskpod
    ```

------------------------------------------------------------------------

## 10. Best Practices

-   Use **Declarative YAML** over Imperative commands.\

-   Keep YAML files under **version control (Git)**.\

-   Use **namespaces** for environment separation (dev/prod).\

-   Always check resource status:

    ``` bash
    kubectl get all -n <namespace>
    ```

-   Delete unused resources to save cluster resources.

------------------------------------------------------------------------

## 📖 References & Useful Links

-   [Kubernetes Official Documentation](https://kubernetes.io/docs/)\
-   [kubectl Cheat
    Sheet](https://kubernetes.io/docs/reference/kubectl/cheatsheet/)\
-   [Killer Koda Free Labs](https://killercoda.com/kubernetes)\
-   [YAML Basics](https://www.yaml.org/spec/)\
-   [Kubernetes Python
    Client](https://github.com/kubernetes-client/python)

------------------------------------------------------------------------

✅ With this guide, a beginner can clearly understand how to communicate
with Kubernetes, create Pods, and learn YAML basics.
