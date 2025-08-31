# 极空间 Docker 搭建 QQ 鸣潮机器人（NoneBot2 + NapCat + GsCore）

## Docker Compose 内容

1. 创建与打包包含 nonebot-plugin-genshinuid 的 NoneBot2 环境
    - 安装脚手架
        - 安装 pipx
       ```shell
        pipx install nonebot2
       ```
        - 安装脚手架
       ```shell
       pipx install nb-cli
       ```
        - 创建项目
            - 使用脚手架来创建一个项目
              ```shell
               nb create
               ```
            - 模板选择 bootstrap
            - 驱动选择 FastAPI
            - 内置插件 echo
            - 安装插件 nb plugin install nonebot-plugin-genshinuid
            - 打包镜像
              ```shell
               # 指定架构打包
                docker build --platform linux/amd64 -t nonebot .
              ```
              ```shell
              # 保存镜像到当前路径下
                docker save -o nonebot.tar nonebot
           ```
          - 111
2. 启动 NapCatQQ 客户端
3. 部署 GsCore

## 部署
1. 复制当前项目 app下文件到 .data/docker/pod/nonebot/app
2. 启动 Docker compose
``` json
networks:`
  wwbot_net:
    driver: bridge
services:
  napcat:
    environment:
      - NAPCAT_UID=1000
      - NAPCAT_GID=1000
      # - WS_URLS='["ws://127.0.0.1:3002/onebot/v11/ws"]'
    ports:
      # - 3000:3000 # HTTP 服务端
      # - 3001:3001 # WS 服务端
      - 6099:6099 # WebUI
    container_name: napcat
    restart: always
    image: mlikiowa/napcat-docker:latest
    volumes:
      - .data/docker/pod/napcat/config:/app/napcat/config
      - .data/docker/pod/napcat/qq_config:/app/.config/QQ
    networks:
      - wwbot_net

  gsuidcore:
    image: lilixxs666/gsuid-core:dev
    container_name: gsuidcore
    restart: always
    volumes:
      - .data/docker/pod/gscore/gscore_data:/gsuid_core/data
      - .data/docker/pod/gscore/gscore_plugins:/gsuid_core/gsuid_core/plugins
    ports:
      - 8765:8765
    environment:
      TZ: Asia/Shanghai
      GSCORE_HOST: 127.0.0.1
    networks:
      - wwbot_net

  nonebot:
    image: nonebot
    pull_policy: never
    container_name: nonebot
    restart: always
    ports:
      - 3002:3002
    volumes:
      - .data/docker/pod/nonebot/app:/app
    environment:
      TZ: Asia/Shanghai
    networks:
      - wwbot_net
   ```

## 配置

### 1. NapCat

#### 1. 获取 WebUI token

- 可以通过`docker logs napcat`查看容器日志，在其中找到形如
  `[WebUI] WebUI Local Panel Url: http://127.0.0.1:6099/webui?token=xxxx` 的 token 信息。

- 也可打开 napcat/config/webui.json 文件，在其中找到 token。

#### 2. 登录 WebUI `http://127.0.0.1:6099/webui/`，进行如下操作：

- 进入 QQ 登录，点击 `QRCode` 进行二维码登录

- 登录成功后，进入网络配置，点击 "新建" 创建 `Websocket 客户端`，URL 填写 `ws://nonebot:3002/onebot/v11/ws`， Token
  自行设置，点击启用后保存

### 2. NoneBot

- 将创建 Websocket 客户端 时填写的 `Token` 写入配置文件 `WutheringWavesUID_in_Docker/nonebot/app/.env` 中
  `ONEBOT_ACCESS_TOKEN=设置的token`

#### 3. GsCore

#### 1. 登录 GsCore 网页控制台： `http://127.0.0.1:8765/genshinuid/`，默认账号 `root/root`，进入之后请**务必**修改密码

#### 2. 进入目录 `gscore/gscore_plugins`，下载鸣潮插件

   ```shell
   cd /gsuid_core/gsuid_core/plugins
   # WutheringWavesUID 鸣潮Bot插件 (可以在 GsCore 网页控制台插件管理中下载)
   git clone -b master https://hk.gh-proxy.com/https://github.com/tyql688/WutheringWavesUID.git --depth=1 --single-branch
   # RoverSign 鸣潮签到插件
   git clone -b main https://hk.gh-proxy.com/https://github.com/tyql688/RoverSign.git --depth=1 --single-branch
   ```

#### 3. 下载完成后重启 GsCore

#### 4. 鸣潮插件配置

1. 登录 GsCore 网页控制台，进入`插件管理/修改插件设定`
2. 进入 WutheringWavesUID 配置
   - 库街区登录配置参照[登录帮助](https://wiki.wavesuid.top/docs/login/yun)、
3. 鸣潮命令 ww帮助
4. 鸣潮签到命令 rs帮助

## 资源/资料

- [早柚核心Docs](https://docs.sayu-bot.com/)
- [鸣潮插件](https://github.com/tyql688/WutheringWavesUID)
- [鸣潮签到插件](https://github.com/tyql688/RoverSign)
- [NoneBot](https://nonebot.dev/)
- [NapCatQQ](https://napneko.github.io/)
- [项目参考](https://github.com/sklun/WutheringWavesUID_in_Docker)