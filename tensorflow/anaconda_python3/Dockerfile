FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget virtualenv \
                       python3-pip python3-dev python3-virtualenv
RUN adduser --gecos "" --disabled-password tf
USER tf:tf
RUN cd /home/tf && \
#    Anaconda 4.3.0 uses python 3.6, and it's currently not easy to get a
#    tensorflow wheel that works with this python version
    wget https://repo.continuum.io/archive/Anaconda3-4.3.0-Linux-x86_64.sh && \
    echo "e9169c3a5029aa820393ac92704eb9ee0701778a085ca7bdc3c57b388ac1beb6  Anaconda3-4.3.0-Linux-x86_64.sh" | sha256sum -c --strict - && \
    bash ./Anaconda3-4.3.0-Linux-x86_64.sh -b && \
#    wget https://repo.continuum.io/archive/Anaconda3-4.2.0-Linux-x86_64.sh && \
#    echo "73b51715a12b6382dd4df3dd1905b531bd6792d4aa7273b2377a0436d45f0e78  Anaconda3-4.2.0-Linux-x86_64.sh" | sha256sum -c --strict - && \
#    bash ./Anaconda3-4.2.0-Linux-x86_64.sh -b && \
    PATH="/home/tf/anaconda3/bin:$PATH" && \
    conda create -n tensorflow && \
    # Cant use the anaconda libstd++ with tensorflow, so remove the bundled and fall back
    # on system ones. Seems to work :-)
    rm /home/tf/anaconda3/lib/libstd*

USER root:root

# It remains to copy the tensorflow wheel to the image and
# run as tf:
#   source activate tensorflow
#   pip3 install --ignore-installed --upgrade /tmp/tensorflow.whl
