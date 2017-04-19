FROM nvidia/cuda:8.0-cudnn6-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim

RUN apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev \
                       libopencv-dev libhdf5-serial-dev protobuf-compiler \
                       libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev \
                       python-dev python-numpy
RUN apt-get install -y --no-install-recommends libboost-all-dev
RUN adduser --gecos "" --disabled-password caffe
USER caffe:caffe
RUN cd /home/caffe && \
    git clone https://github.com/BVLC/caffe
RUN cd /home/caffe/caffe && \
    cp Makefile.config.example Makefile.config && \
    sed -e 's|^# USE_CUDNN := 1|USE_CUDNN := 1|' -i Makefile.config && \
    echo "LIBRARIES += hdf5_serial hdf5_serial_hl" >> Makefile && \
    echo "INCLUDE_DIRS := \$(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/" >> Makefile.config && \
    echo "LIBRARY_DIRS := \$(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config && \
    make clean && \
    make all -j8 && \
    make pycaffe -j8 && \
    make test -j8
# Tests dont work when building since nvidia-docker doesnt set up the
# neccessary files during build. Has to be run in the image instead.
#RUN cd /home/caffe/caffe && \
#    make runtest

USER root:root
