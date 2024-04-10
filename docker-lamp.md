
# LAMP sans docker compose avec réseau

## 1. Créer un réseau Docker

La première étape consiste à créer un réseau Docker pour permettre à vos conteneurs de communiquer entre eux de manière isolée.

```bash
docker network create lamp-network
```

## 2. Lancer le conteneur MySQL

Créez et lancez un conteneur pour le serveur MySQL. Vous devez définir un mot de passe pour l'utilisateur root via la variable d'environnement `MYSQL_ROOT_PASSWORD`.

```bash
docker run --name mysql-container --network lamp-network -e MYSQL_ROOT_PASSWORD=rootpassword -d mysql:5.7
```

## 3. Lancer le conteneur Apache avec PHP

Pour le serveur web, utilisez une image qui contient à la fois Apache et PHP. Montez un dossier local sur votre machine hôte dans le conteneur pour rendre vos fichiers PHP accessibles.

```bash
docker run --name apache-php-container --network lamp-network -p 8888:80 -v ${pwd}/html:/var/www/html -d php:7.4-apache
```

## Installer `mysqli` dans le conteneur

Dans le shell du conteneur, exécutez les commandes suivantes pour installer l'extension `mysqli`.

```bash
docker exec -it apache-php-container /bin/bash
docker-php-ext-install mysqli
docker-php-ext-enable mysqli
```

## 4. Lancer phpMyAdmin

Pour gérer votre base de données MySQL via une interface web, vous pouvez lancer un conteneur phpMyAdmin connecté à votre réseau Docker.

```bash
docker run --name phpmyadmin-container --network lamp-network -e PMA_HOST=mysql-container -p 8080:80 -d phpmyadmin/phpmyadmin
```

## Accès aux services

- **Site Web**: Après avoir démarré le conteneur Apache avec PHP, votre site devrait être accessible via `http://localhost:8888`.
- **phpMyAdmin**: Si vous avez choisi de lancer phpMyAdmin, il sera accessible via `http://localhost:8080`. Utilisez `root` comme nom d'utilisateur et `rootpassword` comme mot de passe.
