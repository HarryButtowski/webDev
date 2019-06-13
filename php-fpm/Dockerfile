FROM php:7.2-fpm

RUN apt-get update && \
    apt-get install -y --no-install-recommends \
        git \
        libmemcached-dev \
        libz-dev \
        libpq-dev \
        libssl-dev libssl-doc libsasl2-dev \
        libmcrypt-dev \
        libxml2-dev \
        zlib1g-dev libicu-dev g++ \
        libldap2-dev libbz2-dev \
        curl libcurl4-openssl-dev \
        libenchant-dev libgmp-dev firebird-dev libib-util \
        re2c libpng++-dev \
        libwebp-dev libjpeg-dev libjpeg62-turbo-dev libpng-dev libvpx-dev libfreetype6-dev \
        libmagick++-dev \
        libmagickwand-dev \
        zlib1g-dev libgd-dev \
        libtidy-dev libxslt1-dev libmagic-dev libexif-dev file \
        sqlite3 libsqlite3-dev libxslt-dev \
        libmhash2 libmhash-dev libc-client-dev libkrb5-dev libssh2-1-dev \
        unzip libpcre3 libpcre3-dev \
        poppler-utils ghostscript libmagickwand-6.q16-dev libsnmp-dev libedit-dev libreadline6-dev libsodium-dev \
        freetds-bin freetds-dev freetds-common libct4 libsybdb5 tdsodbc libreadline-dev librecode-dev libpspell-dev

# fix for docker-php-ext-install pdo_dblib
# https://stackoverflow.com/questions/43617752/docker-php-and-freetds-cannot-find-freetds-in-know-installation-directories
RUN ln -s /usr/lib/x86_64-linux-gnu/libsybdb.so /usr/lib/

RUN docker-php-ext-configure hash --with-mhash && \
    docker-php-ext-install hash
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl && \
    docker-php-ext-install imap iconv

RUN docker-php-ext-install bcmath bz2 calendar ctype curl dba dom enchant 
RUN docker-php-ext-install fileinfo exif ftp gd gettext gmp 
RUN docker-php-ext-install interbase intl json ldap mbstring mysqli 
RUN docker-php-ext-install opcache pcntl pspell
RUN docker-php-ext-install pdo pdo_dblib pdo_mysql pdo_pgsql pdo_sqlite pgsql phar posix 
RUN docker-php-ext-install readline recode 
RUN docker-php-ext-install session shmop simplexml soap sockets sodium 
RUN docker-php-ext-install sysvmsg sysvsem sysvshm

# fix for docker-php-ext-install xmlreader
# https://github.com/docker-library/php/issues/373
RUN export CFLAGS="-I/usr/src/php" && docker-php-ext-install xmlreader xmlwriter xml xmlrpc xsl

RUN docker-php-ext-install tidy tokenizer wddx zend_test

# install pecl extension
RUN pecl install ds && \
    pecl install imagick && \
    pecl install igbinary && \
    pecl install mongodb && \
    pecl install ssh2-1.0 && \
    pecl install redis-4.0.1 && \
    pecl install xdebug-2.6.0 && \
    pecl install memcached-3.0.4 && \
    docker-php-ext-enable ds imagick igbinary ssh2 redis memcached mongodb xdebug

# install APCu
RUN pecl install apcu-5.1.8 && \
    pecl install apcu_bc-1.0.3 && \
    docker-php-ext-enable apcu --ini-name docker-php-ext-10-apcu.ini && \
    docker-php-ext-enable apc  --ini-name docker-php-ext-20-apc.ini

# Install the PHP pdo_pgsql extention
RUN docker-php-ext-install pdo_pgsql

# set locale to utf-8
RUN apt-get install -y locales
RUN echo "en_US.UTF-8 UTF-8" > /etc/locale.gen && locale-gen
ENV LANG='en_US.UTF-8' LANGUAGE='en_US:en' LC_ALL='en_US.UTF-8'

# clean up
RUN apt-get remove -y git && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN mkdir -p /var/www
WORKDIR /var/www
VOLUME ["/var/www"]

EXPOSE 9000

CMD ["php-fpm"]
