<center>Netdata监控</center>





[toc]









## Netdata监控

> Netdata 是一款开源、实时、全栈的系统和应用监控工具，主打“即装即用、零配置、极低资源消耗、秒级可视化”。
>
> 它可以监控服务器、容器、数据库、中间件、网络等数百种指标，内置丰富的图表和告警，支持本地和云端集中管理。
>
> [github](https://github.com/netdata/netdata)







### 1. 安装

```shell
mkdir netdata
sudo chown -R 1001:1001 ./netdata
sudo chmod -R 777 ./netdata
```

```yaml
services:
  netdata:
    image: netdata/netdata:latest
    container_name: netdata
    restart: always
    ports:
      - 19999:19999
    cap_add:
      - SYS_PTRACE
    security_opt:
      - apparmor:unconfined
    volumes:
      - ./config:/etc/netdata
      - ./lib:/var/lib/netdata
      - ./cache:/var/cache/netdata
      - /etc/passwd:/host/etc/passwd:ro
      - /etc/group:/host/etc/group:ro
      - /proc:/host/proc:ro
      - /sys:/host/sys:ro
      - /etc/os-release:/host/etc/os-release:ro
    environment:
      - TZ=Asia/Shanghai
```

> \> - 19999 端口为 Netdata Web UI
>
> \> - ./lib、./cache、./config 实现数据和配置持久化
>
> \> - 通过挂载宿主机的 /proc、/sys 等目录，Netdata 能完整监控主机