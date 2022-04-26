FROM ubuntu

LABEL maintainer="Jiangda He. mail: hejiangda@gmail.com"
ENV BUILD="unzip \
    wget \
    build-essential \
    cmake \
    curl \
    git \
    pkg-config \
    wget \
    libasound2-dev "

ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get install -y ${BUILD}

WORKDIR /tmp
RUN wget https://go.dev/dl/go1.18.linux-amd64.tar.gz
RUN rm -rf /usr/local/go && tar -C /usr/local -xzf go1.18.linux-amd64.tar.gz
ENV PATH $PATH:/usr/local/go/bin
RUN go version

WORKDIR /root

