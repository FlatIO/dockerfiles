FROM flat/ubuntu-dev:latest
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# Install Chrome
RUN apt-get update && apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  hicolor-icon-theme \
  libcanberra-gtk* \
  libgl1-mesa-dri \
  libgl1-mesa-glx \
  libpango1.0-0 \
  libpulse0 \
  libv4l-0 \
  fonts-symbola \
  --no-install-recommends \
  && curl -sSL https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list \
  && apt-get update && apt-get install -y \
  google-chrome-stable \
  --no-install-recommends \
  && apt-get purge --auto-remove -y curl \
  && rm -rf /var/lib/apt/lists/*

# Install node / git / ssh
ENV NODE_VERSION 8.11.3
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
