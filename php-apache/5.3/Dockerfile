FROM       ubuntu:12.04
MAINTAINER Sergey Khaladzinski <shanderlam@gmail.com>

RUN apt-get update && apt-get install -y \
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

RUN sed -i -e"s/^;\s*daemonize\s*=\s*yes/daemonize = no/" /etc/php5/fpm/php-fpm.conf
RUN sed -i -e"s/^listen\s*=\s*127.0.0.1:9000/listen = 12001/" /etc/php5/fpm/pool.d/www.conf

RUN rm /etc/nginx/sites-available/default
RUN echo "daemon off;" >> /etc/nginx/nginx.conf
RUN \
  echo "security.limit_extensions = " >> /etc/php5/fpm/pool.d/www.conf && \
  echo "php_admin_value[error_log] = /proc/self/fd/2 " >> /etc/php5/fpm/pool.d/www.conf && \
  echo "catch_workers_output = yes" >> /etc/php5/fpm/pool.d/www.conf && \
  echo "error_reporting=30711" >> /etc/php5/fpm/php.ini && \
  echo "error_reporting=30711" >> /etc/php5/cli/php.ini

COPY conf/php/xdebug.ini /etc/php5/conf.d/
COPY conf/nginx/mirtrik.local /etc/nginx/sites-enabled/
COPY conf/supervisor/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

EXPOSE 9999

CMD ["/usr/bin/supervisord"]
