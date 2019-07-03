FROM node:10-alpine AS build-env

MAINTAINER Will Gordon "wgordon@redhat.com"

RUN apk --no-cache --virtual build-dependencies add \
        python \
        make \
        g++ \
        git \
    && git clone https://github.com/openshift-cs/shifty-demo.git /opt/app-root/src \
    && cd /opt/app-root/src \
    && npm install \
    && npm run build --if-present \
    && npm prune \
    && rm $(npm config get cache)* -rf \
    && rm -rf $(npm config get tmp)/npm-* \
    && apk del build-dependencies

# Stage 2

FROM node:10-alpine

ENV NODE_ENV=production

COPY --from=build-env /opt/app-root/src ./

EXPOSE 8080

USER 1001

CMD ["npm", "run", "-d", "start"]
