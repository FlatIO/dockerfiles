FROM flat/node:24-noble
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

ENV GLAB_VERSION=1.74.0
ENV MONGOSH_VERSION=2.5.8
ENV PATH="/root/.local/bin:${PATH}"

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
    libgl1 \
    libxi6 \
    s3cmd \
    unzip \
    python3-pip \
    make \
    g++ \
    curl \
    bash \
    git \
    zip \
    pkg-config \
  # Sentry
  && curl -sL https://sentry.io/get-cli/ | bash \
  # Bun
  && curl -fsSL https://bun.sh/install | bash \
  # MongoSH
  && curl -fsSL https://downloads.mongodb.com/compass/mongodb-mongosh_${MONGOSH_VERSION}_amd64.deb -o /tmp/mongosh.deb \
  && dpkg -i /tmp/mongosh.deb \
  && rm /tmp/mongosh.deb \
  # Gitlab CLI
  && curl -L https://gitlab.com/gitlab-org/cli/-/releases/v${GLAB_VERSION}/downloads/glab_${GLAB_VERSION}_linux_amd64.deb -o /tmp/glab.deb \
  && dpkg -i /tmp/glab.deb \
  && rm /tmp/glab.deb \
  # Kubectl
  && curl -L "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" -o /usr/local/bin/kubectl \
  && chmod 755 /usr/local/bin/kubectl \
  # Google Cloud CLI
  && curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | gpg --dearmor -o /usr/share/keyrings/cloud.google.gpg \
  && echo "deb [signed-by=/usr/share/keyrings/cloud.google.gpg] https://packages.cloud.google.com/apt cloud-sdk main" | tee -a /etc/apt/sources.list.d/google-cloud-sdk.list \
  && apt-get update \
  && apt-get install -y google-cloud-cli \
  # NPM Deps
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
    @socketsecurity/cli \
  # AWS CLI v2
  && curl -fsSL "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o /tmp/awscliv2.zip \
  && unzip -q /tmp/awscliv2.zip -d /tmp \
  && /tmp/aws/install \
  && rm -rf /tmp/awscliv2.zip /tmp/aws \
  # uv (Python package manager)
  && curl -LsSf https://astral.sh/uv/install.sh | sh \
  # Cleanup layer
  && rm -rf /var/lib/apt/lists/* \
  # Smoke test
  && npm --version \
  && node --version \
  && git --version \
  && glab --version \
  && kubectl version --client \
  && gcloud --version \
  && mongosh --version \
  && aws --version \
  && uv --version