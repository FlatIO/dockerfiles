# Swift toolchain plus SwiftFormat, for Swift CI jobs.
# SwiftFormat is built from source: its official image has no shell for CI jobs, and the 0.60.1
# Linux release binaries crash.
FROM swift:6.4
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

ARG SWIFTFORMAT_VERSION=0.60.1

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install -y --no-install-recommends ca-certificates git \
  && rm -rf /var/lib/apt/lists/* \
  && git clone --quiet --depth 1 --branch "${SWIFTFORMAT_VERSION}" https://github.com/nicklockwood/SwiftFormat /tmp/swiftformat \
  && swift build --package-path /tmp/swiftformat -c release --product swiftformat \
  && install -m 0755 /tmp/swiftformat/.build/release/swiftformat /usr/local/bin/swiftformat \
  && rm -rf /tmp/swiftformat \
  # smoke tests
  && swift --version \
  && swiftformat --version
