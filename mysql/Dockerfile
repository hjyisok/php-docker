ARG MYSQL_VERSION
FROM mysql:${MYSQL_VERSION}

ARG CHANGE_SOURCE=true
ARG TIMEZONE=Asia/Shanghai
RUN set -x \
    && if [ ${CHANGE_SOURCE} = true ]; then \
      sed -i 's/deb.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list && \
      sed -i 's/security.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list && \
      sed -i 's/security-cdn.debian.org/mirrors.aliyun.com/' /etc/apt/sources.list \
    ;fi \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo "${TIMEZONE}" > /etc/timezone \
    && apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y --no-install-recommends \
        curl iputils-ping telnet vim apt-utils \
    ## Clean up
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && rm /var/log/lastlog /var/log/faillog

COPY my.cnf /etc/mysql/conf.d/my.cnf

RUN chown -R mysql:root /var/lib/mysql/ \
    && chmod 0444 /etc/mysql/conf.d/my.cnf

EXPOSE 3306
CMD ["mysqld"]
