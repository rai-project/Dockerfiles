FROM ppc64le/debian:jessie
MAINTAINER James Phillips <james@hashicorp.com> (@slackpad)

# Build-time metadata as defined at http://label-schema.org
ARG BUILD_DATE
ARG VCS_REF
ARG VCS_URL
ARG VERSION
ARG ARCH
ARG CONSUL_VERSION
LABEL org.carml.consul.build-date=$BUILD_DATE \
      org.carml.consul.name="consul" \
      org.carml.consul.description="" \
      org.carml.consul.url="https://www.carml.org/" \
      org.carml.consul.vcs-ref=$VCS_REF \
      org.carml.consul.vcs-url=$VCS_URL \
      org.carml.consul.vendor="CarML" \
      org.carml.consul.arch=$ARCH \
      org.carml.consul.version=$VERSION \
      org.carml.consul.consul_version=$CONSUL_VERSION \
      org.carml.consul.schema-version="1.0"


# This is the release of Consul to pull in.
ENV CONSUL_VERSION=$CONSUL_VERSION

# This is the release of https://github.com/hashicorp/docker-base to pull in order
# to provide HashiCorp-built versions of basic utilities like dumb-init and gosu.
ENV DOCKER_BASE_VERSION=0.0.4

# This is the location of the releases.
ENV HASHICORP_RELEASES=https://releases.hashicorp.com

RUN addgroup consul && \
    useradd -g consul consul && \
    apt-get update && apt-get install -y ca-certificates curl gnupg libcap-dev openssl git make gcc wget && \
    mkdir -p /tmp/build && \
    cd /tmp/build && \
     git clone https://github.com/Yelp/dumb-init && cd dumb-init && make && cp dumb-init /bin && \
     wget https://github.com/tianon/gosu/releases/download/1.10/gosu-ppc64el && cp gosu-ppc64el /bin/gosu && chmod +x /bin/gosu 

#Install Go
ENV GIMME_GO_VERSION "1.8.3"
ENV GIMME_OS "linux"
ENV GIMME_ARCH $ARCH

LABEL org.golang.version=$GIMME_GO_VERSION
LABEL org.golang.os=$GIMME_OS
LABEL org.golang.arch=$GIMME_ARCH

ADD https://raw.githubusercontent.com/travis-ci/gimme/master/gimme /usr/bin/gimme
RUN chmod +x /usr/bin/gimme
RUN gimme

ENV GOROOT "/root/.gimme/versions/go${GIMME_GO_VERSION}.${GIMME_OS}.${GIMME_ARCH}"
ENV PATH ${GOROOT}/bin:${PATH}

ENV GOPATH "/go"
ENV PATH $GOPATH/bin:$PATH

RUN which go && \
    mkdir -p $GOPATH/src/github.com/hashicorp && cd $GOPATH/src/github.com/hashicorp && \
    git clone https://github.com/hashicorp/consul.git && \
    cd consul && \
    go get github.com/mitchellh/gox && \
    gox -os=linux -arch=ppc64le -output=ppc64le/consul && cp ppc64le/consul /bin && chmod +x /bin/consul && \
    mkdir -p /consul/data && \
    mkdir -p /consul/config && \
    chown -R consul:consul /consul && \
    apt-get purge git wget make gcc curl --force-yes -y && apt-get autoremove -y && \
    rm -rf /tmp/build && rm -rf /root/.gnupg 

VOLUME /consul/data

# request forwarding.
EXPOSE 8300
EXPOSE 8301 8301/udp 8302 8302/udp
EXPOSE 8400 8500 8600 8600/udp
COPY docker-entrypoint.sh /usr/local/bin/docker-entrypoint.sh
ENTRYPOINT ["docker-entrypoint.sh"]

CMD ["agent", "-dev", "-client", "0.0.0.0"]

