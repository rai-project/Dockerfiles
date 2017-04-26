FROM ppc64le/debian:jessie
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>

RUN apt-get update && apt-get install -y --no-install-recommends \
    git 

WORKDIR /tmp

RUN git clone https://github.com/firehol/netdata && \
    cd netdata && \
    chmod +x ./docker-build.sh && \
    sync && \
    sleep 1 && \
    ./docker-build.sh

WORKDIR /

ENV NETDATA_PORT 19999
EXPOSE $NETDATA_PORT

CMD /usr/sbin/netdata -D -s /host -p ${NETDATA_PORT}