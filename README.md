# Kubernetes Minikube Nginx Deployment

This project demonstrates how to set up a Kubernetes cluster using Minikube, deploy a sample Nginx application, expose it using a service, and access it through a browser.

##  Tools Used

- **Minikube** - Local Kubernetes cluster
- **Docker** - Container runtime
- **kubectl** - Kubernetes CLI
- **YAML** - Configuration files for Deployment & Service

## Project Structure

```
task5-devops/
│
├── deployment.yaml       # Kubernetes Deployment file
├── service.yaml          # Kubernetes Service (NodePort) file
├── README.md             # Documentation
└── screenshots/          # Terminal & browser screenshots
```

## ⚙️ Prerequisites

Before starting, ensure you have the following installed:

1. **Docker Desktop** - [Download here](https://www.docker.com/products/docker-desktop/)
2. **Minikube** - [Installation guide](https://minikube.sigs.k8s.io/docs/start/)
3. **kubectl** - [Installation guide](https://kubernetes.io/docs/tasks/tools/)

## 🚀 Step-by-Step Instructions

### Step 1: Start Minikube Cluster

Start Minikube with Docker driver:

```bash
minikube start --driver=docker
```

Verify if the node is running:

```bash
kubectl get nodes
```

Expected output:
```
NAME       STATUS   ROLES           AGE   VERSION
minikube   Ready    control-plane   1m    v1.28.3
```

### Step 2: Create Deployment

The `deployment.yaml` file is already configured with:

- **Name**: nginx-deployment
- **Replicas**: 2
- **Image**: nginx:latest
- **Port**: 80

Apply the deployment:

```bash
kubectl apply -f deployment.yaml
```

Verify pods are running:

```bash
kubectl get pods
```

Expected output:
```
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          30s
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          30s
```

### Step 3: Expose Deployment using Service

The `service.yaml` file is configured with:

- **Type**: NodePort
- **Port**: 80
- **Target Port**: 80
- **Node Port**: 30008

Apply the service:

```bash
kubectl apply -f service.yaml
```

Verify service is created:

```bash
kubectl get svc
```

Expected output:
```
NAME            TYPE        CLUSTER-IP      EXTERNAL-IP   PORT(S)        AGE
kubernetes      ClusterIP   10.96.0.1       <none>        443/TCP        2m
nginx-service   NodePort    10.96.xxx.xxx   <none>        80:30008/TCP   30s
```

### Step 4: Access Application

#### ✅ Method 1: Using Minikube Service Command (Recommended)

```bash
minikube service nginx-service
```

This will automatically open your browser with a URL like:
```
http://127.0.0.1:61004/
```

#### ✅ Method 2: Manual URL Access

Get Minikube IP:

```bash
minikube ip
```

Example output: `192.168.49.2`

Access the application in your browser:
```
http://<minikube-ip>:30008
```

Example: `http://192.168.49.2:30008`

### Step 5: Scale Deployment

Scale the deployment to 5 replicas:

```bash
kubectl scale deployment nginx-deployment --replicas=5
```

Verify scaling:

```bash
kubectl get pods
```

Expected output (5 pods):
```
NAME                               READY   STATUS    RESTARTS   AGE
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          2m
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          2m
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          30s
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          30s
nginx-deployment-xxxxxxxxx-xxxxx   1/1     Running   0          30s
```

### Step 6: Useful Commands

#### Check deployment status:
```bash
kubectl get deployments
```

#### Check service details:
```bash
kubectl describe service nginx-service
```

#### View pod logs:
```bash
kubectl logs -l app=nginx
```

#### Get detailed pod information:
```bash
kubectl describe pods -l app=nginx
```

### Step 7: Cleanup

#### Stop cluster:
```bash
minikube stop
```

#### Delete cluster completely:
```bash
minikube delete
```

#### Delete specific resources:
```bash
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml
```

##
