FROM rai/nccl:8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

WORKDIR /usr/local
RUN git clone https://github.com/cuMF/cumf_als && \
    git clone https://github.com/cuMF/cumf_sgd && \
    git clone https://github.com/cuMF/cumf_ccd && \
    cd cumf_sgd && \
    cd data/netflix && \
    curl -fsSL http://www.select.cs.cmu.edu/code/graphlab/datasets/netflix_mme -O && \
    curl -fsSL http://www.select.cs.cmu.edu/code/graphlab/datasets/netflix_mm -O && \
    prepare.sh && \
    cd ../.. && \
    make