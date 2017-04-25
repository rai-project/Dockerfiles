FROM rai/cuda_ppc64le:devel-8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

########## REQUIRED DEPENDENCIES ################
RUN apt-get update && apt-get install -y --no-install-recommends \
    git ca-certificates 

ENV NCCL_ROOT=/opt/nccl
WORKDIR $NCCL_ROOT

# Install NCCL for multi-GPU communication
RUN git clone https://github.com/NVIDIA/nccl.git $NCCL_ROOT && \
    cd $NCCL_ROOT && \
    make CUDA_HOME=/usr/local/cuda -j"$(nproc)" && \
    make PREFIX=$NCCL_ROOT install
ENV LD_LIBRARY_PATH=$NCCL_ROOT/lib:$LD_LIBRARY_PATH
