FROM ubuntu:20.04
RUN apt-get update && apt-get install wget -y && apt-get install curl -y && apt-get install git -y && apt-get install unzip -y

WORKDIR /root/tools/
RUN wget -c https://dl.google.com/go/go1.17.linux-amd64.tar.gz -O - | tar -xz -C / && \
BIN="/usr/local/bin" && \
VERSION="0.52.0" && \
BINARY_NAME="buf" && \
  curl -sSL \
    "https://github.com/bufbuild/buf/releases/download/v${VERSION}/${BINARY_NAME}-$(uname -s)-$(uname -m)" \
    -o "${BIN}/${BINARY_NAME}" && \
  chmod +x "${BIN}/${BINARY_NAME}" && \
  /go/bin/go install google.golang.org/protobuf/cmd/protoc-gen-go@v1.26 && \
  /go/bin/go install google.golang.org/grpc/cmd/protoc-gen-go-grpc@v1.1 && \
  /go/bin/go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-grpc-gateway@v2.5.0 && \
  /go/bin/go install github.com/grpc-ecosystem/grpc-gateway/v2/protoc-gen-openapiv2@v2.5.0 && \
  /go/bin/go install github.com/pseudomuto/protoc-gen-doc/cmd/protoc-gen-doc@v1.5.0 && \
  git clone https://github.com/googleapis/gapic-generator-go.git --branch v0.21.8 --single-branch && \
  cd gapic-generator-go && \
  /go/bin/go install ./cmd/protoc-gen-go_gapic && \
  cd /root/tools/ && \
  git clone https://github.com/fiorix/protoc-gen-cobra.git && \
  cd protoc-gen-cobra && \
  /go/bin/go mod init protoc-gen-cobra && \
  /go/bin/go mod tidy && \
  /go/bin/go build -o /root/go/bin/protoc-gen-cobra 

ENV PROTOBUF_VERSION=3.17.3
RUN mkdir -p /root/tools/protoc && \
      curl -sSL https://github.com/protocolbuffers/protobuf/releases/download/v${PROTOBUF_VERSION}/protoc-${PROTOBUF_VERSION}-linux-x86_64.zip -o /root/tools/protoc/protoc.zip && \
      cd protoc && unzip protoc.zip && \
      mv bin/protoc /usr/local/bin/

WORKDIR /root
COPY .netrc .
RUN mv /root/go/bin/* /usr/local/bin/ && rm -rf tools /root/go
