FROM rai/cuda_ppc64le:devel-8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

ENV CUDNN_VERSION 6
LABEL com.nvidia.cudnn.version="6"

RUN apt-get update && apt-get install -y \
        curl && \
    rm -rf /var/lib/apt/lists/*
    
RUN curl -fsSL http://developer.download.nvidia.com/compute/redist/cudnn/v6.0/cudnn-8.0-linux-ppc64le-v6.0.tgz -O && \
    tar -xzf cudnn-8.0-linux-ppc64le-v6.0.tgz -C /usr/local && \
    rm cudnn-8.0-linux-ppc64le-v6.0.tgz && \
    ldconfig