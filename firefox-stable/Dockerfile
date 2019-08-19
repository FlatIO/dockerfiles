FROM flat/ubuntu-dev:latest
LABEL maintainer "Vincent Giersch <vincent+docker@flat.io>"

# Install Firefox
RUN apt-get update && apt-get install -y --no-install-recommends \
  curl \
  net-tools \
  wget \
  ca-certificates \
  tar \
  bzip2 \
  libgtk-3-0 \
  libdbus-1-3 \
  libdbus-glib-1-2 \
  libxtst6 \
  libatk1.0-0 \
  libcairo2 \
  libfreetype6 \
  libgdk-pixbuf2.0-0 \
  libglib2.0-0 \
  libpango-1.0-0 \
  libx11-6 \
  libx11-xcb1 \
  libxcb-shm0 \
  libxext6 \
  libxfixes3 \
  libxrender1 \
  libxt6 \
  libxcb1 \
  libxcb-shm0 \
  && rm -rf /var/lib/apt/lists/*

RUN wget -O firefox.tar.bz2 "https://download-installer.cdn.mozilla.net/pub/firefox/releases/66.0.5/linux-x86_64/en-US/firefox-66.0.5.tar.bz2" && \
  tar -xvjf firefox* && mv firefox /usr/lib/firefox && ln -s /usr/lib/firefox/firefox /usr/bin/firefox && \
  rm firefox.tar.bz2

RUN groupadd -g 999 user && \
    useradd -m -d /home/user -r -u 999 -g user user
USER user
