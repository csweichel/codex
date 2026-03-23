# This Dockerfile is kept for reference / manual Docker builds.
# The devcontainer.json uses a standard base image with features instead.
#
# For manual Docker builds:
#   docker build -t codex-linux-dev .devcontainer/
#   docker run --rm -it -v "$PWD":/workspaces/codex -w /workspaces/codex/codex-rs codex-linux-dev

FROM ubuntu:24.04

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    software-properties-common && \
    add-apt-repository --yes universe

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
    build-essential curl git ca-certificates \
    pkg-config libcap-dev clang musl-tools libssl-dev just && \
    rm -rf /var/lib/apt/lists/*

RUN useradd -m -s /bin/bash dev
USER dev

RUN curl -sSf https://sh.rustup.rs | sh -s -- -y --profile minimal && \
    . ~/.cargo/env && \
    ARCH=$(uname -m) && \
    if [ "$ARCH" = "x86_64" ]; then \
      rustup target add x86_64-unknown-linux-musl; \
    else \
      rustup target add aarch64-unknown-linux-musl; \
    fi && \
    rustup component add clippy rustfmt

ENV PATH="/home/dev/.cargo/bin:${PATH}"
WORKDIR /workspaces/codex
