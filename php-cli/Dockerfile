ARG PHP_VERSION
FROM chinayin/php:${PHP_VERSION}-cli-slim

COPY ./aliases.sh /root/aliases.sh

RUN set -eux \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        curl iputils-ping apt-utils telnet vim \
        git wget zsh \
    # Install composer and add its bin to the PATH
    && curl -s http://getcomposer.org/installer | php \
    && mv composer.phar /usr/local/bin/composer \
    && composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/ \
    # aliases
    && sed -i 's/\r//' ~/aliases.sh \
    && echo "" >> ~/.bashrc \
    && echo "# Load Custom Aliases" >> ~/.bashrc \
    && echo "source ~/aliases.sh" >> ~/.bashrc \
    && echo "" >> ~/.bashrc \
    # Clean up
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR /var/www

CMD ["sh"]
