FROM flat/ubuntu-dev:latest
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# Install Chrome
RUN apt-get update \
  && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg \
  hicolor-icon-theme \
  libcanberra-gtk* \
  libgl1-mesa-dri \
  libgl1-mesa-glx \
  libpango1.0-0 \
  libpulse0 \
  libv4l-0 \
  fonts-symbola \
  fonts-liberation \
  libappindicator3-1 \
  libnspr4 \
  libnss3 \
  libxss1 \
  lsb-release \
  xdg-utils \
  --no-install-recommends \
  && curl https://dl.google.com/linux/direct/google-chrome-unstable_current_amd64.deb > chrome.deb \
  && dpkg -i chrome.deb \
  && apt-get purge --auto-remove -y curl \
  && rm -rf /var/lib/apt/lists/* chrome.deb

RUN groupadd -g 999 user && \
    useradd -m -d /home/user -r -u 999 -g user user
USER user
