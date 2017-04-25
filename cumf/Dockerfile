FROM rai/nccl:8.0
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

WORKDIR /usr/local
RUN git clone https://github.com/cuMF/cumf_als && \
    git clone https://github.com/cuMF/cumf_sgd && \
    git clone https://github.com/cuMF/cumf_ccd

WORKDIR /usr/local/cumf_sgd/data/netflix/
ADD http://www.select.cs.cmu.edu/code/graphlab/datasets/netflix_mme .
ADD http://www.select.cs.cmu.edu/code/graphlab/datasets/netflix_mm .

RUN cd /usr/local/cumf_sgd/data/netflix && \
    make transform && \
    ./transform netflix_mme && \
    ./transform netflix_mm
    