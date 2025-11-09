# Module 2 Commands: Automating Kubernetes Deployments Using GitOps

## Clip 1: Demo: Setting up Git Repository for GitOps

Configure Git username for commits:
```bash
git config --global user.name "<your-name>"
```

Configure Git email for commits:
```bash
git config --global user.email "<your-email>"
```

## Clip 2: Demo: Configuring GitOps Repository Automation

Create ArgoCD namespace:
```bash
kubectl create namespace argocd
```

Install ArgoCD in the cluster:
```bash
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

Wait for ArgoCD server deployment to be ready:
```bash
kubectl wait --for=condition=available --timeout=300s deployment/argocd-server -n argocd
```

Get the initial admin password for ArgoCD:
```bash
argocd admin initial-password -n argocd
-fFrhO8ZLXF6m6NB
```

Port forward ArgoCD server to local machine:
```bash
kubectl port-forward svc/argocd-server -n argocd 8080:443 &
```

Download ArgoCD CLI binary for Linux:
```bash
curl -sSL -o argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
```

Make ArgoCD CLI executable:
```bash
chmod +x argocd
```

Move ArgoCD CLI to system path:
```bash
sudo mv argocd /usr/local/bin/
```

Get initial admin password again (for CLI login):
```bash
argocd admin initial-password -n argocd
```
-fFrhO8ZLXF6m6NB


Login to ArgoCD via CLI:
```bash
argocd login localhost:8080 --username admin --password fFrhO8ZLXF6m6NB --insecure
```

Export GitHub username environment variable:
```bash
export GIT_USERNAME=ialpay
```

Export GitHub token environment variable:
```bash
export GIT_TOKEN=

```

Add Git repository to ArgoCD:
```bash
argocd repo add https://github.com/CodeWithPraveen/ps-course-implementing-gitops-for-k8s-apps.git \
  --username $GIT_USERNAME \
  --password $GIT_TOKEN
```


argocd repo add https://github.com/CodeWithPraveen/ps-implementing-gitops.git \
  --username $GIT_USERNAME \
  --password $GIT_TOKEN

List repositories in ArgoCD:
```bash
argocd repo list
```

## Clip 3: Demo: Deploying Applications with GitOps

Clone the GitOps repository:
```bash
git clone https://github.com/CodeWithPraveen/ps-course-implementing-gitops-for-k8s-apps.git
```

Change to repository directory:
```bash
cd ps-course-implementing-gitops-for-k8s-apps
```

List repository contents:
```bash
ls
```

Display ArgoCD application configuration:
```bash
cat argocd/applications/globomantics-app-prod.yaml
```

Apply the ArgoCD application:
```bash
kubectl apply -f argocd/applications/globomantics-app-prod.yaml
```

List applications managed by ArgoCD:
```bash
argocd app list
```

Get detailed information about specific application:
```bash
argocd app get globomantics-app-prod
```

View resources in Kubernetes namespace:
```bash
kubectl get all -n globomantics-prod
```

## Clip 5: Demo: Implementing Automated Rollbacks

Edit deployment patch file:
```bash
vim apps/prod/deployment-patch.yaml
```

Add all changes to Git staging:
```bash
git add .
```

Commit changes with message:
```bash
git commit -m "Updated replica count"
```

Push changes to main branch:
```bash
git push origin main
```

Synchronize application in ArgoCD:
```bash
argocd app sync globomantics-app-prod
```

Check resource status in namespace:
```bash
kubectl get all -n globomantics-prod
```

Revert the last Git commit:
```bash
git revert HEAD -n
```

Commit the revert:
```bash
git commit -m "Reverted last commit"
```

Push the revert to main branch:
```bash
git push origin main
```

Synchronize application again:
```bash
argocd app sync globomantics-app-prod
```

Check deployment status after rollback:
```bash
kubectl get all -n globomantics-prod
```

Delete the application:
```bash
kubectl delete -f argocd/applications/globomantics-app-prod.yaml
```

Delete the namespace:
```bash
kubectl delete ns globomantics-prod
```