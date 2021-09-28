FROM chinayin/nginx:1.19

RUN set -eux \
    ## install packages
    && install_packages logrotate openssl bash curl \
    ## Create 'messages' file used from 'logrotate'
    && touch /var/log/messages

COPY logrotate/nginx /etc/logrotate.d/
COPY nginx.conf /etc/nginx/

# Set upstream conf and remove the default conf
ARG PHP_UPSTREAM_CONTAINER=php-fpm
ARG PHP_UPSTREAM_PORT=9000
ARG PHP72_UPSTREAM_CONTAINER=php-fpm72
ARG PHP72_UPSTREAM_PORT=9002
RUN set -x  \
    && echo "upstream php-upstream { server ${PHP_UPSTREAM_CONTAINER}:${PHP_UPSTREAM_PORT}; }" >> /etc/nginx/conf.d/upstream.conf \
    && echo "upstream php72-upstream { server ${PHP72_UPSTREAM_CONTAINER}:${PHP72_UPSTREAM_PORT}; }" >> /etc/nginx/conf.d/upstream.conf \
    && rm /etc/nginx/conf.d/default.conf \
    && mkdir /etc/nginx/vhost

EXPOSE 80 443
WORKDIR /var/www

ADD ./startup.sh /opt/startup.sh
RUN sed -i 's/\r//g' /opt/startup.sh
CMD ["/bin/bash", "/opt/startup.sh"]
