# ingress-nginx

This component provides external HTTP(S) access via NodePort on a single-node VM.

## Install (M2 manual)

helm repo add ingress-nginx https://kubernetes.github.io/ingress-nginx
helm repo update

helm upgrade --install ingress-nginx ingress-nginx/ingress-nginx \
  --namespace ingress-nginx \
  --create-namespace \
  --version 4.14.1 \
  -f values.yaml

## Validate

kubectl -n ingress-nginx get pods
kubectl -n ingress-nginx get svc
kubectl get ingressclass

## Notes

- NodePort HTTP: 30080
- NodePort HTTPS: 30443
