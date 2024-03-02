# v2ray-v2fly-core
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
```


##v2ray客户端推荐
