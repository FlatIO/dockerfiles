FROM flat/ubuntu:26.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

ENV RUSTUP_HOME=/usr/local/rustup
ENV CARGO_HOME=/usr/local/cargo
ENV PATH="/usr/local/cargo/bin:${PATH}"

ARG ZIG_VERSION=0.14.1
ARG ZIG_SHA256=24aeeec8af16c381934a6cd7d95c807a8cb2cf7df9fa40d359aa884195c4716c
ARG CARGO_ZIGBUILD_VERSION=0.23.4
ARG CARGO_ZIGBUILD_SHA256=9e3cf73485edbd45905c8aadbc0fdf869c7ddc3848f0c898229f2680db52e44b

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
    # callgrind, which the instruction-count benchmarks run on (gungraun)
    valgrind \
  && rm -rf /var/lib/apt/lists/* \
  # rustup (checksum-verified installer)
  && curl -fsSLO "https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init" \
  && curl -fsSL "https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init.sha256" | awk '{print $1 "  rustup-init"}' | sha256sum -c - \
  && chmod +x rustup-init \
  && ./rustup-init -y --no-modify-path --profile minimal --default-toolchain stable --component clippy,rustfmt \
  && rm rustup-init \
  # macOS arm64 cross-builds (cargo zigbuild --target aarch64-apple-darwin): the target's std, zig as
  # the linker and cargo-zigbuild. zig stays on 0.14: 0.15 and later misread the
  # -exported_symbols_list rustc passes for a macOS cdylib.
  && rustup target add aarch64-apple-darwin \
  && curl -fsSL -o zig.tar.xz "https://ziglang.org/download/${ZIG_VERSION}/zig-x86_64-linux-${ZIG_VERSION}.tar.xz" \
  && echo "${ZIG_SHA256}  zig.tar.xz" | sha256sum -c - \
  && mkdir /usr/local/zig \
  && tar -xJf zig.tar.xz -C /usr/local/zig --strip-components=1 \
  && rm zig.tar.xz \
  && ln -s /usr/local/zig/zig /usr/local/bin/zig \
  && curl -fsSL -o cargo-zigbuild.tar.xz "https://github.com/rust-cross/cargo-zigbuild/releases/download/v${CARGO_ZIGBUILD_VERSION}/cargo-zigbuild-x86_64-unknown-linux-musl.tar.xz" \
  && echo "${CARGO_ZIGBUILD_SHA256}  cargo-zigbuild.tar.xz" | sha256sum -c - \
  && tar -xJOf cargo-zigbuild.tar.xz cargo-zigbuild-x86_64-unknown-linux-musl/cargo-zigbuild > /usr/local/bin/cargo-zigbuild \
  && chmod 0755 /usr/local/bin/cargo-zigbuild \
  && rm cargo-zigbuild.tar.xz \
  # Let the non-root user (uid 1000) write to the cargo registry and toolchains
  && chmod -R a+w "$RUSTUP_HOME" "$CARGO_HOME" \
  # smoke tests
  && rustup --version \
  && rustc --version \
  && cargo --version \
  && cargo clippy --version \
  && cargo fmt --version \
  && valgrind --version \
  && zig version \
  && cargo zigbuild --version \
  && cd /tmp \
  && cargo new --quiet smoke \
  && cargo run --quiet --manifest-path /tmp/smoke/Cargo.toml \
  && rm -rf /tmp/smoke
