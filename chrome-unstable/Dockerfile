FROM flat/ubuntu-dev:jammy
LABEL maintainer="Vincent Giersch <vincent+docker@tutteo.com>"

RUN apt-get update \
  && curl https://dl.google.com/linux/direct/google-chrome-unstable_current_amd64.deb > chrome.deb \
  && apt install --no-install-recommends -y ./chrome.deb \
  && rm -rf /var/lib/apt/lists/* chrome.deb
