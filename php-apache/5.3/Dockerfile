FROM       ubuntu:12.04
MAINTAINER Shander Lam <shanderlam@gmail.com>

RUN apt-get update && apt-get install -y \
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
    nginx

RUN sed -i -e"s/^;\s*daemonize\s*=\s*yes/daemonize = no/" /etc/php5/fpm/php-fpm.conf
RUN sed -i -e"s/^listen\s*=\s*127.0.0.1:9000/listen = 9000/" /etc/php5/fpm/pool.d/www.conf

EXPOSE 9000

CMD ["php5-fpm"]
