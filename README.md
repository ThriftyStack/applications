# Workload applications

Platform GitOps (Argo CD roots, cluster profiles, Vault/Istio/infrastructure) lives in the sibling repository [`sudoceohq/argocd`](https://github.com/sudoceohq/argocd). This repository holds **workload Helm charts only**.

No application is selected yet. This repository is not watched by an automatic generator and contains no deployable Application. Adding a folder does not enroll a workload.

## Layout

| Path | Purpose |
|------|---------|
| `charts/<name>/` | Shared customized Helm chart for one workload |

Per-cluster / environment overrides and Argo CD `Application` / workload `AppProject` objects stay in the **platform** repository under `clusters/<id>/applications/` and hub-reviewed declarations. The hub owns Application objects; destination clusters run workloads.

## Enrollment rules

1. Finish Vault / issuer / mesh qualification and renewal gates in the platform repo.
2. Review an explicit workload AppProject (sources **only** this repository; no cluster-scoped resources by default; no hub `argocd` namespace destination).
3. Review an explicit Application pointing at `charts/<name>` here, with immutable image digests and an explicit destination.
4. Opt the target namespace into the mesh deliberately; do not reuse platform namespaces.

Preserve unique application names, explicit destinations, and per-cluster rollout decisions. Supply registry/image, service/port, metrics endpoint, and traffic/error/latency requirements before Rollouts. No controller or public endpoint is created by this guide.

See the platform repository README for promotion and multicluster onboarding.
