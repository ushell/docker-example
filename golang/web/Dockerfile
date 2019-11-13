FROM golang:alpine3.10

#编译参数
ARG GO111MODULE=on
ARG GOPROXY=https://goproxy.io

#镜像仓库
RUN sed -i 's/dl-cdn.alpinelinux.org/mirrors.aliyun.com/g' /etc/apk/repositories

RUN apk add --no-cache curl && rm -rf /var/cache/apk/*

#复制源码                                                       
COPY ./src/projectName $GOPATH/src/projectName

WORKDIR $GOPATH/src/projectName

#依赖安装
RUN go mod download

#编译
RUN go build -o server main.go

EXPOSE 8000

CMD ["./server"]
