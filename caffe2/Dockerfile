FROM rai/cuda_ppc64le:cudnn-8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

# caffe2 install with gpu support

########## REQUIRED DEPENDENCIES ################
RUN apt-get update && apt-get install -y --no-install-recommends \
    python-dev \
    python-pip \
    git \
    build-essential \
    cmake \
    libprotobuf-dev \
    protobuf-compiler \
    libgoogle-glog-dev

########## OPTIONAL DEPENDENCIES ################

RUN apt-get install -y --no-install-recommends \
    libgflags-dev \
    libgoogle-glog-dev \
    libgflags-dev \
    libgtest-dev \
    libiomp-dev \
    libleveldb-dev \
    liblmdb-dev \
    libopencv-dev \
    libopenmpi-dev \
    libsnappy-dev \
    openmpi-bin \
    openmpi-doc \
    python-numpy \
    python-pydot \
    python-setuptools \
    python-scipy \
    wget

RUN pip install -U setuptools


########## INSTALLATION STEPS ###################

WORKDIR /tmp

RUN git clone --single-branch --depth 1 --recursive https://github.com/caffe2/caffe2.git

WORKDIR /tmp/caffe2

RUN mkdir build && \
    cd build && \
    cmake .. $(python ../scripts/get_python_cmake_flags.py) -DBUILD_TEST=OFF && \
    make -j $(nproc) && \
    make install


ENV PYTHONPATH /usr/local
ENV LD_LIBRARY_PATH /usr/local/lib:$LD_LIBRARY_PATH
ENV LIBRARY_PATH /usr/local/lib:$LD_LIBRARY_PATH
RUN python -c 'from caffe2.python import core' 2>/dev/null && echo "Success" || echo "Failure"

########## TROUBLESHOOTING ######################
# this is added due to some issues that were found

# libdc1394 error
# for some reason once opencv is installed you may get errors with libdc1394
# http://stackoverflow.com/questions/12689304/ctypes-error-libdc1394-error-failed-to-initialize-libdc1394
# RUN ln /dev/null /dev/raw1394
# But that's not persistent http://stackoverflow.com/questions/31768441/how-to-persist-ln-in-docker-with-ubuntu
# CMD sh -c 'ln -s /dev/null /dev/raw1394'
# or when instantiating the container use:
# --device /dev/null:/dev/raw1394