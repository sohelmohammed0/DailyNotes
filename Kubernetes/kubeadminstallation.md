# Kubernetes Cluster Setup with Docker & cri-dockerd

This guide provides step-by-step instructions to set up a Kubernetes
cluster using **Docker** and **cri-dockerd** on Ubuntu.\
It includes installation steps for both **Master** and **Worker** nodes.

------------------------------------------------------------------------

## 1. Update all nodes

``` bash
sudo apt update && sudo apt upgrade -y
```

------------------------------------------------------------------------

## 2. Install Docker (on ALL nodes)

``` bash
curl -fsSL https://get.docker.com -o install-docker.sh
sh install-docker.sh

# Add user to Docker group (optional)
sudo usermod -aG docker $USER
newgrp docker

# Enable Docker
sudo systemctl enable docker
sudo systemctl start docker
```

------------------------------------------------------------------------

## 3. Install cri-dockerd (on ALL nodes)

``` bash
wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.4.0/cri-dockerd_0.4.0.3-0.ubuntu-jammy_amd64.deb
sudo dpkg -i cri-dockerd_0.4.0.3-0.ubuntu-jammy_amd64.deb || sudo apt -f install -y

# Enable cri-dockerd
sudo systemctl enable cri-docker
sudo systemctl start cri-docker
```

------------------------------------------------------------------------

## 4. Install kubeadm, kubelet, kubectl (on ALL nodes)

``` bash
sudo apt-get install -y apt-transport-https ca-certificates curl gpg

# Create keyrings directory if missing
sudo mkdir -p -m 755 /etc/apt/keyrings

# Add Kubernetes APT repo
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key |   sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /" |   sudo tee /etc/apt/sources.list.d/kubernetes.list

# Update repos and install packages
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl
```

------------------------------------------------------------------------

## 5. Initialize Kubernetes (ONLY MASTER NODE)

``` bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16   --cri-socket=unix:///var/run/cri-dockerd.sock

# Configure kubectl for the master user
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config
```

------------------------------------------------------------------------

## 6. Install CNI Plugin (ONLY MASTER NODE)

``` bash
# Example: Weave Net
kubectl apply -f https://github.com/weaveworks/weave/releases/download/v2.8.1/weave-daemonset-k8s.yaml
```

------------------------------------------------------------------------

## 7. Join Worker Nodes (ONLY WORKERS)

``` bash
# Run the join command that was output by kubeadm init.
# Replace <TOKEN> with the actual token you receive after running kubeadm init
# Example:

sudo kubeadm join <MASTER_PRIVATE_IP>:6443 --token <TOKEN>   --discovery-token-ca-cert-hash sha256:<HASH>   --cri-socket=unix:///var/run/cri-dockerd.sock
```

------------------------------------------------------------------------

## ⚡ Important Notes & Best Practices

-   Ensure the following ports are **open** on Master and Worker nodes:
    -   TCP **6443** (Kubernetes API server)\
    -   TCP **10250** (kubelet API)\
    -   TCP **10259** (kube-scheduler, master only)\
    -   TCP **10257** (kube-controller-manager, master only)\
    -   UDP **8285, 8472** (Weave/Flannel CNI)
-   Always run `kubeadm` commands with **sudo/root** privileges.\
-   Use the same Kubernetes version on all nodes.\
-   For production clusters:
    -   Use strong firewall rules.\
    -   Enable role-based access control (RBAC).\
    -   Regularly update Kubernetes components.

------------------------------------------------------------------------

## 🔗 Useful Links

-   [Kubernetes Official
    Documentation](https://kubernetes.io/docs/home/)\
-   [Kubeadm
    Reference](https://kubernetes.io/docs/reference/setup-tools/kubeadm/)\
-   [Weave Net
    CNI](https://www.weave.works/docs/net/latest/kubernetes/kube-addon/)\
-   [cri-dockerd GitHub](https://github.com/Mirantis/cri-dockerd)\
-   [Docker Official Docs](https://docs.docker.com/)

------------------------------------------------------------------------

✅ With this setup, you'll have a Kubernetes cluster running with Docker
as the container runtime (via cri-dockerd).
