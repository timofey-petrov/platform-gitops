# Monitoring (kube-prometheus-stack)

## Install CRDs (k3s)

CRDs are installed once using server-side apply to avoid annotation size limits.

helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update

helm show crds prometheus-community/kube-prometheus-stack --version 81.1.0 | \
  kubectl apply --server-side -f -

## Install (M3 manual)

helm upgrade --install monitoring prometheus-community/kube-prometheus-stack \
  --namespace monitoring \
  --create-namespace \
  --version 81.1.0 \
  -f values.yaml

## Validate

kubectl -n monitoring get pods
kubectl -n monitoring get prometheus
