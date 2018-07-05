FROM ubuntu:latest
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

RUN wget -O firefox.tar.bz2 "https://download.mozilla.org/?product=firefox-beta-latest&os=linux64&lang=en-US" && \
  tar -xvjf firefox* && mv firefox /usr/lib/firefox && ln -s /usr/lib/firefox/firefox /usr/bin/firefox && \
  rm firefox.tar.bz2

# Running user
RUN useradd -d /home/user -m user &&mkdir /profile && chown user: /profile
USER user
ENV HOME /home/user
WORKDIR /home/user

# Minimal profile
RUN mkdir -p /home/user/.mozilla/firefox
COPY profiles.ini /home/user/.mozilla/firefox/
COPY user.js /profile/