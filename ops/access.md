# Access map

Primary endpoints (TLS via cert-manager):

- https://argocd.platform-gitops.ru
- https://grafana.platform-gitops.ru
- https://alertmanager.platform-gitops.ru
- https://demo-dev.platform-gitops.ru
- https://demo-stage.platform-gitops.ru

demo-app routes:

- https://demo-dev.platform-gitops.ru/health
- https://demo-dev.platform-gitops.ru/metrics
- https://demo-stage.platform-gitops.ru/health
- https://demo-stage.platform-gitops.ru/metrics

Argo CD access:

- Admin user: `admin`
- Read-only user: `readonly` / `readonly-argo-2026`

Grafana access:

- Admin user: `admin`
- Read-only user: `readonly` / `readonly-grafana-2026`

Fallback (nip.io, direct 80/443):

- http://argocd.158.160.209.82.nip.io
- http://grafana.158.160.209.82.nip.io
- http://alertmanager.158.160.209.82.nip.io
- http://demo-dev.158.160.209.82.nip.io
- http://demo-stage.158.160.209.82.nip.io

## M2 checks

kubectl -n ingress-nginx get pods
kubectl -n ingress-nginx get svc
kubectl get ingressclass
