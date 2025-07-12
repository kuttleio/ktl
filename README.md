# KTL.ai DevPortal Helm Chart

KTL.ai turns any Kubernetes cluster into a self-hosted DevOps platform / PaaS with full control.

**Key features**
- Isolated dev/stage/prod environments via CRD & UI
- One-click stop / start, status & live logs
- Built-in Ingress with TLS, DNS automation & WAF
- CI/CD ready: Helm, GitHub Actions, GitOps
- Secure by default: RBAC, NetworkPolicies, image scanning

---

## Quick install (kind / minikube / any cluster)
```bash
# Add the Helm repo (S3):
helm repo add ktl https://ktl-helm-charts.s3.amazonaws.com
helm repo update
helm install ktl ktl/ktl \
  --namespace ktl --create-namespace

# Forward the UI to your workstation
kubectl -n ktl port-forward svc/klient 8080:80
# Open http://localhost:8080
```

## AWS EKS
```bash
helm install ktl ktl/ktl \
  --namespace ktl --create-namespace \
  -f profiles/values-eks.yaml
# Wait for the LoadBalancer to get an address
kubectl get svc -n ktl klient
```

> GKE / AKS: create a profile file the same way or pass `--set service.type=LoadBalancer` plus provider-specific annotations.

---

### Local demo on kind (full flow)
```bash
kind create cluster --name ktl
helm repo add ktl https://kuttleio.github.io/ktl && helm repo update
helm install ktl ktl/ktl --namespace ktl --create-namespace
kubectl -n ktl port-forward svc/klient 8080:80
```

---

Defaults (`values.yaml`) are cloud-agnostic (ClusterIP, no cloud annotations).  Cloud-specific settings live in profile files or can be set inline with `--set`.
