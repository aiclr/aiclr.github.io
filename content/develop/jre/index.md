+++
title = 'JRE Docker image'
date = '2024-07-07'
author = 'aiclr'
categories = ['develop']
tags = ['jre','docker']
Summary='jre docker 基础镜像'
+++

## jre7

### [server-jre not jre](https://www.oracle.com/java/technologies/javase/javase7-archive-downloads.html)

### [jce](https://www.oracle.com/java/technologies/javase-jce7-downloads.html)

```shell
curl -L -C - -b "oraclelicense=accept-securebackup-cookie" https://download.oracle.com/otn-pub/java/jce/7/UnlimitedJCEPolicyJDK7.zip -o jce_policy-7.zip
```

> 浏览器 [download](https://www.oracle.com/java/technologies/javase-jce7-downloads.html) 需要登陆

### [glibc](https://github.com/sgerrand/alpine-pkg-glibc/releases)

```shell
curl -SL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-2.34-r0.apk -o glibc-2.34-r0.apk
curl -SL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-bin-2.34-r0.apk -o glibc-bin-2.34-r0.apk
curl -SL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-i18n-2.34-r0.apk -o glibc-i18n-2.34-r0.apk
```

### [Dockerfile](conf/jre7/Dockerfile)

```dockerfile
FROM alpine:3.15 as jre7

MAINTAINER aiclr <aiclr@qq.com>

WORKDIR app

ARG SERVER_JRE_PKG=server-jre-7u60-linux-x64.tar.gz
ARG SERVER_JRE=jdk1.7.0_60
ARG GLIBC=glibc-2.34-r0.apk
ARG GLIBC_BIN=glibc-bin-2.34-r0.apk
ARG GLIBC_I18N=glibc-i18n-2.34-r0.apk
ARG RSA_PUB=sgerrand.rsa.pub
COPY ${SERVER_JRE_PKG} ${GLIBC} ${GLIBC_BIN} ${GLIBC_I18N} ${RSA_PUB} /tmp/

RUN set -ex && \
    tar xzf /tmp/${SERVER_JRE_PKG} -C /opt && \
    find /opt/${SERVER_JRE} -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf && \
    sed -i 's/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/g' /opt/${SERVER_JRE}/jre/lib/security/java.security && \
    ln -s /opt/${SERVER_JRE}/jre/bin /opt/${SERVER_JRE}/bin && \
    ln -s /opt/${SERVER_JRE} /opt/jdk && \
    rm -rf /opt/jdk/jre/plugin \
           /opt/jdk/jre/bin/javaws \
           /opt/jdk/jre/bin/orbd \
           /opt/jdk/jre/bin/pack200 \
           /opt/jdk/jre/bin/policytool \
           /opt/jdk/jre/bin/rmid \
           /opt/jdk/jre/bin/rmiregistry \
           /opt/jdk/jre/bin/servertool \
           /opt/jdk/jre/bin/tnameserv \
           /opt/jdk/jre/bin/unpack200 \
           /opt/jdk/jre/lib/javaws.jar \
           /opt/jdk/jre/lib/deploy* \
           /opt/jdk/jre/lib/desktop \
           /opt/jdk/jre/lib/*javafx* \
           /opt/jdk/jre/lib/*jfx* \
           /opt/jdk/jre/lib/amd64/libdecora_sse.so \
           /opt/jdk/jre/lib/amd64/libprism_*.so \
           /opt/jdk/jre/lib/amd64/libfxplugins.so \
           /opt/jdk/jre/lib/amd64/libglass.so \
           /opt/jdk/jre/lib/amd64/libgstreamer-lite.so \
           /opt/jdk/jre/lib/amd64/libjavafx*.so \
           /opt/jdk/jre/lib/amd64/libjfx*.so \
           /opt/jdk/jre/lib/ext/jfxrt.jar \
           /opt/jdk/jre/lib/oblique-fonts \
           /opt/jdk/jre/lib/plugin.jar && \
    tar czf jre.tar.gz /opt/${SERVER_JRE} /opt/jdk /tmp/${GLIBC} /tmp/${GLIBC_BIN} /tmp/${GLIBC_I18N} && \
    rm -rf /tmp/* /opt/${SERVER_JRE} /opt/jdk /var/cache/apk/* /root/.cache

FROM alpine:3.15

MAINTAINER bougainvilleas <caddyren@qq.com>

ARG SERVER_JRE=jdk1.7.0_60
ARG ALPINE_REPO=https://mirrors.bfsu.edu.cn/alpine/v3.15/main/
ARG LANGUAGE=en_US
ARG CHARCODE=UTF-8
ARG JRE=jre.tar.gz
ARG RSA_PUB=sgerrand.rsa.pub

ENV TZ=Asia/Shanghai \
    LANG=en_US.UTF-8 \
    ZONE=Asia/Shanghai \
    JAVA_HOME=/opt/jdk \
    PATH=${PATH}:/opt/jdk/bin \
    ACTIVE=${ACTIVE}:pro

COPY --from=jre7 app/${JRE} /tmp/

RUN set -ex && \
    tar xzf /tmp/${JRE} -C /tmp && \
    mv /tmp/tmp/${RSA_PUB} /etc/apk/keys/${RSA_PUB} && \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.bfsu.edu.cn/g' /etc/apk/repositories && \
    apk -U upgrade && \
    apk add --no-cache tzdata libstdc++ curl java-cacerts lsof && \
    cp -v /usr/share/zoneinfo/${ZONE} /etc/localtime && \
    echo "${ZONE}" >> /etc/timezone && \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf && \
    apk add --allow-untrusted /tmp/tmp/*.apk && \
    /usr/glibc-compat/bin/localedef -i ${LANGUAGE} -f ${CHARCODE} ${LANG} && \
    /usr/glibc-compat/sbin/ldconfig /lib /usr/glibc-compat/lib && \
    mv /tmp/opt/${SERVER_JRE} /opt/ && \
    mv /tmp/opt/jdk /opt/ && \
    ln -sf /etc/ssl/certs/java/cacerts ${JAVA_HOME}/jre/lib/security/cacerts && \
    apk del glibc-i18n && \
    rm -rf /var/cache/apk/* /root/.cache /tmp/* /etc/apk/keys/${RSA_PUB}
```

### [build](conf/jre7/build.sh)

```shell
#!/bin/bash

image_name="server-jre"
version="7u60-alpine"

echo -e "\n==> begin delete all containers of " $image_name
docker rm $(docker stop $(docker ps -a | grep $image_name | awk '{print $1}'))

echo -e "\n==> begin delete all images of " $image_name
docker rmi -f $(docker images | grep $image_name | awk '{print $3}')

echo -e "\n==> begin build your images of " $image_name
docker build -f Dockerfile -t $image_name:$version .

echo -e "\n==> begin to package your image to tar file"
docker save $image_name:$version > ./$image_name-$version.tar
```


## jre8

### [server-jre not jre](https://www.oracle.com/java/technologies/downloads/)

### [jce](https://www.oracle.com/cn/java/technologies/javase-jce8-downloads.html)

```shell
curl -L -C - -b "oraclelicense=accept-securebackup-cookie" https://download.oracle.com/otn-pub/java/jce/8/jce_policy-8.zip -o jce_policy-8.zip
```

> 浏览器 [download](https://www.oracle.com/cn/java/technologies/javase-jce8-downloads.html) 需要登陆

### [glibc](https://github.com/sgerrand/alpine-pkg-glibc/releases)

```shell
curl -SL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-2.34-r0.apk -o glibc-2.34-r0.apk
curl -SL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-bin-2.34-r0.apk -o glibc-bin-2.34-r0.apk
curl -SL https://github.com/sgerrand/alpine-pkg-glibc/releases/download/2.34-r0/glibc-i18n-2.34-r0.apk -o glibc-i18n-2.34-r0.apk
```

### [Dockerfile](conf/jre8/Dockerfile)

```dockerfile
FROM alpine:3.15 as jre8

MAINTAINER bougainvilleas <caddyren@qq.com>

WORKDIR app

ARG SERVER_JRE_PKG=server-jre-8u311-linux-x64.tar.gz
ARG SERVER_JRE=jdk1.8.0_311
ARG JCE=jce_policy-8.zip
ARG GLIBC=glibc-2.34-r0.apk
ARG GLIBC_BIN=glibc-bin-2.34-r0.apk
ARG GLIBC_I18N=glibc-i18n-2.34-r0.apk
COPY ${SERVER_JRE_PKG} ${JCE} ${GLIBC} ${GLIBC_BIN} ${GLIBC_I18N} /tmp/

RUN set -ex && \
    tar xzf /tmp/${SERVER_JRE_PKG} -C /opt && \
    find /opt/${SERVER_JRE} -maxdepth 1 -mindepth 1 | grep -v jre | xargs rm -rf && \
    unzip /tmp/${JCE} -d /tmp && \
    cp -v /tmp/UnlimitedJCEPolicyJDK8/*.jar /opt/${SERVER_JRE}/jre/lib/security/ && \
    sed -i 's/#networkaddress.cache.ttl=-1/networkaddress.cache.ttl=10/g' /opt/${SERVER_JRE}/jre/lib/security/java.security && \
    ln -s /opt/${SERVER_JRE}/jre/bin /opt/${SERVER_JRE}/bin && \
    ln -s /opt/${SERVER_JRE} /opt/jdk && \
    rm -rf /opt/jdk/jre/plugin \
           /opt/jdk/jre/bin/javaws \
           /opt/jdk/jre/bin/orbd \
           /opt/jdk/jre/bin/pack200 \
           /opt/jdk/jre/bin/policytool \
           /opt/jdk/jre/bin/rmid \
           /opt/jdk/jre/bin/rmiregistry \
           /opt/jdk/jre/bin/servertool \
           /opt/jdk/jre/bin/tnameserv \
           /opt/jdk/jre/bin/unpack200 \
           /opt/jdk/jre/lib/javaws.jar \
           /opt/jdk/jre/lib/deploy* \
           /opt/jdk/jre/lib/desktop \
           /opt/jdk/jre/lib/*javafx* \
           /opt/jdk/jre/lib/*jfx* \
           /opt/jdk/jre/lib/amd64/libdecora_sse.so \
           /opt/jdk/jre/lib/amd64/libprism_*.so \
           /opt/jdk/jre/lib/amd64/libfxplugins.so \
           /opt/jdk/jre/lib/amd64/libglass.so \
           /opt/jdk/jre/lib/amd64/libgstreamer-lite.so \
           /opt/jdk/jre/lib/amd64/libjavafx*.so \
           /opt/jdk/jre/lib/amd64/libjfx*.so \
           /opt/jdk/jre/lib/ext/jfxrt.jar \
           /opt/jdk/jre/lib/oblique-fonts \
           /opt/jdk/jre/lib/plugin.jar && \
    tar czf jre.tar.gz /opt/${SERVER_JRE} /opt/jdk /tmp/${GLIBC} /tmp/${GLIBC_BIN} /tmp/${GLIBC_I18N} && \
    rm -rf /tmp/* /opt/${SERVER_JRE} /opt/jdk /var/cache/apk/* /root/.cache

FROM alpine:3.15

MAINTAINER aiclr <aiclr@qq.com>

ARG SERVER_JRE=jdk1.8.0_311
ARG ALPINE_REPO=https://mirrors.bfsu.edu.cn/alpine/v3.15/main/
ARG LANGUAGE=en_US
ARG CHARCODE=UTF-8
ARG JRE=jre.tar.gz

ENV TZ=Asia/Shanghai \
    LANG=en_US.UTF-8 \
    ZONE=Asia/Shanghai \
    JAVA_HOME=/opt/jdk \
    PATH=${PATH}:/opt/jdk/bin \
    ACTIVE=${ACTIVE}:pro

COPY --from=jre8 app/${JRE} /tmp/

RUN set -ex && \
    tar xzf /tmp/${JRE} -C /tmp && \
    sed -i 's/dl-cdn.alpinelinux.org/mirrors.bfsu.edu.cn/g' /etc/apk/repositories && \
    apk -U upgrade && \
    apk add --no-cache tzdata libstdc++ curl java-cacerts lsof && \
    cp -v /usr/share/zoneinfo/${ZONE} /etc/localtime && \
    echo "${ZONE}" >> /etc/timezone && \
    echo 'hosts: files mdns4_minimal [NOTFOUND=return] dns mdns4' >> /etc/nsswitch.conf && \
    apk add --allow-untrusted /tmp/tmp/*.apk && \
    /usr/glibc-compat/bin/localedef -i ${LANGUAGE} -f ${CHARCODE} ${LANG} && \
    /usr/glibc-compat/sbin/ldconfig /lib /usr/glibc-compat/lib && \
    mv /tmp/opt/${SERVER_JRE} /opt/ && \
    mv /tmp/opt/jdk /opt/ && \
    ln -sf /etc/ssl/certs/java/cacerts ${JAVA_HOME}/jre/lib/security/cacerts && \
    apk del glibc-i18n && \
    rm -rf /var/cache/apk/* /root/.cache /tmp/*
```

### [build](conf/jre8/build.sh)

```shell
#!/bin/bash
image_name="server-jre"
version="8u311-alpine"

echo -e "\n==> begin delete all containers of " $image_name
docker rm $(docker stop $(docker ps -a | grep $image_name | awk '{print $1}'))

echo -e "\n==> begin delete all images of " $image_name
docker rmi -f $(docker images | grep $image_name | awk '{print $3}')

echo -e "\n==> begin build your images of " $image_name
docker build -f Dockerfile -t $image_name:$version .

echo -e "\n==> begin to package your image to tar file"
docker save $image_name:$version > ./$image_name-$version.tar
```