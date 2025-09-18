FROM flat/ubuntu:24.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN \
  # cURL
  apt-get update \
  && apt-get install -y curl \
  # Kubectl
  && curl -LO https://dl.k8s.io/release/$(curl -Ls https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl \
  && install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl \
  && rm kubectl \
  && kubectl version --client \
  # Cleanup
  && apt-get purge curl -y \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

USER ubuntu