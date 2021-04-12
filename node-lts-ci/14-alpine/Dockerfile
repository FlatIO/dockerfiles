FROM node:14-alpine3.13
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# NOTE(vince): bower will be removed in a near future
RUN apk add --update-cache curl bash jq py3-pip make g++ && rm -rf /var/cache/apk/* \
  && pip3 install s3cmd \
  && npm install -g npm-check-updates bower grunt-cli \
  && curl -sL https://sentry.io/get-cli/ | bash \
  && yarn policies set-version 1.19.2
