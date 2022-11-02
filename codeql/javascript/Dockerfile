FROM flat/node:18-jammy
LABEL maintainer "Vincent Giersch <vincent+docker@tutteo.com>"

# Inspired by https://github.com/microsoft/codeql-container/blob/main/Dockerfile
# https://github.com/github/codeql-cli-binaries/releases
ENV CODEQL_VERSION v2.11.2
ENV CODEQL_HOME=/opt/codeql

RUN groupadd -g 999 user \
  && useradd -m -d ${CODEQL_HOME} -r -u 999 -g user user \
  && apt-get update \
  && apt-get upgrade -y \
  && apt-get install -y --no-install-recommends \
    curl \
    wget \
    unzip \
    git \
  && apt-get clean

USER user

# Code binaries
RUN wget -q https://github.com/github/codeql-cli-binaries/releases/download/${CODEQL_VERSION}/codeql-linux64.zip -O /tmp/codeql_linux.zip \
  && unzip /tmp/codeql_linux.zip -d $CODEQL_HOME

# Latest queries
RUN git clone --depth 1 https://github.com/github/codeql ${CODEQL_HOME}/codeql-repo \
  && git --git-dir ${CODEQL_HOME}/codeql-repo/.git log --pretty=reference -1 > ${CODEQL_HOME}/codeql-repo-last-commit

ENV PATH="${CODEQL_HOME}/codeql:${PATH}"

# Pre-compile our queries to save time later
RUN codeql query compile ${CODEQL_HOME}/codeql-repo/javascript/ql/src/codeql-suites/*.qls --additional-packs=.