# Configuration du VPS

Se connecter sur le VPS

```text
ssh root@your_server_ip
```

Créer un utilisateur non-Root

```text
adduser sammy
```

Ajouter les droits "sudo" pour l'utilisateur créer

```text
usermod -aG sudo sammy
```

Installation Docker et Docker-Compose

```text
sudo apt update
sudo apt install apt-transport-https ca-certificates curl software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
sudo apt update
apt-cache policy docker-ce
sudo apt install docker-ce
```

Ajout de l'utilisateur non-root pour utiliser Docker

```text
sudo usermod -aG docker sammy
```

Fermer et ouvrir une session avec l'user non-root




