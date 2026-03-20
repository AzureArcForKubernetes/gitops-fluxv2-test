# Scale Test – Flux GitOps Template

This folder contains a **tenant-agnostic** Flux GitOps template designed for multi-tenant scale testing.

## How it works

- Multiple Flux `Kustomization` / `FluxConfig` objects can point to the **same path** (`scaleTest/podinfo`).
- Each Kustomization injects a unique namespace at runtime using `postBuild.substitute`:

  ```yaml
  postBuild:
    substitute:
      NAMESPACE: tenant-42
  ```

- The `${NAMESPACE}` placeholder in the manifests is replaced by Flux before the resources are applied to the cluster.

## What's included

| File                  | Description                                      |
|-----------------------|--------------------------------------------------|
| `namespace.yaml`      | Namespace resource using `${NAMESPACE}`           |
| `helmrepository.yaml` | HelmRepository pointing to the podinfo Helm repo |
| `helmrelease.yaml`    | HelmRelease deploying the podinfo chart           |
| `kustomization.yaml`  | Kustomize manifest listing all resources          |

## Key design decisions

- **No tenant-specific folders or hardcoded names** – everything is parameterized via `${NAMESPACE}`.
- **Plain Kustomize** – no Helm templating inside the repo; Flux handles variable substitution.
- **Single source of truth** – one set of manifests serves all tenants.
