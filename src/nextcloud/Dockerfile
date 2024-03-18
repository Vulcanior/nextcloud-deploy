FROM alpine:3.19 as src

RUN apk add --no-cache git
RUN git clone -b v28.0.3 --depth=1 https://github.com/nextcloud/server.git /nextcloud
WORKDIR /nextcloud
RUN git submodule update --init

FROM alpine:3.19

# https://pkgs.alpinelinux.org/packages
# https://docs.nextcloud.com/server/latest/admin_manual/installation/php_configuration.html
# docker compose run nextcloud php83 -m
RUN apk add --no-cache php83-fpm php83-ctype php83-curl php83-dom php83-fileinfo php83-gd php83-mbstring php83-openssl php83-posix php83-session php83-xml php83-simplexml php83-xmlreader php83-xmlwriter php83-zip php83-pdo_mysql nginx s6-overlay execline curl

ENV S6_LOGGING=0

# RUN mkdir -p /var/www/html/ && curl -s -L https://github.com/nextcloud/server/archive/refs/tags/v28.0.3.tar.gz | tar -xz -C /var/www/html/ && mv /var/www/html/server-28.0.3 /var/www/html/nextcloud
COPY --from=src /nextcloud /var/www/html

COPY services /etc/s6-overlay/s6-rc.d/
RUN touch /etc/s6-overlay/s6-rc.d/user/contents.d/nginx
RUN touch /etc/s6-overlay/s6-rc.d/user/contents.d/php

COPY conf/nginx.conf /etc/nginx/nginx.conf
COPY conf/php.conf /etc/php83/php-fpm.d/www.conf

RUN nginx -t

ENTRYPOINT [ "/init" ]