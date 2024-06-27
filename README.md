# Build and run your own intelligent application based on open source using Semantic Kernel and Kaito

## Prerequisites

### Create Cluster

```bash
# Initialize the terraform configuration
terraform init
# Run the terraform plan and apply
terraform plan
terraform apply -auto-approve
```



### Install Kaito

- GPU provisioner config -> `./k8s/gpu-provisioner-values.yaml`
  - The  GPU provisioner leverages Workload Identity. For this we need to pass our config to the helm chart:
  - Update all `ENVs` and `controller.settings.azure.clustername` according to your setup
  - Terraform outputs the Client ID for Kaito, please update `controller.workloadIdentity.clientId`

```bash
# Install to GPU provisioner Helm chart with workload identity custom configuration
helm upgrade -i --namespace gpu-provisioner --create-namespace gpu-provisioner kaito/gpu-provisioner -f ./k8s/gpu-provisioner-values.yaml
# Install to Kaito workspace Helm chart
helm upgrade -i  --namespace workspace --create-namespace workspace kaito/workspace
# Install ingress-nginx
helm upgrade -i  --namespace ingress-nginx --create-namespace ingress-nginx ingress-nginx/ingress-nginx
```

### Install Falcon LLM

```bash
# Create falcon namespace
kubectl create ns falcon
# Install the wokrspace with the preset configuration
kubectl -n falcon apply -f ./k8s/falcon.yaml
# Expose the wokrspace with the ingress controller
kubectl -n falcon apply -f ./k8s/falcon-ingress.yaml
```
