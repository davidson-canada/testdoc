# Configuration du VPS

{% hint style="warning" %}
Pre-requis : 

Connaissance Intermédiaire Linux   
Connaissance de Docker, Docker-Compose et Docker-Swarm  
Connaissance de Kubernetes est un plus  
{% endhint %}

## O.S

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

### Installation Docker

```text
sudo apt-get update
sudo apt-get install curl -y
curl -sSL https://get.docker.com/ | sh
```

### Installation Docker-Compose

```text
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
```

Ajout de l'utilisateur non-root pour utiliser Docker

```text
sudo usermod -aG docker sammy
```

{% hint style="warning" %}
Fermer et ouvrir une session avec l'user non-root
{% endhint %}

## Sécurité

### Configuration du Firewall

```text
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22/tcp
ufw enable
sudo ufw status
```

### Configuration Fail2Ban

### Générer un mot de passe chiffré

```text
sudo install apache2-utils
htpasswd -nb -B admin <password> | cut -d ":" -f 2
```

## Stockage

Installation GlusterFS

```text
sudo apt update && sudo apt upgrade
sudo systemctl stop docker
sudo apt install -y glusterfs-server
sudo systemctl start glusterfs-server
#Create GlusterFS storage for bricks:
sudo mkdir -p /gluster/data /swarm/volumes
```

Paramétrage GlusterFS sur chaque Nœud

```text
sudo mkfs.xfs /dev/xvdb 
sudo mount /dev/xvdb /gluster/data/
```

Test de vérification depuis le nœud 1

```text
sudo gluster peer probe node2
peer probe: success. 
sudo gluster peer probe node3
peer probe: success.
```

Créer le volume miroir

```text
sudo gluster volume create swarm-vols replica 3 node1:/gluster/data node2:/gluster/data node3:/gluster/data force
volume create: swarm-vols: success: please start the volume to access data
```

Autoriser les accès depuis le localhost

```text
sudo gluster volume set swarm-vols auth.allow 127.0.0.1
volume set: success
```

Démarrer le volume

```text
sudo gluster volume start swarm-vols
volume start: swarm-vols: success
```

Sur chaque noeud Gluster, on monte le miroir partage localement

```text
sudo mount.glusterfs localhost:/swarm-vols /swarm/volumes
```

