# Talos Linux Instructions

## Efficient and Effective Instructions

- Always include `kubernetes.io/os: linux` node selectors in manifests.
- Use Longhorn storage classes for persistent volumes with XFS filesystem.
- Enforce pod security contexts: `readOnlyRootFilesystem: true`, `runAsNonRoot: true`, `allowPrivilegeEscalation: false`, drop all capabilities, use seccomp `RuntimeDefault`.
- Mount `emptyDir` for `/tmp` and `/var/tmp`; avoid writing to `/` or using `hostPath` unless strictly required.
- Set explicit non-root IDs: `runAsUser: 65532`, `runAsGroup: 65532`; add `fsGroup` only when needed.
- Use Pod Disruption Budgets for HA workloads and graceful node maintenance.
- Prefer Helm helpers for labels, nodeSelector, and security contexts.
- Validate container images for cgroup v2 compatibility; avoid systemd, Docker socket, or privileged mode.
- Use namespace-level Pod Security Admission labels: `enforce`, `audit`, and `warn` set to `restricted`.
