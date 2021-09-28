FROM mongo:latest

#COPY mongo.conf /usr/local/etc/mongo/mongo.conf
#USER root
ARG TIMEZONE=Asia/Shanghai
RUN set -x \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo "${TIMEZONE}" > /etc/timezone

VOLUME /data/db /data/configdb
