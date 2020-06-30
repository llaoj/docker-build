## why

due to network and other reasons, the download and Compilation of grpc_php_plugin is very difficult, so I made this image to use. enjoy it.

## protoc && grpc_php_plugin

- you can use this image to generate php grpc code
- build from grpc `https://github.com/grpc/grpc`
- the tag/version is consistent with the grpc repo
- installed protoc grpc_php_plugin

## example

```
cd protobuf-file-path

protoc --proto_path=. \
--php_out=. \
--grpc_out=. \
--plugin=protoc-gen-grpc=/opt/grpc/bin/grpc_php_plugin \
./*.proto

```

## tag <=> version

|docker tag|protoc|grpc_php_plugin|
|-|-|-|
|1.28.1|3.11.4|1.28.1|
|1.30.0|3.12.3|1.30.0|