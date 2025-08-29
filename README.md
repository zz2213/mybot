## 鸣潮机器人
QQ机器人 NoneBot 镜像构建

### 注意
**仅供学习交流使用，请勿用于非法用途**

## 1. NoneBot 镜像构建
[url https://nonebot.dev/docs/]

### 项目依赖管理
**Poetry**
Poetry 是一个 Python 项目的依赖管理工具。它可以通过声明项目所依赖的库，为你管理（安装/更新）它们。
Poetry 提供了一个 poetry.lock 文件，以确保可重复安装，并可以构建用于分发的项目。

Poetry 会在安装依赖时自动生成 poetry.lock 文件，在项目目录下执行以下命令：
```
# 初始化 poetry 配置
poetry init
# 添加项目依赖，这里以 nonebot2[fastapi] 为例
poetry add nonebot2[fastapi]
```
**PDM**
PDM 是一个现代 Python 项目的依赖管理工具。它采用 PEP621 标准，依赖解析快速；同时支持 PEP582 和 virtualenv。PDM 提供了一个 pdm.lock 文件，
以确保可重复安装，并可以构建用于分发的项目。

PDM 会在安装依赖时自动生成 pdm.lock 文件，在项目目录下执行以下命令：
```
# 初始化 pdm 配置
pdm init
# 添加项目依赖，这里以 nonebot2[fastapi] 为例
pdm add nonebot2[fastapi]
```
### 安装 Docker
Docker 是一个应用容器引擎，可以让开发者打包应用以及依赖包到一个可移植的镜像中，然后发布到服务器上。

我们可以参考 Docker 官方文档 来安装 Docker 。

在 Linux 上，我们可以使用以下一键脚本来安装 Docker 以及 Docker Compose Plugin：
```
curl -fsSL https://get.docker.com | sh -s -- --mirror Aliyun
```
在 Windows/macOS 上，我们可以使用 Docker Desktop 来安装 Docker 以及 Docker Compose Plugin。

### 安装脚手架 Docker 插件
我们可以使用 nb-cli-plugin-docker 来快速部署机器人。

插件可以帮助我们生成配置文件并构建 Docker 镜像，以及启动/停止/重启机器人。使用以下命令安装脚手架 Docker 插件：

```
nb self install nb-cli-plugin-docker
```

### Docker 部署

我们需要事先生成 Docker 配置文件，再到生产环境进行部署；或者自动生成的配置文件并不能满足复杂场景，
需要根据实际需求手动修改配置文件。我们可以使用以下命令来生成基础配置文件：
```
nb docker generate
```

nb-cli 将会在项目目录下生成 docker-compose.yml 和 Dockerfile 等配置文件。在 nb-cli 完成配置文件的生成后，
我们可以根据部署环境的实际情况使用 nb-cli 或者 Docker Compose 来启动机器人。
我们可以参考 Dockerfile 文件规范和 Compose 文件规范修改这两个文件。
修改完成后我们可以直接启动或者手动构建镜像：

```
# 启动机器人
nb docker up
# 手动构建镜像
nb docker build
```