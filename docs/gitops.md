# GitOps Delivery Model

GitLab CI builds, tests, and publishes artifacts, but does not deploy them.
Delivery is handled by Argo CD, which reconciles the desired state from this
repository onto the cluster.

This separation keeps CI focused on quality and artifact production, while CD
remains declarative, auditable, and reversible through Git history.

## App-of-Apps

Argo CD uses a root Application that points to `argocd/apps/`. This root app
creates and manages child applications for platform components and environments.

## Sync policies

- dev: automated sync with self-heal and prune enabled
- stage: manual sync for controlled promotion

Argo CD is the only delivery mechanism. CI never applies manifests directly.
