FROM rai/cuda_ppc64le:cudnn-8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

ENV CONDA_DIR /opt/conda
ENV PATH $CONDA_DIR/bin:$PATH

RUN mkdir -p $CONDA_DIR && \
    echo export PATH=$CONDA_DIR/bin:'$PATH' > /etc/profile.d/conda.sh && \
    apt-get update && \
    apt-get install -y wget git libhdf5-dev g++ graphviz && \
    wget --quiet https://repo.continuum.io/miniconda/Miniconda3-4.3.14-Linux-ppc64le.sh && \
    /bin/bash Miniconda3-4.3.14-Linux-ppc64le.sh -f -b -p $CONDA_DIR && \
    rm Miniconda3-4.3.14-Linux-ppc64le.sh