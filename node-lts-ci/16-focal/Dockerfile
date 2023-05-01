FROM flat/node:16-focal
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

# NOTE(vince): bower will be removed in a near future
RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y jq libpng-dev libjpeg-dev s3cmd python3-pip make g++ curl bash git \
  && rm -rf /var/lib/apt/lists/* \
  && npm install -g \
    npm@8 \
    node-gyp@latest \
    npm-check-updates \
    audit-ci \
    bower \
    grunt-cli \
    semantic-release \
    @semantic-release/commit-analyzer \
    @semantic-release/release-notes-generator@10.0.3 \
    @semantic-release/npm@9.0.2 \
    @semantic-release/exec \
    @semantic-release/git \
    @semantic-release/changelog \
    semantic-release-monorepo \
  && curl -sL https://sentry.io/get-cli/ | bash
