# Runbook: High Error Rate

## Symptom

High ratio of 5xx responses for demo-app.

## Impact

User-facing errors and failed requests.

## Immediate actions

1) Confirm alert in Grafana Explore (Prometheus):

```
sum(rate(demo_app_requests_total{status=~"5..", env="<env>"}[5m]))
/
sum(rate(demo_app_requests_total{env="<env>"}[5m]))
```

2) Check demo-app health:

```
kubectl -n app-<env> get pods
kubectl -n app-<env> get deploy demo-app
```

## Investigate

- Grafana Explore (Loki):

```
{app="demo-app", env="<env>"} | json | status >= 500
```

- Inspect recent rollout:

```
kubectl -n app-<env> rollout history deploy/demo-app
```

## Mitigation

- Revert the last image tag in GitOps overlay and let Argo roll back.
- If a specific endpoint is failing, isolate by path label in metrics:

```
sum(rate(demo_app_requests_total{status=~"5..", env="<env>"}[5m])) by (path)
```

## Follow-up

- Add tests for the failing path.
- Tighten alert thresholds if noise is acceptable.
