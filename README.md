# iac-github-images

Pinned container images that bake the heavy CI tools used by the
[`Uaena1711/iac-github`](https://github.com/Uaena1711/iac-github) catalog, so its `tf-env` /
`cfn-env` / `tf-docs` pipelines install **nothing heavy at runtime** (jq is the only tolerated
auto-install; everything else comes from these images).

Published to **GHCR (public → unlimited pulls)**:

| Image | Bakes | Base (GHCR public — no Docker Hub / no ECR) |
|-------|-------|---------------------------------------------|
| `ghcr.io/uaena1711/iac-github-tf` | terraform, tflint, terraform-docs, jq, git | `ghcr.io/terraform-linters/tflint` (Alpine) |
| `ghcr.io/uaena1711/iac-github-cfn` | aws-cli v2, cfn-lint, jq, git | `ghcr.io/astral-sh/uv` (Debian 12, glibc + python) |

## Why a separate repo
Kept out of the catalog (no monorepo). Bases are GHCR public so **builds never touch Docker Hub**
(anon 100/6h) or ECR; the published images are GHCR public so **consumers get unlimited pulls**.

## Pinning
Consumers pin by **digest** (`…@sha256:…`) — immutable. `.github/workflows/publish.yml` builds
multi-arch (amd64+arm64) on push/release and prints each digest to the run summary; the catalog's
`tf-env`/`cfn-env` default `*_image` inputs reference those digests.

## Tool versions
terraform 1.15.7 · tflint 0.63.1 · terraform-docs 0.24.0 · aws-cli 2.35.13 · cfn-lint 1.52.1
(pinned via `ARG` in each Dockerfile; downloads sha256-verified).

## License
[MIT](LICENSE).
