## 🚀 **Installation**

1. Add the Kuttle Helm repository:
   ```bash
   helm repo add ktl https://ktlai-install.s3.us-west-2.amazonaws.com/helm-charts/
   helm repo update
   ```

2. Install Kuttle with default settings (latest version):
   ```bash
   helm install ktl ktl/ktl --namespace ktl --create-namespace
   ```

3. Install Kuttle with a specific version:
   ```bash
   helm install ktl ktl/ktl --namespace ktl --create-namespace --version 1.0.1
   ```

4. Customize installation:
   To customize your installation, provide additional parameters during installation. For example:
   ```bash
   helm install ktl ktl/ktl --namespace ktl --create-namespace \
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
   helm upgrade ktl ktl/ktl --namespace ktl
   ```

3. Upgrade Kuttle to a specific version:
   ```bash
   helm upgrade ktl ktl/ktl --namespace ktl --version 1.0.1
   ```

---

## 📦 **Customizing Installation**

| **Parameter**      | **Default**          | **Description**                |
|--------------------|----------------------|--------------------------------|
| `namespace`        | `ktl`                | Namespace for Kuttle resources |
| `operator.image`   | `kuttleio/ktl:v1.0.0`| Image for the Kuttle operator  |
| `operator.replicas`| `1`                  | Number of replicas             |
| `ingress.host`     | `app.ktl.ai`         | Custom domain for access       |
| `license.token`    | `FREE`               | License token                  |

To customize your installation:
- Modify the `values.yaml` file directly, or
- Pass parameters via the `--set` flag during installation.

Example:
```bash
helm install ktl ktl/ktl --namespace ktl --create-namespace \
  --set ingress.host=custom.yourdomain.com \
  --set operator.replicas=2
```
