# Postmortem: INC-001 High Latency (stage)

Date: 2026-01-31
Environment: stage (app-stage)

## Summary

High latency p95 alert fired for demo-app in stage during a manual latency simulation.
The issue was caused by sustained requests to `/slow`, which intentionally adds
server-side delay. The service recovered after stopping the load. Follow-up
items were captured to improve visibility and guardrails.

## Impact

- Elevated latency in stage for ~12 minutes.
- No data loss; availability remained intact.

## Detection

Alert `HighLatencyP95Stage` fired in Alertmanager with `env=stage`.

## Timeline (MSK)

- 16:40 — Started synthetic load against `/slow?ms=900` in stage.
- 16:44 — p95 latency exceeded 300ms in Prometheus query.
- 16:45 — Alert `HighLatencyP95Stage` fired in Alertmanager.
- 16:46 — Investigation started (Grafana Explore + Loki).
- 16:49 — Root cause identified: sustained `/slow` traffic.
- 16:52 — Load stopped, latency began to recover.
- 16:57 — Alert cleared.

## Root Cause

Sustained traffic to the intentionally slow endpoint `/slow` in stage drove
p95 latency above the configured threshold.

## Resolution

Stopped the synthetic load. No code change was required for recovery.
Runbook improvements were committed in GitOps (`docs/runbooks/high-latency-p95.md`).

## What went well

- Alerts fired quickly with clear labels (`env`, `severity`, `service`).
- Prometheus and Loki data aligned and made diagnosis fast.
- Runbook provided consistent investigative steps.

## What went wrong

- No guardrails to separate test endpoints from SLO evaluation.
- No dashboard panel highlighting slow endpoints by path.

## Action Items

| Item | Priority | Owner | Due |
| --- | --- | --- | --- |
| Add separate latency alert for `/slow` and exclude it from main SLO | P1 | tim | 2026-02-07 |
| Add Grafana panel "Top slow paths" by `path` label | P1 | tim | 2026-02-07 |
| Add rate-limit or feature-flag for `/slow` in stage | P2 | tim | 2026-02-10 |
| Update demo-app metrics to expose path latency summary | P2 | tim | 2026-02-10 |
| Add synthetic load script under docs for repeatable tests | P3 | tim | 2026-02-14 |
| Update runbook with exact PromQL and Loki queries | P1 | tim | 2026-02-07 |

## Evidence / Links

- PromQL (latency):
  ```
  histogram_quantile(
    0.95,
    sum by (le) (rate(demo_app_request_duration_seconds_bucket{env="stage"}[5m]))
  )
  ```
- Loki query:
  ```
  {app="demo-app", env="stage"} | json | duration_ms > 300
  ```
- Alert: `HighLatencyP95Stage`
- Runbook: `docs/runbooks/high-latency-p95.md`
