FROM rust:1.96.1-slim-bookworm

RUN apt-get update && apt-get install -y --no-install-recommends \
      pkg-config \
      libssl-dev \
      wget \
      unzip \
      sudo \
    && rm -rf /var/lib/apt/lists/*

ARG MOLD_VERSION=2.41.0
ARG TARGETARCH
RUN case "${TARGETARCH}" in \
      amd64) MOLD_ARCH=x86_64 ;; \
      arm64) MOLD_ARCH=aarch64 ;; \
      *) echo "unsupported arch: ${TARGETARCH}" >&2; exit 1 ;; \
    esac && \
    wget -qO /tmp/mold.tar.gz \
      "https://github.com/rui314/mold/releases/download/v${MOLD_VERSION}/mold-${MOLD_VERSION}-${MOLD_ARCH}-linux.tar.gz" && \
    tar -xzf /tmp/mold.tar.gz -C /usr/local --strip-components=1 && \
    rm /tmp/mold.tar.gz

ENV RUSTFLAGS="-C link-arg=-fuse-ld=mold"

RUN cargo install sqlx-cli --no-default-features --features postgres,rustls --locked
