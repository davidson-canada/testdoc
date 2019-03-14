# Docker-Compose

How to Build, Ship and Run avec Docker Compose

```text
export TAG=v0.2
docker-compose -f dockercoins.yml build
docker-compose -f dockercoins.yml push
docker stack deploy -c dockercoins.yml dockercoins
```

