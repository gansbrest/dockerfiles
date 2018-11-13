FROM       ubuntu:14.04
MAINTAINER Sergey Khaladzinski <shanderlam@gmail.com>

RUN apt-get update && apt-get -y upgrade && apt-get -y install software-properties-common \
    && DEBIAN_FRONTEND=noninteractive add-apt-repository -y ppa:ondrej/php \
    && apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C

RUN apt-get update \
    && apt-get install -y \
    make \
    php-pear \
    php7.2-cli \
    php7.2-dev \
    php7.2-common \
    php7.2-curl \
    php7.2-fpm \
    php7.2-gd \
    php7.2-mysql \
    php7.2-xmlrpc \
    php7.2-dom \
    gcc \
    make \
    autoconf \
    libc-dev \
    pkg-config \
    libmcrypt-dev \
    nginx \
    supervisor \
    vim \
    net-tools \
    mc \
    php-xdebug

RUN pecl install mcrypt-1.0.1

# install phpredis extension
ENV PHPREDIS_VERSION 3.1.6

RUN curl -L -o /tmp/redis.tar.gz https://github.com/phpredis/phpredis/archive/$PHPREDIS_VERSION.tar.gz \
    && cd /tmp \
    && tar xfz /tmp/redis.tar.gz \
    && cd /tmp/phpredis-$PHPREDIS_VERSION \
    && phpize \
    && ./configure \
    && make && make install \
    && rm -rf /tmp/* \
    && echo "extension=redis.so" > /etc/php/7.2/mods-available/redis.ini

#install drush 6
ENV DRUSH_VERSION 6.6.0

RUN curl -L -o /tmp/drush.tar.gz https://github.com/drush-ops/drush/archive/$DRUSH_VERSION.tar.gz \
    && cd /tmp \
    && tar -xvzf /tmp/drush.tar.gz \
    && mkdir -p /drush \
    && mv /tmp/drush-$DRUSH_VERSION/* /drush \
    && sed '12irequire("/app/vendor/autoload.php");' /drush/drush.php > /drush/drush2.php \
    && rm /drush/drush.php && mv /drush/drush2.php /drush/drush.php \
    && ln -s /drush/drush /bin/drush \
    && rm -rf /tmp/* \
    && mkdir -p /root/.drush \
    && echo "<?php \$aliases['local'] = array('root' => '/app', 'uri'  => 'mirtrik.local', 'path-aliases' => array( '%dump-dir' => '/tmp', ), );" > /root/.drush/mirtrik.aliases.drushrc.php

#install phpunit
ENV PHPUNIT_VERSION 6.5

RUN curl -L -o /tmp/phpunit.phar https://phar.phpunit.de/phpunit-$PHPUNIT_VERSION.phar \
    && chmod +x /tmp/phpunit.phar \
    && mv /tmp/phpunit.phar /bin/phpunit

# Configure locales
RUN locale-gen en_US.UTF-8  
ENV LANG en_US.UTF-8  
ENV LANGUAGE en_US:en  
ENV LC_ALL en_US.UTF-8

RUN sed -i -e"s/^;\s*daemonize\s*=\s*yes/daemonize = no/" /etc/php/7.2/fpm/php-fpm.conf \
 && sed -i -e"s/^error_log\s*=\s*.*$/error_log = \/proc\/self\/fd\/2/" /etc/php/7.2/fpm/php-fpm.conf \
 && sed -i -e"s/^pid = \/run\/php\/php7.2-fpm.pid/pid = \/run\/php7.2-fpm.pid/" /etc/php/7.2/fpm/php-fpm.conf \
 && sed -i -e"s/^listen\s*=\s*.*$/listen = 12001/" /etc/php/7.2/fpm/pool.d/www.conf

RUN rm /etc/nginx/sites-available/default \
 && echo "daemon off;" >> /etc/nginx/nginx.conf \
 && echo "security.limit_extensions = " >> /etc/php/7.2/fpm/pool.d/www.conf && \
    echo "catch_workers_output = yes" >> /etc/php/7.2/fpm/pool.d/www.conf && \
    echo "env[XDEBUG_CONFIG] = \$XDEBUG_CONFIG" >> /etc/php/7.2/fpm/pool.d/www.conf && \
    echo "error_reporting=30711" >> /etc/php/7.2/fpm/php.ini && \
    echo "error_reporting=30711" >> /etc/php/7.2/cli/php.ini && \
    echo "memory_limit=512M" >> /etc/php/7.2/fpm/php.ini && \
    echo "memory_limit=512M" >> /etc/php/7.2/cli/php.ini

COPY conf/php/xdebug.ini /etc/php/7.2/mods-available/
COPY conf/nginx /etc/nginx/sites-enabled/
COPY conf/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN phpenmod -v 7.2 xdebug redis

EXPOSE 9999

CMD ["/usr/bin/supervisord"]
