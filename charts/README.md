# Workload charts

Place one Helm chart per application directory (`charts/<name>/`). Charts here must not install platform controllers, CRDs, or cluster-scoped RBAC — those remain in the platform repository.
