# Quick-Start

{% hint style="info" %}
[https://container.training/swarm-selfpaced.yml.html\#1](https://container.training/swarm-selfpaced.yml.html#1)\`\`

`docker --help`
{% endhint %}

\`\`

## Images Docker

Le site [Docker Hub](https://hub.docker.com/) fournit une base d'image preconfigure et fonctionnel.

```text
#Telecharge une image depuis un registre (Dockerhub, quay.io)
docker pull busybox
# Envoyer une image vers un registre (Dockerhub, quay.io)
docker push <private registre>
```

## Run Docker

Lancer des containers

```text
#Lance un conteneur Busybox

docker run busybox

#le paramètre < -it > Pour lancer les sessions interactives <tty>

docker run -it busybox sh

#le paramètre < -rm > permet de supprimer le container juste après l'avoir arrêter.

docker run -it -rm prakhar1989/foodtrucks-web bash
```

> Pratique pour les tests &lt;run Once&gt;

Arrêter les containers

```text
docker stop busybox
docker rm busybox
```

Pour afficher les containers en cours d’exécutions

```text
docker ps
docker ps -a
```

Gérer les images docker

```text
docker images
docker rmi <images>
```

## Reseaux

Pour afficher les interfaces réseaux

```text
docker network ls
```

## Volumes

```text
docker volume list
docker volume rm VOLUME-NAME
```

## Construire une Image Docker

Dockerfile est une simple fichier texte \(sans extension\) qui contient une liste de commande auquel le client Docker fait appel quand il crée une image.  
C'est un moyen d'automatiser le processus de création et de personnalisation d'images.

Dans un fichier &lt;Dockerfile&gt;

```text
# our base image
FROM python:3-onbuild

# specify the port number the container should expose
EXPOSE 5000

# run the application
CMD ["python", "./app.py"]
```

Pour "builder" une image :

```text
docker build -t prakhar1989/catnip .
```

## Logs

```text
docker logs <image-name>
```

