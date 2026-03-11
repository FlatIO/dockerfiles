FROM flat/node:24-noble
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get upgrade -y \
  # Build essentials for native modules (node-gyp, canvas, etc.)
  && apt-get install -y \
    jq \
    libpng-dev \
    libpixman-1-dev\
    libcairo2-dev \
    libjpeg-dev \
    libpango1.0-dev \
    libgif-dev \
    libgl1 \
    libxi6 \
    python3-pip \
    make \
    g++ \
    curl \
    bash \
    git \
    pkg-config \
  # Sentry CLI (sourcemap upload during build)
  && curl -sL https://sentry.io/get-cli/ | bash \
  # Cleanup
  && rm -rf /var/lib/apt/lists/* \
  # Smoke test
  && node --version \
  && npm --version \
  && sentry-cli --version
