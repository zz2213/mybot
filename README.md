## 鸣潮机器人
QQ机器人 NoneBot 镜像构建

### 注意
**仅供学习交流使用，请勿用于非法用途**

## 1. NoneBot 镜像构建


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
# Docker Compose 搭建 QQ 鸣潮机器人（NoneBot2 + NapCat + GsCore）

## Docker Compose 内容

1. 部署包含 nonebot-plugin-genshinuid 的 NoneBot2 环境
2. 启动 NapCatQQ 客户端
3. 部署 GsCore



## 部署

1. Docker 环境安装不再赘述

2. 下载项目

   ```shell
   git clone https://github.com/sklun/WutheringWavesUID_in_Docker.git
   ```

3. 构建 NoneBot 镜像

   ```shell
   cd WutheringWavesUID_in_Docker/nonebot
   docker build -t nonebot . 
   ```

4. 启动 Docker compose

   ```shell
   cd ..
   docker compose up -d
   ```

5. 部署完成后的目录结构

   ```shell
   .
   ├── compose.yaml
   ├── gscore
   │   ├── gscore_data
   │   └── gscore_plugins
   ├── napcat
   │   ├── config
   │   └── qq_config
   ├── nonebot
   │   ├── app
   │   └── Dockerfile
   └── README.md
   ```




## 配置

1. NapCat

1. 获取 WebUI token

- 可以通过`docker logs napcat`查看容器日志，在其中找到形如 `[WebUI] WebUI Local Panel Url: http://127.0.0.1:6099/webui?token=xxxx` 的 token 信息。

- 也可打开 napcat/config/webui.json 文件，在其中找到 token。

2. 登录 WebUI `http://127.0.0.1:6099/webui/`，进行如下操作：

- 进入 QQ 登录，点击 `QRCode` 进行二维码登录

- 登录成功后，进入网络配置，点击 "新建" 创建 `Websocket 客户端`，URL 填写 `ws://nonebot:3002/onebot/v11/ws`， Token 自行设置，点击启用后保存

2. NoneBot

- 将创建 Websocket 客户端 时填写的 `Token` 写入配置文件 `WutheringWavesUID_in_Docker/nonebot/app/.env` 中 `ONEBOT_ACCESS_TOKEN=设置的token`

3. GsCore

1. 登录 GsCore 网页控制台： `http://127.0.0.1:8765/genshinuid/`，默认账号 `root/root`，进入之后请**务必**修改密码

2. 进入目录 `gscore/gscore_plugins`，下载鸣潮插件

   ```shell
   # WutheringWavesUID 鸣潮Bot插件 (可以在 GsCore 网页控制台插件管理中下载)
   git clone -b master https://github.com/tyql688/WutheringWavesUID.git --depth=1 --single-branch
   # RoverSign 鸣潮签到插件
   git clone -b main https://github.com/tyql688/RoverSign.git --depth=1 --single-branch
   ```

3. 下载完成后重启 GsCore

   ```shell
   docker restart gsuidcore


4. 鸣潮插件配置

1. 登录 GsCore 网页控制台，进入`插件管理/修改插件设定`
2. 进入 WutheringWavesUID 配置

3. 库街区登录配置参照插件[补充文档地址](https://wiki.wavesuid.top/)中的"鸣潮登录帮助"

## 资源/资料

- [早柚核心Docs](https://docs.sayu-bot.com/)
- [鸣潮插件](https://github.com/tyql688/WutheringWavesUID)
- [鸣潮签到插件](https://github.com/tyql688/RoverSign)
- [NoneBot](https://nonebot.dev/)
- [NapCatQQ](https://napneko.github.io/)
- [项目参考](https://github.com/sklun/WutheringWavesUID_in_Docker)