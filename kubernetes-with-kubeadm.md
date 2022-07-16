# Deploy Kubernetes On AWS EC2

## 1. On each server, install Docker
(Follow Installation guide: https://docs.docker.com/engine/install/ubuntu/)

 - sudo apt-get update
 
 - sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
- sudo mkdir -p /etc/apt/keyrings
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

- echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null 
  
- sudo apt-get update
- sudo apt-get install -y docker-ce

## 2. On each server, install kubernetes
(Installation guide: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

- sudo apt-get update
- sudo apt-get install -y apt-transport-https
- sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg
- echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list
- sudo apt-get update
- sudo apt-get install -y kubelet kubeadm kubectl
- sudo apt-mark hold kubelet kubeadm kubectl

## 3. On each server, enable the use of iptables 
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

## 4. On the Master server only, initialize the cluster
- sudo kubeadm init --pod-network-cidr=10.244.0.0/16

Note that you may face this issue
...
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR CRI]: container runtime is not running: output: time="2022-07-24T11:49:16Z" level=fatal msg="getting status of runtime failed: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
, error: exit status 1

Solution: Ref (https://github.com/containerd/containerd/issues/4581#issue-708113921)

- sudo rm /etc/containerd/config.toml
- sudo systemctl restart containerd
- sudo kubeadm init --pod-network-cidr=10.244.0.0/16


(After this command finishes, copy kubeadm join provided)

## 5. On the Master server only, set up the kubernetes configuration file for general usage
- mkdir -p $HOME/.kube
- sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
- sudo chown $(id -u):$(id -g) $HOME/.kube/config

## 6. On the Master server only, apply a common networking plugin. In this case, Flannel
- kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml

## 7. On the Worker servers only, join them to the cluster using the command you copied earlier. 
- sudo kubeadm join 172.31.37.80:6443 --token ... --discovery-token-ca-cert-hash ...


YOUTUBE VIDEO https://youtu.be/vpEDUmt_WKA



