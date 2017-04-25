FROM rai/cuda_ppc64le:cudnn-8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

# install the core library
RUN apt-get update && apt-get install -y build-essential git libopenblas-dev libopencv-dev

WORKDIR /

RUN git clone --recursive https://github.com/dmlc/mxnet && cd mxnet && \ 
    cp make/config.mk . && \
    echo "USE_CUDA=1" >>config.mk && \
    echo "USE_CUDA_PATH=/usr/local/cuda" >>config.mk && \
    echo "USE_CUDNN=1" >>config.mk && \
    echo "USE_BLAS=openblas" >>config.mk && \
    make -j$(nproc) ADD_LDFLAGS=-L/usr/local/cuda/lib64/stubs
ENV LD_LIBRARY_PATH /usr/local/cuda/lib64:$LD_LIBRARY_PATH
