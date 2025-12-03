# RunBanBot
[English](./README.md)

使用docker/podman启动banbot：
```shell
git clone https://github.com/banbox/runbanbot.git
```
访问币安api需要设置VPN代理，您可进入`runbanbot`中编辑`.env`文件，修改`BANBOT_PROXY`值为您的本地VPN代理，如：`http://127.0.0.1:10808`

然后执行下面命令启动即可：
```shell
cd runbanbot
docker compose up -d
```

然后浏览器端打开[localhost:8000](http://localhost:8000/zh-CN/)即可访问。

您也可使用docker仅启动数据库，不启动banbot：
```shell
docker compose up -d timescaledb
```

> 容器内已自动配置`BanDataDir`和`BanStratDir`环境变量，您在执行文档相关命令时无需再次配置

## 下一步？
* **回测已有策略**：[文档](https://docs.banbot.site/zh-CN/guide/backtest)
* **添加新策略**：[文档](https://docs.banbot.site/zh-CN/guide/strat_custom)
* **实时运行**：[文档](https://docs.banbot.site/zh-CN/guide/live_trading)
* **高级自定义**：如果您想进行更高级研究，如使用banbot获取一些品种同一时段K线，计算其相关性等，您可下载[banbot](https://github.com/banbox/banbot)源代码，在AI IDE中打开，附加[doc/help.md](https://docs.banbot.site/zh-CN/guide/live_trading)作为知识库，让AI帮你完成所需代码

## 常见问题
#### Command 'docker' not found, but can be installed with
您本地未安装docker，您可选择安装docker或podman其一
* [docker](https://docs.docker.com/desktop/)
* [podman](https://podman.io/docs/installation)

> podman安装完成后，您需要[配置registries.conf](https://podman.io/docs/installation#registriesconf)默认使用docker hub镜像，否则拉取镜像时会失败  
> docker安装自带`docker compose`；如果使用poaman，您需要额外安装`docker-compose-v2`，可问AI：如何基于podman socket使用docker compose？

#### cannot listen on the TCP port: listen tcp4 :5432: bind: address already in use
您本地已有postgresql在运行，您可修改`.env`中的`PG_PORT`为其他端口，如`5433`，然后重新启动即可

#### no matching manifest for linux/arm64/v8 in the manifest list entries
您电脑CPU架构是arm，需要修改`docker-compose.yml`对第30行取消注释，指定`platform: linux/amd64`
