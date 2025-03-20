FROM flat/ubuntu-dev:jammy
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN apt-get update \
  && curl -sSL https://dl.google.com/linux/linux_signing_key.pub | apt-key add - \
  && echo "deb [arch=amd64] https://dl.google.com/linux/chrome/deb/ stable main" > /etc/apt/sources.list.d/google.list \
  && apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  google-chrome-stable \
  --no-install-recommends \
  && rm -rf /var/lib/apt/lists/*
