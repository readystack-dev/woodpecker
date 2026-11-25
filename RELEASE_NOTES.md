# Woodpecker v3.20.1 — ReadyStack release `v3.20.1-CE-ubuntu24.04-r2`

Generated 2026-11-13 from this release's build records and strategy. Statements that come from a single record name it in brackets; the others come from this release's strategy and build log.

## Release

- **Image:** `readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2` (linux/amd64). Index digest `sha256:cf5deacba0a709a244dbef42535a6018f68821d5304750a5fa67216259a22326` [`docs/provenance/LOCKED_DIGEST.txt`].
- **Image manifest digest:** `sha256:5bfc6ee2996955d554260ecf7057f2b7e17908460299aa905d79d51c2f73add8` [`TARGET_DIGEST.txt`].
- **Upstream:** Woodpecker v3.20.1 built from source at `v3.20.1` of https://github.com/woodpecker-ci/woodpecker — upstream notes: https://github.com/woodpecker-ci/woodpecker/releases/tag/v3.20.1 [`docs/provenance/BUILD_RECIPE.json`].
- **Base image:** none (`FROM scratch`): a chiselled Ubuntu 24.04 runtime, the files Canonical's chisel cut from Ubuntu's own packages (`vendored/chisel/slices.txt`, `vendored/chisel/packages.tsv`); runtime user `woodpecker`.
- **Build:** `source-rebuild-go`, compiled offline with `--network=none`.
- **Supersedes:** ReadyStack release `v3.20.1-CE-ubuntu24.04-r1`.

## What changed in this release

- Upstream version unchanged (v3.20.1); ReadyStack rebuild.
- Minimum dependency versions ReadyStack enforces on top of upstream (raised only where upstream's own version was lower); shipped versions from the SBOM [`docs/sbom/SBOM.json`]:
  - `golang.org/x/crypto` at least `v0.58.0`: not raised, upstream already pins `v0.58.0` (shipped: `v0.58.0`)
  - `golang.org/x/net` at least `v0.62.0`: not raised, upstream already pins `v0.62.0` (shipped: `v0.62.0`)
  - `google.golang.org/grpc` at least `v1.83.2`: not raised, upstream already pins `v1.85.0` (shipped: `v1.85.0`)
  - `golang.org/x/image` at least `v0.45.0`: not raised, upstream already pins `v0.47.0` (shipped: `v0.47.0`)
  - `golang.org/x/mod` at least `v0.40.0`: not raised, upstream already pins `v0.41.0` (shipped: `v0.41.0`)
- Woodpecker v3.20.1 is unchanged from the release this supersedes (v3.20.1-CE-ubuntu24.04-r1); this release rebuilds it. Changes: the image is rebuilt on the current ubuntu:24.04 image (sha256:2198e4c65076…), which its maintainers updated after v3.20.1-CE-ubuntu24.04-r1 was built on sha256:fe81560990fa…. Settings, ports and data volumes are those of v3.20.1-CE-ubuntu24.04-r1: replace the container with this image on the same volumes.

## Security status at release (measured 2026-11-13)

- **Docker Scout:** 0 fixable critical/high vulnerabilities; runs as `woodpecker` (non-root); SBOM + provenance attestations present (SPDX SBOM + SLSA v1) [`docs/provenance/SCOUT_VERDICT.json`].
- **OSV cross-check:** no fixable critical/high vulnerability beyond Docker Scout's findings.
- **Signature:** cosign signature verified by ReadyStack's release gate before publishing; check it yourself with `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2` (signatures are recorded in the public Sigstore transparency log).
- **ReadyStack release check at release:** passed (ReadyStack's own test, not a Docker Scout grade). Vulnerabilities are disclosed every day and the image never changes, so it is re-checked daily, beside Docker Scout's own dated grade, at https://readystack.dev/queue.

## Reproducibility

Rebuilds offline from this archive to the identical image digest `sha256:5bfc6ee2996955d554260ecf7057f2b7e17908460299aa905d79d51c2f73add8`: run `./BUILD_OFFLINE.sh`, then `./VERIFY_OFFLINE.sh` (add `--online` to confirm the Docker Hub tag holds the same digest). Before publishing, ReadyStack rebuilt the kit with `./BUILD_OFFLINE.sh` and compared the result with the published digest [`docs/provenance/REPRO_VERIFICATION.json`], then ran `./BUILD_OFFLINE.sh` and `./VERIFY_OFFLINE.sh` from this archive (without `--online`).

## Validation

- 2 of 2 use cases passed (3 authored, 2 kept in the suite; 1 not exercised, see below) against the running image [`docs/validation/test-results.json`].
- The cases are `GET /healthz` (204), `GET /version` (200); the suite checks nothing else.
- 1 further authored check was not exercised and is not counted above: it (`GET /api/info`) was answered with the same page the build's test container served for a made-up path, so it could not show the path exists (listed under Not exercised in `docs/validation/FAILED_TESTS.md`).

## Known issues

- None recorded.

## Defects fixed

- 58 problems found before release and fixed before it shipped (58 by review); each is listed in `docs/defects/defects.json`.

## Verify this archive

```
sha256sum -c CHECKSUMS.sha256
./BUILD_OFFLINE.sh && ./VERIFY_OFFLINE.sh --online
cosign verify --key https://readystack.dev/keys/cosign.pub readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2
```

ReadyStack records every signature in the public Sigstore transparency log (Rekor), so the command needs no extra flags.

