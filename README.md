# Certified Kubernetes Application Developer (CKAD) 

## Installation

Installing docker desktop : https://docs.docker.com/get-docker/
Go to settings > kubernetes > Check enable Kubernetes

## Basic command 

```
# Check kub version
kubectl version

# View cluster info
kubectl cluster-info

# Information about Kub ( Pods,Deployments,Services etc ...)
kubectl get all

# Create a Pod
kubectl run [container-name] --image=[image-name]

# Forward a port to allow external access
kubectl port-forward [pod] [ports]                  
```