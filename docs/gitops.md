# GitOps Delivery Model

GitLab CI builds, tests, and publishes artifacts, but does not deploy them.
Delivery is handled by Argo CD, which reconciles the desired state from this
repository onto the cluster.

This separation keeps CI focused on quality and artifact production, while CD
remains declarative, auditable, and reversible through Git history.
