FROM openjdk:8u292-jre-buster
LABEL org.opencontainers.image.authors="content-noreplay@gmail.com"

ENV XMS=4g                                                 \
    XMX=4g                                                 \
    XMN=2g                                                 \
    METASPACE=128m                                         \
    MAXMETASPACE=320m                                      \
    BROKER_NAME=RaftNode00                                 \
    NAMESRV_ADDR=127.0.0.1:9876                            \
    LISTEN_PORT=30911                                      \
    STORE_PATH_ROOT_DIR=/tmp/rmqstore/node00               \
    STORE_PATH_COMMITLOG=/tmp/rmqstore/node00/commitlog    \
    D_LEGER_GROUP=RaftNode00                               \
    D_LEGER_PEERS=n0-127.0.0.1:40911;n1-127.0.0.1:40912;n2-127.0.0.1:40913 \
    D_LEGER_SELF_ID=n0                                     \
    SEND_MESSAGE_THREAD_POOL_SIZE=16

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

CMD sed -i "s|-Xms8g|-Xms${XMS}|" bin/runbroker.sh \
    && sed -i "s|-Xmx8g|-Xmx${XMX}|" bin/runbroker.sh \
    && sed -i "s|-Xmn4g|-Xmn${XMN}|" bin/runbroker.sh \
    && sed -i "s|brokerName=RaftNode00|brokerName=${BROKER_NAME}|" conf/dledger/broker-n0.conf \
    && sed -i "s|listenPort=30911|listenPort=${LISTEN_PORT}|" conf/dledger/broker-n0.conf \
    && sed -i "s|namesrvAddr=127.0.0.1:9876|namesrvAddr=${NAMESRV_ADDR}|" conf/dledger/broker-n0.conf \
    && sed -i "s|storePathRootDir=/tmp/rmqstore/node00|storePathRootDir=${STORE_PATH_ROOT_DIR}|" conf/dledger/broker-n0.conf \
    && sed -i "s|storePathCommitLog=/tmp/rmqstore/node00/commitlog|storePathCommitLog=${STORE_PATH_COMMITLOG}|" conf/dledger/broker-n0.conf \
    && sed -i "s|dLegerGroup=RaftNode00|dLegerGroup=${D_LEGER_GROUP}|" conf/dledger/broker-n0.conf \
    && sed -i "s|dLegerPeers=n0-127.0.0.1:40911;n1-127.0.0.1:40912;n2-127.0.0.1:40913|dLegerPeers=${D_LEGER_PEERS}|" conf/dledger/broker-n0.conf \
    && sed -i "s|dLegerSelfId=n0|dLegerSelfId=${D_LEGER_SELF_ID}|" conf/dledger/broker-n0.conf \
    && sed -i "s|sendMessageThreadPoolNums=16|sendMessageThreadPoolNums=${SEND_MESSAGE_THREAD_POOL_SIZE}|" conf/dledger/broker-n0.conf \
    && sh bin/mqbroker -c conf/dledger/broker-n0.conf
