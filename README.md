# 🚀 Kuttle - Self-hosted Dev Portal

**Kuttle** is a self-hosted Dev Portal to manage isolated environments within your Kubernetes cluster. With Kuttle, you can easily create, update, and delete fully isolated environments for development, testing, and production use.

---

## 📦 **Features**
- **Full isolation**: Each environment is created in its own namespace.
- **CRD (Custom Resource Definition)**: Define and manage your environments directly in Kubernetes.
- **One-click updates**: Update Kuttle with a single Helm command.
- **Customizable setup**: Use Helm parameters to customize the namespace, domain, and other options.

---

## 🚀 **Installation**

1. Add the Kuttle Helm repository:
   ```bash
   helm repo add kuttle https://ktlai-install.s3.us-west-2.amazonaws.com/helm-charts/
   ```

2. Install Kuttle with default settings:
   ```bash
   helm install kop https://ktlai-install.s3.us-west-2.amazonaws.com/helm-charts/ktl-1.0.1.tgz --namespace kuttle --create-namespace
   ```

3. Customize installation:
To customize your installation, provide additional parameters during installation. For example:
   ```bash
   helm install kop https://ktlai-install.s3.us-west-2.amazonaws.com/helm-charts/ktl-1.0.1.tgz --namespace kuttle \
    --set operator.replicas=2 \
    --set ingress.host=custom.yourdomain.com
   ```

---

## 📦 **How to Update**

1. Update the Helm repository:
   ```bash
   helm repo update
   ```

2. Upgrade Kuttle to the latest version:
   ```bash
   helm upgrade kop https://ktlai-install.s3.us-west-2.amazonaws.com/helm-charts/ktl-1.0.1.tgz --namespace kuttle
   ```

---

## 📦 **Customizing Installation**

| **Parameter**      | **Default**          | **Description**               |
|-------------------|---------------------|-------------------------------|
| `namespace`        | `kuttle`            | Namespace for Kuttle resources |
| `operator.image`   | `kuttleio/ktl:v1.0.0`| Image for the Kuttle operator  |
| `operator.replicas`| `1`                  | Number of replicas             |
| `ingress.host`     | `app.ktl.ai`        | Custom domain for access       |
| `license.token`    | `FREE`              | License token                  |

To customize your installation:
- Modify the `values.yaml` file directly, or
- Pass parameters via the `--set` flag during installation.

Example:
```bash
helm install kop https://ktlai-install.s3.us-west-2.amazonaws.com/helm-charts/ktl-1.0.1.tgz --namespace kuttle \
  --set ingress.host=custom.yourdomain.com \
  --set operator.replicas=2
```

---
