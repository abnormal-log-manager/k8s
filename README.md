# Connecting to a Kubernetes Cluster with Kubeconfig and CA Cert

## 1. Use the Kubeconfig File

If you have a valid `kubeconfig.yaml`, you can use it to connect to a Kubernetes cluster:

```bash
kubectl --kubeconfig=/path/to/your/kubeconfig.yaml get nodes

Or set it as an environment variable:
export KUBECONFIG=/path/to/your/kubeconfig.yaml
kubectl get nodes
```
## 2. Certificate Authority (CA) Handling
Your kubeconfig.yaml will include either a path to the certificate authority file or the base64-encoded content.
### Option A: Using a File
```yaml
clusters:
- name: my-cluster
  cluster:
    server: https://your-cluster-endpoint
    certificate-authority: /path/to/ca.crt
```
### Option B: Using Embedded Base64 Data
```yaml
clusters:
- name: my-cluster
  cluster:
    server: https://your-cluster-endpoint
    certificate-authority-data: LS0tLS1...   # base64 encoded content
```
To generate base64 from a CA file
```bash
base64 -w0 /path/to/ca.crt
```
## 3. Common Certificate Errors
If you see this error:
```bash
Unable to connect to the server: x509: certificate signed by unknown authority
```
- Ensure the certificate-authority file path is correct.
- Or embed the CA using certificate-authority-data.
### Temporary Workaround (Not Recommended for Production)
You can skip TLS verification:
```bash
kubectl --insecure-skip-tls-verify=true --kubeconfig=./kubeconfig.yaml get pods
```
Or modify your config:
```yaml
insecure-skip-tls-verify: true
```
## 4. Example Kubeconfig with Embedded CA
```yaml
apiVersion: v1
kind: Config
clusters:
- name: demo
  cluster:
    server: https://1.2.3.4:6443
    certificate-authority-data: LS0tLS1...   # base64 ca.crt
users:
- name: demo-user
  user:
    client-certificate-data: LS0t...         # Optional
    client-key-data: LS0t...                 # Optional
contexts:
- name: demo-context
  context:
    cluster: demo
    user: demo-user
current-context: demo-context
```
