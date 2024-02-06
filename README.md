# Palworld Arm64 Dedicated Server

Runs SteamCMD and Palworld with [FEX](https://github.com/FEX-Emu/FEX) (Tested On Ubuntu 22.04 arm64 Raspberry Pi)

[Docker Hub](https://hub.docker.com/r/charliejadek/palworld-arm64-dedicated-server)

## Getting started

1. Create `game` sub-directory on the folder and run `chmod 777 game` for full permissions or use `chown -R 1000:1000 game/`.
2. Download the [docker-compose.yml](docker-compose.yml) and [default.env](default.env) files in a folder of your choice
3. Set up Port-Forwarding or NAT for the ports in the Docker-Compose file.
   - The default port for your server is 8211 (UDP)
   - To use rcon, you need to open 25575 (TCP)
4. Start via `docker compose up -d` (Starts detached, you can use `docker compose down` to stop it)

## Docker Compose
```yml
version: '3.9'
services:
  palworld-server:
    #build: .
    image: 'charliejadek/palworld-arm64-dedicated-server:latest'
    container_name: 'palworld-server'
    ports:
      - target: 8211 # Gamerserver port inside of the container
        published: 8211 # Gamerserver port on your host
        protocol: udp
        mode: host
      - target: 25575 # RCON port inside of the container
        published: 25575 # RCON port on your host
        protocol: tcp
        mode: host
    env_file:
      - ./default.env
    restart: 'unless-stopped'
    volumes:
      - './game:/palworld'
```

### Environment variables

It's based on jammsen/docker-palworld-dedicated-server Environment file.
You need to check jammsen/docker-palworld-dedicated-server [README_ENV.md](https://github.com/jammsen/docker-palworld-dedicated-server/blob/develop/README_ENV.md)

## Run RCON commands

Open a shell into your container via `docker exec -ti palworld-server bash`, then you can run commands against the gameserver via the command `FEXBash 'rconcli --config /home/steam/rcon.yaml'`

```shell
$:~/steamcmd$ FEXBash 'rconcli --config /home/steam/rcon.yaml showplayers'
name,playeruid,steamid
$:~/steamcmd$ FEXBash 'rconcli --config /home/steam/rcon.yaml info'
Welcome to Pal Server[v0.1.3.0] jammsen-docker-generated-20384
$:~/steamcmd$ FEXBash 'rconcli --config /home/steam/rcon.yaml save'
Complete Save
```

> **Important:** Please research the RCON-Commands on the official source: https://tech.palworldgame.com/server-commands

### More information on multithreading and community server (and other settings): https://tech.palworldgame.com/dedicated-server-guide

## Webhook integration

To enable webhook integration, you need to set the following environment variables in the `default.env`:

```shell
WEBHOOK_ENABLED=true
WEBHOOK_URL="https://your.webhook.url"
```
After that the server should send messages in a Discord-Compatible way to your webhook.

### Supported events
* Server starting
* Server stopped

## Deploy with Helm

A Helm chart to deploy this container can be found at [palworld-helm](https://github.com/caleb-devops/palworld-helm).

## If you check the dlmopen error with steamclient.so

It just occur when SteamCMD start. There's no problem with updating palworld server files and running palworld dedicated server.
There was an error because the steamclient.so file could not be found, but by creating symbolic links in the appropriate locations, there are no issues running the PalWorld Dedicated Server.

## Software used

- TeriyakiGod SteamCMD - SteamCMD build file for arm64
- Supercronic - https://github.com/aptible/supercronic
- rcon-cli - https://github.com/gorcon/rcon-cli
- Palworld Dedicated Server (APP-ID: 2394010 - https://steamdb.info/app/2394010/config/)

## Based on
- https://github.com/TeriyakiGod/steamcmd-docker-arm64
- https://github.com/jammsen/docker-palworld-dedicated-server
