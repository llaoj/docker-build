FROM debian:stretch-slim

COPY ./kubectl /usr/bin/

WORKDIR /root

RUN chmod a+x /usr/bin/kube* \
    && mkdir .kube