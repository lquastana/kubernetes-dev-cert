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

# List only Pods
kubectl get pods

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


# Pods

## Basic command 

```
# Running a Pod
kubectl run [container-name] --image=[image-name]

# List only Pods
kubectl get pods

# Forward a port to allow external access
kubectl port-forward [pod] 8080:20 (External:Internal)

# Delete a pod to be recreated
kubectl delete pod [pod]

# Delete deployment that manages the pod
kubectl delete deployment [pod]

```

## YAML for pod definition

```
# Create a pod using YAML 
# --validate arg is for validating the YAML
kubectl create -f .\nginx.pod.yml --save-config

# Create/ modify
kubectl apply -f .\nginx.pod.yml

# delete Pod with yaml file
kubectl delete pod -f .\nginx.pod.yml

# Get saved config 
kubectl get pod my-nginx -o yaml

# Get pod description
kubectl describe pod my-nginx

# Jump to pod 
kubectl exec my-nginx -it sh

# Edit pod 
kubectl edit -f .\nginx.pod.yml

# Delete pod 
kubectl delete -f .\nginx.pod.yml

```
