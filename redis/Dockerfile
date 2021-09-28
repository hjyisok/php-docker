FROM redis:alpine
ARG TIMEZONE=Asia/Shanghai
RUN set -x  \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo "${TIMEZONE}" > /etc/timezone \
    && mkdir -p /etc/redis
COPY redis.conf /etc/redis/redis.conf
VOLUME /data
EXPOSE 6379
CMD ["redis-server", "/etc/redis/redis.conf"]
