# RunBanBot
[中文](./README_cn.md)

Use docker/podman to start banbot:

```shell
git clone https://github.com/banbox/runbanbot.git
cd runbanbot
docker compose up -d
```

Then open [localhost:8000](http://localhost:8000/en-US/) in your browser to access it.

If your network environment cannot access the Binance API directly, you will need to set a VPN proxy. You can open the `runbanbot` directory and edit the `.env` file, then change the value of `BANBOT_PROXY` to your local VPN proxy, for example: `http://host.docker.internal:7890`.

> The `host.docker.internal` above is the address used by a Docker container to access the host machine. You may also run `ipconfig` to check your LAN IP and replace it.  
> Note that you also need to enable “allow connections from the LAN” (or similar) in your VPN/proxy software.

You can also start the database only using Docker, without starting banbot:

```shell
docker compose up -d timescaledb
```

> The `BanDataDir` and `BanStratDir` environment variables are already configured within the container, so you do not need to configure them again when executing commands related to the documentation.

## What’s next?

[Banbot: From Beginner to Advanced](https://www.bilibili.com/video/BV1b72CBXEQu/)

* **Backtest existing strategies**: [Documentation](https://docs.banbot.site/en-US/guide/backtest)
* **Add new strategies**: [Documentation](https://docs.banbot.site/en-US/guide/strat_custom)
* **Live trading**: [Documentation](https://docs.banbot.site/en-US/guide/live_trading)
* **Advanced customization**: If you want to perform more advanced research, such as using banbot to obtain K-lines of multiple assets during the same period and calculate their correlation, you can download the [banbot](https://github.com/banbox/banbot) source code, open it in an AI IDE, attach [doc/help.md](https://docs.banbot.site/en-US/guide/live_trading) as a knowledge base, and let AI help you write the required code.

## FAQ

#### how to upgrade banbot?
```shell
docker compose pull banbot
docker compose up -d banbot
```

#### Command 'docker' not found, but can be installed with

Docker is not installed on your machine. You may install either docker or podman:

* [docker](https://docs.docker.com/desktop/)
* [podman](https://podman.io/docs/installation)

> After installing podman, you need to [configure registries.conf](https://podman.io/docs/installation#registriesconf) to use Docker Hub by default; otherwise, image pulling will fail.
> Docker installation includes `docker compose`; if you use podman, you need to install `docker-compose-v2` separately. You may ask AI: How to use docker compose based on podman socket?

#### cannot listen on the TCP port: listen tcp4 :5432: bind: address already in use

PostgreSQL is already running on your machine. You can change `PG_PORT` in `.env` to another port, such as `5433`, then restart.

#### no matching manifest for linux/arm64/v8 in the manifest list entries

Your computer's CPU architecture is ARM. You need to modify `docker-compose.yml` and uncomment line 30, specifying `platform: linux/amd64`.
