FROM ccr.ccs.tencentyun.com/hejd/gocv-arm32:v0.1

LABEL maintainer="Jiangda He. mail: hejiangda@gmail.com"

WORKDIR /version

#COPY qemu-arm-static /usr/bin/qemu-arm-static

RUN go mod init gocv-version && go mod tidy
COPY cgo_static.go /root/go/pkg/mod/gocv.io/x/gocv@v0.30.0/
RUN go build -tags static -o . && ./gocv-version
