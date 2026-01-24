# GitOps Delivery Model

CI builds, tests, and publishes artifacts, but does not deploy them.
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

## Image tag updates

Delivery is triggered by updating the image tag in the GitOps repo. The demo app
uses the `images` block in `apps/demo-app/overlays/dev/kustomization.yaml` and
`apps/demo-app/overlays/stage/kustomization.yaml`. CI commits a new `newTag`
value for dev on every successful pipeline, and stage is promoted manually.
