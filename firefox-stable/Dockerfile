FROM flat/ubuntu-dev:noble
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

# Install Firefox
RUN export DEBIAN_FRONTEND=noninteractive \
  && apt-get update && apt-get install -y --no-install-recommends \
  net-tools \
  tar \
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
  libasound2-dev \
  && rm -rf /var/lib/apt/lists/*

RUN wget -q -O - "https://download.mozilla.org/?product=firefox-latest&os=linux64&lang=en-US" | tar -xj -C /usr/lib \
  && ln -s /usr/lib/firefox/firefox /usr/bin/firefox
