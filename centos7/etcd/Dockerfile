FROM ghcr.io/mogublog/centos:7.9.2009
RUN mkdir -p /usr/local/etcd/{bin,conf} && export ETCD_VERSION=v3.3.26 \
  && curl -L https://github.com/coreos/etcd/releases/download/${ETCD_VERSION}/etcd-${ETCD_VERSION}-linux-amd64.tar.gz | tar -zxC /usr/local/etcd/bin/ --strip-components=1
ENV PATH /usr/local/etcd/bin/:$PATH
CMD ["/usr/local/etcd/bin/etcd"]
