FROM alpine:edge

# This is a combo image with node 6 and mongodb, based on alpine.
# I needed the latest version of NPM + mongo on alpine, which doesn't yet exist.
# NOTE: you will need to launch the mongodb daemon in your script with  `mongod`

# install node.
# All credit for this block goes to https://hub.docker.com/r/smebberson/alpine-nodejs/
ENV NODE_VERSION=v6.3.0 NPM_VERSION=3

RUN apk upgrade --update && \
    apk add --update git curl make gcc g++ python linux-headers libgcc libstdc++ && \
    curl -sSL https://nodejs.org/dist/${NODE_VERSION}/node-${NODE_VERSION}.tar.gz | tar -xz && \
    cd /node-${NODE_VERSION} && \
    ./configure --prefix=/usr --without-snapshot && \
    make -j$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
    make install && \
    cd / && \
    npm install -g npm@${NPM_VERSION} && \
    apk del gcc g++ linux-headers && \
    rm -rf /etc/ssl /node-${NODE_VERSION} /usr/include \
    /usr/share/man /tmp/* /var/cache/apk/* /root/.npm /root/.node-gyp \
    /usr/lib/node_modules/npm/man /usr/lib/node_modules/npm/doc /usr/lib/node_modules/npm/html

# install mongo
# all credit for this block goes to https://hub.docker.com/r/mvertes/alpine-mongo/
RUN \
echo http://dl-4.alpinelinux.org/alpine/edge/testing >> /etc/apk/repositories && \
apk upgrade --update && \
#apk add boost-dev && \
apk add --no-cache mongodb && \
rm /usr/bin/mongosniff /usr/bin/mongoperf

VOLUME /data/db
