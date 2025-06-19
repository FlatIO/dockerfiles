FROM flat/ubuntu:24.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN userdel ubuntu \
  && rm -rf /home/ubuntu \
  && groupadd --gid 1000 python \
  && useradd --uid 1000 --gid python --shell /bin/bash --home-dir /app --create-home python \
  && export DEBIAN_FRONTEND=noninteractive \
  && apt-get update \
  && apt-get install python3 python3-pip python3-venv curl -y \
  && rm -rf /var/lib/apt/lists/* \
  && curl -LsSf https://astral.sh/uv/install.sh | sh

ENV PATH="/app/.local/bin/:$PATH" 
