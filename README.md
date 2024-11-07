# basic-istio-config

Here’s a concise `README.md` for your GitHub repository:

```markdown
# Istio Demo with Spring Boot Microservices on GKE

This project demonstrates deploying a Spring Boot microservice in a Google Kubernetes Engine (GKE) cluster with Istio service mesh for advanced traffic management.

## Prerequisites

- **Google Cloud SDK** with `kubectl` and `gcloud` configured
- **Docker** installed and running
- **Java 17** (or compatible JDK)

## Steps

### 1. Set up GKE Cluster

Create a GKE cluster with necessary configurations:
```bash
gcloud container clusters create istio-demo-cluster --zone us-central1-a --machine-type e2-standard-4 --num-nodes 3 --enable-ip-alias
```

### 2. Install Istio

Install Istio CLI and set up Istio on the cluster:
```bash
curl -L https://istio.io/downloadIstio | sh -
export PATH="$PATH:/path/to/istio/bin"
istioctl install --set profile=demo -y
```

### 3. Deploy Spring Boot Service

Navigate to the `service-1` directory and build the Docker image:
```bash
docker build -t gcr.io/your-project-id/service-1:latest .
docker push gcr.io/your-project-id/service-1:latest
```

Apply Kubernetes manifests:
```bash
kubectl apply -f k8s/
```

### 4. Configure Istio Gateway and VirtualService

Deploy Istio Gateway and VirtualService to route traffic:
```bash
kubectl apply -f istio/gateway.yaml
kubectl apply -f istio/virtual-service.yaml
```

### 5. Access the Application

Get the external IP of the Istio Ingress Gateway:
```bash
kubectl get svc istio-ingressgateway -n istio-system
```

Access the service at `http://<EXTERNAL_IP>/hello`.

## Testing

- Verify the microservice by port-forwarding:
  ```bash
  kubectl port-forward svc/service-1 8080:80 -n sumanns
  ```
- Access locally at `http://localhost:8080/hello`.

## Cleanup

Delete resources:
```bash
kubectl delete -f k8s/
kubectl delete -f istio/
gcloud container clusters delete istio-demo-cluster --zone us-central1-a
```

