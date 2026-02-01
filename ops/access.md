# Access map

Primary endpoints (TLS via cert-manager):

- https://argocd.platform-gitops.ru
- https://grafana.platform-gitops.ru
- https://alertmanager.platform-gitops.ru
- https://demo-dev.platform-gitops.ru
- https://demo-stage.platform-gitops.ru

Fallback (nip.io, NodePort):

- http://argocd.158.160.209.82.nip.io:30080
- http://grafana.158.160.209.82.nip.io:30080
- http://alertmanager.158.160.209.82.nip.io:30080
- http://demo-dev.158.160.209.82.nip.io:30080
- http://demo-stage.158.160.209.82.nip.io:30080

## M2 checks

kubectl -n ingress-nginx get pods
kubectl -n ingress-nginx get svc
kubectl get ingressclass
