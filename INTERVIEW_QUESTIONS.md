# Kubernetes Interview Questions and Answers

This document contains answers to common Kubernetes interview questions based on practical experience with deploying an NGINX app using Minikube.

## 1. What is Kubernetes?
Kubernetes is an open-source platform for automating the deployment, scaling, and management of containerized applications. It orchestrates containers (e.g., Docker) across a cluster of nodes, ensuring high availability, load balancing, and self-healing (e.g., restarting failed pods). It abstracts infrastructure complexity, allowing developers to focus on application logic. In my Minikube task, I used Kubernetes to deploy and manage an NGINX app locally.

## 2. What is the role of kubelet?
Kubelet is an agent that runs on each node in a Kubernetes cluster. It ensures containers described in pod specifications are running and healthy by communicating with the container runtime (e.g., Docker). It registers the node with the cluster, reports its status, and executes commands from the Kubernetes control plane (e.g., starting or stopping pods). In my Minikube setup, kubelet managed the NGINX pods on the single-node cluster.

## 3. Explain pods, deployments, and services.
- **Pods**: The smallest deployable unit in Kubernetes, consisting of one or more containers that share storage and network resources. In my task, each NGINX pod ran a single container.
- **Deployments**: A higher-level resource that manages pod replicas and ensures the desired state (e.g., 2 or 4 replicas). It handles updates and rollbacks. I created a deployment (`deployment.yaml`) to manage NGINX pods.
- **Services**: An abstraction to expose pods via a stable IP and DNS name, enabling communication. I used a `NodePort` service (`service.yaml`) to expose NGINX on port 30007, accessible via `minikube service`.

## 4. How do you scale in Kubernetes?
Scaling in Kubernetes adjusts the number of pod replicas to handle load. You can:
- **Manually**: Use `kubectl scale` to change replicas (e.g., `kubectl scale deployment/nginx-deployment --replicas=4` increased my NGINX deployment from 2 to 4 pods).
- **Automatically**: Use Horizontal Pod Autoscaler (HPA) to scale based on CPU/memory metrics, though I didn’t use this in my task.
- After scaling, `kubectl get pods` verified the new pod count. Kubernetes ensures the desired state is maintained.

## 5. What is a namespace?
A namespace is a virtual cluster within a Kubernetes cluster to isolate resources (e.g., pods, services). It’s useful for multi-team or multi-environment setups (e.g., dev, prod). By default, resources like my NGINX deployment and service were in the `default` namespace. You can create custom namespaces with `kubectl create namespace`.

## 6. Difference between ClusterIP, NodePort, LoadBalancer.
- **ClusterIP**: Default service type, exposes the service on a cluster-internal IP, accessible only within the cluster (e.g., for internal communication).
- **NodePort**: Exposes the service on a specific port (30000-32767) of each node, making it accessible externally (e.g., my NGINX service on 30007).
- **LoadBalancer**: Exposes the service externally using a cloud provider’s load balancer, assigning an external IP (not used in my local Minikube setup).
In my task, `NodePort` allowed browser access to NGINX via the Minikube IP.

## 7. What are config maps?
ConfigMaps are Kubernetes resources to store configuration data (e.g., environment variables, files) separately from application code. They decouple configuration from containers, enabling dynamic updates. For example, you could use a ConfigMap to store NGINX configuration settings and mount it into pods, though I didn’t use it in my task.

## 8. How do you perform rolling updates? Explain the task in detail.
A rolling update in Kubernetes updates an application (e.g., changing the NGINX image version) without downtime by gradually replacing old pods with new ones. Here’s a step-by-step process:

#### Step 1: Set Up the Environment
- Install Minikube and kubectl, then start Minikube with `minikube start`.

#### Step 2: Create the Initial Deployment
- Create `deployment.yaml`:
  ```yaml
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
    labels:
      app: nginx
  spec:
    replicas: 2
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
          image: nginx:1.19
          ports:
          - containerPort: 80