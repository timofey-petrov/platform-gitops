# GitOps Kubernetes Platform with Observability

Production-like pet project demonstrating DevOps / Platform Engineering practices

## Project Overview

This project is a production-grade, showcase GitOps Kubernetes platform built to
demonstrate mature DevOps / Platform Engineering practices.

The platform is designed around clear ownership boundaries, GitOps-based delivery,
and observability as a system, rather than a collection of tools.

Primary goal: demonstrate readiness for a DevOps / Platform Engineer role.

## Key Highlights (TL;DR for recruiters)

- GitOps delivery using Argo CD (App-of-Apps pattern)
- Kubernetes platform (k3s) bootstrapped via Ansible (day-1 ops)
- No imperative deploys (kubectl apply is never used in CI)
- CI/CD via GitLab CI with image promotion through Git commits
- Full observability stack:
  - Metrics: Prometheus + Grafana
  - Logs: Loki + fluent-bit
  - Alerts, SLI/SLO, runbooks
- Incident simulation with postmortems
- HTTPS everywhere using cert-manager + Let's Encrypt

## Architecture

### High-level delivery flow

```
Developer
  -> GitLab CI (demo-app)
      -> build / test / push image
      -> commit image tag to GitOps repo
  -> GitOps repository (platform-gitops)
      -> Argo CD (App-of-Apps)
          -> Kubernetes cluster (k3s)
              -> platform components + applications
```

### Responsibility split

| Component | Responsibility |
| --- | --- |
| Ansible | Cluster bootstrap (day-1) |
| Argo CD | Delivery & reconciliation (day-2) |
| GitLab CI | Build, test, image publishing |
| Kubernetes | Runtime & scheduling |

## Environments Model

Single Kubernetes cluster, multiple environments via namespaces:

| Environment | Namespace | Sync Policy | Replicas |
| --- | --- | --- | --- |
| dev | app-dev | auto-sync + self-heal | 1 |
| stage | app-stage | manual sync | 2 |

Promotion model:
Changes are promoted via Git (commit / merge request), not via imperative commands.

## Observability

Observability is treated as a first-class system, not an afterthought.

### Monitoring

- Prometheus Operator (kube-prometheus-stack)
- Application-level metrics via ServiceMonitor
- SLI-based alerts:
  - High error rate
  - High latency (p95)
  - CrashLoop detection

### Logging

- Centralized logging via Loki
- Logs collected by fluent-bit
- Structured JSON logs with consistent labels:
  - env
  - namespace
  - app

### Alerting & Response

- Alertmanager routing by environment and severity
- Runbooks linked directly from alerts
- Incident simulations with documented postmortems

## Security & Access

- HTTPS for all endpoints using cert-manager + Let's Encrypt
- Namespace isolation (argocd, monitoring, logging, app-*)
- Principle of least privilege (no cluster-admin for applications)

## Repository Structure

```
platform-gitops/
├── argocd/              # App-of-Apps and child applications
├── apps/                # Application manifests (Kustomize)
├── infra/               # Helm values for platform components
├── ops/                 # Ansible bootstrap and access docs
├── docs/                # Architecture, GitOps, observability, SLOs
│   ├── runbooks/
│   └── postmortems/
└── README.md
```

## Key Engineering Decisions

- GitOps over imperative deploys
  Argo CD is the only system allowed to apply changes to the cluster.

- Ansible vs Terraform
  Ansible is used for cluster bootstrap (day-1). Terraform is considered an
  optional add-on for future baseline provisioning.

- Single cluster, multiple environments
  Keeps complexity manageable while still demonstrating promotion and isolation.

## Documentation

- docs/architecture.md - platform design and responsibilities
- docs/gitops.md - GitOps delivery model
- docs/observability.md - metrics, logs, alerts
- docs/slos.md - SLI/SLO definitions
- docs/runbooks/ - operational runbooks
- docs/postmortems/ - incident postmortems

## Project Status

Completed. This project was built as a learning + showcase platform and is not
intended for production use.

## Contact

- Telegram: @holdonwaitt
