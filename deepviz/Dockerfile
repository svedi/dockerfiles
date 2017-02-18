FROM nvidia/cuda:8.0-cudnn5-devel-ubuntu16.04

RUN sed -e s:archive.ubuntu.com:se.archive.ubuntu.com:g -i /etc/apt/sources.list
RUN apt-get update
RUN apt-get upgrade -y
RUN apt-get install -y build-essential git ca-certificates vim

RUN apt-get install -y libprotobuf-dev libleveldb-dev libsnappy-dev \
                       libopencv-dev libhdf5-serial-dev protobuf-compiler \
                       libatlas-base-dev libgflags-dev libgoogle-glog-dev liblmdb-dev \
                       python-dev python-numpy \
                       python-opencv python-scipy python-skimage wget
RUN apt-get install -y --no-install-recommends libboost-all-dev
RUN adduser --gecos "" --disabled-password caffe
USER caffe:caffe
RUN cd /home/caffe && \
    git clone https://github.com/BVLC/caffe && \
    cd /home/caffe/caffe && \
    git remote add yosinski https://github.com/yosinski/caffe.git && \
    git fetch --all && \
    git checkout --track -b deconv-deep-vis-toolbox yosinski/deconv-deep-vis-toolbox && \
    cp Makefile.config.example Makefile.config && \
    sed -e 's|^# USE_CUDNN := 1|USE_CUDNN := 1|' -i Makefile.config && \
    echo "LIBRARIES += hdf5_serial hdf5_serial_hl" >> Makefile && \
    echo "INCLUDE_DIRS := \$(PYTHON_INCLUDE) /usr/local/include /usr/include/hdf5/serial/" >> Makefile.config && \
    echo "LIBRARY_DIRS := \$(PYTHON_LIB) /usr/local/lib /usr/lib /usr/lib/x86_64-linux-gnu/hdf5/serial/" >> Makefile.config && \
    make clean && \
    make all -j8 && \
    make pycaffe -j8 && \
    make test -j8 && \
    cd /home/caffe && \
    git clone https://github.com/yosinski/deep-visualization-toolbox && \
    cd /home/caffe/deep-visualization-toolbox && \
    cp models/caffenet-yos/settings_local.template-caffenet-yos.py settings_local.py && \
    sed -e s:/path/to/caffe:/home/caffe/caffe: -i settings_local.py && \
    cd /home/caffe/deep-visualization-toolbox/models/caffenet-yos && \
    ./fetch.sh

# Check LD_LIBRARY_PATH for root in order to run the toolbox as a normal
# user. Might have to add cuda libraries too it aswell.

USER root:root
