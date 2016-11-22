FROM issmirnov/node-mongo:latest

# This image build upon node-mongo to add go.
# You might be wondering why I don't simply pull from the official go
# image and tack on nodejs/mongo. The answer is that the mongodb alpine
# image I found only works with the bleeding edge release of alpine. Hence,
# for now I'm copy pasting the config from the tree recipes and hosting this.

# everything below is pasted verbatim from https://github.com/docker-library/golang/blob/master/1.7/alpine/Dockerfile

RUN apk add --no-cache ca-certificates

ENV GOLANG_VERSION 1.7.3
ENV GOLANG_SRC_URL https://golang.org/dl/go$GOLANG_VERSION.src.tar.gz
ENV GOLANG_SRC_SHA256 79430a0027a09b0b3ad57e214c4c1acfdd7af290961dd08d322818895af1ef44

# https://golang.org/issue/14851
COPY no-pic.patch /

RUN set -ex \
    && apk add --no-cache --virtual .build-deps \
        bash \
        gcc \
        musl-dev \
        openssl \
        go \
    \
    && export GOROOT_BOOTSTRAP="$(go env GOROOT)" \
    \
    && wget -q "$GOLANG_SRC_URL" -O golang.tar.gz \
    && echo "$GOLANG_SRC_SHA256  golang.tar.gz" | sha256sum -c - \
    && tar -C /usr/local -xzf golang.tar.gz \
    && rm golang.tar.gz \
    && cd /usr/local/go/src \
    && patch -p2 -i /no-pic.patch \
    && ./make.bash \
    \
    && rm -rf /*.patch \
    && apk del .build-deps

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

COPY go-wrapper /usr/local/bin/
