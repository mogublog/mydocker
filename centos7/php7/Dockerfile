FROM ghcr.io/mogublog/centos:7.9.2009 AS build
WORKDIR /usr/local
RUN yum install -y gcc gcc-c++ make perl autoconf automake libtool openssl-devel curl-devel libxml2-devel libjpeg-devel libpng-devel readline-devel freetype-devel zlib-devel libicu-devel sqlite-devel

RUN export VERSION=1.3.2 \
  && curl -O https://libzip.org/download/libzip-${VERSION}.tar.gz \
  && tar -zxvf libzip-${VERSION}.tar.gz \
  && cd libzip-${VERSION} \
  && ./configure \
  && make \
  && make install

RUN export VERSION=6.9.4 \
  && curl -o oniguruma-${VERSION}.tar.gz -L https://github.com/kkos/oniguruma/archive/v${VERSION}.tar.gz \
  && tar -xvf oniguruma-${VERSION}.tar.gz \
  && cd oniguruma-${VERSION} \
  && ./autogen.sh \
  && ./configure \
  && make \
  && make install

RUN export VERSION=7.4.24 \
  && export PKG_CONFIG_PATH=PKG_CONFIG_PATH:"/usr/local/lib/pkgconfig/" \
  && curl -O https://www.php.net/distributions/php-${VERSION}.tar.gz \
  && tar -zxf php-${VERSION}.tar.gz \
  && cd php-${VERSION} \
  && ./configure --prefix=/usr/local/php \
  --with-curl --with-freetype --with-gettext --with-iconv-dir --with-jpeg --with-openssl --with-pear --with-readline --with-xmlrpc --with-zip --with-zlib \
  --with-mysqli=mysqlnd --with-pdo-mysql=mysqlnd \
  --enable-bcmath --enable-calendar --enable-exif --enable-embed --enable-fpm --enable-ftp --enable-gd --enable-intl --enable-mbstring --enable-mysqlnd --enable-pcntl \
  --enable-soap --enable-sockets --disable-fileinfo \
  && make \
  && make install

RUN cd /usr/local/php && strip bin/php && strip bin/php-cgi && strip bin/phpdbg && strip sbin/php-fpm && strip lib/libphp7.so

FROM ghcr.io/mogublog/centos:7.9.2009
WORKDIR /usr/local/
RUN yum install -y autoconf automake openssl curl libxml2 libpng libjpeg readline freetype zlib libicu && yum clean all
COPY --from=build /usr/local/php php
COPY --from=build /usr/local/lib lib
ENV PATH /usr/local/php/bin/:$PATH
