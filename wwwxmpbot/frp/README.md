## FRP内网穿透（NAS + 阿里云windows服务器）

#### 1.下载文件
```shell
Invoke-WebRequest -Uri https://github.com/fatedier/frp/releases/download/v0.64.0/frp_0.64.0_windows_amd64.zip -OutFile frp.zip -UseBasicParsing
Expand-Archive -Path frp.zip -DestinationPath C:\frp
```
#### 2.编辑配置文件frps.toml
```shell
bindPort = 7000
auth.method = "token"
auth.token = "your token"
```

#### 启动
```shell
frps -c frps.toml
```

### 客户端配置

```shell
serverAddr = "服务器公网ip"
serverPort = 7000
auth.method = "token"
auth.token = "your token"

[[proxies]]
name = "nas-cilent-4"
type = "tcp"
localIP = "192.168.3.60"
localPort = 5700 # 本地端口
remotePort = 80 # 远程端口
```

