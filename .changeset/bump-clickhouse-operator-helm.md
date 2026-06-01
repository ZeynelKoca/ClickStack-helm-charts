---
"helm-charts": minor
---

fix(clickstack-operators): bump clickhouse-operator-helm dependency to `>=0.0.5, <0.1.0`

The previous `~0.0.2` constraint resolved narrowly to v0.0.2 only (Masterminds semver behavior for pre-1.0 versions), pinning the operator to an old release. v0.0.5 ships:

- New CRD schema with the `spec.podDisruptionBudget` field on both `ClickHouseCluster` and `KeeperCluster` (lets users override the auto-generated PDB).
- Smart default for `ClickHouseCluster` with `replicas <= 1`: `maxUnavailable=1` instead of `minAvailable=1`, so single-replica deployments no longer deadlock on node drains.
- RBAC additions (e.g. `Jobs` informer) required by the v0.0.5 controller manager.

Users on `clickstack-operators` v1.0.0 cannot benefit from any of these because the chart resolved the dependency to v0.0.2; the v0.0.5 binary cannot run against v0.0.2's RBAC or CRD schema.
