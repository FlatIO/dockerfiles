FROM flat/ubuntu:24.04
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN \
  # Setup the kubectl repository of the latest stable Kubernetes minor
  apt-get update \
  && apt-get install -y apt-transport-https ca-certificates curl gnupg \
  && export K8S_STABLE=$(curl -fsSL https://dl.k8s.io/release/stable.txt) \
  && export K8S_MINOR=$(echo "$K8S_STABLE" | sed -nE 's/^(v[0-9]+\.[0-9]+)\..*$/\1/p') \
  && if [ -z "$K8S_MINOR" ]; then echo "Unable to resolve the latest stable Kubernetes release" >&2 ; exit 1 ; fi \
  && echo "==> Using the Kubernetes ${K8S_MINOR} apt repository (latest stable is ${K8S_STABLE})" \
  && mkdir -p -m 755 /etc/apt/keyrings \
  && curl -fsSL "https://pkgs.k8s.io/core:/stable:/${K8S_MINOR}/deb/Release.key" | gpg --dearmor -o /etc/apt/keyrings/kubernetes-apt-keyring.gpg \
  && chmod 644 /etc/apt/keyrings/kubernetes-apt-keyring.gpg \
  && echo "deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/${K8S_MINOR}/deb/ /" | tee /etc/apt/sources.list.d/kubernetes.list \
  && chmod 644 /etc/apt/sources.list.d/kubernetes.list \
  # Install kubectl and envsubst
  && apt-get update \
  && apt-get install -y kubectl gettext-base \
  # Cleanup
  && apt-get purge -y apt-transport-https ca-certificates curl gnupg \
  && apt-get autoremove -y \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/* \
  # Smoke test
  && kubectl version --client \
  && kubectl version --client | grep -q "Client Version: ${K8S_MINOR}\."

USER ubuntu
