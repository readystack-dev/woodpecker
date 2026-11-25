# Woodpecker: ReadyStack build kit

The offline build kit for the Docker image `readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.20.1-CE-ubuntu24.04-r2](https://github.com/readystack-dev/woodpecker/releases/tag/v3.20.1-CE-ubuntu24.04-r2)
- Image: [`docker.io/readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2`](https://hub.docker.com/r/readystack/woodpecker/tags), manifest digest `sha256:5bfc6ee2996955d554260ecf7057f2b7e17908460299aa905d79d51c2f73add8`
- Product page: https://readystack.dev/products/woodpecker
- Source and build recipe: https://readystack.dev/products/woodpecker/recipe
- Build trail: https://readystack.dev/products/woodpecker/trail/v3.20.1-CE-ubuntu24.04-r2
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=woodpecker

## The image

Single container running the Woodpecker SERVER (web UI + API + gRPC). State in an embedded SQLite database. Connects to a git forge (GitHub/Gitea/GitLab) and to one or more agents (run separately) that execute pipelines.

## Who uses Woodpecker and why

Development teams who want a lightweight, self-hosted CI/CD engine for the repositories on their Git server.

Woodpecker is a simple CI/CD engine. A workflow file in each repository lists the steps to build, test and deploy the code, and Woodpecker runs them on every change; plugins add common tasks.

A typical use: A small team hosts its own Git server and wants tests on every push. With Woodpecker connected to it, each repository's workflow file runs on every change: the code is tested, the application is built, and it is deployed when the main branch passes.

From the upstream project's own description: https://github.com/woodpecker-ci/woodpecker, https://woodpecker-ci.org/docs/intro, https://woodpecker-ci.org/docs/usage/intro, https://woodpecker-ci.org/docs/usage/workflow-syntax.

## This release

- Docker Scout policy grade at release: **A** (94%), graded 2026-11-13 with `docker scout policy` (CLI 1.26.0, docker-default configuration).
  - Docker Scout counts 11 packages with copyleft licences (GPL, LGPL, MPL), all of them operating-system packages (it counts only packages whose licence it can read; the SBOM lists every component).
- Signed with ReadyStack's release key (fingerprint `04daa92344a52dbc`): https://readystack.dev/keys/cosign.pub.
- Runs as a non-root user (`woodpecker`).
- Attestations on the image: an SPDX SBOM and SLSA provenance v1.
- Rebuilt from this kit with network egress blocked on 2026-11-13; the rebuilt image digest is the published one.
- Base: Ubuntu 24.04.

The current grade, measured again every day, is on https://readystack.dev/products/woodpecker.

Woodpecker is distributed under its upstream licence (Apache-2.0); the kit contains upstream's source under that licence, and each vendored component under its own.

## Download

From the [v3.20.1-CE-ubuntu24.04-r2 release](https://github.com/readystack-dev/woodpecker/releases/tag/v3.20.1-CE-ubuntu24.04-r2): `ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.tar.gz` (487.1 MB), SHA-256 `536371bdff5dd6073cc487bea425c890e3e6e13480d0a9e302846041181b6180`.

```sh
sha256sum --check --ignore-missing SHA256SUMS
```

## Verify and rebuild

The rebuild statement is signed with the ReadyStack release key. Check the signature before you rely on it:

```sh
cosign verify-blob --key https://readystack.dev/keys/cosign.pub \
  --bundle ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.rebuild.json.sigstore ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.rebuild.json
```

Unpack with `-p` so file modes are kept, then follow the kit's own README.md and docs/REBUILD.md:

```sh
tar -xpzf ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.tar.gz
cd v3.20.1-CE-ubuntu24.04-r2
less README.md docs/REBUILD.md
```

The build trail (`ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.trail.json`, signed the same way, and its Markdown copy) records how this release was built and checked.

## This repository

`recipe/` holds the kit's recipe files (Dockerfiles, build scripts, pins) as published at https://readystack.dev/products/woodpecker/recipe. The complete kit is the release download. Each release is a tag here.

— ReadyStack · https://readystack.dev
