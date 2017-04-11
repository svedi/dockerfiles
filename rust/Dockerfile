FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y build-essential git ca-certificates libssl-dev \
                       curl

RUN adduser --gecos "" --disabled-password rust
USER rust:rust
RUN cd /home/rust && \
    curl -sSf https://static.rust-lang.org/rustup/dist/x86_64-unknown-linux-gnu/rustup-init > rustup-init && \
    chmod +x rustup-init && \
    ./rustup-init -y --no-modify-path && \
    rm rustup-init
USER root:root
