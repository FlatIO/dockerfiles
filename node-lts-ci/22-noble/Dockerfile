FROM flat/node:22-noble
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

ENV GLAB_VERSION=1.52.0

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get upgrade -y \
  # Build deps
  && apt-get install -y \
    jq \
    libpng-dev \
    libpixman-1-dev\
    libcairo2-dev \
    libjpeg-dev \
    libpango1.0-dev \
    libgif-dev \
    s3cmd \
    python3-pip \
    make \
    g++ \
    curl \
    bash \
    git \
    zip \
    pkg-config \
  && rm -rf /var/lib/apt/lists/* \
  # Sentry
  && curl -sL https://sentry.io/get-cli/ | bash \
  # Bun
  && curl -fsSL https://bun.sh/install | bash \
  # Gitlab CLI
  && curl -L https://gitlab.com/gitlab-org/cli/-/releases/v${GLAB_VERSION}/downloads/glab_${GLAB_VERSION}_linux_amd64.deb -o /tmp/glab.deb \
  && dpkg -i /tmp/glab.deb \
  && rm /tmp/glab.deb \
  # Kubectl
  && curl -L "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" -o /usr/local/bin/kubectl \
  && chmod 755 /usr/local/bin/kubectl \
  && npm install -g \
    npm@latest \
    node-gyp@latest \
    npm-check-updates \
    audit-ci \
    bower \
    grunt-cli \
    semantic-release \
    @semantic-release/commit-analyzer@9.0.2 \
    @semantic-release/release-notes-generator@10.0.3 \
    @semantic-release/npm@9.0.2 \
    @semantic-release/exec \
    @semantic-release/git \
    @semantic-release/changelog \
    semantic-release-monorepo@7.0.8 \
    wrangler \
    ibm-openapi-validator \
    @socketsecurity/cli
