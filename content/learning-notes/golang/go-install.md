+++
title = "Go 安装"
description = ""
weight = 2
+++

## 开发环境

使用 VSCode 编辑器。

在 Debian 11 远程开发环境上开发。

## 安装

```bash
wget https://golang.google.cn/dl/go1.17.4.linux-amd64.tar.gz

rm -rf /usr/local/go && tar -C /usr/local -xzf go1.17.4.linux-amd64.tar.gz

echo -e "\nexport PATH=\$PATH:/usr/local/go/bin:\$GOPATH/bin" | tee -a ~/.profile

source ~/.profile

go version
```

## 环境

```
go env -w GO111MODULE=on

go env -w GOPROXY=https://goproxy.io,direct
```

## 编辑器

安装 [vscode-go](https://github.com/golang/vscode-go) 插件。

再安装开发工具。

![20211208214119](https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211208214119.png)

![20211208214445](https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211208214445.png)

## 语法检测

```bash
go install github.com/golangci/golangci-lint/cmd/golangci-lint@latest
```




