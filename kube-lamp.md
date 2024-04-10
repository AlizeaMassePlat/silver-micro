
# Configuration de LAMP avec Kubernetes

Ce guide détaille les étapes pour configurer un environnement LAMP (Linux, Apache, MySQL, PHPMyAdmin) en utilisant Kubernetes.

## 1. Pousser les Images dans le Dépôt Privé

Avant de commencer, assurez-vous d'avoir tagué et poussé vos images dans un dépôt privé sur Docker Hub.

```bash
# Tag web
docker tag compose-lamp-web:latest alizeamasseplat/myprivaterepo:web-latest
docker push alizeamasseplat/myprivaterepo:web-latest
```

## 2. Utilisation de la Clé Secrète pour Kubernetes

La clé secrète nécessaire pour accéder au registre privé a déjà été créée dans le cadre de la configuration pour MERN. La même clé sera utilisée ici.

## 3. Création des Déploiements Kubernetes

### Pour le Web

Créez un déploiement pour le serveur web en utilisant l'image dans votre dépôt privé et exposez-le sur le port 80.

```bash
kubectl create deployment web --image=alizeamasseplat/myprivaterepo:web-latest
kubectl patch deployment web -p "{"spec":{"template":{"spec":{"imagePullSecrets":[{"name":"myregistrykey"}]}}}}"
kubectl expose deployment web --type=NodePort --port=80 --target-port=80
```

### PhpMyAdmin

Déployez PhpMyAdmin en utilisant une image publique et exposez-le.

```bash
kubectl create deployment phpmyadmin --image=phpmyadmin/phpmyadmin
kubectl expose deployment phpmyadmin --type=NodePort --port=8080 --target-port=80
```

### MySQL

Configurez un déploiement pour MySQL, définissez un mot de passe root et exposez le service sur le port 3306.

```bash
kubectl create deployment mysql --image=mysql:5.7
kubectl set env deployment/mysql MYSQL_ROOT_PASSWORD=root
kubectl expose deployment mysql --type=NodePort --port=3306 --target-port=3306
```

Utilisez ces commandes pour configurer un environnement LAMP complet sur Kubernetes, incluant un serveur web, PhpMyAdmin pour la gestion de la base de données, et MySQL comme système de gestion de base de données.
