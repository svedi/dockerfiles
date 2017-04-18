FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y build-essential git ca-certificates \
                       gcc make git exuberant-ctags bc libssl-dev \
                       clang valgrind vim tmux

RUN adduser --gecos "" --disabled-password dev
