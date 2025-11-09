# Module 4 Commands: Implementing Infrastructure as Code (IaC)

## Clip 1: Demo: Managing Kubernetes Clusters with Terraform

Display Terraform main configuration:
```bash
cat main.tf
```

Display Terraform variables configuration:
```bash
cat variables.tf
```

Display Terraform outputs configuration:
```bash
cat outputs.tf
```

Initialize Terraform project:
```bash
terraform init
```

Generate Terraform execution plan:
```bash
terraform plan -out globomantics-staging.tfplan
```

Apply Terraform configuration:
```bash
terraform apply globomantics-staging.tfplan
```

List Kubernetes namespaces:
```bash
kubectl get namespaces
```

List all resources in staging namespace:
```bash
kubectl get all -n globomantics-staging
```

Destroy Terraform-managed resources:
```bash
terraform destroy
```

## Clip 2: Demo: GitOps-Managed Cluster Configuration

No commands are used in this clip.

## Clip 3: Demo: Kustomize and Helm for Infrastructure Updates

No commands are used in this clip.