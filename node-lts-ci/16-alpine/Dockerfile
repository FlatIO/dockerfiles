FROM node:16-alpine
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

# NOTE(vince): bower will be removed in a near future
RUN apk add --update-cache curl bash git openssh jq py3-pip make g++ git && rm -rf /var/cache/apk/* \
  && ln -s /usr/bin/python3 /usr/bin/python \
  && pip3 install s3cmd \
  && npm install -g npm-check-updates audit-ci bower grunt-cli semantic-release @semantic-release/git @semantic-release/changelog semantic-release-monorepo \
  && curl -sL https://sentry.io/get-cli/ | bash \
  && yarn policies set-version 1.19.2
