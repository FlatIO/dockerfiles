FROM flat/node:20-jammy
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y jq libpng-dev libjpeg-dev s3cmd python3-pip make g++ curl bash git \
  && rm -rf /var/lib/apt/lists/* \
  && curl -sL https://sentry.io/get-cli/ | bash \
  && curl -L "https://dl.k8s.io/release/$(curl -sL https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" -o /usr/local/bin/kubectl \
  && chmod 755 /usr/local/bin/kubectl \
  && npm install -g \
    npm@8 \
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
    semantic-release-monorepo \
    wrangler
