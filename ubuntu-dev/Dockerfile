FROM flat/node:16-jammy
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

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
  && rm -rf /var/lib/apt/lists/*