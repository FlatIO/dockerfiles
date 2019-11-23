FROM verdaccio/verdaccio:4
USER root
ENV NODE_ENV=production
RUN npm install -g verdaccio-gitlab verdaccio-audit verdaccio-s3-storage
USER verdaccio