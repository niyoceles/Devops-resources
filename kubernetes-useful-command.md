# Kubernetes resources

## Starting cluster on Mac
```minikube start --driver=hyperkit```

## Starting cluster on Windows
```minikube start --driver=hyperv```

## Alternatively start cluster
```minikube start --driver=docker```

## Kubernetes dashboard
```minikube dashboard```

## Deployment in Cluster using IMPERATIVE APPROACH
```kubectl create deployment first-app --image=niyoceles/node-web-application```
```......................... app name ..image (from docker registry)```
## Get all deployments
```kubectl get deployments```

## Get all running services
```kubectl get services```

## Exposing a Deployment with a Service
```kubectl expose deployment first-app --type=LoadBalancer --port=80```


## Get url link of the running service
```minikube service first-app --url flag```

