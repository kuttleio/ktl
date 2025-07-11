# KTL.ai DevPortal Helm Chart

KTL DevPortal lets you create and manage fully-isolated Kubernetes *environments* with one Helm install.

---

## Quick install (local kind / minikube / any cluster)
```bash
helm repo add ktl https://kuttleio.github.io/ktl
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

*(Add profiles for GKE / AKS the same way if required.)*

---

### Local demo on kind
```bash
kind create cluster --name ktl
helm repo add ktl https://kuttleio.github.io/ktl && helm repo update
helm install ktl ktl/ktl --namespace ktl --create-namespace
kubectl -n ktl port-forward svc/klient 8080:80
```

---

`values.yaml` defaults are cloud-agnostic (ClusterIP, no cloud annotations).  Cloud-specific options live in separate profile files or can be passed via `--set`.
