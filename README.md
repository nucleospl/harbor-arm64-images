# Harbor ARM64 Images

Unofficially built ARM64 (`linux/arm64`) images for [Harbor](https://github.com/goharbor/harbor) — the open-source container registry.

Harbor does not publish official ARM64 images. This repo fills that gap.

## Available images

All images are published to `ghcr.io/nucleospl/harbor-arm64-images/`:

| Image | Description |
|---|---|
| `harbor-core` | Main Harbor API service |
| `harbor-jobservice` | Background job execution |
| `harbor-portal` | Web UI (nginx + Angular) |
| `harbor-db` | PostgreSQL with Harbor schema |
| `harbor-log` | Centralized log collector |
| `harbor-registryctl` | Registry controller |
| `harbor-exporter` | Prometheus metrics exporter |
| `nginx-photon` | Reverse proxy / entry point |
| `registry-photon` | Docker Distribution registry |
| `trivy-adapter-photon` | Vulnerability scanner adapter |
| `prepare` | Config preparation tool |

## Usage

Replace the official Harbor images in your `docker-compose.yml` or Helm values:

```yaml
# docker-compose.yml example
services:
  core:
    image: ghcr.io/nucleospl/harbor-arm64-images/harbor-core:v2.12.0
  jobservice:
    image: ghcr.io/nucleospl/harbor-arm64-images/harbor-jobservice:v2.12.0
  portal:
    image: ghcr.io/nucleospl/harbor-arm64-images/harbor-portal:v2.12.0
  # ... etc
```

Tags follow upstream Harbor versioning: `v2.12.0`, `v2.11.1`, etc. `latest` always points to the newest built version.

## Verification

Images are signed with [cosign](https://github.com/sigstore/cosign) using keyless signing (OIDC). Verify before use:

```bash
cosign verify \
  --certificate-identity-regexp="https://github.com/nucleospl/harbor-arm64-images/.*" \
  --certificate-oidc-issuer="https://token.actions.githubusercontent.com" \
  ghcr.io/nucleospl/harbor-arm64-images/harbor-core:v2.12.0
```

## Build process

- Source: cloned from `goharbor/harbor` at a **pinned commit SHA** (not just tag) — resistant to tag tampering
- Runner: `ubuntu-24.04-arm` — native ARM64, no QEMU emulation
- Scan: Trivy (CRITICAL vulnerabilities block the build)
- Sign: cosign keyless (Rekor transparency log)
- Auto-update: checks upstream for new releases every 6 hours

## Supported versions

See [versions.json](versions.json) for the currently built version.

## Disclaimer

These are **unofficial** community-built images. Not affiliated with or endorsed by the Harbor project or VMware/Broadcom.
