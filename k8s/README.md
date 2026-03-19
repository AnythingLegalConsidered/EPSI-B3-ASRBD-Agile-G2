# Kubernetes Manifests - NTL Platform

## Architecture

- **Backend** : Symfony/PHP (`bitnami/symfony:latest`) - port 8000
- **Frontend** : nginx:alpine serving static HTML - port 80
- **Ingress** : Traefik (K3s default) with TLS
- **Cluster** : K3s on 3 nodes (192.168.10.100-102)

## Deployment Order

Apply manifests in this order:

### 1. Namespaces

```bash
kubectl apply -f namespace.yaml
```

### 2. ConfigMaps and Secrets

```bash
kubectl apply -f configmap-frontend.yaml
kubectl apply -f configmap-backend.yaml
kubectl apply -f secrets.yaml
```

> **Note:** Edit `secrets.yaml` with real values before applying.
> For TLS, generate certificates first (see `tls-secret.yaml` for instructions).

### 3. Deployments

```bash
kubectl apply -f backend-deployment.yaml
kubectl apply -f frontend-deployment.yaml
```

### 4. Services

```bash
kubectl apply -f backend-service.yaml
kubectl apply -f frontend-service.yaml
```

### 5. Ingress

```bash
# Without TLS:
kubectl apply -f ingress.yaml

# With TLS (after creating the tls-secret):
kubectl apply -f ingress-tls.yaml
```

### 6. HorizontalPodAutoscalers

```bash
kubectl apply -f hpa-backend.yaml
kubectl apply -f hpa-frontend.yaml
```

> **Prerequisite:** metrics-server must be running (included by default in K3s).

### 7. Monitoring

```bash
# Deploy Kubernetes Dashboard
kubectl apply -f https://raw.githubusercontent.com/kubernetes/dashboard/v2.7.0/aio/deploy/recommended.yaml

# Create admin user
kubectl apply -f monitoring/dashboard.yaml

# Get login token
kubectl -n kubernetes-dashboard create token admin-user
```

## Quick Deploy (All at once)

```bash
kubectl apply -f namespace.yaml
kubectl apply -f configmap-frontend.yaml -f configmap-backend.yaml -f secrets.yaml
kubectl apply -f backend-deployment.yaml -f frontend-deployment.yaml
kubectl apply -f backend-service.yaml -f frontend-service.yaml
kubectl apply -f ingress-tls.yaml
kubectl apply -f hpa-backend.yaml -f hpa-frontend.yaml
kubectl apply -f monitoring/dashboard.yaml
```

## Verify Deployment

```bash
# Check all resources in prod namespace
kubectl get all -n prod

# Check pods status
kubectl get pods -n prod -o wide

# Check HPA status
kubectl get hpa -n prod

# Check ingress
kubectl get ingress -n prod

# View logs
kubectl logs -l app=backend -n prod
kubectl logs -l app=frontend -n prod
```

## DNS Configuration

Add to `/etc/hosts` on your local machine (or the machine accessing the cluster):

```
192.168.10.100  k8s.local
```

Then access:
- Frontend: https://k8s.local/
- Backend API: https://k8s.local/api
