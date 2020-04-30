FROM debian:buster-slim
 
RUN apt-get update && apt-get install -y \
    curl \
    unzip \
    git \
    build-essential \
    autoconf \
    libtool

WORKDIR /root    

RUN curl -LO https://github.com/protocolbuffers/protobuf/releases/download/v3.11.4/protoc-3.11.4-linux-x86_64.zip \
    && unzip protoc-3.11.4-linux-x86_64.zip -d /opt/protoc \
    && rm protoc-3.11.4-linux-x86_64.zip

ENV PATH $PATH:/opt/protoc/bin

RUN git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc \
    && cd grpc \
    && git submodule update --init \
    && make grpc_php_plugin \
    && mkdir -p /opt/grpc \
    && mv ./bins/opt /opt/grpc/bin \
    && rm -rf /root/grpc