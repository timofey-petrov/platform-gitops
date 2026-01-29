# Runbook: Target Down

## Symptom

Prometheus cannot scrape demo-app target.

## Impact

Loss of visibility into metrics for the service.

## Immediate actions

```
kubectl -n app-<env> get pods
kubectl -n app-<env> get svc demo-app
kubectl -n app-<env> get endpoints demo-app
```

## Investigate

- Check ServiceMonitor:

```
kubectl -n app-<env> get servicemonitor demo-app -o yaml
```

- In Grafana Explore (Prometheus), verify `up`:

```
up{job="demo-app", namespace="app-<env>"}
```

## Mitigation

- Ensure the ServiceMonitor selector matches the Service labels.
- Restart demo-app pods if the `/metrics` endpoint is stuck.

## Follow-up

- Add a synthetic probe to validate `/metrics` externally.
