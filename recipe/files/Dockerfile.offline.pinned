# GENERATED L-009 hermetic build (--network=none, SBOM+SLSA-attestation-producing).
# Pinned bases from BASE_IMAGE_DIGESTS.lock; vendored Go tree + debs from Dockerfile.vendor.
FROM golang:1.27.4-bookworm@sha256:4fa7c968ba93f1635670522dbb01a2c78150443e1385746924566079f36570ea AS build_woodpecker-server
WORKDIR /src
COPY vendored/woodpecker-src/ .
ENV CGO_ENABLED=1 GOTOOLCHAIN=local GOWORK=off GOFLAGS="-mod=vendor -buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1794451837
RUN go build -trimpath -ldflags='-s -w -X go.woodpecker-ci.org/woodpecker/v3/version.Version=3.20.1' -o /woodpecker-server ./cmd/server && /woodpecker-server --version && \
    { readelf -lW /woodpecker-server | sed -n 's/.*Requesting program interpreter: \(.*\)]$/\1/p'; readelf -dW /woodpecker-server | sed -n 's/.*Shared library: \[\(.*\)\]$/\1/p'; } > /woodpecker-server.needs
# rs-probe: the HEALTHCHECK helper (standard library only, from the kit's rs-probe.go)
FROM golang:1.27.4-bookworm@sha256:4fa7c968ba93f1635670522dbb01a2c78150443e1385746924566079f36570ea AS build_rs_probe
WORKDIR /probe
COPY rs-probe.go main.go
COPY rs-probe.mod go.mod
ENV CGO_ENABLED=0 GOTOOLCHAIN=local GOFLAGS="-buildvcs=false" GOPROXY=off SOURCE_DATE_EPOCH=1794451837
RUN go build -trimpath -ldflags='-s -w' -o /rs-probe .
# the chiselled rootfs (vendored/chisel, cut by Dockerfile.vendor), the service user, the binaries
FROM ubuntu:24.04@sha256:2198e4c65076e9cb0b3d695219df667c0e3b0072da64ca3c2648aca17dcecaf5 AS rootfs
ARG SOURCE_DATE_EPOCH=1794451837
ADD vendored/chisel/rootfs.tar /rootfs/
RUN printf 'woodpecker:x:999:999::/var/lib/woodpecker:/bin/sh\n' >> /rootfs/etc/passwd && printf 'woodpecker:x:999:\n' >> /rootfs/etc/group && \
    mkdir -p /rootfs/var/lib/woodpecker /rootfs/etc/woodpecker && chown -R 999:999 /rootfs/var/lib/woodpecker /rootfs/etc/woodpecker
COPY --from=build_woodpecker-server /woodpecker-server /rootfs/usr/bin/woodpecker-server
COPY --from=build_woodpecker-server /woodpecker-server.needs /tmp/needs/woodpecker-server
COPY --from=build_rs_probe /rs-probe /rootfs/usr/local/bin/rs-probe
RUN chmod 755 /rootfs/usr/bin/woodpecker-server /rootfs/usr/local/bin/rs-probe
RUN --mount=type=bind,source=vendored/chisel,target=/chisel set -eu; \
    for n in /tmp/needs/*; do while read -r lib; do [ -n "$lib" ] || continue; \
      case "$lib" in /*) [ -e "/rootfs$lib" ] ;; *) [ -n "$(find /rootfs/usr/lib /rootfs/usr/lib64 -name "$lib" 2>/dev/null | head -1)" ] ;; esac || \
      { echo "$(basename "$n") needs $lib, which the chiselled rootfs does not have: add its slice to runtime_base.slices" >&2; exit 1; }; \
    done < "$n"; done; rm -rf /tmp/needs; \
    [ -e /rootfs/etc/os-release ] || { echo "no /etc/os-release: base-files_release-info is missing" >&2; exit 1; }; \
    [ "$(grep -c '^Package: ' /rootfs/var/lib/dpkg/status)" = "$(wc -l < /chisel/packages.tsv)" ] || \
      { echo "/var/lib/dpkg/status does not list every package of vendored/chisel/packages.tsv" >&2; exit 1; }; \
    rm -rf /rootfs/var/log/dpkg.log /rootfs/var/log/alternatives.log /rootfs/var/log/apt /rootfs/var/cache/ldconfig/aux-cache /rootfs/var/cache/fontconfig
FROM scratch
ARG SOURCE_DATE_EPOCH=1794451837
COPY --from=rootfs /rootfs/ /
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin WOODPECKER_HOST=http://localhost:8000 WOODPECKER_DATABASE_DRIVER=sqlite3 WOODPECKER_DATABASE_DATASOURCE=/var/lib/woodpecker/woodpecker.sqlite
LABEL org.opencontainers.image.licenses="Apache-2.0"
LABEL org.opencontainers.image.title="Woodpecker"
LABEL org.opencontainers.image.description="Woodpecker automates software delivery by connecting your code repository to independent agents that build and test your projects. This self-hosted platform gives you full control over your continuous integration workflows without relying on external services."
LABEL org.opencontainers.image.source="https://github.com/woodpecker-ci/woodpecker"
LABEL org.opencontainers.image.version="v3.20.1-CE-ubuntu24.04-r2"
LABEL org.opencontainers.image.revision="3f1bb0bd0db28e410ad797034d75780116122c91"
HEALTHCHECK --interval=5s --timeout=60s --start-period=15s \
  CMD /usr/local/bin/rs-probe http://127.0.0.1:8000/healthz || exit 1
USER woodpecker
EXPOSE 8000 9000
ENTRYPOINT ["woodpecker-server"]
