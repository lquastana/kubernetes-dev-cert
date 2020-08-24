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

## Pod's Health

https://kubernetes.io/docs/tasks/configure-pod-container/configure-liveness-readiness-startup-probes/


Types of probes 
- Liveness Probe
- Readness probes

Failed Pod are recreated by default

You have several types of command :
- exec
  ```
      livenessProbe:
      exec:
        command:
        - cat
        - /tmp/healthy
      initialDelaySeconds: 5
      periodSeconds: 5
  ```
- httpGet
  ```
      livenessProbe:
        httpGet:
          path: /index.html
          port: 80
        initialDelaySeconds: 15
        timeoutSeconds: 2 # Default is 1
        periodSeconds: 5 # Default is 10
        failureThreshold: 1 # Default is 3
  ```
- tcpSocket
  ```
      readinessProbe:
      tcpSocket:
        port: 8080
      initialDelaySeconds: 5
      periodSeconds: 10

  ```

# Deployments

**ReplicaSet** is a declarative way to manage Pods
A **Deployment** is a declarative way to manage Pods using a ReplicaSet

Deployments and ReplicaSets ensure Pods stay running and can be used to scale Pods

ReplicaSet is a Pod controller :
- Self-Healing mechanism
- Ensure the number of Pods are available
- Provide Fault tolerance
- Pods scaling
- Relies on a Pod template

A deployment manages Pods : 
- Pods are managed using ReplicaSets
- Scale ReplicatSets,which scale Pods
- 0-downtime update by creating and destroying ReplicaSets
- Provides rollback functionality
- Creates a unique label that is assigned to the ReplicaSet and generated Pods
- Yaml is very similar to a ReplicaSet

```
# Create a Deployment
kubectl create -f .\nginx.deployment.yml
kubectl apply -f .\nginx.deployment.yml --save-config

# List deplyments
kubectl get deployments --show-labels
kubectl get deployments -l app=my-nginx

# Delete deployment
kubectl delete deployment [deployment-name] 

# Scale Pods to 5
kubectl scale deployment [deply-name] --replicas=5
```
# Deployments Options

Zero downtime deployments

Several options are available :
- Rolling updates
- Blue-green deployments
- Canary deployments
- Rollbacks

Rolling updates is the default deployment method

```
cd ./node-app

# Build all docker images inside v1,v2,v3,v4
docker build --pull --rm -f "node-app\v1\dockerfile" -t node-app:1.0 "node-app\v1"
docker build --pull --rm -f "node-app\v2\dockerfile" -t node-app:2.0 "node-app\v2"
docker build --pull --rm -f "node-app\v3\dockerfile" -t node-app:3.0 "node-app\v3"
docker build --pull --rm -f "node-app\v4\dockerfile" -t node-app:4.0 "node-app\v4"

# Run load balancer
kubectl apply -f .\node-app.service.yml

# Run deployment
kubectl apply -f .\node-app-v1.deployment.yml

# Run migration
kubectl.exe apply -f .\node-app-v2.deployment.yml
kubectl.exe apply -f .\node-app-v3.deployment.yml

```

# Creating services

## Services core concept 

A service provide a single point of entry for accessing one or more Pods

- Pods are ephemeral , you cant rely two Pods on IP adress.
- Services can obstract Pod IP and manage load balancing between Pods
- Relies on labels to associate a Service with a Pod
- Nods kube-proxy creates a virtual IP for services
- Layer 4
- Service are not ephemeral

## Services types

Services can be define in deffirent ways : 

- ClusterIP : Expose the service on cluster internal IP
  - Only pods within the cluster can talk to the service
  - Allow pods to talk to other Pods
- NodePort : Expose the service on each Nodes IP at a static port
  - Allocate a port for a range
  - Each Node proxies the allocated port
- LoadBalancer : Provision an external IP to act as a load balancer 
  - NodePort and ClusterIP services are created
- ExternalName : Maps a service to a DNS name

## Creating a service with kubectl


```
# Listen on port 8080 locally and forward to port 80 in a pod
kubectl port-forward pod/[name] 8080:80

# Listen a port 8080 and forward to deployment's Pod
kubectl port-forward deployment/[name] 8080:80

# Listen a port 8080 and forward to service'ss Pod
kubectl port-forward service/[name] 8080:80
```

## Implementing a service configuration

Implementing a service : 
- [Example port forwarding](services/sample.service.yml)
- [Example nodePort](services/nodeport.service.yml)
- [Example loadBalancer](services/loadbalancer.service.yml)
- [Example externalService](services/externalname.service.yml)

## Creating a service with file

```
# Create a service
kubectl create -f file.service.yml

# Update a service
# Assumes --save-configd was used with create
kubectl apply -f file.service.yml


# Delete a service
kubectl delete -f file.service.yml

# Test pod service with curl
kubectl exec [pod-name] -- curl -s http://podIP

# Install and use curl
kubectl exec [pod-name] -it sh
apk add curl
curl -s http://podIP

```

## Pratice service


```
# Create frontend pods
kubectl.exe apply -f .\nginx.deployment.yml 

# Verify pods
kubectl get all

# Get pod info (like ip) 
kubectl.exe pod frontend-69f46949bc-pp9q9 -o yaml

# Creating a service
kubectl.exe apply -f .\clusterIP.service.yml

# See services 
kubectl.exe get services

# Service is like dns name
curl -s nginx-clusterip:8080

# Delete service
kubectl.exe delete service nginx-clusterip

# Create node port
kubectl apply -f .\services\nodeport.service.yml

# Create load balancer
kubectl apply -f .\services\loadbalancer.service.yml

```
