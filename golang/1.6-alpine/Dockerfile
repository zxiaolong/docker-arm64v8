FROM arm64v8/alpine:3.5

RUN apk add --no-cache ca-certificates

ENV GOLANG_VERSION 1.6.4
ENV GOLANG_SRC_URL https://golang.org/dl/go$GOLANG_VERSION.src.tar.gz
ENV GOLANG_SRC_SHA256 8796cc48217b59595832aa9de6db45f58706dae68c9c7fbbd78c9fdbe3cd9032

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
        && ./make.bash \
        \
        && rm -rf /*.patch \
        && apk del .build-deps

ENV GOPATH /go
ENV PATH $GOPATH/bin:/usr/local/go/bin:$PATH

RUN mkdir -p "$GOPATH/src" "$GOPATH/bin" && chmod -R 777 "$GOPATH"
WORKDIR $GOPATH

COPY go-wrapper /usr/local/bin/

