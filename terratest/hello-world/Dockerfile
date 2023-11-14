FROM golang:1.21
LABEL maintainer="dipti"

ARG tf_version=1.5.3-1
ARG terratest_version=v0.43.13
ARG app_home=$GOPATH/src/app

RUN mkdir -p ${app_home} && chmod -R 777 $GOPATH
COPY test/hello-world_test.go ${app_home}/test/

RUN wget -O- https://apt.releases.hashicorp.com/gpg | gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg \
    && apt-get update && apt-get install -y gnupg software-properties-common curl sudo \
    && curl -fsSL https://apt.releases.hashicorp.com/gpg | apt-key add - \   
    && echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | tee /etc/apt/sources.list.d/hashicorp.list \
    && apt-get update \
    && apt-get install terraform=${tf_version}

WORKDIR ${app_home}/test/
RUN go mod init && go mod tidy && go mod vendor
