
# Configuration de LAMP avec Docker Compose

Ce document décrit les étapes pour configurer une pile LAMP (Linux, Apache, MySQL et PHP) en utilisant Docker Compose.

## Pour commencer

Assurez-vous d'avoir Docker et Docker Compose installés sur votre système. Naviguez jusqu'au répertoire contenant votre fichier `docker-compose.yml` qui définit les services de la pile LAMP.

## Exécution de Docker Compose

1. **Changer de Répertoire**

   Tout d'abord, déplacez-vous dans le répertoire où se trouve votre configuration Docker Compose pour LAMP. Si elle est stockée sous `Desktop/compose-lamp`, utilisez la commande suivante pour vous y rendre :

   ```bash
   cd Desktop/compose-lamp
   ```

2. **Construire et Démarrer les Services**

   Utilisez Docker Compose pour construire et démarrer les services définis dans votre fichier `docker-compose.yml`. L'option `--build` assure que les images Docker sont construites en utilisant la dernière version du code de votre projet. Exécutez la commande suivante :

   ```bash
   docker-compose up --build
   ```

   Cette commande démarrera tous les services définis dans votre fichier Docker Compose, tels qu'Apache, MySQL et PHP, dans des conteneurs. Docker Compose construira également toutes les images qui n'existent pas encore.

## Accès aux Services

- **Serveur Web** : Une fois que Docker Compose a terminé la configuration de la pile, vous pouvez accéder au serveur Apache via `http://localhost` sur votre navigateur web.
- **Base de Données** : Le service MySQL peut être accédé en utilisant les outils clients MySQL, en fournissant les identifiants définis dans votre fichier `docker-compose.yml`.

## Gestion de la Pile

Pour arrêter les services, vous pouvez utiliser la commande `docker-compose down`, qui arrête et supprime les conteneurs créés par `docker-compose up`. Pour plus de commandes de gestion et d'options, référez-vous à la documentation de Docker Compose.
