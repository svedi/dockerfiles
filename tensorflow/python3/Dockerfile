FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget virtualenv \
                       python3-pip python3-dev python3-virtualenv
RUN adduser --gecos "" --disabled-password tf
USER tf:tf
RUN cd /home/tf && \
    virtualenv --python=python3 --system-site-packages tf
USER root:root

# It remains to copy the tensorflow wheel to the image and
# run as tf:
#   source tf/bin/activate
#   pip3 install -upgrade /tmp/tensorflow.whl
