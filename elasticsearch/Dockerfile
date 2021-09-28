ARG ELK_VERSION
FROM elasticsearch:${ELK_VERSION}

ARG CHANGE_SOURCE=true
ARG TIMEZONE=Asia/Shanghai
RUN set -x \
    && if [ ${CHANGE_SOURCE} = true ]; then \
      mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo_bak \
      && curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo \
    ;fi \
    && cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && echo "${TIMEZONE}" > /etc/timezone \
    && yum -y update \
    ## Clean up
    && yum clean all

EXPOSE 9200 9300
