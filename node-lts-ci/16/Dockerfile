FROM node:16
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# NOTE(vince): bower will be removed in a near future
RUN apt-get update && apt-get upgrade -y && apt-get install -y jq s3cmd && rm -rf /var/lib/apt/lists/* \
  && npm install -g npm-check-updates bower grunt-cli semantic-release @semantic-release/git @semantic-release/changelog \
  && curl -sL https://sentry.io/get-cli/ | bash \
  && yarn policies set-version 1.19.2
