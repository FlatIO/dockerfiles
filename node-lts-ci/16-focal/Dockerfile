FROM flat/node:16-focal
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

# NOTE(vince): bower will be removed in a near future
RUN apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y jq s3cmd python3-pip make g++ curl bash git \
  && rm -rf /var/lib/apt/lists/* \
  && npm install -g npm-check-updates bower grunt-cli semantic-release @semantic-release/git @semantic-release/changelog \
  && curl -sL https://sentry.io/get-cli/ | bash