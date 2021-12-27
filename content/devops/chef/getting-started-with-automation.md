+++
title = "Chef - 自动化服务器配置管理工具入门"
description = ""
weight = 1
+++

在我的学习过程中，先通过实践去了解一门技术，了解这个技术可以做什么。然后在通过系统的学习去深入细节是最有效的方法。

本文会使用 Chef 实现自动化管理，通过实践来对 Chef 的各个组件有一个大概的了解。

## Part 1 - Chef Infra Server

虽然我们可以通过使用 Chef Zero 在不运行完整的 Chef Infra Server 体验 Chef，但要获得完整的使用体验，最好运行本地或基于云端的 Chef Infra Server。部署完整的 Chef Infra Server 并不需要太多资源，它可以提供在目标节点之间存储和分发代码的完整体验。

### 先决条件

- 硬件最低配置：4 核 CPU、16GB 内存和 80GB 磁盘空间。
- Linux 内核版本 3.2 以上并使用 `systemd` 作为 Init 系统。例如，Ubuntu 20.04 或 CentOS 8.
- 具有 sudo 权限的 非 root 用户。
- 具有静态 IP 地址并能够与你的 Workstation 和目标节点网络互通。
- 子网上具备 DNS 解析，实现 Chef Infra Server、节点和 Workstation 通过 FQDN 进行通信。如果没有内网 DNS 服务器，需要在 Chef Infra Server 和目标节点上的 hosts 文件，以便可以通过 name 互相访问。

### 部署 Chef Infra Server



```
```
