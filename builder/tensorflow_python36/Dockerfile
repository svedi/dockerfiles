FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim \
                       zip unzip wget openjdk-8-jdk \
                       # Some of the following are needed for building python 3.6
                       libssl-dev \
                       libsqlite3-dev sqlite3 bzip2 libbz2-dev \
                       libreadline-dev libncurses5-dev libssl1.0.0 \
                       tk8.5-dev zlib1g-dev liblzma-dev
#                       python3-numpy python3-dev python3-pip python3-wheel
##                       libcupti-dev


RUN adduser --gecos "" --disabled-password tf
USER tf:tf
RUN cd /home/tf/ && \
    wget https://www.python.org/ftp/python/3.6.1/Python-3.6.1.tgz && \
    echo "2d0fc9f3a5940707590e07f03ecb08b9  Python-3.6.1.tgz" | md5sum -c --strict - && \
    tar xvf Python-3.6.1.tgz && \
    cd /home/tf/Python-3.6.1 && \
    # Optimizations results in failing to build lots of modules atm
    #./configure --enable-optimizations --prefix=/home/tf/python36 && \
    ./configure --prefix=/home/tf/python36 && \
    make -j8 install
RUN cd /home/tf/ && \
    mkdir src && \
    cd src && \
    wget https://github.com/bazelbuild/bazel/releases/download/0.4.5/bazel-0.4.5-dist.zip && \
    mkdir bazel-0.4.5-dist && \
    cd bazel-0.4.5-dist && \
    unzip ../bazel-0.4.5-dist.zip && \
    bash ./compile.sh && \
    cd /home/tf/src && \
    git clone https://github.com/tensorflow/tensorflow
#    cd /home/tf/src/tensorflow && \
#    git checkout r1.1-rc1
#   pip3.6 install numpy wheel

USER root:root

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
