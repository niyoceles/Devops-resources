# Deploy application in Kubernetes On AWS EC2 with kubeadm

## Let have two instances
- master
- worker1 (you may have multiple worker machine but let have one)

## 1. On each server, install Docker
(Follow Installation guide: https://docs.docker.com/engine/install/ubuntu/)

 - ```sudo apt-get update```
 
 - sudo apt-get install \
    ca-certificates \
    curl \
    gnupg \
    lsb-release
    
- ```sudo mkdir -p /etc/apt/keyrings```
- ```curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg```

- ```echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null```
  
- ```sudo apt-get update```
- ```sudo apt-get install -y docker-ce```

## 2. On each server, install kubernetes
(Installation guide: https://kubernetes.io/docs/setup/production-environment/tools/kubeadm/install-kubeadm/)

- ```sudo apt-get update```

- ``` sudo apt-get install -y apt-transport-https```

- ```sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg```

- ```echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee``` ```/etc/apt/sources.list.d/kubernetes.list```

- ```sudo apt-get update```

- ```sudo apt-get install -y kubelet kubeadm kubectl```

- ```sudo apt-mark hold kubelet kubeadm kubectl```

## 3. On each server, enable the use of iptables 
- ```echo "net.bridge.bridge-nf-call-iptables=1" | sudo tee -a /etc/sysctl.conf```

- ```sudo sysctl -p```

## 4. On the Master server only, initialize the cluster
- ```sudo kubeadm init --pod-network-cidr=10.244.0.0/16```

__Note that you may face this ISSUE__

> [preflight] Running pre-flight checks
error execution phase preflight: [preflight] Some fatal errors occurred:
	[ERROR CRI]: container runtime is not running: output: time="2022-07-24T11:49:16Z" level=fatal msg="getting status of runtime failed: rpc error: code = Unimplemented desc = unknown service runtime.v1alpha2.RuntimeService"
, error: exit status 1

__Solution: Ref (https://github.com/containerd/containerd/issues/4581#issue-708113921)__

- ```sudo rm /etc/containerd/config.toml```
- ```sudo systemctl restart containerd```
- ```sudo kubeadm init --pod-network-cidr=10.244.0.0/16```


(After this command finishes, copy kubeadm join provided)

## 5. On the Master server only, set up the kubernetes configuration file for general usage
- ```mkdir -p $HOME/.kube```
- ```sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config```
- ```sudo chown $(id -u):$(id -g) $HOME/.kube/config```

## 6. On the Master server only, apply a common networking plugin. In this case, Flannel
- ```kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/master/Documentation/kube-flannel.yml```

## 7. On the Worker servers only, join them to the cluster using the command you copied earlier. 
- kubeadm join 172.31.15.189:6443 --token mb5d8g.9j1hnpftkttqsg8m \
        --discovery-token-ca-cert-hash sha256:545c1be6b8e4f3ea6dd4fdeff3938b4f6956c91b6190f9bff3cb80c1b98b5e09

## 8. Deployment in Cluster using IMPERATIVE APPROACH
```kubectl create deployment first-app --image=niyoceles/node-web-application```
( .......................... app name  and ... image from docker registry)

## 9. Get all deployments
```kubectl get deployments```

## 10 Exposing a Deployment with a Service
```kubectl expose deployment first-app --type=LoadBalancer --port=80```

## 11. Get all running services
```kubectl get services```



