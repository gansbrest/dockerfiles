# Use phusion/baseimage as base image. To make your builds reproducible, make
# sure you lock down to a specific version, not to `latest`!
# See https://github.com/phusion/baseimage-docker/blob/master/Changelog.md for
# a list of version numbers.
FROM phusion/baseimage:0.9.16

# Use baseimage-docker's init system.
CMD ["/sbin/my_init"]

# Add libraries directory
ADD ./lib /opt

##########################
## INSTALL DEPENDENCIES ##
##########################

# Install packages
RUN DEBIAN_FRONTEND=noninteractive \
 && apt-get update \
 && apt-get upgrade -y \
 && apt-get install -y \
  autoconf \
  build-essential \
  imagemagick \
  libgd3 \
  libgd-dev \
  mcrypt \
  libmcrypt-dev \
  libbz2-dev \
  libcurl4-openssl-dev \
  libevent-dev \
  libffi-dev \
  libfreetype6-dev \
  libglib2.0-dev \
  libjpeg-dev \
  libmagickcore-dev \
  libmagickwand-dev \
  libmysqlclient-dev \
  libncurses-dev \
  libpq-dev \
  libreadline-dev \
  libsqlite3-dev \
  libssl-dev \
  libxml2-dev \
  libxslt-dev \
  libyaml-dev \
  zlib1g-dev

#RUN apt-get install -y mysql-client-5.6

RUN \
  mkdir -p /usr/local/include/freetype2/freetype && \
  ln -s /usr/include/freetype2/freetype.h /usr/local/include/freetype2/freetype/freetype.h

# Build and install PHP
RUN \
  cd /opt && tar -xvf php-5.3.10.tar.gz && cd php-5.3.10 && \
  ./configure --enable-fpm --with-mysql=mysqlnd --with-mysqli=mysqlnd --with-zlib --with-jpeg-dir --with-gd --with-freetype-dir --with-curl --with-openssl --with-pdo-mysql --with-mcrypt && make clean && make && make install && cp sapi/fpm/php-fpm /usr/local/bin

# Clean up APT when done.
RUN apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
