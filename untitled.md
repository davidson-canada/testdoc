# Configuration du VPS

{% hint style="warning" %}
Pre-requis : 

Connaissance Intermediaire Linux   
Connaissance de Docker et Docker-Compose  
{% endhint %}

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

Installation Docker

```text
sudo apt-get update
sudo apt-get install curl -y
curl -sSL https://get.docker.com/ | sh
```

Installation Docker-Compose

```text
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Ajout de l'utilisateur non-root pour utiliser Docker

```text
sudo usermod -aG docker sammy
```

{% hint style="info" %}
Fermer et ouvrir une session avec l'user non-root
{% endhint %}

Configuration du Firewall

```text
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22/tcp
ufw enable
sudo ufw status
```

Générer un mot de passe chiffre

```text
sudo install apache2-utils
htpasswd -nb -B admin <password> | cut -d ":" -f 2
```

