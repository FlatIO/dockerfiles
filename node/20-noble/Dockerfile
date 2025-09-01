FROM flat/ubuntu:24.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN userdel ubuntu \
  && rm -rf /home/ubuntu \
  && groupadd --gid 1000 node \
  && useradd --uid 1000 --gid node --shell /bin/bash --create-home node \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install gpg curl -y \
  && rm -rf /var/lib/apt/lists/* \
  # Import all Node.js release keys from https://github.com/nodejs/release-keys
  && curl -fsSL https://raw.githubusercontent.com/nodejs/release-keys/refs/heads/main/keys.list -o /tmp/node-keys.list \
  && while IFS= read -r key; do \
    curl -fsSL "https://raw.githubusercontent.com/nodejs/release-keys/refs/heads/main/keys/${key}.asc" | gpg --batch --import || \
    gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$key" || \
    gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key" ; \
  done < /tmp/node-keys.list \
  && rm /tmp/node-keys.list \
  && export NODE_VERSION=$(curl -sL https://nodejs.org/dist/latest-v20.x | sed -nE 's|.*>node-v(.*)\.pkg</a>.*|\1|p') \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 --no-same-owner \
  && rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
  # npm/pnpm/yarn
  && npm install -g pnpm@latest npm@latest yarn@$YARN_VERSION \
  # smoke tests
  && node --version \
  && npm --version \
  && pnpm --version
