# Deploy Kubernetes On AWS EC2

## 1. On each server, install Docker
(Installation guide: https://docs.docker.com/engine/instal...)
curl -fsSL https://download.docker.com/linux/ubu... | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get update
sudo apt-get install -y docker-ce

2. On each server, install kubernetes
(Installation guide: https://kubernetes.io/docs/setup/prod...)
curl -s https://packages.cloud.google.com/apt... | sudo apt-key add -
cat &lt&lt EOF | sudo tee /etc/apt/sources.list.d/kubernetes.list
deb https://apt.kubernetes.io/ kubernetes-xenial main
EOF
sudo apt-get update
sudo apt-get install -y kubelet kubeadm kubectl

3. On each server, enable the use of iptables 
echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p

4. On the Master server only, initialize the cluster
sudo kubeadm init --pod-network-cidr=10.244.0.0/16

(After this command finishes, copy kubeadm join provided)

5. On the Master server only, set up the kubernetes configuration file for general usage
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

6. On the Master server only, apply a common networking plugin. In this case, Flannel
kubectl apply -f https://raw.githubusercontent.com/cor...

7. On the Worker servers only, join them to the cluster using the command you copied earlier. 
kubeadm join 172.31.37.80:6443 --token ... --discovery-token-ca-cert-hash ...


YOUTUBE VIDEO https://youtu.be/vpEDUmt_WKA

## ISUUE 
# Configure containerd
mkdir -p /etc/containerd
containerd config default > /etc/containerd/config.toml
systemctl restart containerd

kubeadm init
...
[preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR CRI]: container runtime is not running: output: time="2020-09-24T11:49:16Z" level=fatal msg="getting status of runtime failed: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
, error: exit status 1

## SOLUTION https://github.com/containerd/containerd/issues/4581#issue-708113921

