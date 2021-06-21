FROM openjdk:8u292-jre-buster
LABEL org.opencontainers.image.authors="content-noreplay@gmail.com"

ENV XMS=4g            \
    XMX=4g            \
    XMN=2g            \
    METASPACE=128m    \
    MAXMETASPACE=320m 

WORKDIR /opt/rocketmq/

RUN apt-get update \
    && apt-get install -y wget \
    && apt-get install -y unzip \
    && mkdir -p /opt/rocketmq \
    && wget https://downloads.apache.org/rocketmq/4.9.0/rocketmq-all-4.9.0-bin-release.zip \
    && unzip rocketmq-all-4.9.0-bin-release.zip \
    && mv rocketmq-all-4.9.0-bin-release/* . \
    && rm -rf rocketmq-all-4.9.0-bin-release \
    && rm -rf rocketmq-all-4.9.0-bin-release.zip \
    && chmod -R +x *
    
CMD ["sh", "bin/mqnamesrv"]