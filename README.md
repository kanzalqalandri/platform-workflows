# platform-workflows

**DevOps-owned** reusable CD workflows for the
[GitOps Platform Design v2](https://apportoteam.atlassian.net/wiki/spaces/DevOps/pages/2250047490).

App repos call these via thin callers; they are opinionated and not modifiable by
developers (Standardization Over Configuration).

| Workflow | Purpose |
|---|---|
| `deploy.yml` | Auto-locate a tenant across clusters (`tenants/*/<tenant>/<app>`), write `image.tag` into its `values.yaml`, and commit. |
| `offboard.yml` | Locate and delete a tenant/app directory → ArgoCD cascades the cleanup. |
| `list-tenants.yml` | List `cluster / tenant` pairs that run a given app. |

All three:
- write to [`platform-config`](https://github.com/kanzalqalandri/platform-config)
  using the `PLATFORM_CONFIG_TOKEN` secret;
- take **`tenant` + `app`** (no `environment`/`cluster` input — the cluster is
  derived from where the tenant lives);
- commit with the change **authored by the triggering user** (`github.triggering_actor`,
  committer = `github-actions[bot]`) plus a `Triggered-by:` / `Source-run:` link
  for auditability.

> Phase 2: `deploy-canary.yml` / `adjust-canary.yml` / `finalize-canary.yml`
> (Cilium Gateway API + Argo Rollouts) plug in here without changing the app repos.
