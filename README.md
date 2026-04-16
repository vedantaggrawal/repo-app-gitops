# repo-app-gitops

GitOps configuration repository for application Helm values.

## Structure

```
apps/
  <app-name>/
    local/    # local k3d/kind development
    dev/      # shared dev cluster
    stg/      # staging (pre-prod gate)
    prod/     # production
```

Each directory holds a `values.yaml` consumed by the corresponding ArgoCD Application.

## Branching strategy

All environments live in `main`. Environment isolation is enforced by:

- **CODEOWNERS** — `prod/` and `stg/` changes require a code owner review; no self-merge to prod.
- **Branch protection** on `main` — require at least 1 approving review + passing status checks before merge.
- **PR template** — promotes checklist discipline (lower-env validation, no `latest` tags, no plain-text secrets).

## Promotion flow

```
local → dev → stg → prod
```

Each promotion is a PR that modifies only the target environment's `values.yaml`. The PR must be reviewed and merged; ArgoCD picks up the change automatically via its path-based app configuration.
