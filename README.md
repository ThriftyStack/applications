# Workload applications

Separate GitOps repository for application charts and their per-cluster values. Platform services (Vault, cert-manager, Istio, storage, Crossplane) live in [`sudoceohq/platform`](https://github.com/sudoceohq/platform.git) under `infrastructure/`.

No application is selected yet. This repository is not watched by an automatic generator and contains no deployable chart or Application. Adding a folder does not enroll a workload.

## Layout

| Path | Purpose |
|------|---------|
| `charts/<name>/` | Shared customized Helm chart for one workload |
| `clusters/<id>/<name>.yaml` | Per-cluster / environment value overlays (same repo as the chart so Argo can reference `valueFiles`) |

## Enrollment

1. Complete Vault / issuer / mesh qualification and renewal gates in the platform repo.
2. Review an explicit **workload** AppProject in the platform repo (`workloadProjects` on the management cluster profile). That project may source only this repository and only approved workload namespaces — never cluster-scoped resources and never the hub `argocd` namespace.
3. Review an Application owned by the hub that points `spec.source.repoURL` at this repository and a chart under `charts/`. Destination clusters run workloads; the hub owns Application objects.
4. Preserve unique application names, explicit destinations, immutable image digests, and per-cluster rollout decisions.

Supply source chart path, registry/image digests, target cluster/environment, namespace, service/port and metrics endpoint before activation. Define traffic/error/latency requirements and test canary abort/recovery before adding Rollouts. No controller or public endpoint is created by this guide.

See the platform repo for cluster onboarding and reviewed application delivery.
