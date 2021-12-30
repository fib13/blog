+++
title = "Chef - 自动化服务器配置管理工具入门"
description = ""
weight = 1
+++

在学习过程中，先通过实践去了解一门技术，了解这个技术可以做什么。然后在通过系统的学习去深入细节是最有效的方法。

本文会使用 Chef 实现自动化管理，通过实践来对 Chef 的各个组件有一个大概的了解。

## 1. Chef Infra Server

虽然我们可以通过使用 Chef Zero 在不运行完整的 Chef Infra Server 体验 Chef，但要获得完整的使用体验，最好运行本地或基于云端的 Chef Infra Server。部署完整的 Chef Infra Server 并不需要太多资源，它可以提供在目标节点之间存储和分发代码的完整体验。

### 1.1. 先决条件

- 硬件最低配置：4 核 CPU、16GB 内存和 80GB 磁盘空间。
- Linux 内核版本 3.2 以上并使用 `systemd` 作为 Init 系统。例如，Ubuntu 20.04 或 CentOS 8.
- 具有 sudo 权限的 非 root 用户。
- 具有静态 IP 地址并能够与你的 Workstation 和目标节点网络互通。
- 子网上具备 DNS 解析，实现 Chef Infra Server、节点和 Workstation 通过 FQDN 进行通信。如果没有内网 DNS 服务器，需要在 Chef Infra Server 和目标节点上的 hosts 文件，以便可以通过 name 互相访问。

本次 Chef Infra Server 实验环境为：
| | |
| :----: | :-----------------------------: |
| 系统 | Debian 11 |
| 内核 | 5.10.0-10-amd64 |
| 主机名 | automate.chef.lab |
| 配置 | 8 CPU、16GB 内存、50GB 磁盘空间 |
| 用户 | sysadmin |

### 1.2. 部署 Chef Infra Server

1. 使用 `hostnamectl set-hostname` _`hostname`_ 设置完全限定域名。之后的安装过程中默认将使用 FQDN 设置证书，因此配置文件中的 FQDN 值必须与系统的 `hostname -f` 值匹配。

   ```bash
   $ sudo hostnamectl set-hostname automate.chef.lab
   ```

2. 安装 `chef-automate` 工具。

   ```bash
   curl https://packages.chef.io/files/current/latest/chef-automate-cli/chef-automate_linux_amd64.zip | gunzip -> chef-automate && chmod +x chef-automate
   ```

3. 调整系统参数。

   ```bash
   $ echo "vm.max_map_count = 262144" | sudo tee -a /etc/sysctl.d/20-chef.conf
   $ echo "vm.dirty_expire_centisecs = 20000" | sudo tee -a /etc/sysctl.d/20-chef.conf
   $ sudo systemctl restart systemd-sysctl.service
   ```

4. 使用 `chef-automate` 安装 Chef Infra Server。

   ```bash
   $ sudo ./chef-automate deploy --product infra-server
   ...
   Bootstrapping Chef Automate
   Fetching Release Manifest
   Installing Habitat
   Installing Habitat 1.6.181/20201030172917
   Installing the Chef Automate deployment-service
   Installing supplementary Habitat packages
   Installing Habitat package automate-cli
   Installing Habitat package rsync
   Installing Habitat package hab-sup
   Installing Habitat package hab-launcher
   Installing Habitat systemd unit
   Skipping user and group creation (both already exist)
   Starting Habitat with systemd

   Bootstrapping deployment-service on localhost
   Configuring deployment-service
   Starting deployment-service
   Waiting for deployment-service to be ready
   Initializing connection to deployment-service

   Applying Deployment Configuration

   Starting deploy
   Installing deployment-service
   Installing automate-cli
   Installing backup-gateway
   Installing automate-postgresql
   Installing automate-pg-gateway
   Installing automate-elasticsearch
   Installing automate-es-gateway
   Installing pg-sidecar-service
   Installing es-sidecar-service
   Installing license-control-service
   Installing automate-cs-bookshelf
   Installing automate-cs-oc-bifrost
   Installing automate-cs-oc-erchef
   Installing automate-cs-nginx
   Installing automate-load-balancer
   Configuring deployment-service
   Starting backup-gateway
   Starting automate-postgresql
   Starting automate-pg-gateway
   Starting automate-elasticsearch
   Starting automate-es-gateway
   Starting pg-sidecar-service
   Starting es-sidecar-service
   Starting license-control-service
   Starting automate-cs-bookshelf
   Starting automate-cs-oc-bifrost
   Starting automate-cs-oc-erchef
   Starting automate-cs-nginx
   Starting automate-load-balancer

   Checking service health

   Deploy Complete
   Users of this Automate deployment may elect to share user-anonymized usage data with
   Chef Software, Inc. Chef uses this shared data to improve Automate.
   Please visit https://chef.io/privacy-policy for more information about the
   information Chef collects, and how that information is used.
   ```

