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

# Expose a port for a Deployment/Pod
kubectl expose ...

# Create a resource
kubectl create [resource]

# Create/modify a resource
kubectl apply [resource]
```

## Web UI Dashboard

### Installation 

Follow this link https://github.com/kubernetes/dashboard

To deploy Dashboard, execute following command:


```
# Create the Pod
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.0.3/aio/deploy/recommended.yaml

# Locate service-account-token and copy then
kubectl describe secret -n kube-system

# Create a proxy 
kubectl proxy
```
Now access Dashboard at:

http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/
