FROM crunchgeek/php-fpm:7.2

# install jre
RUN mkdir -p /usr/share/man/man1 \
    && apt-get update -y \
    && apt-get install -y default-jre

# install PhpStorm
RUN mkdir -p /home/developer \
    && cd /home/developer \
    && curl -SL http://download.jetbrains.com/webide/PhpStorm-2018.3.6.tar.gz |tar zx

# clean up
RUN apt-get remove -y git && apt-get autoremove -y && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Run PhpStorm
ENTRYPOINT /home/developer/PhpStorm-183.6156.29/bin/phpstorm.sh
