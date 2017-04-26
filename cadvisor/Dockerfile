FROM ppc64le/golang:1.8.1
MAINTAINER Abdul Dakkak <dakkak@illinois.edu>


RUN apt-get install -y git 
RUN git clone https://github.com/google/cadvisor.git $GOPATH/src/github.com/google/cadvisor
RUN cd $GOPATH/src/github.com/google/cadvisor && make VERBOSE=1 build

RUN mv $GOPATH/src/github.com/google/cadvisor/cadvisor /usr/bin/cadvisor

ENTRYPOINT ["/usr/bin/cadvisor"]