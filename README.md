# DockRadar Docker images

Patched ("fixed") versions of public container images, published by
[DockRadar](https://dockradar.romain33.fr) on Docker Hub under
[`dockradar`](https://hub.docker.com/u/dockradar).

Each image is the original one with every OS package upgraded to its latest
patched version by [Copacetic](https://project-copacetic.github.io/copacetic/):
same entrypoint, volumes and settings. Application dependencies bundled in the
image (Java, npm, pip...) are not changed.

| Original | Fixed |
| --- | --- |
| `itzg/minecraft-server:latest` | `dockradar/itzg-minecraft-server:latest` |
| `postgres:16-alpine` | `dockradar/postgres:16-alpine` |
| `ghcr.io/org/app:1` | `dockradar/ghcr-io-org-app:1` |

Every build also gets a dated tag (`latest-20261030`) that never moves.

## How it works

DockRadar starts `.github/workflows/fixed-image.yml` (workflow_dispatch) when
someone asks for a fixed image on a container's Security tab. The workflow
scans the image with Trivy, patches it, scans it again, pushes it and reports
the result back to DockRadar.

## Setup

Repository secrets:

- `FIXED_IMAGE_CALLBACK_SECRET`: same value as DockRadar's (`openssl rand -hex 32`)
- `FIXED_IMAGE_REGISTRY_USERNAME`: the Docker Hub account (`dockradar`)
- `FIXED_IMAGE_REGISTRY_TOKEN`: a Docker Hub access token with Read & Write

Optional repository variable `FIXED_IMAGE_REPOSITORY` to publish under another
namespace or registry (`registry.example.com/dockradar`).

DockRadar's `.env`: `FIXED_IMAGE_GITHUB_REPO=MaxLaMenace33/DockRadar_DockerImages`
and `FIXED_IMAGE_GITHUB_TOKEN`, a fine-grained token with Actions read and write
on this repository.

The images keep the license of their original image.
