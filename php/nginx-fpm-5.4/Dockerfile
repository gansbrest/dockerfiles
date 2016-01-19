FROM       ubuntu:12.04
MAINTAINER Sergey Khaladzinski <shanderlam@gmail.com>

RUN apt-get update && apt-get -y install python-software-properties \
    && DEBIAN_FRONTEND=noninteractive add-apt-repository ppa:ondrej/php5-oldstable

RUN apt-get update \
    && apt-get install -y \
    make \
    php-apc \
    php-pear \
    php5-cli \
    php5-dev \
    php5-common \
    php5-curl \
    php5-fpm \
    php5-gd \
    php5-mysql \
    php5-xsl \
    php5-xmlrpc \
    nginx \
    supervisor \
    vim \
    net-tools \
    drush

# Configure locales
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

# Pecl extensions
RUN pecl install xdebug-2.2.6

# Install phpredis client
RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  echo "extension=redis.so" > /etc/php5/conf.d/redis.ini

RUN sed -i -e"s/^;\s*daemonize\s*=\s*yes/daemonize = no/" /etc/php5/fpm/php-fpm.conf
RUN sed -i -e"s/^error_log\s*=\s*.*$/error_log = \/proc\/self\/fd\/2/" /etc/php5/fpm/php-fpm.conf
RUN sed -i -e"s/^listen\s*=\s*.*$/listen = 12001/" /etc/php5/fpm/pool.d/www.conf

RUN rm /etc/nginx/sites-available/default
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN \
  echo "security.limit_extensions = " >> /etc/php5/fpm/pool.d/www.conf && \
  echo "catch_workers_output = yes" >> /etc/php5/fpm/pool.d/www.conf && \
  echo "env[XDEBUG_CONFIG] = \$XDEBUG_CONFIG" >> /etc/php5/fpm/pool.d/www.conf && \
  echo "error_reporting=30711" >> /etc/php5/fpm/php.ini && \
  echo "error_reporting=30711" >> /etc/php5/cli/php.ini && \
  echo "memory_limit=512M" >> /etc/php5/fpm/php.ini && \
  echo "memory_limit=512M" >> /etc/php5/cli/php.ini

COPY conf/php/xdebug.ini /etc/php5/conf.d/
COPY conf/nginx /etc/nginx/sites-enabled/
COPY conf/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 9999

CMD ["/usr/bin/supervisord"]
