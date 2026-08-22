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
  # Latest 1.4.x release: the releases list is newest-first, keep the first 1.4 tag
  && export BUN_VERSION=$(curl -sL "https://api.github.com/repos/oven-sh/bun/releases?per_page=100" | sed -nE 's/^[[:space:]]*"tag_name":[[:space:]]*"bun-v(1\.4\.[0-9]+)",?$/\1/p' | head -1) \
  && if [ -z "$BUN_VERSION" ]; then echo "Unable to resolve the latest bun 1.4.x release" >&2 ; exit 1 ; fi \
  && curl -fsSLO --compressed "https://github.com/oven-sh/bun/releases/download/bun-v${BUN_VERSION}/bun-linux-x64.zip" \
  && unzip bun-linux-x64.zip \
  && mv bun-linux-x64/bun /usr/local/bin/ \
  && rm -rf bun-linux-x64.zip bun-linux-x64 \
  && chmod +x /usr/local/bin/bun \
  # smoke tests
  && bun --version \
  && case "$(bun --version)" in 1.4.*) ;; *) echo "Expected bun 1.4.x, got $(bun --version)" >&2 ; exit 1 ;; esac
