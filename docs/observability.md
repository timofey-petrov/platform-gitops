# Observability Design

This platform treats monitoring, logging, and alerting as first-class systems.
It defines metrics, logs, and alerts alongside the applications they observe.

## Metrics and logs

- Prometheus scrapes ServiceMonitors across namespaces.
- Grafana exposes Prometheus and Loki in one UI.
- Fluent Bit parses JSON logs and ships to Loki with `app`, `env`, and `namespace` labels.

## Dashboards

- Service Overview (Golden Signals) for `demo-app` is provisioned via Grafana sidecar.
- Dashboard JSON is stored in `infra/monitoring/dashboards/service-overview.yaml`.

## Planned SLIs

- Error rate (5xx)
- Latency p95
- Pod restarts / CrashLoop
- Resource saturation (CPU / memory)
