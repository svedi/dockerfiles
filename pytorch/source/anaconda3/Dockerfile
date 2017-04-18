FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget virtualenv \
                       python3-pip python3-dev python3-virtualenv
RUN adduser --gecos "" --disabled-password pytorch
USER pytorch:pytorch
RUN cd /home/pytorch && \
    wget https://repo.continuum.io/archive/Anaconda3-4.3.1-Linux-x86_64.sh && \
    echo "4447b93d2c779201e5fb50cfc45de0ec96c3804e7ad0fe201ab6b99f73e90302  Anaconda3-4.3.1-Linux-x86_64.sh" | sha256sum -c --strict - && \
    bash ./Anaconda3-4.3.1-Linux-x86_64.sh -b && \
    # Cant use the anaconda libstd++ with tensorflow, so remove the bundled and fall back
    # on system ones. Seems to work :-)
    rm /home/pytorch/anaconda3/lib/libstd* && \
    rm /home/pytorch/Anaconda3-4.3.1-Linux-x86_64.sh
    # Install pytorch
RUN cd /home/pytorch && \
    export PATH="/home/pytorch/anaconda3/bin:$PATH" && \
    conda create -n pytorch && \
    git clone https://github.com/pytorch/pytorch.git && \
    export CMAKE_PREFIX_PATH="/home/pytorch/anaconda3" && \
    conda install numpy pyyaml mkl setuptools cmake gcc cffi && \
    conda install -c soumith magma-cuda80 && \
    cd "/home/pytorch/pytorch" && \
    python setup.py install

USER root:root
