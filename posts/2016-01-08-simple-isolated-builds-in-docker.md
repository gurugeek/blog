title: Simple Isolated Builds in Docker
categories:
  - Linux
  - Docker
  - Rust
published_date: "2016-01-08 01:52:25 +1100"
layout: default.liquid
data:
  layout: post
  comments: true
---
I needed a simple, automated way to build a Rust project for deployment to a
Debian system. Docker seems to be popular for deploying though I'm using it as a
chroot with a nice CLI. Eventually I would like the build product to be a deb
though at the moment an ELF binary is output. My current application is self
contained with all state contained in a Postgres database so this works for the
moment. The build system is made up of two parts. A `Dockerfile` and a
`build.sh` script.

## Dockerfile

The `Dockerfile` is simple with nothing tying it to a particular project. It
contains the required packages to support compiling Rust projects along with the
Rust compiler itself. A single user is created to avoid running the build as
root.

```
# Dockerfile
# based on https://github.com/jimmycuadra/docker-rust

FROM debian:jessie

ENV RUST_VERSION=1.5.0
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y \
    build-essential ca-certificates curl git libssl-dev \
&&  useradd build -m -u 1000 -s /bin/bash \
&&  curl -sO https://static.rust-lang.org/dist/rust-$RUST_VERSION-x86_64-unknown-linux-gnu.tar.gz \
&&  tar -xzf rust-$RUST_VERSION-x86_64-unknown-linux-gnu.tar.gz \
&&  ./rust-$RUST_VERSION-x86_64-unknown-linux-gnu/install.sh --without=rust-docs \
&&  rm -rf rust-$RUST_VERSION-x86_64-unknown-linux-gnu \
           rust-$RUST_VERSION-x86_64-unknown-linux-gnu.tar.gz \
           /var/lib/apt/lists/* \
           /tmp/* \
```

## build.sh

The `build.sh` script needs a little more work. In particular I'm pretty sure I
can simplify the instance creation and file copying stages. I'm still coming to
terms with Docker. The file copying stage in particular is fairly awful at the
moment. I'm purposely not mounting any of the host file system into the
container, which seems to be popular when doing this sort of thing, to isolate
the build as much as possible.

```
#!/bin/bash

PROJECT=example
NAME=${PROJECT}-build

msg() {
    echo -e "\033[0;32m:: ${1}\033[0m"
}

msg "Starting build instance."
docker run --name=${NAME} -w /home/build -d jessie-rust sleep 900

# Copy files into docker instance
docker cp . ${NAME}:/home/build/
docker exec ${NAME} rm -rf /home/build/target
docker exec ${NAME} chown -R build:build /home/build

msg "Building..."
docker exec -u build ${NAME} cargo build --release
msg "Build complete."

# Copy build result out
docker cp ${NAME}:/home/build/target/release/${PROJECT} .

msg "Stopping and killing build instance."
docker kill ${NAME}
docker rm ${NAME}
```

## Thoughts

There are a couple of things that need tidying up and/or fixing.

 - The Build should output a deb package. I'll look at this at some point.
 - As mentioned above the instance creation and file copying stages can be
 simplified. E.g. the hack of running `sleep 900` probably isn't required as you
 can supposedly copy files into a stopped container. The copying itself can be
 improved by using `tar` and excluding the required directories all together to
 avoid removing them at a later point. `docker cp` can take a `tar` stream as
 input.
 - The build instance is made fresh each time so Rust hast to download and
 compile libraries each time. This makes the build a little slower though it
 also ensures each build is isolated and not influenced by previous builds in
 any way. For this reason I don't mind.

## Sample run

This isn't much to it however here is a sample run:

<script type="text/javascript" src="https://asciinema.org/a/33132.js" id="asciicast-33132" async></script>
