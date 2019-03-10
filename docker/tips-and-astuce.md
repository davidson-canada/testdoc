# Tips & Astuce

## Nettoyage facon Docker

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

