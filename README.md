# v2ray&v2fly-core
## v2ray服务端搭建

### docker安装
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

1. Qv2ray  github链接

跨平台 V2Ray 客户端，支持 Linux、Windows、macOS，可通过插件系统支持 SSR / Trojan / Trojan-Go / NaiveProxy 等协议

2. SagerNet  github链接

SagerNet 是一个基于 V2Ray 的 Android 通用代理应用。

3. V2rayN  github链接

V2RayN 是一个基于 V2Ray 内核的 Windows 客户端。
