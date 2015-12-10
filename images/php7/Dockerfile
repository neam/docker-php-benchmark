# Dockerfile for running PHP ORM Benchmarks
# ===============================================================
#   docker build -f Dockerfile -t neam/php-benchmark:ubuntu-15.04-fpm-7.0.0-hhvm-3.10.1-phalcon-2.0.9 .
#   docker push neam/php-benchmark:ubuntu-15.04-fpm-7.0.0-hhvm-3.10.1-phalcon-2.0.9
# ===============================================================

FROM neam/php-benchmark:ubuntu-15.04-fpm-5.6.4-hhvm-3.10.1-phalcon-2.0.9

MAINTAINER Fredrik Wollsén <fredrik@neam.se>

# PHP 7.0 PPA including workaround from https://github.com/oerdnj/deb.sury.org/issues/56
RUN apt-get update && \
        apt-get install -y -q language-pack-en-base && \
        LC_ALL=en_US.UTF-8 apt-add-repository ppa:ondrej/php && \
        apt-get update

# Install specific version of PHP 7
RUN apt-get purge -y -q php5-fpm && \
      apt-get install -y -q \
        php7.0-cli \
        php7.0-fpm=7.0.0* \
  || (echo "php7.0-cli \n $(apt-cache show php7.0-cli | grep -i version)" && \
      echo "php7.0-fpm \n $(apt-cache show php7.0-fpm | grep -i version)" && \
      exit 1)

# Install PHP extensions available via apt-get (required for PHP-FPM, but can also be useful to HHVM since default configurations for the extensions are incorporated in the docker image)
RUN apt-get install -y -q \
        php7.0-intl \
        php7.0-mysql \
        php7.0-sqlite3 \
        php7.0-pgsql

# Clean apt caches
RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Set environment var for where php configuration is stored
ENV PHP_CONF_DIR /etc/php/7.0
