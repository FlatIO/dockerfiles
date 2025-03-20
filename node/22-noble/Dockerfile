FROM flat/ubuntu:24.04
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

RUN userdel ubuntu \
  && rm -rf /home/ubuntu \
  && groupadd --gid 1000 node \
  && useradd --uid 1000 --gid node --shell /bin/bash --create-home node \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install gpg curl -y \
  && rm -rf /var/lib/apt/lists/* \
  # gpg keys listed at https://github.com/nodejs/node#release-keys
  && for key in \
    C0D6248439F1D5604AAFFB4021D900FFDB233756 \
    DD792F5973C6DE52C432CBDAC77ABFA00DDBF2B7 \
    CC68F5A3106FF448322E48ED27F5E38D5B0A215F \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    890C08DB8579162FEE0DF9DB8BEAB4DFCF555EF4 \
    C82FA3AE1CBEDC6BE46B9360C43CEC45C17AB93C \
    108F52B48DB57BB0CC439B2997B01419BD92F80A \
    A363A499291CBBC940DD62E41F10027AF002F8B0 \
  ; do \
    gpg --batch --keyserver hkps://keys.openpgp.org --recv-keys "$key" || \
    gpg --batch --keyserver keyserver.ubuntu.com --recv-keys "$key" ; \
  done \
  && export NODE_VERSION=$(curl -sL https://nodejs.org/dist/latest-v22.x | sed -nE 's|.*>node-v(.*)\.pkg</a>.*|\1|p') \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.gz" \
  && curl -fsSLO --compressed "https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc" \
  && gpg --batch --decrypt --output SHASUMS256.txt SHASUMS256.txt.asc \
  && grep " node-v$NODE_VERSION-linux-x64.tar.gz\$" SHASUMS256.txt | sha256sum -c - \
  && tar -xzf "node-v$NODE_VERSION-linux-x64.tar.gz" -C /usr/local --strip-components=1 --no-same-owner \
  && rm "node-v$NODE_VERSION-linux-x64.tar.gz" SHASUMS256.txt.asc SHASUMS256.txt \
  && ln -s /usr/local/bin/node /usr/local/bin/nodejs \
  # npm/pnpm
  && npm install -g pnpm@^9.13.2 npm@latest \
  # smoke tests
  && node --version \
  && npm --version \
  && pnpm --version
