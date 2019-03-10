# Swarm-Mode

{% hint style="info" %}
Pre-requis:

Vos nœuds serveurs doivent avoir le client Docker Installe.  
Étapes : 

* Initialiser Docker Swarm pour former un Swarm cluster.
* Créer un réseau Overlay
* Créer les services Pour l'application web et la base MySQL
* Connecter les applications sur le réseau
{% endhint %}

Pour mettre la haute disponibilité de l'infrastructure, on va configurer les nœuds Docker.

Initialiser Docker Swarm  sur le serveur manager

```text
docker swarm init --advertise-addr <IP Publique>
```

> Le paramètre &lt; **--advertise-addr** &gt; configure le nœud manager pour que les autres nœuds du cluster soit visible par le manager.

Joindre le nœud au cluster en tant que Manager

```text
docker swarm join --token SWMTKN-1-3pu6hszjas19xyp7ghgosyx9k8atbfcr8p2is99znpy26u2lkl-7p73s1dx5in4tatdymyhg9hu2 <ip publique>:2377
```

Créer un réseau Overlay pour que les noeuds puissent communiquer entre eux

```text
docker network create -d overlay myoverlaynetwork
```

