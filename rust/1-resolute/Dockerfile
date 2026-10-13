FROM flat/ubuntu:26.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

ENV RUSTUP_HOME=/usr/local/rustup
ENV CARGO_HOME=/usr/local/cargo
ENV PATH="/usr/local/cargo/bin:${PATH}"

RUN userdel ubuntu \
  && rm -rf /home/ubuntu \
  && groupadd --gid 1000 rust \
  && useradd --uid 1000 --gid rust --shell /bin/bash --create-home rust \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y --no-install-recommends \
    ca-certificates \
    curl \
    git \
    build-essential \
    pkg-config \
    libssl-dev \
  && rm -rf /var/lib/apt/lists/* \
  # rustup (checksum-verified installer)
  && curl -fsSLO "https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init" \
  && curl -fsSL "https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init.sha256" | awk '{print $1 "  rustup-init"}' | sha256sum -c - \
  && chmod +x rustup-init \
  && ./rustup-init -y --no-modify-path --profile minimal --default-toolchain stable --component clippy,rustfmt \
  && rm rustup-init \
  # Let the non-root user (uid 1000) write to the cargo registry and toolchains
  && chmod -R a+w "$RUSTUP_HOME" "$CARGO_HOME" \
  # smoke tests
  && rustup --version \
  && rustc --version \
  && cargo --version \
  && cargo clippy --version \
  && cargo fmt --version \
  && cd /tmp \
  && cargo new --quiet smoke \
  && cargo run --quiet --manifest-path /tmp/smoke/Cargo.toml \
  && rm -rf /tmp/smoke
