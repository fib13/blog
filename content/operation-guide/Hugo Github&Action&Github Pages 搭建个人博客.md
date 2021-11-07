+++
title = "Hugo&Github Action&Github Pages 搭建个人博客"
description = ""
weight = 1
+++

## 介绍

大致的流程如下：

1. 首先通过 Hugo 生成博客网站，并进行配置、添加内容。
2. 然后在 Github 上创建一个 blog 仓库，并将博客源码推送到 blog 仓库的 `main` 分支。
3. 接着使用 Github Action 构建出静态网站，并自动推送至 `gh-pages 分支`。
4. 最后配置 Github Pages 绑定 `gh-pages` 分支上的内容，就可以使用生成或配置的域名进行访问。

## 先决条件

### 准备 Git（必须）

如未安装 Git，参考 [Git 安装指南](https://git-scm.com/book/en/v2/Getting-Started-Installing-Git) 进行安装。

安装完成后，可以通过 `git version` 命令进行验证。

```bash
$ git version
git version 2.29.2.windows.2
```

如果你是第一次安装，则需要初始化 Git 用户信息。将以下内容中邮箱和名字替换为合适的值并运行。

```bash
$ git config --global user.email "you@example.com"
$ git config --global user.name "Your Name"
```

### 准备 Github 仓库（必须）

在 Github 上创建 blog 仓库，你也可以选择其他合适的名称作为博客的仓库。

{{< button style="primary" link="https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211107183013.png" >}} 点击查看配置仓库页面 {{< /button >}}

### 准备 Hugo（必须）

如未安装 Hugo，参考 [Hugo 安装指南](https://gohugo.io/getting-started/installing/) 进行安装。

安装完成后，可以通过 `hugo version` 命令进行验证。

```bash
$ hugo version
hugo v0.89.1-B6A4AE4A+extended windows/amd64 BuildDate=2021-11-05T15:44:32Z VendorInfo=gohugoio
```

### 准备域名（可选）

部署至 Github 上以后，默认会使用 `<username>.github.io` 域名访问你的博客。如果你想使用自己的域名，需要购买一个域名备用。可在[腾讯云](https://buy.cloud.tencent.com/domain)、[阿里云](https://wanwang.aliyun.com/)或其他域名供应商购买。

## 创建博客

### 生成博客

使用 `hugo new site blog` 命令来生成博客源码仓库。

```bash
$ hugo new site blog
Congratulations! Your new Hugo site is created in C:\Users\Peng.Gao\Desktop\Blog\blog.

Just a few more steps and you are ready to go:

1. Download a theme into the same-named folder.
   Choose a theme from https://themes.gohugo.io/ or
   create your own with the "hugo new theme <THEMENAME>" command.
2. Perhaps you want to add some content. You can add single files
   with "hugo new <SECTIONNAME>\<FILENAME>.<FORMAT>".
3. Start the built-in live server via "hugo server".

Visit https://gohugo.io/ for quickstart guide and full documentation.
```

切换当前目录至 `blog` 目录，可以使用 `tree` 命令查看目录的结构。

```bash
$ cd blog

$ tree -L 1
.
|-- archetypes
|-- config.toml
|-- content
|-- data
|-- layouts
|-- static
`-- themes

6 directories, 1 file
```

### 应用主题

你可以选择一个喜欢的[主题](https://themes.gohugo.io/)应用于你的博客，此博客使用的主题为 [Ace documentation](https://themes.gohugo.io/themes/ace-documentation/)。

```bash
$ git submodule add https://github.com/vantagedesign/ace-documentation themes/ace-documentation
Cloning into 'C:/Users/Peng.Gao/Desktop/Blog/docs/themes/ace-documentation'...
remote: Enumerating objects: 555, done.
remote: Counting objects: 100% (139/139), done.
remote: Compressing objects: 100% (90/90), done.
remote: Total 555 (delta 57), reused 88 (delta 35), pack-reused 416
Receiving objects: 100% (555/555), 2.15 MiB | 683.00 KiB/s, done.
Resolving deltas: 100% (191/191), done.
warning: LF will be replaced by CRLF in .gitmodules.
The file will have its original line endings in your working directory
```

接下来需要根据你应用主题的教程来配置 `config.toml` 文件，主题配置教程在刚才你寻找的主题页面上查看。

还可以根据主题的样例去新添加一些内容，在此不过多说明。

### 构建博客

配置完成后，在 `blog` 目录下可以使用 `hugo server` 启动 Web 服务器。Hugo 会构建站点内容到内存中并在检测到文件更改后重新渲染，方便在开发环境中实时预览对站点所做的更改。

```bash
$ hugo server
Start building sites …
hugo v0.89.1-B6A4AE4A+extended windows/amd64 BuildDate=2021-11-05T15:44:32Z VendorInfo=gohugoio

                   | EN
-------------------+-----
  Pages            | 18
  Paginator pages  |  0
  Non-page files   |  0
  Static files     | 26
  Processed images |  0
  Aliases          |  0
  Sitemaps         |  1
  Cleaned          |  0

Built in 173 ms
Watching for changes in C:\Users\Peng.Gao\Desktop\Blog\docs\{archetypes,content,data,layouts,static,themes}
Watching for config changes in C:\Users\Peng.Gao\Desktop\Blog\docs\config.toml
Environment: "development"
Serving pages from memory
Running in Fast Render Mode. For full rebuilds on change: hugo server --disableFastRender
Web Server is available at http://localhost:1313/ (bind address 127.0.0.1)
Press Ctrl+C to stop
```

如果你在远端开发，可以使用 `hugo server --bind 0.0.0.0` 配置绑定的网卡。
如果你想更换端口号，也可以使用 `hugo server -p 80` 配置监听端口号。
如果在调试阶段需要输出处于草稿状态的文章（通过在文章注释中添加 `draft: true` 配置），则需要使用 `hugo server -D` 来渲染草稿。

想了解更多的选项，运行 `hugo --help` 来查看它们的说明。

预览效果满意后，可以使用 `hugo` 命令进行构建。

```bash
$ hugo
Start building sites …
hugo v0.89.1-B6A4AE4A+extended windows/amd64 BuildDate=2021-11-05T15:44:32Z VendorInfo=gohugoio

                   | EN
-------------------+-----
  Pages            | 18
  Paginator pages  |  0
  Non-page files   |  0
  Static files     | 26
  Processed images |  0
  Aliases          |  0
  Sitemaps         |  1
  Cleaned          |  0

Total in 200 ms
```

构建完成后，再观察 `blog` 目录，会发现出现了新的目录为 `public`，这个目录就是我们构建出来的静态网站，通过 `tree` 命令观察 `public`，就是一个静态页面的结构。

```bash
$ tree -L 1 public/
public/
|-- 404.html
|-- categories
|-- css
|-- img
|-- index.html
|-- index.json
|-- js
|-- learn-notes
|-- lib
|-- operation-guide
|-- plugins
|-- sitemap.xml
|-- tags
`-- webfonts

10 directories, 4 files
```

## 部署至 Github

### 推送博客源码

现在，我们就可以将 `blog` 目录中的源码推送至 Github 远程仓库的 `main` 分支了。

但在推送前，应该还需要忽略一些文件。思考一下介绍时所描述的大致步骤，你的博客源码是存储在 `main` 分支的，经过自动构建才会最新生成的 `public` 文件中的内容推送至 `gh-pages` 分支，我们并不需要将 `public` 目录推送至 `main` 分支，因此需要忽略 `public` 目录。此外还可能需要忽略一些其他的文件或目录，比如启动 Web 服务时所生成的 `.hugo_build.lock` 文件和 `resources` 目录，或者由编辑器产生的本地配置目录及文件。以下是我在 `.gitignore` 文件中忽略的内容，仅供参考。

```gitignore
*.lock
*.vscode
*.log
*.idea
resources/
public/
```

添加完 `.gitignore` 文件以后，你可以添加 Github 远程仓库并推送至 `main` 分支。

```bash
$ git init
$ git add .
$ git commit -m "init"
$ git branch -M main
$ git remote add origin https://github.com/jugggao/blog.git
$ git push -u origin main
```

{{< alert style="warning" >}}
**注意**  
需要更换远程仓库 URL 中的名称为你的 Github 用户名。
{{< /alert >}}

### 使用 Github Action 自动构建

我们将博客源码上传至 `main` 分支之后，就需要通过 Github Action 进行自动构建了，并将生成的 `public` 目录中的内容推送至 `gh-pages` 分支。

在 `blog` 目录下新建 `.github/workflows/gh-pages.yml` 文件，文件内容如下。

```yaml
name: Github Pages

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout
        uses: actions/checkout@v2
        with:
          submodules: true
          fetch-depth: 0

      - name: Read .env
        id: hugo-version
        run: |
          . ./.env
          echo "::set-output name=HUGO_VERSION::${HUGO_VERSION}"

      - name: Setup Hugo
        uses: peaceiris/actions-hugo@v2
        with:
          hugo-version: '${{ steps.hugo-version.outputs.HUGO_VERSION }}'
          extended: true

      - name: Build
        run: hugo --minify

      - name: Deploy
        uses: peaceiris/actions-gh-pages@v3
        with:
          github_token: ${{ secrets.ACTIONS_TOKEN }}
          publish_branch: gh-pages
          publish_dir: ./public
          cname: www.shisanshiji.com
          user_name: 'jugggao'
          user_email: 'jugg.gao@qq.com'
          commit_message: ${{ github.event.head_commit.message }}
          enable_jekyll: false
          allow_empty_commit: true
```

此 Github Action 的动作如下：
- 通过 `on.push.branches: ['main']` 指定了只有在推送至 `main` 分支时才会触发此 Action。
- Checkout 阶段进行拉取代码，包括子模块中的代码（上面你克隆的主题代码仓库）。
- Read .env 阶段会读取当前目录中的 `.env` 文件中的 `HUGO_VERSION` 的值，并配置为当前 ACTION 的环境变量。
- Setup hugo 阶段会根据刚才配置的 `HUGO_VERSION` 的版本号配置 Hugo 环境。
- Build 阶段会进行源码构建，构建的静态网站存储在 `public` 目录。
- Deploy 阶段通过 `secrets.ACTIONS_TOKEN` 值进行认证，认证过后再将构建的 `public` 目录中的内容推送到 `gh-pages` 分支。

{{< alert style="warning" >}}
**注意**  
1. 需要替换 Deploy 阶段 `with.user_name` 与 `with.user_email` 的值。
2. 如果你不希望每次都拉取应用主题仓库的最新代码，可以将 Checkout 阶段的 `with.submodules` 的值改为 `False`。
3. 如果你使用 Github 提供的默认域名，则需要删除 `with.cname` 这一行配置。
{{< /alert >}}

你会发现在推送之前还需要两个操作：
1. 创建 `.env` 文件，并指定 Hugo 版本。
2. 生成 Github Token 并在 blog 仓库中配置。

首先，在 `blog` 目录下 `.env` 文件，内容如下所示。你还可以将版本替换为你本地正在使用的版本以避免不确定的错误。

```env
HUGO_VERSION=0.89.1
```

然后，在 Github 中点击用户头像依次点击「Settings」 -> 「Developer settings」->「Personal access tokens」->「Generate new token」 来生成 Token。

{{< button style="primary" link="https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211107205838.png" >}}
点击查看生成 Token 配置页面
{{< /button >}}

最后，回到你的 blog 仓库，点击「Settings」->「Secrets」->「New repository secret」 添加刚才生成的 Token。名称需要和 `gh-pages.yml` 中引用的 `ACTIONS_TOKEN` 环境变量保持一致。

{{< button style="primary" link="https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211107210803.png" >}}
点击查看添加 Token 配置页面
{{< /button >}}

准备完成之后，将新添加的文件推送至远程仓库。

```bash
$ git add .
$ git commit -m "添加 Github Action 相关文件"
$ git push -u origin main
```

这时，你可以在 blog 仓库页面中点击「Actions」观察 Wrokflows 的运行状况。如果发生错误，可以对每个阶段进行展开查看运行日志进行排查。

{{< button style="primary" link="https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211107213117.png" >}}
点击查看 Github Action 运行页面
{{< /button >}}

Github Action 运行成功后，你会发现在 blog 仓库中新增了 `gh-pages` 分支，分支的内容正是之前我们手动构建时生成的 `public` 目录下静态网站。

### 配置 Github Pages

在 blog 仓库页面中依次点击「Settings」->「Pages」。确定你开启了 Pages 功能，配


{{< button style="primary" link="https://cdn.jsdelivr.net/gh/jugggao/image-hosting/images_for_blogs/20211107213117.png" >}}
点击查看 Github Action 运行页面
{{< /button >}}

