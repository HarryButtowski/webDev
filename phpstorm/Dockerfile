FROM ubuntu:18.04

ENV DEBIAN_FRONTEND=noninteractive

# install php
RUN apt-get update \
    && apt-get install -y software-properties-common \
    && add-apt-repository ppa:ondrej/php \
    && apt-get update \
    && apt-get install -y \
        php7.2-cli \
        php7.2-common \
        php7.2-curl \
        php7.2-json \
        php7.2-xml \
        php7.2-mbstring \
        php7.2-pgsql \
        php7.2-zip \
        php7.2-bcmath \
        php7.2-gd \
        php7.2-dev

# install app
RUN mkdir -p /usr/share/man/man1 \
    && apt-get install -y default-jre \
    firefox \
    chromium-browser \
    git \
    curl \
    sudo

# install npm
RUN apt update \
    && apt install -y npm

# create www-data user
ARG USER_ID=1000
ARG GROUP_ID=1000

RUN userdel -f www-data &&\
    if getent group www-data ; then groupdel www-data; fi &&\
    groupadd -g ${GROUP_ID} www-data &&\
    useradd -l -u ${USER_ID} -g www-data www-data &&\
    install -d -m 0755 -o www-data -g www-data /home/www-data

RUN usermod -aG sudo www-data \
    && echo 'sudo visudo' >> /etc/sudoers \
    && echo 'www-data     ALL=(ALL) NOPASSWD:ALL' > /etc/sudoers

# clean up
RUN apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install PhpStorm
RUN cd /home/www-data \
    && curl -SL http://download.jetbrains.com/webide/PhpStorm-2018.3.6.tar.gz |tar zx

# install composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    && php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    && php composer-setup.php --filename=composer \
    && php -r "unlink('composer-setup.php');" \
    && mv composer /usr/bin/

USER www-data

EXPOSE 9001

# Run PhpStorm
ENTRYPOINT /home/www-data/PhpStorm-183.6156.29/bin/phpstorm.sh
