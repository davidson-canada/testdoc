# Capistrano

Installation de Capistrano sur le server leader

```text
sudo apt-get install bundler && sudo apt-get install ruby-full
gem install capistrano
```

Capistrano a besoin d'une arborescence pour fonctionner.  
Lancer la commande ci-dessous.

```text
bundle exec cap install
```

![](../.gitbook/assets/capistrano%20%281%29.png)

Le fichier `deploy.rb` va nous permettre de configurer les variables par défaut dont on va avoir besoin pour le déploiement.

Dans le dossier `config/deploy`, on va retrouver un fichier par environnement

Dans le dossier `lib/capistrano`, on va retrouver un ou plusieurs fichiers contenant un ensemble de tâches disponibles qu'on pourra appeler en fonction des environnements.  
Elles sont optionnelles, mais elles permettent de séparer et regrouper les actions à réaliser pour plus de lisibilité.



