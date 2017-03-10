FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget openjdk-8-jdk \
                       python3-numpy python3-dev python3-pip python3-wheel
#                       libcupti-dev

RUN adduser --gecos "" --disabled-password tf
USER tf:tf
RUN cd /home/tf/ && \
    mkdir src && \
    cd /home/tf/src && \
    wget https://github.com/bazelbuild/bazel/releases/download/0.4.4/bazel-0.4.4-dist.zip && \
    mkdir bazel-0.4.4-dist && \
    cd /home/tf/src/bazel-0.4.4-dist && \
    unzip ../bazel-0.4.4-dist.zip && \
    bash ./compile.sh && \
    cd /home/tf/src && \
    git clone https://github.com/tensorflow/tensorflow && \
    cd /home/tf/src/tensorflow && \
    git checkout r1.0
USER root:root
RUN apt-get install -y python

# The rest of this script does not work very well for some reason, atleast
# not during building of the image, however, the commands work when running
# the image. Perhaps because nvidia-docker doesn't set up all the links etc
# during build?
#
#USER tf:tf
#RUN mkdir /home/tf/bin && \
#    cp /home/tf/src/bazel-0.4.4-dist/output/bazel /home/tf/bin/ && \
#    cd /home/tf/src/tensorflow && \
#    export PATH="/home/tf/bin/:$PATH" && \
#    ./configure && \
#    bazel build --config=opt --config=cuda //tensorflow/tools/pip_package:build_pip_package && \
#    bazel-bin/tensorflow/tools/pip_package/build_pip_package /tmp/tensorflow_pkg
