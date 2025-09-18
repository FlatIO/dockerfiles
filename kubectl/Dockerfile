FROM flat/ubuntu:24.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN \
  # Setup kubectl repository
  apt-get update \
  && apt-get install -y apt-transport-https ca-certificates curl gnupg \
  && mkdir -p -m 755 /etc/apt/keyrings \
  && curl -fsSL https://pkgs.k8s.io/core:/stable:/v1.33/deb/Release.key | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg \
  && chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg \
  && echo 'deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.33/deb/ /' | tee /etc/apt/sources.list.d/kubernetes.list \
  && chmod 644 /etc/apt/sources.list.d/kubernetes.list \
  # Install kubectl
  && apt-get update \
  && apt-get install -y kubectl \
  # Cleanup
  && apt-get purge -y apt-transport-https ca-certificates curl gnupg \
  && apt-get autoremove -y \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  # Smoke test
  && kubectl version --client

USER ubuntu