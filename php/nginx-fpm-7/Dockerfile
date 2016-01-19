FROM       ubuntu:14.04
MAINTAINER Sergey Khaladzinski <shanderlam@gmail.com>

RUN apt-get update && apt-get -y install software-properties-common \
    && DEBIAN_FRONTEND=noninteractive add-apt-repository -y ppa:ondrej/php-7.0 \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C

RUN apt-get update \
    && apt-get install -y \
    make \
    php-pear \
    php7.0-cli \
    php7.0-dev \
    php7.0-common \
    php7.0-curl \
    php7.0-fpm \
    php7.0-gd \
    php7.0-mysql \
    php7.0-xmlrpc \
    nginx \
    supervisor \
    vim \
    net-tools \
    php-xdebug

# install phpredis extension
ENV PHPREDIS_VERSION php7

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && cd /tmp \
    && tar xfz /tmp/redis.tar.gz \
    && cd /tmp/phpredis-$PHPREDIS_VERSION \
    && phpize \
    && ./configure \
    && make && make install \
    && rm -rf /tmp/* \
    && echo "extension=redis.so" > /etc/php/mods-available/redis.ini

# Configure locales
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

RUN sed -i -e"s/^;\s*daemonize\s*=\s*yes/daemonize = no/" /etc/php/7.0/fpm/php-fpm.conf
RUN sed -i -e"s/^error_log\s*=\s*.*$/error_log = \/proc\/self\/fd\/2/" /etc/php/7.0/fpm/php-fpm.conf
RUN sed -i -e"s/^pid = \/run\/php\/php7.0-fpm.pid/pid = \/run\/php7.0-fpm.pid/" /etc/php/7.0/fpm/php-fpm.conf
RUN sed -i -e"s/^listen\s*=\s*.*$/listen = 12001/" /etc/php/7.0/fpm/pool.d/www.conf

RUN rm /etc/nginx/sites-available/default
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN \
  echo "security.limit_extensions = " >> /etc/php/7.0/fpm/pool.d/www.conf && \
  echo "catch_workers_output = yes" >> /etc/php/7.0/fpm/pool.d/www.conf && \
  echo "env[XDEBUG_CONFIG] = \$XDEBUG_CONFIG" >> /etc/php/7.0/fpm/pool.d/www.conf && \
  echo "error_reporting=30711" >> /etc/php/7.0/fpm/php.ini && \
  echo "error_reporting=30711" >> /etc/php/7.0/cli/php.ini && \
  echo "memory_limit=512M" >> /etc/php/7.0/fpm/php.ini && \
  echo "memory_limit=512M" >> /etc/php/7.0/cli/php.ini

COPY conf/php/xdebug.ini /etc/php/mods-available/
COPY conf/nginx /etc/nginx/sites-enabled/
COPY conf/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN phpenmod -v 7.0 xdebug redis

EXPOSE 9999

CMD ["/usr/bin/supervisord"]
