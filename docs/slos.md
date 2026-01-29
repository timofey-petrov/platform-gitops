# SLOs

These SLOs apply to demo-app and are evaluated per environment. Stage is
stricter than dev and drives higher severity alerts.

## Availability

SLI (per env):

```
1 - (
  sum(rate(demo_app_requests_total{status=~"5..", env="<env>"}[5m]))
  / sum(rate(demo_app_requests_total{env="<env>"}[5m]))
)
```

SLO:

- stage: 99.5% over 7d
- dev: 99.0% over 7d

## Latency (p95)

SLI (per env):

```
histogram_quantile(
  0.95,
  sum(rate(demo_app_request_duration_seconds_bucket{env="<env>"}[5m])) by (le)
)
```

SLO:

- stage: p95 < 300ms over 30m window
- dev: p95 < 500ms over 30m window

## Stability

SLI:

```
increase(kube_pod_container_status_restarts_total{container="demo-app", namespace="<ns>"}[10m])
```

SLO:

- stage: 0 restarts in 10m
- dev: <= 1 restart in 10m
