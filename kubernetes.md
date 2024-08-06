# Kubernetes Guide

## Kubernetes

Kubernetes is an open-source container orchestration platform that automates many of the manual processes involved in deploying, managing, and scaling containerized applications.

### Setting up a Kubernetes Cluster

1. Install kubectl:
   ```bash
   curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl"
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   ```

2. Install Minikube (for local testing):
   ```bash
   curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
   sudo install minikube-linux-amd64 /usr/local/bin/minikube
   ```

3. Start Minikube:
   ```bash
   minikube start
   ```

### Basic Kubernetes Concepts

1. Pods: The smallest deployable units in Kubernetes.
2. Services: An abstract way to expose an application running on a set of Pods.
3. Deployments: Describe the desired state for Pods and ReplicaSets.

### Example Kubernetes Deployment

1. Create a deployment YAML file (`deployment.yaml`):

   ```yaml
   apiVersion: apps/v1
   kind: Deployment
   metadata:
     name: nginx-deployment
   spec:
     replicas: 3
     selector:
       matchLabels:
         app: nginx
     template:
       metadata:
         labels:
           app: nginx
       spec:
         containers:
         - name: nginx
           image: nginx:1.14.2
           ports:
           - containerPort: 80
   ```

2. Apply the deployment:
   ```bash
   kubectl apply -f deployment.yaml
   ```

3. Expose the deployment:
   ```bash
   kubectl expose deployment nginx-deployment --type=LoadBalancer --port=80
   ```

4. Check the status:
   ```bash
   kubectl get deployments
   kubectl get pods
   kubectl get services
   ```

This is just a basic introduction to Kubernetes. The platform offers much more advanced features for complex deployments and scaling.
