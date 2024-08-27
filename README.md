# v2ray&v2fly-core
## v2ray服务端搭建

官方网站：https://www.v2fly.org/  不翻墙看不到
官方github：https://github.com/v2fly/v2ray-core

### docker安装  【别告诉 我你不会docker】
1. 编排文件

```
version: '3'  #版本

services: #docker-conpose2版本以上需要该设置services
  v2ray:
    image: v2fly/v2fly-core #docker官方源 安全卫生 别瞎搞其他的 小心被挖矿挂马
    restart: always
    container_name: v2fly-core
    network_mode: host
    volumes:
      - /etc/v2ray/config.json:/etc/v2ray/config.json #配置文件位置   服务器的v2ray配置/docker容器系统中的位置

    command:  run -c /etc/v2ray/config.json  #注意  v5和低版本的命令不同此处基于V5版本
``` 


2. 服务器配置文件

```
{
  "log": {
    "loglevel": "warning"
  },
  "routing": {
    "domainStrategy": "IPIfNonMatch",
    "rules": [
      {
        "type": "field",
        "ip": [
          "geoip:private"
        ],
        "outboundTag": "block"
      }
    ]
  },
  "inbounds": [
    {
      "listen": "0.0.0.0",
      "port": 8888,
      "protocol": "vmess",
      "settings": {
        "clients": [
          {
            "id": "此处是要设置的UUID，可以在线工具或者使用V2RAY客户端生成",
            "alterId": 0
          }
        ]
      },
      "streamSettings": {
        "network": "tcp"
      }
    }
  ],
  "outbounds": [
    {
      "protocol": "freedom",
      "tag": "direct"
    },
    {
      "protocol": "blackhole",
      "tag": "block"
    }
  ]
}
```


## v2ray客户端推荐

1. Qv2ray  官方github：https://github.com/Qv2ray/Qv2ray

跨平台 V2Ray 客户端，支持 Linux、Windows、macOS，可通过插件系统支持 SSR / Trojan / Trojan-Go / NaiveProxy 等协议

2. SagerNet  官方github：https://github.com/SagerNet/SagerNet

SagerNet 是一个基于 V2Ray 的 Android 通用代理应用。

3. V2rayN  官方github：https://github.com/2dust/v2rayN

V2RayN 是一个基于 V2Ray 内核的 Windows 客户端。


## 其他安装方式

1. Linuxbrew 包管理器安装（mac也能用，不过应该没人拿mac当服务器飞翔吧。。。）
Linuxbrew 包管理器的使用方式与 Homebrew 一致：brew install v2ray

2. 官方一键安装（不信自己去看：https://github.com/v2fly/fhs-install-v2ray）
   
安装：
`# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh)`

卸载：
`# bash <(curl -L https://raw.githubusercontent.com/v2fly/fhs-install-v2ray/master/install-release.sh) --remove`

3. 源码安装 （算了 前面都不会 这个更白瞎）


## shadowsocket编排文件

version: '3'
services:
  shadowsocks:
    image: shadowsocks/shadowsocks-libev
    environment:
      - PASSWORD=123456
      - SERVER_PORT=8388
      - TIMEOUT=300
      - SERVER_ADDR=0.0.0.0
      - DNS_ADDRS=8.8.8.8,8.8.4.4
      - TZ=Asia/Shanghai
      - METHOD=chacha20-ietf-poly1305
    ports:
      - "8388:8388"
    restart: always
   
