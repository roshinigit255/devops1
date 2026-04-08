## K8 master node worker node setup

- Kuberenetes Setup
- Connect your machines to Putty or Mobaxtreme
- Take-Three Ubuntu 20.04 instances one for k8s master and the other two for worker.

```bash
sudo apt update
sudo apt install curl
curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
kubectl version --client
```

- Part 1 ----------Master Node------------

```bash
sudo hostnamectl set-hostname K8s-Master
```

- ----------Worker Node------------

```bash
sudo hostnamectl set-hostname K8s-Worker
```

- Part 2 ------------Both Master & Node ------------

Update System Packages

```bash
sudo apt-get update 
sudo apt-get upgrade -y
```
Install & Configure containerd (Container Runtime)

```bash
sudo apt install -y containerd

sudo mkdir -p /etc/containerd
containerd config default | sudo tee /etc/containerd/config.toml

sudo sed -i 's/SystemdCgroup = false/SystemdCgroup = true/' /etc/containerd/config.toml

sudo systemctl restart containerd
sudo systemctl enable containerd
```
Load Required Kernel Modules

```bash
sudo modprobe overlay
sudo modprobe br_netfilter
```
Configure Sysctl Parameters for Kubernetes Networking

```bash
cat <<EOF | sudo tee /etc/sysctl.d/k8s.conf
net.bridge.bridge-nf-call-iptables = 1
net.ipv4.ip_forward = 1
net.bridge.bridge-nf-call-ip6tables = 1
EOF

sudo sysctl --system
```
Install Required Dependencies

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl gnupg
```
Add Kubernetes Repository & GPG Key

```bash
sudo mkdir -p -m 755 /etc/apt/keyrings

curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.28/deb/Release.key | \
sudo gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg

echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.28/deb/ /' | \
sudo tee /etc/apt/sources.list.d/kubernetes.list
```
Install Kubernetes Components

```bash
sudo apt-get update

sudo apt install -y kubelet kubeadm kubectl
sudo apt-mark hold kubelet kubeadm kubectl

```

- Part 3 --------------- Master ---------------

```bash
sudo kubeadm init --pod-network-cidr=10.244.0.0/16
# in case your in root exit from it and run below commands
mkdir -p $HOME/.kube
sudo cp /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl apply -f https://docs.projectcalico.org/manifests/calico.yaml
```

- ----------Worker Node------------

```bash
sudo kubeadm join <master-node-ip>:<master-node-port> --token <token> --discovery-token-ca-cert-hash <hash>
```

- Copy the config file to Jenkins master or the local file manager and save it
- copy it and save it in documents or another folder save it as secret-file.txt
- Note: create a secret-file.txt in your file explorer save the config in it and use this at the kubernetes credential section.
- Install Kubernetes Plugin, Once it's installed successfully
- goto manage Jenkins --> manage credentials --> Click on Jenkins global --> add credentials

