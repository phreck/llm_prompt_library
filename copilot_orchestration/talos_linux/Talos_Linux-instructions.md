<!-- Use this file to provide workspace-specific custom instructions to Copilot. For more details, visit https://code.visualstudio.com/docs/copilot/copilot-customization#_use-a-githubcopilotinstructionsmd-file -->

# Talos Linux Kubernetes Project Instructions

This is a Kubernetes project specifically designed for Talos Linux environments with the following technology stack:

## Technology Stack
- **Kubernetes Distribution**: Talos Linux (immutable Kubernetes OS)
- **Storage**: Longhorn distributed storage system
- **Ingress Controller**: Traefik v3 with LoadBalancer service
- **TLS Management**: cert-manager with Cloudflare DNS challenge
- **Certificate Authority**: Let's Encrypt
- **Package Manager**: Helm 3

## Project Structure Guidelines (Talos-focused)

For generic Helm/Kubernetes best practices, defer to upstream docs. Below are Talos-specific defaults and where to find working examples in this repo.

### Talos Linux Specific Considerations
- Always include `kubernetes.io/os: linux` node selectors
- Consider Talos' immutable nature when designing persistent storage
- Use Longhorn storage classes for persistent volumes with XFS filesystem
- Implement proper pod security contexts aligned with Talos security model:
  - `readOnlyRootFilesystem: true` (aligns with Talos immutability)
  - `runAsNonRoot: true` (Talos enforces this)
  - `allowPrivilegeEscalation: false` (security best practice)
  - Drop all capabilities and add only what's necessary
  - Use seccomp profiles (`seccompProfile.type: RuntimeDefault`)
  - Set explicit non-root IDs (example): `runAsUser: 65532`, `runAsGroup: 65532`; add `fsGroup` only when a volume needs group write
- Consider Talos machine configuration annotations when relevant
- Use Pod Disruption Budgets for graceful handling of Talos node maintenance
- Leverage Talos' API-driven management through machine config patches
- Ensure containers are compatible with immutable root filesystem
- Writable paths: mount `emptyDir` for `/tmp` and `/var/tmp` (or dedicated subPaths on PVCs). Do not write to `/`; avoid `hostPath` writes unless strictly required.
- Configure appropriate shutdown timeouts for graceful Talos node shutdowns: set `terminationGracePeriodSeconds` and ensure probes/timeouts account for shutdown. Keep PDBs for HA workloads.
- Runtime specifics: Talos uses cgroup v2. Validate images and sidecars that assume cgroup v1; avoid dependencies on systemd, Docker socket, or privileged mode.
- Capabilities: avoid `CAP_SYS_ADMIN`; add the minimal set only when absolutely necessary.

### Storage Configuration
- Default to Longhorn storage class (`longhorn`). Example manifests and values:
  - `cluster/infrastructure/longhorn/longhorn-namespace.yaml`
  - `cluster/infrastructure/longhorn/values.yaml`

### Ingress and TLS Configuration
- Traefik v3 with Gateway API and MetalLB: see
  - `cluster/infrastructure/traefik/traefik-namespace.yaml`
  - `cluster/infrastructure/traefik/traefik-values.yaml`
- TLS via cert-manager + Cloudflare DNS-01:
  - `cluster/infrastructure/cert-manager/cert-manager-namespace.yaml`
  - `cluster/infrastructure/cert-manager/cert-manager-cloudflare.yaml`
  - `cluster/infrastructure/cert-manager/cert-manager-cloudflare-secret.md`

### Security Baseline (Talos-specific)
- Enforce Pod Security Standards (restricted) via namespace labels (below).
- Run as non-root with read-only rootfs; drop ALL capabilities; seccomp RuntimeDefault.
- Use the Helm helpers to apply these consistently across charts:
  - `cluster/helm-helpers/templates/_helpers.tpl`
  
Namespace-level Pod Security Admission (PSA) labels (recommended):

- `pod-security.kubernetes.io/enforce: restricted`
- `pod-security.kubernetes.io/audit: restricted`
- `pod-security.kubernetes.io/warn: restricted`

Seccomp/AppArmor:

- Use `seccompProfile: { type: RuntimeDefault }` by default
- AppArmor profiles can be used where available but are optional

### Deployment and Operations (pointers)
- MetalLB address pools and Traefik LB service: `cluster/infrastructure/metallb/` and `cluster/infrastructure/traefik/`.
- Longhorn install/verification: `cluster/infrastructure/longhorn/` and `cluster/talos/talos_README.md`.

## Helm usage
- Prefer shared helpers for labels, nodeSelector, and security contexts: `cluster/helm-helpers/`.
- Keep chart-specific logic minimal; surface options in `values.yaml`.

## Documentation pointers
- Traefik install and troubleshooting: `cluster/infrastructure/traefik/traefik_README.md`.
- cert-manager Cloudflare setup: `cluster/infrastructure/cert-manager/cert-manager-cloudflare-secret.md`.

When generating code or configurations, ensure compatibility with Talos Linux and prefer reusing helpers and examples from the paths above.
