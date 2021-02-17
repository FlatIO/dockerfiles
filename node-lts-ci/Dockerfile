FROM node:12
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# NOTE(vince): bower will be removed in a near future
RUN apt-get update && apt-get install -y jq s3cmd && rm -rf /var/lib/apt/lists/* \
  && npm install -g npm-check-updates bower grunt-cli ibm-openapi-validator \
  && curl -sL https://sentry.io/get-cli/ | bash \
  && yarn policies set-version 1.19.2
