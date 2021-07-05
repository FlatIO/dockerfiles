FROM flat/ubuntu:latest
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
    build-essential \
  && wget --no-verbose -O /tmp/node.tar.xz "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && tar -xJf /tmp/node.tar.xz -C /usr/local --strip-components=1 --no-same-owner \
  && rm -rf /var/lib/apt/lists/* /tmp/node.tar.xz \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs
