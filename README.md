# platform-workflows

**DevOps-owned** reusable CD workflows for the
[GitOps Platform Design v2](https://apportoteam.atlassian.net/wiki/spaces/DevOps/pages/2250047490).

App repos call these via thin callers; they are opinionated and not modifiable by
developers (Standardization Over Configuration).

| Workflow | Purpose |
|---|---|
| `deploy.yml` | Write `image.tag` into a tenant's `values.yaml` in `platform-config` and commit. |
| `offboard.yml` | Delete a tenant/app directory → ArgoCD cascades cleanup. |
| `list-tenants.yml` | List tenants that have a given app configured. |

All three write to [`platform-config`](https://github.com/kanzalqalandri/platform-config)
using the `PLATFORM_CONFIG_TOKEN` secret.

> Phase 2: `deploy-canary.yml` / `adjust-canary.yml` / `finalize-canary.yml`
> (Cilium Gateway API + Argo Rollouts) plug in here without changing the app repos.
