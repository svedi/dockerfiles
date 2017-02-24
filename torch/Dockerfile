FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:de.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget

# Extracted from install-deps
RUN apt-get install -y python-software-properties
RUN apt-get install -y software-properties-common \
                libgraphicsmagick1-dev libfftw3-dev sox libsox-dev \
                libsox-fmt-all
RUN apt-get install -y build-essential gcc g++ curl \
            cmake libreadline-dev git-core libqt4-dev libjpeg-dev \
            libpng-dev ncurses-dev imagemagick libzmq3-dev gfortran \
            unzip gnuplot gnuplot-x11 ipython
RUN apt-get install -y gcc-4.9 libgfortran-4.9-dev g++-4.9
# Taken from caffe install instructions, skipping openblas for now
RUN apt-get install -y libatlas-base-dev

RUN adduser --gecos "" --disabled-password torch
USER torch:torch
RUN cd /home/torch && \
    git clone https://github.com/torch/distro.git ~/torch --recursive && \
    cd /home/torch/torch && \
    #bash install-deps && \
    ./install.sh -b
USER root:root
