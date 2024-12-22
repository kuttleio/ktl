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
   helm repo add kuttle https://kuttleio.github.io/ktl/
   ```

2.	Install Kuttle with default settings:
   ```bash
   helm install ktl kuttle/ktl --namespace kuttle
   ```

3.	Customize installation:
To customize your installation, provide additional parameters during installation. For example:
   ```bash
   helm install ktl kuttle/ktl --namespace kuttle \
    --set ingress.host=custom.yourdomain.com \
    --set license.token=abcd-1234-efgh-5678
   ```

## 📦 **How to Update**

1.	Update the Helm repository:
Make sure you have the latest version of the Helm repository:
   ```bash
   helm repo update
   ```

2.	Upgrade Kuttle to the latest version:
Upgrade Kuttle to the latest version:
    ```bash
   helm upgrade ktl kuttle/ktl --namespace kuttle
   ```

## 📦 **Customizing Installation**

| **Parameter**      | **Default**         | **Description**               |
|-------------------|--------------------|-------------------------------|
| `namespace`        | `kuttle`           | Namespace for Kuttle resources |
| `operator.image`   | `kuttleio/ktl:v1.0.0`| Image for the Kuttle operator  |
| `operator.replicas`| `1`                 | Number of replicas             |
| `ingress.host`     | `app.ktl.ai`       | Custom domain for access       |
| `license.token`    | `FREE`             | License token                  |

To customize your installation:
- Modify the `values.yaml` file directly, or
- Pass parameters via the `--set` flag during installation.

Example:
```bash
helm install ktl kuttle/ktl --namespace kuttle \
  --set ingress.host=custom.yourdomain.com \
  --set operator.replicas=2