Pre-requisites:

A compatible Linux Host
for a basic kubernetes(kubeadm) setup we need 2 vcpu and 2 gb ram per machine
Full network conectivity between all nodes (public or private is fine)
Unique  hostname, MAC adress and product_uuid for every node
6443 port opened

Verify that and product_uuid and MAC adress of three nodes must be unique (however we are creating 3 individual vmc so the MAC adress and product_uuid will be automatically unique)

Ensure these ports must are opened 

On control plane:

TCP	Inbound	6443	Kubernetes API server	All
TCP	Inbound	2379-2380	etcd server client API	kube-apiserver, etcd
TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	10259	kube-scheduler	Self
TCP	Inbound	10257	kube-controller-manager	Self

On Worker Nodes:

Worker node(s)
Protocol	Direction	Port Range	Purpose	Used By
TCP	Inbound	10250	Kubelet API	Self, Control plane
TCP	Inbound	10256	kube-proxy	Self, Load balancers
TCP	Inbound	30000-32767	NodePort Services†	All

Update packages and Installing docker

sudo apt update
curl -fsSL https://get.docker.com -o install-docker.sh 
sh install-docker.sh 
sudo usermod -aG docker ubuntu 
exit    # Then re-login to the VM

Installing CRI Dockerd (For using docker with Kubernetes)

wget https://github.com/Mirantis/cri-dockerd/releases/download/v0.4.0/cri-dockerd_0.4.0.3-0.ubuntu-jammy_amd64.deb 
sudo dpkg -i cri-dockerd_0.4.0.3-0.ubuntu-jammy_amd64.deb 

Install COntainerd and Dependencies

sudo apt install -y curl gnupg2 software-properties-common apt-transport-https ca-certificates 
sudo curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/docker.gpg 
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" 
sudo apt update 
sudo apt install -y containerd.io 
# Configure containerd 
containerd config default | sudo tee /etc/containerd/config.toml >/dev/null 2>&1 
sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/g' /etc/containerd/config.toml 
sudo systemctl restart containerd 
sudo systemctl enable containerd

Install Kubeadm, kubelet and kubectl

# Download Kubernetes signing key
curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key \
  | sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

# Add Kubernetes apt repo
echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] \
https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /" \
| sudo tee /etc/apt/sources.list.d/kubernetes.list

# Update apt and install
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl



Change to root user and execute the folowing commands:

From Master Node:

kubeadm init --pod-network-cidr=10.244.0.0/16 --cri-socket=unix:///var/run/cri-dockerd.sock 

From Worker Node:

kubeadm join 172.31.36.164:6443 --token vw2q6y.r1xqeowfbqwkkl3u \  # Update your token here
  --discovery-token-ca-cert-hash sha256:22f9efdcd0444ce93fc2756afebc5c878c53b058d6b7ea6e734be6e42d0fcc66 \ 
  --cri-socket=unix:///var/run/cri-dockerd.sock


Update your token before executing this command:

kubeadm join 172.31.36.164:6443 --token trm3cy.sxvwcbl1okjci7x1 --discovery-token-ca-cert-hash sha256:22f9efdcd0444ce93fc2756afebc5c878c53b058d6b7ea6e734be6e42d0fcc66 --cri-socket=unix:///var/run/cri-dockerd.sock


Token: trm3cy.sxvwcbl1okjci7x1

 kubeadm join 172.31.36.164:6443 --token trm3cy.sxvwcbl1okjci7x1 \ 
  --discovery-token-ca-cert-hash sha256:22f9efdcd0444ce93fc2756afebc5c878c53b058d6b7ea6e734be6e42d0fcc66 \ 
  --cri-socket=unix:///var/run/cri-dockerd.sock 


