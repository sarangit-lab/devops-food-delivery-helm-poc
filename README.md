# FoodHub - Kubernetes + Helm DevOps POC

A beginner-friendly DevOps POC that deploys a food-delivery style website using Docker, Kubernetes, Minikube and Helm.

## Project flow

Browser -> Kubernetes Service -> Nginx Pod(s)

Helm manages the Kubernetes Deployment and Service.

## Prerequisites

- Docker Desktop
- Minikube
- kubectl
- Helm
- Git
- VS Code

## 1. Open the project

Open this folder in VS Code.

## 2. Start Minikube

```powershell
minikube start --driver=docker
```

## 3. Build the Docker image

This project uses Nginx, so Go is NOT required.

```powershell
docker build -t foodhub:1.0 .
```

For Minikube, make the image available inside the Minikube Docker environment:

```powershell
minikube image load foodhub:1.0
```

## 4. Create the namespace

```powershell
kubectl apply -f k8s/namespace.yaml
```

## 5. Check the Helm chart

```powershell
helm lint ./helm/foodhub
helm template foodhub ./helm/foodhub
```

## 6. Install the website

```powershell
helm install foodhub ./helm/foodhub -n foodhub
```

## 7. Check deployment

```powershell
kubectl get pods -n foodhub
kubectl get deployment -n foodhub
kubectl get service -n foodhub
helm list -n foodhub
```

## 8. Open the website

```powershell
minikube service foodhub-foodhub -n foodhub
```

This should open the website in your browser.

## 9. Test scaling

```powershell
kubectl scale deployment foodhub-foodhub --replicas=3 -n foodhub
kubectl get pods -n foodhub
```

Or change `replicaCount` in `helm/foodhub/values.yaml` and run:

```powershell
helm upgrade foodhub ./helm/foodhub -n foodhub
```

## 10. Test Helm rollback

Change the replica count or image tag, then upgrade:

```powershell
helm upgrade foodhub ./helm/foodhub -n foodhub
helm history foodhub -n foodhub
helm rollback foodhub 1 -n foodhub
```

## Useful troubleshooting commands

```powershell
kubectl get all -n foodhub
kubectl describe pod <pod-name> -n foodhub
kubectl logs <pod-name> -n foodhub
helm status foodhub -n foodhub
```

## What this POC demonstrates

- Docker containerization
- Nginx web server
- Kubernetes Deployment
- Kubernetes Pods
- Kubernetes Service
- Namespace
- Helm chart
- Helm values
- Helm install
- Helm upgrade
- Helm rollback
- Kubernetes scaling
- Basic troubleshooting
