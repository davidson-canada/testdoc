# Configuration d'un VPS

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

### Firewall

```text
sudo ufw allow 80/tcp
sudo ufw allow 443/tcp
sudo ufw allow 22/tcp
ufw enable
sudo ufw status
```

### Fail2Ban

Installation

```text
sudo apt-get update && apt-get upgrade -y
sudo apt-get install fail2ban
sudo apt-get install sendmail-bin sendmail
```

Configuration

Changer fail2bail.local

```text
cp /etc/fail2ban/fail2ban.conf /etc/fail2ban/fail2ban.local
```

{% hint style="info" %}
Pour les version Debian et Ubuntu
{% endhint %}

```text
cp /etc/fail2ban/jail.conf /etc/fail2ban/jail.local
```

Modifier jail.local

{% code-tabs %}
{% code-tabs-item title="/etc/fail2ban/jail.local" %}
```bash
# "backend" specifies the backend used to get files modification.
# Available options are "pyinotify", "gamin", "polling", "systemd" and "auto".
# This option can be overridden in each jail as well.

. . .

backend = systemd
```
{% endcode-tabs-item %}
{% endcode-tabs %}

WhitelistIP

{% code-tabs %}
{% code-tabs-item title="/etc/fail2ban/jail.local" %}
```text
[DEFAULT]

# "ignoreip" can be an IP address, a CIDR mask or a DNS host. Fail2ban will not
# ban a host which matches an address in this list. Several addresses can be
# defined using space separator.
ignoreip = 127.0.0.1/8 123.45.67.89
```
{% endcode-tabs-item %}
{% endcode-tabs %}

Configurer les alertes par mail

{% hint style="info" %}
`destemail`: The email address where you would like to receive the emails.

`sendername`: The name under which the email shows up.

`sender`: The email address from which Fail2ban will send emails.
{% endhint %}

Sauvegarder vos modifications et redémarrer le service

```text
service fail2ban restart
```

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

