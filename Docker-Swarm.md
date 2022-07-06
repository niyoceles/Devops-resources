# Docker Swarm

## AWS EC2 instances
https://medium.com/swlh/docker-swarm-tutorial-deploying-spring-boot-microservices-to-multiple-aws-ec2-instances-f28488179d0a

## After two Ec2 up and running, install Docker
```sudo yum install docker```

## Initialize docker swarm
```docker swarm init --advertise-addr its-own-Public-IP```

## Then copy the generated token, paste in another docker machine to join
```docker swarm join --token SWMTKN-1-2p3lnfcxnh3j6s6199gob2hrcar2kkb3q03bri31sd7qfgf82s-2e7dgeatr7vafgkalapwedqs3 3.88.62.213:2377```

## In node Manager, deploy a service to multiple docker node
```docker service create --name "myapp" -p 80:80 --mode global Image```
### --mode global help to deploy a service to multiple docker machine/node

## Create new security Group named docker

### INBOUND
![image](https://user-images.githubusercontent.com/30776949/177512900-b35ea94e-deae-4195-844f-59baf4fa3c47.png)

### OUTBOUND
<img width="1137" alt="image" src="https://user-images.githubusercontent.com/30776949/177513141-6b6bef34-b3f3-49e5-9019-69586ed2ce60.png">
