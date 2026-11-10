# flat/rust plus a JDK, the Android SDK and NDK and cargo-ndk: Rust crates built for Android and
# the Gradle builds that package them.
FROM flat/rust:1-resolute
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

ARG NDK_VERSION=30.0.16248370
ARG ANDROID_PLATFORM=android-37.0
ARG ANDROID_BUILD_TOOLS=37.0.0
# https://developer.android.com/studio#command-line-tools-only (the build number names the zip)
ARG CMDLINE_TOOLS_BUILD=14742923
ARG CMDLINE_TOOLS_SHA256=04453066b540409d975c676d781da1477479dde3761310f1a7eb92a1dfb15af7
ARG CARGO_NDK_VERSION=4.1.2

ENV ANDROID_HOME=/opt/android-sdk
ENV ANDROID_NDK_HOME=/opt/android-sdk/ndk/${NDK_VERSION}
ENV JAVA_HOME=/usr/lib/jvm/java-21-openjdk-amd64
ENV PATH="/opt/android-sdk/cmdline-tools/latest/bin:${PATH}"

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y --no-install-recommends openjdk-21-jdk-headless unzip zip \
  && rm -rf /var/lib/apt/lists/* \
  # The SDK's command-line tools (checksum-verified), then what the build needs.
  && curl -fsSL -o cmdline-tools.zip "https://dl.google.com/android/repository/commandlinetools-linux-${CMDLINE_TOOLS_BUILD}_latest.zip" \
  && echo "${CMDLINE_TOOLS_SHA256}  cmdline-tools.zip" | sha256sum -c - \
  && mkdir -p "${ANDROID_HOME}/cmdline-tools" \
  && unzip -q cmdline-tools.zip -d "${ANDROID_HOME}/cmdline-tools" \
  && mv "${ANDROID_HOME}/cmdline-tools/cmdline-tools" "${ANDROID_HOME}/cmdline-tools/latest" \
  && rm cmdline-tools.zip \
  && (yes || true) | sdkmanager --licenses > /dev/null \
  && sdkmanager "platform-tools" "platforms;${ANDROID_PLATFORM}" "build-tools;${ANDROID_BUILD_TOOLS}" "ndk;${NDK_VERSION}" > /dev/null \
  # The four Android ABIs, and cargo-ndk to build for them.
  && rustup target add aarch64-linux-android armv7-linux-androideabi x86_64-linux-android i686-linux-android \
  && cargo install --locked cargo-ndk --version "${CARGO_NDK_VERSION}" \
  && rm -rf "$CARGO_HOME/registry" "$CARGO_HOME/git" \
  # Let the non-root user (uid 1000) write to the SDK, the cargo registry and the toolchains
  && chmod -R a+rwX "${ANDROID_HOME}" "$RUSTUP_HOME" "$CARGO_HOME" \
  # smoke tests
  && java -version \
  && sdkmanager --list_installed \
  && cargo ndk --version \
  && "${ANDROID_NDK_HOME}/toolchains/llvm/prebuilt/linux-x86_64/bin/llvm-readelf" --version \
  && rustup target list --installed
