FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y build-essential git ca-certificates \
                       gcc make git exuberant-ctags bc libssl-dev


RUN adduser --gecos "" --disabled-password builder
USER builder:builder
RUN cd /home/builder && \
    git clone git://git.kernel.org/pub/scm/linux/kernel/git/stable/linux-stable.git && \
    cd /home/builder/linux-stable && \
    git checkout v4.10.1 && \
    make defconfig
USER root:root
