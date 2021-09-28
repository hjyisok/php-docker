FROM memcached:alpine
USER root
ARG CHANGE_SOURCE=true
ARG TIMEZONE=Asia/Shanghai
RUN set -x  \
    && if [ ${CHANGE_SOURCE} = true ]; then \
      sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/' /etc/apk/repositories \
    ;fi \
    && apk update && apk upgrade \
    && apk add --no-cache --virtual .build-deps tzdata \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo "${TIMEZONE}" > /etc/timezone \
    && apk del .build-deps
USER memcache
EXPOSE 11211
CMD ["memcached"]