### 1.3. 创建 Admin 用户和组织

使用 Chef Infra Server 需要创建一个管理用户并至少创建一个组织。用来提供 Server 与 Workstation 之间的安全访问。

```bash
# Usage: chef-server-ctl user-create USERNAME FIRST_NAME [MIDDLE_NAME] LAST_NAME EMAIL PASSWORD
$ sudo chef-server-ctl user-create admin Gao Peng peng.gao@ambow.com "password" --filename admin.pem

# Usage: chef-server-ctl org-create ORG_SHORT_NAME ORG_FULL_NAME (options)
$ sudo chef-server-ctl org-create lab "Ambow Lab" --association_user admin --filename lab-validator.pem
```

这两个命令会生成两个证书文件：`admin.pem` 和 `lab-validator.pem`。在第二节中，我们会复制 `admin.pem` 文件到 Worksation 中，以实现 Chef Infra Server 与 Workstation 之间的安全通信。

## 2. Chef WorkStation

### 2.1. 先决条件

- 一个 Linux、Windows 或 MacOS Workstation。
- 能够访问外网。
- 和 Chef Infra Server 在同一个子网并网络互通。

本次 Chef Infra Server 实验环境为：
| | |
| :----: | :----------------------------: |
| 系统 | Debian 11 |
| 内核 | 5.10.0-10-amd64 |
| 主机名 | workstation.chef.lab |
| 配置 | 4 CPU、8GB 内存、50GB 磁盘空间 |
| 用户 | sysadmin |

### 2.2. 安装 Chef Workstation

Chef Workstation 是一组工具，通过它们可以使 Workstation 与 Server 交互。包括：_chef_、_knife_、_inspec_、_cookstyle_、_habitat_ 和 _kitchen_。还包含 Ruby 和相关依赖项，使我们不需要安装其他任何包就可以使用 Chef Workstation 工具。

安装步骤如下：

