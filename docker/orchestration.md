# Quick-Start

![](../.gitbook/assets/flat-550x550-075-f.u1.jpg)

```text
docker --help
```

Pour télécharger une image Docker depuis le site [Docker Hub](https://hub.docker.com/)

`docker pull busybox`

Pour Envoyer une image vers un registre prive \(Dockerhub, quay.io\)

`docker push <private registre>`

Lancer des container

```text
docker run busybox
docker run -it busybox sh
docker run -it -rm prakhar1989/foodtrucks-web bash
```

> le paramètre &lt; -it &gt; Pour lancer les sessions interactives &lt;tty&gt;
>
> le paramètre &lt; -rm &gt; permet de supprimer le container juste après l'avoir arrêter.
>
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

## Networking

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

