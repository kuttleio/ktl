# KTL.ai DevPortal Helm Chart

KTL.ai turns any Kubernetes cluster into a self-hosted DevOps platform / PaaS with full control.

**Key features**
- Isolated dev/stage/prod environments via CRD & UI
- One-click stop / start, status & live logs
- Built-in Ingress with TLS, DNS automation & WAF
- CI/CD ready: Helm, GitHub Actions, GitOps
- Secure by default: RBAC, NetworkPolicies, image scanning

---

## Quick install (kind / minikube)
```bash
# 1) Install NGINX Ingress (if not present)
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx && helm repo update
helm upgrade --install nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace \
  --set controller.admissionWebhooks.enabled=false

# 2) Install KTL DevPortal with Ingress enabled (host left blank → wildcard)
helm repo add ktl https://ktl-helm-charts.s3.amazonaws.com && helm repo update
helm upgrade --install ktl ktl/ktl \
  --namespace ktl --create-namespace \
  --set ingress.enabled=true \
  --set ingress.className=nginx

# 3) Port-forward the ingress controller
kubectl -n ingress-nginx port-forward svc/nginx-ingress-nginx-controller 8080:80
# UI → http://localhost:8080 , API lives at /api/*
```

## AWS EKS
```bash
helm install ktl ktl/ktl \
  --namespace ktl --create-namespace \
  -f profiles/values-eks.yaml
# Wait for the LoadBalancer to get an address
kubectl get svc -n ktl klient
```

## Google GKE
```bash
helm install ktl ktl/ktl \
  --namespace ktl --create-namespace \
  -f profiles/values-gke.yaml
# Wait for the LoadBalancer to get an address
kubectl get svc -n ktl klient
```

## Azure AKS
```bash
helm install ktl ktl/ktl \
  --namespace ktl --create-namespace \
  -f profiles/values-aks.yaml
# Wait for the LoadBalancer to get an address
kubectl get svc -n ktl klient
```

> GKE / AKS: use the profile files above or pass `--set service.type=LoadBalancer` and provider annotations.

---

### Local demo on kind (full flow)
```bash
kind create cluster --name ktl
helm repo add ktl https://ktl-helm-charts.s3.amazonaws.com && helm repo update
helm install ktl ktl/ktl --namespace ktl --create-namespace
# Wait until the pods in namespace ktl are Running
kubectl -n ktl port-forward svc/klient 8080:80
```

---

## Ingress / API routing
KTL DevPortal uses path-based routing ("/api" → kop, everything else → klient).  The Helm chart ships an Ingress template, but
* **kind / minikube** clusters have **no controller by default** – install NGINX Ingress first:
```bash
helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx && helm repo update
helm install nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx --create-namespace
# optional: expose locally
kubectl -n ingress-nginx port-forward svc/nginx-ingress-nginx-controller 8080:80
```
Then install/upgrade the chart with Ingress enabled:
```bash
helm upgrade --install ktl ktl/ktl \
  --namespace ktl --create-namespace \
  --set ingress.enabled=true \
  --set ingress.className=nginx \
  --set ingress.host=dev.ktl.local
```
* **EKS** – make sure the [AWS Load Balancer Controller](https://docs.aws.amazon.com/eks/latest/userguide/aws-load-balancer-controller.html) is installed (or use nginx-ingress).  Quick install:
```bash
helm repo add eks https://aws.github.io/eks-charts && helm repo update
helm install aws-lbc eks/aws-load-balancer-controller \
  --namespace kube-system \
  --set clusterName=<CLUSTER_NAME> \
  --set serviceAccount.create=true \
  --set region=<AWS_REGION> \
  --set vpcId=<VPC_ID>
```
A ready profile exists in `profiles/values-eks.yaml`.
* **GKE / AKS** – use the built-in Ingress controller or install nginx-ingress the same way.

With the controller present, visiting `http://<host>/` loads the UI, while `http://<host>/api/...` is transparently proxied to kop.

---

Defaults (`values.yaml`) are cloud-agnostic (ClusterIP, no cloud annotations).  Cloud-specific settings live in profile files or can be set inline with `--set`.
