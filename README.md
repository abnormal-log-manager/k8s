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
