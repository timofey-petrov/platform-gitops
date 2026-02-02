# GitOps Kubernetes Platform (platform-gitops)

This repository is the single source of truth for the platform.
It is designed for GitOps delivery via Argo CD (App of Apps) and a clear
separation between CI and CD responsibilities.

## Structure (M0)

- argocd/ - root app and child apps for platform components and environments
- apps/ - application manifests (Kustomize)
- infra/ - Helm values for platform components
- ops/ - bootstrap and operational tooling (Ansible)
- docs/ - architecture, GitOps model, observability, versions, runbooks

## Milestones

Status:

- M0: repository bootstrap ✅
- M1: k3s bootstrap via Ansible ✅
- M2: ingress + nip.io ✅
- M3: Argo CD + App-of-Apps ✅
- M4: demo-app + Kustomize overlays ✅
- M5: CI → GitOps (no imperative deploy) ✅
- M6: monitoring + logging ✅
- M7: alerting + SLI/SLO + runbooks ✅
- M8: incident simulation + postmortem ✅
- M9: domain + TLS (cert-manager) ✅

Details:

- docs/architecture.md
- docs/gitops.md
- docs/observability.md
- docs/slos.md
- ops/access.md
