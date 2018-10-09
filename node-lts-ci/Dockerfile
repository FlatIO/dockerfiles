FROM node:8
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# NOTE(vince): nsp & bower will be removed in a near future
RUN apt-get update && apt-get install -y jq s3cmd && rm -rf /var/lib/apt/lists/* \
  && npm install npm@latest -g \
  && npm install -g npm-check-updates nsp bower grunt-cli yarn
