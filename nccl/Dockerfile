FROM rai/cuda_ppc64le:devel-8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

WORKDIR /usr/local
RUN git clone https://github.com/NVIDIA/nccl && \
    cd nccl && \
    make