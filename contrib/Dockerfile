# This file is used to add the nightly Dgraph binaries and assets to Dgraph base
# image.

# This gets built as part of release.sh. Must be run from /tmp/build, with the linux binaries
# already built and placed there.

FROM golang:1.17.3-bullseye AS build

RUN apt-get update && \
  apt-get install -y gcc make git bzip2 && \
  rm -rf /var/lib/apt/lists/* && \
  mkdir /dgraph
  
WORKDIR /dgraph
COPY . .

RUN make install

FROM ubuntu:latest
LABEL maintainer="Dgraph Labs <contact@dgraph.io>"

RUN apt-get update && \
  apt-get install -y --no-install-recommends ca-certificates curl iputils-ping && \
  rm -rf /var/lib/apt/lists/*

COPY --from=build /go/bin/dgraph /usr/local/bin/dgraph

EXPOSE 8080
EXPOSE 9080

RUN mkdir /dgraph
WORKDIR /dgraph

CMD ["dgraph"] # Shows the dgraph version and commands available.
