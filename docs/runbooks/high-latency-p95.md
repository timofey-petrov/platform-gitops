# Runbook: High Latency p95

## Symptom

p95 latency above threshold for demo-app.

## Impact

Slow responses and degraded user experience.

## Immediate actions

1) Confirm in Grafana Explore (Prometheus):

```
histogram_quantile(
  0.95,
  sum(rate(demo_app_request_duration_seconds_bucket{env="<env>"}[5m])) by (le)
)
```

2) Check current pod status:

```
kubectl -n app-<env> get pods
kubectl -n app-<env> top pods
```

## Investigate

- Break down by path:

```
histogram_quantile(
  0.95,
  sum(rate(demo_app_request_duration_seconds_bucket{env="<env>"}[5m])) by (le, path)
)
```

- Look at logs around slow requests:

```
{app="demo-app", env="<env>"} | json | duration_ms > 500
```

## Mitigation

- Scale replicas up in overlay (stage has 2 by default).
- Reduce load by disabling the slow path or increasing timeouts.

## Follow-up

- Add caching or optimize slow endpoints.
- Consider dedicated dashboards for latency by path.
