# Argo CD

Install Argo CD via Helm with ingress for nip.io.

## Install (M3 manual)

helm repo add argo https://argoproj.github.io/argo-helm
helm repo update

helm upgrade --install argocd argo/argo-cd \
  --namespace argocd \
  --create-namespace \
  --version 9.3.4 \
  -f values.yaml

## Validate

kubectl -n argocd get pods
kubectl -n argocd get ingress

## UI access

URL: http://argocd.158.160.209.82.nip.io:30080

Admin password:

kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
