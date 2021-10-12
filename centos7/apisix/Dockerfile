FROM ghcr.io/mogublog/centos:7.9.2009 AS FIRST_PRE
RUN yum update -y && yum install -y gcc make perl zlib-devel openssl-devel pcre-devel
RUN export VERSION=1.19.9.1 \
  && curl -O https://openresty.org/download/openresty-${VERSION}.tar.gz \
  && tar -xvf openresty-${VERSION}.tar.gz \
  && cd openresty-${VERSION} \
  && ./configure && make && make install

RUN export VERSION=2.10.0 \
  && cd /tmp && mkdir /usr/local/apisix \
  && curl -O https://downloads.apache.org/apisix/${VERSION}/apache-apisix-${VERSION}-src.tgz \
  && tar -xvf apache-apisix-${VERSION}-src.tgz -C /usr/local/apisix/

FROM ghcr.io/mogublog/centos:7.9.2009
COPY --from=FIRST_PRE /usr/local/openresty /usr/local/
COPY --from=FIRST_PRE /usr/local/apisix /usr/local/
ENV PATH /usr/local/openresty/bin/:/usr/local/apisix/bin/:$PATH
