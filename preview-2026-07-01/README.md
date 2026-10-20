# FluxConfiguration 2026-07-01-preview fixtures

These paths are consumed by the opt-in GitHub integration tests in the agent
repository. Each case runs in a test-owned namespace. The repository root is not
a Kustomization and must not be applied recursively.

| Path | Purpose |
| --- | --- |
| `base` | ConfigMap, small Deployment and Service for metadata, names, images, replicas and patches |
| `components/extra-label` | Optional Kustomize component, referenced relative to `base` as `../components/extra-label` |
| `substitutions` | Typed postBuild substitution alongside additional configurations |
| `health` | ConfigMap with deliberately false readiness for custom CEL health checks |
| `source-filter` | Two auto-detected valid manifests; source ignore excludes one |
| `sparse/included`, `sparse/excluded` | Independent markers for sparse artifact inclusion and missing-path recovery |
| `revisions/v2` | Same base workload with a different revision value for path/revision transitions |
| `remote` | Marker which must appear on the target cluster, not on the hub |

Source-filter intentionally has no kustomization.yaml. It exercises Flux manifest
auto-detection, so adding one with an explicit resource list would mask ignore
regressions. Negative cases use missing paths/components or overlay input; the
checked-in positive Kustomizations must always build.

Public GitHub proves content reconciliation, not Azure object-level authentication.
Identity cases must mirror these bytes into private supported sources; never put
credentials or generated kubeconfigs in this repository.