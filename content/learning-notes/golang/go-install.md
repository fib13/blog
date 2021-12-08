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

echo -e "\nexport PATH=\$PATH:/usr/local/go/bin" | tee -a ~/.profile

source ~/.profile

go version
```

## 环境



```
```




