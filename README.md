# Woodpecker: ReadyStack build kit

The offline build kit for the Docker image `readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2`. It holds what the image is built from (upstream source, base images, toolchain, pinned dependencies) and the documents, so the image can be rebuilt with no network access (`docker build --network=none`).

- Release: [v3.20.1-CE-ubuntu24.04-r2](https://github.com/readystack-dev/woodpecker/releases/tag/v3.20.1-CE-ubuntu24.04-r2)
- Image: [`docker.io/readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2`](https://hub.docker.com/r/readystack/woodpecker/tags), manifest digest `sha256:5bfc6ee2996955d554260ecf7057f2b7e17908460299aa905d79d51c2f73add8`
- Product page: https://readystack.dev/products/woodpecker
- Defects: this repository's Issues (defects recorded from 25 November 2026 on) and https://readystack.dev/defects.html?product=woodpecker

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
