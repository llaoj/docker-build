FROM debian:buster-slim
 
RUN apt-get update && apt-get install -y \
        curl \
        unzip \
        git \
        build-essential \
        autoconf \
        libtool \
    && apt-get clean && rm -rf /var/lib/apt/lists/*

WORKDIR /root    

RUN curl -LO https://github.com/protocolbuffers/protobuf/releases/download/v3.12.3/protoc-3.12.3-linux-x86_64.zip \
    && unzip protoc-3.12.3-linux-x86_64.zip -d /opt/protoc \
    && rm protoc-3.12.3-linux-x86_64.zip

ENV PATH $PATH:/opt/protoc/bin

RUN git clone -b $(curl -L https://grpc.io/release) https://github.com/grpc/grpc \
    && cd grpc \
    && git submodule update --init \
    && make grpc_php_plugin \
    && mkdir -p /opt/grpc \
    && mv ./bins/opt /opt/grpc/bin \
    && rm -rf /root/grpc