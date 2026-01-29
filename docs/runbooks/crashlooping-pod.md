# Runbook: CrashLooping Pod

## Symptom

demo-app pod restarts repeatedly.

## Impact

Requests may fail or be served by fewer replicas.

## Immediate actions

```
kubectl -n app-<env> get pods
kubectl -n app-<env> describe pod <pod>
```

## Investigate

- Check container logs:

```
kubectl -n app-<env> logs <pod> -c demo-app --tail=200
```

- Check recent rollout and image tag:

```
kubectl -n app-<env> describe deploy demo-app | rg -n \"Image\"
```

## Mitigation

- Roll back to the previous image tag in GitOps overlay.
- If config/env is wrong, fix and re-sync via Argo.

## Follow-up

- Add readiness checks for new failure modes.
- Improve resource requests if OOMKilled.
