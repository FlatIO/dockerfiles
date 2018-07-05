FROM ubuntu:latest
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# Install Firefox
RUN apt-get update && apt-get install -y --no-install-recommends \
  curl \
  net-tools \
  wget \
  ca-certificates \
  tar \
  bzip2 \
  libgtk-3-0 \
  libdbus-1-3 \
  libdbus-glib-1-2 \
  libxtst6 \
  libatk1.0-0 \
  libcairo2 \
  libfreetype6 \
  libgdk-pixbuf2.0-0 \
  libglib2.0-0 \
  libpango-1.0-0 \
  libx11-6 \
  libx11-xcb1 \
  libxcb-shm0 \
  libxext6 \
  libxfixes3 \
  libxrender1 \
  libxt6 \
  libxcb1 \
  libxcb-shm0 \
  && rm -rf /var/lib/apt/lists/*

RUN wget -O firefox.tar.bz2 "https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=en-US" && \
  tar -xvjf firefox* && mv firefox /usr/lib/firefox && ln -s /usr/lib/firefox/firefox /usr/bin/firefox && \
  rm firefox.tar.bz2

# Install node / git / ssh
ENV NODE_VERSION 8.11.2
RUN apt-get update \
  && apt-get install -y wget ca-certificates xz-utils git ssh --no-install-recommends \
  && wget --no-verbose -O /tmp/node.tar.xz "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz" \
  && tar -xJf /tmp/node.tar.xz -C /usr/local --strip-components=1 --no-same-owner \
  && apt-get purge -y xz-utils wget \
  && rm -rf /var/lib/apt/lists/* /tmp/node.tar.xz \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs

# Install python & build-essential
RUN apt-get update \
  && apt-get install python python-dev build-essential -y --no-install-recommends \
  && rm -rf /var/lib/apt/lists/*

RUN groupadd -g 999 user && \
    useradd -m -d /home/user -r -u 999 -g user user
USER user
