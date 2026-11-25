# Provenance — Woodpecker (`v3.20.1-CE-ubuntu24.04-r2`)

Every item below is copied from a record in this archive; the record is named in brackets.

## Source

- Upstream repository: https://github.com/woodpecker-ci/woodpecker
- Revision built: `v3.20.1` (commit `3f1bb0bd0db28e410ad797034d75780116122c91`)
- Source tree: `d66ceaca9c7b409dcb5c895a1aa9e92a77c3345e3a99f9125c0cba89bd06e09c` over 1736 files [`docs/provenance/SOURCE_TREE_HASH.json`]
- Community Edition only: enterprise code is removed or refused before the build (CE audit at acquire).

## Build

- Method: `source-rebuild-go`; base image `scratch + a chiselled Ubuntu 24.04 runtime (vendored/chisel/slices.txt, packages.tsv)`
- Compiled with `--network=none` from vendored source, dependencies and base images; BuildKit pinned to `moby/buildkit:v0.31.2` (shipped in `toolchain/`).
- SOURCE_DATE_EPOCH: `1794451837` [`SOURCE_DATE_EPOCH.txt`]
- Frozen inputs: [`docs/provenance/FREEZE_MANIFEST.json`], build recipe: [`docs/provenance/BUILD_RECIPE.json`].

## Output

- Published image: `readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2` — index digest `sha256:cf5deacba0a709a244dbef42535a6018f68821d5304750a5fa67216259a22326` [`docs/provenance/LOCKED_DIGEST.txt`]
- linux/amd64 image digest `sha256:5bfc6ee2996955d554260ecf7057f2b7e17908460299aa905d79d51c2f73add8` — reproduced from this archive before publishing [`docs/provenance/REPRO_VERIFICATION.json`].

## Attestations and signature

- SBOM + SLSA provenance attached to the image: attached: an SPDX SBOM and SLSA v1 provenance [`docs/provenance/SCOUT_VERDICT.json`]
- cosign signature by the ReadyStack release key: verified before publishing. Check it: `cosign verify --key https://readystack.dev/keys/cosign.pub readystack/woodpecker:v3.20.1-CE-ubuntu24.04-r2`

## Pinned inputs, and why

- 39 pinned inputs, each with where it came from, why this one, how it was checked and when it was decided: `docs/PINS.md` [`docs/provenance/PINS.json`] (upstream source 2, base and build images 4, image digests 3, dependency versions readystack sets 10, pins in the kit's dockerfiles 13, readystack's build engine 5, dependencies upstream chose 2).

## Build trail

- How this release passed through the ReadyStack harness, record by record (every chain, drive, stage, review round, defect and check, each naming the record it is copied from): `ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.trail.json`, published beside this archive and signed with the release key (`ReadyStack-woodpecker-v3.20.1-CE-ubuntu24.04-r2.trail.json.sigstore`), and readable at https://readystack.dev/products/woodpecker/trail/v3.20.1-CE-ubuntu24.04-r2.

