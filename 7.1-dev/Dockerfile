FROM lagun4ik/php7-fpm:7.1.5-r2

MAINTAINER ivan@lagunovsky.com

ENV TERM=xterm \
    PHP_OPCACHE_ENABLE_CLI=1 \
    PHP_XDEBUG_REMOTE_AUTOSTART=Off \
    PHP_XDEBUG_REMOTE_ENABLE=Off \
    PHP_XDEBUG_REMOTE_HANDLER="dbgp" \
    PHP_XDEBUG_REMOTE_HOST="localhost" \
    PHP_XDEBUG_REMOTE_PORT=9001 \
    PHP_XDEBUG_REMOTE_MODE=req \
    PHP_XDEBUG_IDEKEY="PHPSTORM"

RUN apk add --update --no-cache git nano unzip openssh less

RUN apk add --update --no-cache --virtual .build-deps file re2c autoconf make g++ php7-dev=7.1.5-r0 musl && \
    git clone --depth=1 -b XDEBUG_2_5_4 https://github.com/xdebug/xdebug.git /tmp/php-xdebug && \
    cd /tmp/php-xdebug && \
    phpize && ./configure --prefix=/usr && make && make install && \
    cd .. && rm -rf /tmp/php-xdebug/ && \
    apk del .build-deps

COPY ./conf/xdebug.ini /etc/php7/conf.d/xdebug.ini
COPY ./bin/ /usr/bin/

RUN mkdir /usr/local/phar

RUN curl -OsSL https://getcomposer.org/composer.phar \
    && mv composer.phar /usr/local/phar

RUN curl -OsSL https://phar.phpunit.de/phpunit.phar \
    && mv phpunit.phar /usr/local/phar

RUN cd /usr/bin/ && chmod +x composer php phpunit

CMD ["/usr/sbin/php-fpm7", "-R", "-dzend_extension=xdebug.so"]
