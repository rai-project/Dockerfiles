FROM rai/cuda_ppc64le:devel-8.0
MAINTAINER Carl Pearson <pearson@illinois.edu>

RUN apt-get update && apt-get install -y --no-install-recommends \
  wget \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*

ENV MLDL_URL "https://public.dhe.ibm.com/software/server/POWER/Linux/mldl/ubuntu"
ENV MLDL_DEB "mldl-repo-local_3.4.2_ppc64el.deb"

RUN wget --no-check-certificate -q "$MLDL_URL/$MLDL_DEB" \
    && dpkg -i $MLDL_DEB \
    && rm $MLDL_DEB

RUN curl -fsSL "http://developer.download.nvidia.com/compute/redist/cudnn/v5.1/libcudnn5-dev_5.1.10-1%2Bcuda8.0_ppc64el.deb" \
    && dpkg -i "libcudnn5-dev_5.1.10-1+cuda8.0_ppc64el.deb" 

RUN apt-get update && apt-get install -y --no-install-recommends \
  caffe-bvlc \
  && apt-get clean \
  && rm -rf /var/lib/apt/lists/*
