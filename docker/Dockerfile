FROM ubuntu:22.04

# Use bash for the shell
SHELL ["/bin/bash", "-c"]
RUN echo 'debconf debconf/frontend select Noninteractive' | debconf-set-selections

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
  libpq-dev \
  ssl-cert \
  ca-certificates \
  curl \
  apt-transport-https \
  lsb-release \
  file \
  git-core \
  build-essential \
  libssl-dev \
  libgexiv2-dev \
  pkg-config

RUN mkdir -p /opt/build
WORKDIR /opt/build
RUN curl https://sh.rustup.rs -sSf | sh -s -- -y
RUN source /root/.cargo/env && rustup +nightly default && cargo install diesel_cli --no-default-features --features postgres

COPY migrations /opt/build/migrations

CMD ["/root/.cargo/bin/diesel", "migration", "run"]

