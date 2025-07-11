FROM flat/ubuntu:24.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN userdel ubuntu \
  && rm -rf /home/ubuntu \
  && groupadd --gid 1000 bun \
  && useradd --uid 1000 --gid bun --shell /bin/bash --create-home bun \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install curl unzip -y \
  && rm -rf /var/lib/apt/lists/* \
  && export BUN_VERSION=$(curl -sL https://api.github.com/repos/oven-sh/bun/releases/latest | grep '"tag_name":' | sed -E 's/.*"bun-v([^"]+)".*/\1/') \
  && curl -fsSLO --compressed "https://github.com/oven-sh/bun/releases/download/bun-v${BUN_VERSION}/bun-linux-x64.zip" \
  && unzip bun-linux-x64.zip \
  && mv bun-linux-x64/bun /usr/local/bin/ \
  && rm -rf bun-linux-x64.zip bun-linux-x64 \
  && chmod +x /usr/local/bin/bun \
  # smoke tests
  && bun --version