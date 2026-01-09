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

- M0: repository bootstrap (this stage)
- M1+: see docs/architecture.md and milestone notes in docs
