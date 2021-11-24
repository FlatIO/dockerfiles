FROM flat/node:16-focal
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

ENV NODE_VERSION 16.4.1
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    wget \
    curl \ 
    ca-certificates \
    xz-utils \
    git \
    ssh \
    python \
    python-dev \
    build-essential