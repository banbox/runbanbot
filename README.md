# RunBanBot
[中文](./README_cn.md)

Use docker/podman to start banbot:
```shell
git clone https://github.com/banbox/runbanbot.git
```
To access the Binance API, you need to set a VPN proxy. You can enter the `runbanbot` directory and edit the `.env` file, changing the value of `BANBOT_PROXY` to your local VPN proxy, for example: `http://host.docker.internal:7897`

> The `host.docker.internal` above is the address used by a Docker container to access the host machine. You may also run `ipconfig` to check your LAN IP and replace it.  
> Note that you also need to enable "allow connections from the LAN" (or similar) in your VPN/proxy software.

Then execute the following command to start:
```shell
cd runbanbot
docker compose up -d
```

Then open [localhost:8000](http://localhost:8000/en-US/) in your browser to access it.

> The `BanDataDir` and `BanStratDir` environment variables are already configured within the container, so you do not need to configure them again when executing commands related to the documentation.
> The database uses the built-in QuestDB, no need to start a separate database service.

## What's next?

[Banbot: From Beginner to Advanced](https://www.bilibili.com/video/BV1b72CBXEQu/)

* **Backtest existing strategies**: [Documentation](https://docs.banbot.site/en-US/guide/backtest)
* **Add new strategies**: [Documentation](https://docs.banbot.site/en-US/guide/strat_custom)
* **Live trading**: [Documentation](https://docs.banbot.site/en-US/guide/live_trading)
* **Advanced customization**: If you want to perform more advanced research, such as using banbot to obtain K-lines of multiple assets during the same period and calculate their correlation, you can download the [banbot](https://github.com/banbox/banbot) source code, open it in an AI IDE, attach [doc/help.md](https://docs.banbot.site/en-US/guide/live_trading) as a knowledge base, and let AI help you write the required code.

## FAQ

#### How to upgrade banbot?
**Method 1: Upgrade by updating `strats` code:**
```shell
git pull origin main
go mod tidy
go build -o bot
docker compose up -d banbot
```
**Method 2: Upgrade by updating `go.mod`:**
```shell
go get -u github.com/banbox/banbot
go mod tidy
go build -o bot
docker compose up -d banbot
```

#### Command 'docker' not found, but can be installed with

Docker is not installed on your machine. You may install either docker or podman:

* [docker](https://docs.docker.com/desktop/)
* [podman](https://podman.io/docs/installation)

> After installing podman, you need to [configure registries.conf](https://podman.io/docs/installation#registriesconf) to use Docker Hub by default; otherwise, image pulling will fail.
> Docker installation includes `docker compose`; if you use podman, you need to install `docker-compose-v2` separately. You may ask AI: How to use docker compose based on podman socket?

#### no matching manifest for linux/arm64/v8 in the manifest list entries

Your computer's CPU architecture is ARM. You need to modify `docker-compose.yml` and uncomment line 30, specifying `platform: linux/amd64`.
