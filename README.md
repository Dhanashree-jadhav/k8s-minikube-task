# Kubernetes Minikube NGINX Deployment

This project demonstrates how to deploy and manage an NGINX application on a local Kubernetes cluster using Minikube.

## Tools Used
- Docker Desktop (Windows)
- Minikube
- kubectl

## Steps Performed
1. Installed Minikube and started the cluster locally.
2. Created a Deployment for an NGINX container with 2 initial replicas.
3. Exposed the deployment using a Service of type NodePort on port 30007.
4. Verified the deployment and service status with `kubectl get pods` and `kubectl get services`.
5. Accessed the NGINX welcome page in the browser using `minikube service nginx-service`.
6. Scaled the deployment to 4 replicas.
7. Captured screenshots for verification.

## YAML Files
- `deployment.yaml`: Defines the NGINX deployment with 2 initial replicas, scaled to 4.
- `service.yaml`: Exposes the NGINX app with a NodePort on port 30007.

## Screenshots
- `pods-initial.png`: Shows the initial 2 pods in `Running` state.
- `pods-scaled.png`: Shows the deployment scaled to 4 pods.
- `services.png`: Displays the service details, including `nginx-service`.
- `nginx-page.png`: Captures the NGINX welcome page in the browser.

## Commands Used
```bash
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl get pods
kubectl get services
kubectl scale deployment/nginx-deployment --replicas=4
minikube service nginx-service