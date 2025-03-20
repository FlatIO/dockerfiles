FROM flat/node:22-noble
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y --no-install-recommends \
    wget \
    curl \ 
    ca-certificates \
    xz-utils \
    git \
    ssh \
    python3 \
    python3-dev \
    build-essential \
    jq \
  && rm -rf /var/lib/apt/lists/*