1. 通过页面下载[针对不同 OS 的安装包](https://downloads.chef.io/tools/workstation)安装 Chef Workstation。

   ```bash
   $ curl -sLO https://packages.chef.io/files/stable/chef-workstation/21.12.720/debian/11/chef-workstation_21.12.720-1_amd64.deb && sudo dpkg -i chef-workstation_21.12.720-1_amd64.deb

   Selecting previously unselected package chef-workstation.
   (Reading database ... 41805 files and directories currently installed.)
   Preparing to unpack chef-workstation_21.12.720-1_amd64.deb ...
   Unpacking chef-workstation (21.12.720-1) ...
   Setting up chef-workstation (21.12.720-1) ...

   Chef Workstation ships with a toolbar application, the Chef Workstation App.
   To run this application some additional dependencies must be installed.
   Using your platform's package manager to install the 'electron' package is
   the easiest way to meet the dependency requirements.

   You can then launch the App by running 'chef-workstation-app'.
   The App will then be available in the system tray.

   Thank you for installing Chef Workstation!
   You can find some tips on getting started at https://docs.chef.io/workstation/getting_started/
   ```

2. 初始化环境，将 Ruby 可执行路径添加至环境变量中并确保使用的 Ruby 版本为嵌入到 Workstation 中的 Ruby 版本。

   ```bash
   $ echo 'eval "$(chef shell-init bash)"' >> $HOME/.bash_profile
   $ source $HOME/.bash_profile
   ```

3. 验证安装。

   ```bash
   $ chef -v
   Chef Workstation version: 21.12.720
   Chef InSpec version: 4.50.3
   Chef CLI version: 5.4.2
   Chef Habitat version: 1.6.420
   Test Kitchen version: 3.2.2
   Cookstyle version: 7.25.10
   Chef Infra Client version: 17.8.25
   ```

### 2.3. 创建一个 Chef 仓库

使用 Chef 生成器生成一个仓库来存储我们编写的代码。

```bash
$ chef generate repo chef-repo
```

我们通过 `tree` 命令观察生成的目录结构如下：

```bash
$ tree -L 1
.
├── chefignore
├── cookbooks
├── data_bags
├── LICENSE
├── policyfiles
└── README.md
```

可以观察到 `chef generate repo` 命令为我们编写代码提供了基本文件目录结构，简化了开发的所需的步骤。其中 `cookbooks` 目录会存放我们编写的所有食谱和配方，其他目录和文件的含义，我们在后续的学习和实践中会逐渐了解。

### 2.4. 初始化系统

为了使 Workstation 能够与 Chef Infra Server 进行通信和交互，我们需要创建一个凭据。这个凭据需要存放在 HOME 目录下的 `.chef` 目录中。比如 `/home/sysadmin/.chef`。该目录和凭据可以使用生成器创建：

```bash
$ knife configure init-config
WARNING: No knife configuration file found. See https://docs.chef.io/config_rb/ for details.
Please enter the chef server URL: [https://workstation.chef.lab/organizations/myorg] https://automate.chef.lab/organizations/lab
Please enter an existing username or clientname for the API: [sysadmin] admin
*****

You must place your client key in:
  /home/sysadmin/.chef/admin.pem
Before running commands with Knife

*****
Knife configuration file written to /home/sysadmin/.chef/credentials
```

运行上述命令时，终端会交互式向我们确认一些配置。其中包括 Chef Server 的 URL 和设置服务器时创建的用户名。运行完成后会生成 `~/.chef/credentials` 文件，文件内容如下：

```ini
[default]
client_name     = 'admin'
client_key      = '/home/sysadmin/.chef/admin.pem'
chef_server_url = 'https://automate.chef.lab/organizations/lab'
```

编辑凭据文件添加默认的食谱位置，我们使用刚刚创建的 `~/chef-repo/cookbooks` 作为默认的食谱路径，你还可以添加多个路径，比如 `['/path/to/one', '/path/to/two']`。

```ini
cookbook_path   = ['~/chef-repo/cookbooks']
```

最终我们的凭据文件内容如下：

```ini
[default]
client_name     = 'admin'
client_key      = '/home/sysadmin/.chef/admin.pem'
chef_server_url = 'https://automate.chef.lab/organizations/lab'
cookbook_path   = ['~/chef-repo/cookbooks']
```

### 2.5. 从 Chef Infra Server 拷贝用户证书文件至 Workstation

刚才生成的凭据配置文件中 `client_key` 配置项指向 `/home/sysadmin/.chef/admin.pem`，因此需要从 Chef Infra Server 端拷贝至 Workstation 的 `/home/sysadmin/.chef` 目录下。

```bash
$ cd ~/.chef
$ scp sysadmin@automate.chef.lab:/home/sysadmin/admin.pem .
```

### 2.6. 获取并验证证书

使用 `knife ssl fetch` 命令获取证书，证书会将获取到的证书存放在 `~/.chef/trusted_certs` 目录。此目录存放的证书用于提供安全通信，并且会信任此证书。

```$
$ knife ssl fetch
WARNING: Certificates from automate.chef.lab will be fetched and placed in your trusted_cert
       directory (/home/sysadmin/.chef/trusted_certs).

       Knife has no means to verify these are the correct certificates. You should
       verify the authenticity of these certificates after downloading.
Adding certificate for automate_chef_lab in /home/sysadmin/.chef/trusted_certs/automate_chef_lab.crt
```

接下来验证此证书。

```$
$ knife ssl check
Connecting to host automate.chef.lab:443
Successfully verified certificates from `automate.chef.lab'
```

证书成功验证后，使用 `knife client list` 命令来验证 Workstation 和 Chef Infra Server 之间的连接，此命令将返回我们在配置 Chef Infra Server 时，所创建的客户端组织名称。

```bash
$ knife client list
lab-validator
```

### 2.7. 配置开发环境

完成了 Chef Infra Server 和 Chef Workstation 的配置后，我们可以开始使用 Chef 来配置和管理各个节点了。

接着我们使用 VS Code 编辑器进行远程开发并配置好 Chef 插件。

首先添加 Remote - SSH 插件能够在办公电脑上对 Workstation 进行远程开发。

![20211229181839](https://cdn.jsdelivr.net/gh/fib13/image-hosting/images_for_blogs/20211229181839.png)

![20211229183041](https://cdn.jsdelivr.net/gh/fib13/image-hosting/images_for_blogs/20211229183041.png)

然后添加 Chef 扩展，包含自动化完成功能和内置的 Chef 语言参考，能够更加方便编写 Chef 代码。

![20211229194838](https://cdn.jsdelivr.net/gh/fib13/image-hosting/images_for_blogs/20211229194838.png)

## 3. The Chef CookBook

接下来，我们创建一个简单的食谱，可以在运行 _chef-client_ 代理的机器上进行各种操作。

为了让 Chef 实现系统的自动化和合规性检测，Chef Infra Client 会通过

### 3.1. 创建 Cookbook

在 Workstation 上，导航到 `~/chef-repo/cookbooks` 目录并使用 `chef generate cookbook` 生成一个名为 `run-chef-client` 的食谱：

```bash
$ cd ~/chef-repo/cookbooks/

$ chef generate cookbook run-chef-client -k dokken
```

运行完成后会生成 `run-chef-client` 目录，并初始化编写食谱所需的最小文件结构。`-k dokken` 选项会生成使用 Docker 作为驱动器的 Test Kitchen 预配置文件 `kitchen.yml`，我们将在后续步骤使用它。

进入 `run-chef-client` 目录，列出生成的内容，了解 `chef generate cookbook` 生成的所有文件和目录。

```bash
$ cd ~/chef-repo/cookbooks/run-chef-client

$ tree
.
├── CHANGELOG.md
├── chefignore
├── kitchen.yml
├── LICENSE
├── metadata.rb
├── Policyfile.rb
├── README.md
├── recipes
│   └── default.rb
└── test
    └── integration
        └── default
            └── default_test.rb
```

除了这些内容，还需要使用 `chef generate attribute` 生成一个可以保存属性的目录，再手动创建 `./compliance/profiles` 保存合规性配置文件。

```bash
$ cd ~/chef-repo/cookbooks/run-chef-client

$ chef generate attribute default

$ mkdir -p ./compliance/profiles
```

编辑 `metadata.rb` 文件，修改为维护者的信息。这些信息将自动传递到 Chef 生成器创建的文件当中，比如食谱、文件和模板：

```ruby
name 'run-chef-client'
maintainer 'Thirteen'
maintainer_email 'thirteen@chef.lab'
license 'Apache-2.0'
description 'Set chef-client to run every 30 minutes'
version '0.1.0'
chef_version '>= 16.0'
```

我们也可以在生成食谱时就指定 _maintainer_、_maintainer_email_ 和 _license_，例如：

```bash
$ chef generate cookbook run-chef-client -k dokken -C 'Thirteen' -m 'thirteen@chef.lab' -I 'apachev2'
```

### 创建配方

编辑 `./cookbooks/run-chef-client/recipes/default.rb` 文件，并按需更新标题，例如替换作者。并确保用户名和密码能匹配你的环境。这个配方同时可以处理 Windows 和 Linux 系统上的操作。以后可能会有很多不同名称的配方，

```ruby
#
# Cookbook:: run-chef-client
# Recipe:: default
#
# Copyright:: 2021, Thirteen, All Rights Reserved.

include_profile 'run-chef-client::client-run'

if platform?('windows')
  windows_task 'run-chef-client' do
    user 'WIN10\jugggao'
    password ''
    command 'chef-client'
    run_level :highest
    frequency :minute
    frequency_modifier 30
  end
else
  cron 'Run chef-client every 30 minutes' do
    minute '0, 30'
    user 'root'
    command '/usr/bin/chef-client'
    action :create
  end
end
```

## The Chef InSpec Profile

Chef InSpec Profiles 提供了对节点状态及合规性的洞察。也就是说，它会告诉我们所应用的食谱是否处于期望状态。
