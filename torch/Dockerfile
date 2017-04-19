FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget \
                       # Extracted from install-deps, commit 4eb67c55261d902f1abe84d91770c7c8f81d8a34
                       python-software-properties \
                       software-properties-common \
                       libgraphicsmagick1-dev libfftw3-dev sox libsox-dev \
                       libsox-fmt-all \
                       build-essential gcc g++ curl \
                       cmake libreadline-dev git-core libqt4-dev libjpeg-dev \
                       libpng-dev ncurses-dev imagemagick libzmq3-dev gfortran \
                       unzip gnuplot gnuplot-x11 ipython \
                       gcc-4.9 libgfortran-4.9-dev g++-4.9 \
                       # Taken from caffe install instructions, skipping openblas for now
                       libatlas-base-dev \
                       # For iTorch
                       ipython-notebook python-zmq libssl-dev
RUN adduser --gecos "" --disabled-password torch
USER torch:torch
RUN mkdir /home/torch/src && \
    cd /home/torch/src && \
    git clone https://github.com/torch/distro.git ~/src/torch --recursive && \
    cd /home/torch/src/torch && \
    export PREFIX="/home/torch/torch" && \
    mkdir "$PREFIX" && \
    ./install.sh -b && \
    cd /home/torch/src && \
    git clone https://github.com/facebook/iTorch.git && \
    cd /home/torch/src/iTorch && \
    "$PREFIX/bin/luarocks" make
USER root:root
