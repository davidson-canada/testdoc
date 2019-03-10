# Tips & Astuces

Pour purger les dockers fantômes:

```text
#!/bin/bash
for vol in $(docker volume ls | awk '{print $2}' | grep -v VOLUME)
do
  docker volume rm $vol
Done
```

Pour connaître IP sur une NIC

```text
ip addr show eth0 | grep inet | awk '{ print $2; }' | sed 's/\/.*$//' 

curl -4 icanhazip.com
```

Vérifier si le site web est fonctionnel

```text
curl -f http://localhost/ || exit 1
```

