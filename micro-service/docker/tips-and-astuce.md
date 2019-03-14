# Tips & Astuce

Controle de l'usage disque

```text
docker system df
```

## Nettoyage facon Docker

Supprime tout dans docker

```text
docker system prune -f
```

Supprime les containers

```text
docker rm $(docker ps -a -q)
```

Efface toutes les images

```text
docker rmi $(docker images -q)
```

Efface tous les volumes

```text
docker volume rm $(docker volume list -q)
```

Efface les images non-tagues

```text
docker rmi $(docker images | grep "^<none>" | awk "{print $3}")
```

## Secrets

Generer un secret

```text
base64 /dev/urandom | head -c16 | docker secret create arewesecureyet -
```

Acceder a un secret

```text
Find the ID of the container for the dummy service:
CID=$(docker ps -q --filter label=com.docker.swarm.service.name=dummyservice)

Enter the container:
docker exec -ti $CID sh
Check the files in /run/secrets

Check the contents of the secret files:
docker exec $CID grep -r . /run/secrets


```

