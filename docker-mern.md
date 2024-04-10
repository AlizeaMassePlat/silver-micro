
# Configuration de MERN sans Docker Compose ni Dockerfile

Ce guide décrit les étapes pour configurer un environnement MERN (MongoDB, Express.js, React.js, Node.js) en utilisant des conteneurs Docker sans se reposer sur Docker Compose ou des Dockerfiles.

## Création d'un Réseau Docker

Tout d'abord, vérifiez si le réseau `mern_network` existe ; sinon, créez-le :

```bash
if [ -z "$(docker network ls --filter name=^mern_network$ --format="{{ .Name }}")" ]; then
  docker network create mern_network
fi
```

## Démarrage du Conteneur MongoDB

Lancez un conteneur MongoDB au sein du `mern_network` :

```bash
docker run --name mongo_container --network mern_network -p 27017:27017 -d mongo
```

## Démarrage du Conteneur Node.js (API)

Démarrez un conteneur Node.js pour l'API backend. Montez votre répertoire de projet backend comme un volume Docker et définissez l'URL MongoDB :

```bash
docker run --name node_container --network mern_network -v /mnt/c/Users/RYZEN/Desktop/mern_app/backend:/usr/src -w /usr/src -e MONGO_URL="mongodb://mongo_container:27017/mern_app" -p 3000:3000 -d node /bin/bash -c "npm update && npm i && npm start"
```

## Démarrage du Conteneur React.js (Frontend)

Lancez un conteneur React.js pour le frontend. Montez votre répertoire de projet frontend comme un volume Docker et définissez l'URL de l'API :

```bash
docker run --name react_container --network mern_network -v /mnt/c/Users/RYZEN/Desktop/mern_app/frontend:/usr/src -w /usr/src -e REACT_APP_API_URL="http://localhost:3000" -e NODE_OPTIONS=--openssl-legacy-provider -p 3030:3000 -d node /bin/bash -c "npm install && npm start"
```

## Accès aux Applications

- **Serveur Express** : Accessible à `localhost:3000` pour les requêtes API backend.
- **Application React** : Le frontend peut être accédé à `localhost:3030`.
- **MongoDB** : Utilisez MongoDB Compass pour vous connecter à `mongodb://localhost:27017/mern_app` pour accéder à la base de données.
