FROM ubuntu:18.04

# install jre
RUN mkdir -p /usr/share/man/man1 \
    && apt-get update \
    && apt-get install -y default-jre \
    firefox \
    chromium-browser \
    git \
    curl \
    sudo

# create www-data user
ARG USER_ID=1000
ARG GROUP_ID=1000

RUN userdel -f www-data &&\
    if getent group www-data ; then groupdel www-data; fi &&\
    groupadd -g ${GROUP_ID} www-data &&\
    useradd -l -u ${USER_ID} -g www-data www-data &&\
    install -d -m 0755 -o www-data -g www-data /home/www-data

RUN usermod -aG sudo www-data

# clean up
RUN apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# install PhpStorm
RUN cd /home/www-data \
    && curl -SL http://download.jetbrains.com/webide/PhpStorm-2018.3.6.tar.gz |tar zx

USER www-data

EXPOSE 9001

# Run PhpStorm
ENTRYPOINT /home/www-data/PhpStorm-183.6156.29/bin/phpstorm.sh
