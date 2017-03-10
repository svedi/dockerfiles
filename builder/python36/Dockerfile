FROM ubuntu:16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget \
                       # Some of the following are needed for building python 3.6
                       libssl-dev \
                       libsqlite3-dev sqlite3 bzip2 libbz2-dev \
                       libreadline-dev libncurses5-dev libssl1.0.0 \
                       tk8.5-dev zlib1g-dev liblzma-dev
#                       python3-numpy python3-dev python3-pip python3-wheel
##                       libcupti-dev
RUN apt-get build-dep -y python3


RUN adduser --gecos "" --disabled-password python
USER python:python
RUN cd /home/python/ && \
    wget https://www.python.org/ftp/python/3.6.0/Python-3.6.0.tgz && \
    echo "3f7062ccf8be76491884d0e47ac8b251  Python-3.6.0.tgz" | md5sum -c --strict - && \
    tar xvf Python-3.6.0.tgz && \
    cd /home/python/Python-3.6.0 && \
    # Optimizations results in failing to build lots of modules atm
    #./configure --enable-optimizations --prefix=/home/python/python36 && \
    ./configure --prefix=/home/python/python36 && \
    make -j8 install

USER root:root
