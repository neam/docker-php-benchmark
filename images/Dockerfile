# Dockerfile for running PHP ORM Benchmarks
# ===============================================================
#   docker build -f Dockerfile -t neam/php-benchmark:ubuntu-15.04-fpm-5.6.4-hhvm-3.10.1-phalcon-2.0.9 .
#   docker push neam/php-benchmark:ubuntu-15.04-fpm-5.6.4-hhvm-3.10.1-phalcon-2.0.9
# ===============================================================

FROM ubuntu:15.04

MAINTAINER Fredrik Wollsén <fredrik@neam.se>

# Prepare Debian environment
ENV DEBIAN_FRONTEND noninteractive

# Performance optimization - see https://gist.github.com/jpetazzo/6127116
# this forces dpkg not to call sync() after package extraction and speeds up install
RUN echo "force-unsafe-io" > /etc/dpkg/dpkg.cfg.d/02apt-speedup
# we don't need and apt cache in a container
RUN echo "Acquire::http {No-Cache=True;};" > /etc/apt/apt.conf.d/no-cache

# Update the below commented date time to match the time docker fetched the system package information - an update will trigger docker to fetch the information anew
RUN apt-get update && \
    apt-get upgrade -y -q # 2015-12-09 13:09

# Enable locales, editors and general tools
ENV TERM xterm
RUN apt-get install -y -q \
        locales \
        software-properties-common \
        curl \
        wget \
        less \
        nano \
        vim \
        zip \
        sudo

# For compilation of php extensions
RUN apt-get install -y -q \
        php5-dev \
        libpcre3-dev \
        gcc \
        make \
        re2c

# Database clients
RUN apt-get install -y -q \
        mysql-client

# Version control systems
RUN apt-get install -y -q \
        git-flow \
        git-svn \
        mercurial \
        subversion

# HHVM and Phalcon repositories
RUN apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0x5a16e7281be7a449 && \
        apt-add-repository "deb http://dl.hhvm.com/ubuntu $(lsb_release -sc) main" && \
        apt-add-repository ppa:phalcon/stable && \
        apt-get update

# Install specific version of PHP-FPM (if not available - show what versions were available in debian jessie at the moment of APT_DOCKER_CACHE_TRIGGER above)
RUN apt-get install -y -q \
        php5-cli \
        php5-fpm=5.6.4* \
  || (echo "php5-cli \n $(apt-cache show php5-cli | grep -i version)" && \
      echo "php5-fpm \n $(apt-cache show php5-fpm | grep -i version)" && \
      exit 1)

# HHVM binaries
RUN apt-get install -y -q \
        hhvm=3.10.1* \
  || (echo "hhvm \n $(apt-cache show hhvm | grep -i version)" && \
      exit 1)

# PHP Phalcon
RUN apt-get install -y -q \
        php5-phalcon=2.0.9* \
  || (echo "php5-phalcon \n $(apt-cache show php5-phalcon | grep -i version)" && \
      exit 1)

# Composer (so that we use a clean composer install)
RUN curl -sS https://getcomposer.org/installer | php && \
        mv /composer.phar /usr/local/bin/composer

# Global asset plugin (required by Yii 2)
RUN composer global require "fxp/composer-asset-plugin:~1.1.1"

# Ice for PHP extension
RUN wget https://github.com/ice/framework/archive/1.0.36.zip && \
        unzip 1.0.36.zip && \
        cd framework-1.0.36 && \
        ./install && \
        cd .. && \
        rm -r framework-1.0.36

RUN echo "[ice]\nextension = ice.so" | tee /etc/php5/mods-available/ice.ini && php5enmod ice

# Install PHP extensions available via apt-get (required for PHP-FPM, but can also be useful to HHVM since default configurations for the extensions are incorporated in the docker image)
RUN apt-get install -y -q \
        php5-intl \
        php5-mysqlnd \
        php5-sqlite \
        php5-pgsql \
        php5-mcrypt && \
        php5enmod mcrypt

# Clean apt caches
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Set environment var for where php configuration is stored
ENV PHP_CONF_DIR /etc/php5
