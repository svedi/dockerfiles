FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y build-essential git ca-certificates \
                       gcc make git exuberant-ctags bc libssl-dev \
                       clang valgrind vim tmux tmux \
                       # From stack install instructions
                       g++ gcc libc6-dev libffi-dev libgmp-dev make \
                       xz-utils zlib1g-dev git gnupg

RUN apt-get install wget
RUN adduser --gecos "" --disabled-password dev
USER dev:dev
RUN cd /home/dev/ \
 && wget https://www.stackage.org/stack/linux-x86_64-static \
 && tar xvzf linux-x86_64-static \
 && mkdir /home/dev/bin \
 && mv /home/dev/stack*/stack /home/dev/bin/ \
 && rm /home/dev/linux-x86_64-static \
 && rm -r /home/dev/stack* \
 && /home/dev/bin/stack setup
USER root:root
