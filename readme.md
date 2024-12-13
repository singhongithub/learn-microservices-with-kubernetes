# Kubernetes Docker Project with Microservices

This project demonstrates the deployment of two microservices, **Currency Exchange** and **Currency Conversion**, using Docker and Kubernetes.

## Overview
The primary objective of this project is to:

1. Containerize the microservices using Docker.
2. Deploy and manage the microservices using Kubernetes.
3. Enable service discovery, centralized configuration, and scaling for the microservices.

## Steps to Create This Project

### Step 1: Set Up Kubernetes Cluster
1. Create a Kubernetes cluster using Google Kubernetes Engine (GKE) or any other cloud provider.
2. Install `kubectl` and configure it to connect to your cluster:

   ```bash
   gcloud container clusters get-credentials <my-cluster> --zone us-central1-c --project <project-id>
   ```

### Step 2: Create Docker Images for the Microservices
1. Build Docker images for both microservices:

   ```bash
   docker login
   docker push ajayondocker/mmv3-currency-exchange-service:0.0.14-SNAPSHOT
   docker push ajayondocker/mmv3-currency-conversion-service:0.0.14-SNAPSHOT
   ```

### Step 3: Deploy Microservices to Kubernetes
1. Deploy the **Currency Exchange Service**:

   ```bash
   kubectl create deployment currency-exchange --image=ajayondocker/mmv3-currency-exchange-service:0.0.14-SNAPSHOT
   kubectl expose deployment currency-exchange --type=LoadBalancer --port=8000
   ```

2. Deploy the **Currency Conversion Service**:

   ```bash
   kubectl create deployment currency-conversion --image=ajayondocker/mmv3-currency-conversion-service:0.0.14-SNAPSHOT
   kubectl expose deployment currency-conversion --type=LoadBalancer --port=8100
   ```

### Step 4: Verify the Deployment
- Check the services and their external IPs:

  ```bash
  kubectl get svc
  ```

- Access the services using the following URLs:
  - **Currency Exchange Service**: `http://<external-ip>:8000/currency-exchange/from/USD/to/INR`
  - **Currency Conversion Service**: `http://<external-ip>:8100/currency-conversion-feign/from/USD/to/INR/quantity/10`

### Step 5: Manage and Scale the Services
1. Scale the deployments:

   ```bash
   kubectl scale deployment currency-exchange --replicas=3
   kubectl scale deployment currency-conversion --replicas=3
   ```

2. Monitor the pods:

   ```bash
   kubectl get pods
   kubectl logs <pod-name>
   ```

### Step 6: Create Kubernetes YAML Configuration
1. Export the deployment and service configuration to YAML files:

   ```bash
   kubectl get deployment currency-exchange -o yaml > currency-exchange-deployment.yaml
   kubectl get service currency-exchange -o yaml > currency-exchange-service.yaml
   ```

2. Modify and apply the configurations:

   ```bash
   kubectl apply -f currency-exchange-deployment.yaml
   kubectl apply -f currency-exchange-service.yaml
   ```

## Additional Features
- **Liveness and Readiness Probes**: Ensure microservices are healthy and ready to serve traffic.
- **Autoscaling**: Automatically scale deployments based on CPU usage.

   ```bash
   kubectl autoscale deployment currency-exchange --min=1 --max=3 --cpu-percent=50
   ```

## Useful Links
- [Google Kubernetes Engine Documentation](https://cloud.google.com/kubernetes-engine/docs)
- [Kubernetes Official Documentation](https://kubernetes.io/docs/)
- [Docker Documentation](https://docs.docker.com/)
