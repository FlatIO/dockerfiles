FROM verdaccio/verdaccio:5
USER root
ENV NODE_ENV=production
RUN apk add --no-cache -X http://dl-cdn.alpinelinux.org/alpine/edge/testing s3cmd
RUN npm i -g verdaccio-gitlab verdaccio-audit verdaccio-s3-storage
ENV PYTHONPATH=/usr/lib/python3.10/site-packages/
USER verdaccio
