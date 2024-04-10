
# Installation de la pile MERN sur Debian

## Prérequis

- Un système Debian en cours d'exécution.
- Accès root ou sudo au système.

## Étapes

1. **Mettre à jour la liste des paquets**

```bash
apt-get update -y
```

2. **Installer les Dépendances**

```bash
apt-get install gnupg2 curl -y
```

3. **Installation de MongoDB**

- Importer la clé GPG de MongoDB et ajouter le dépôt, puis installer MongoDB :

```bash
apt-get install mongodb-org -y
```

- Démarrer MongoDB et l'activer pour qu'il démarre au boot :

```bash
systemctl start mongod
systemctl enable mongod
```

4. **Installation de Node.js**

- Ajouter le dépôt NodeSource et installer Node.js :

```bash
apt-get install nodejs -y
```

5. **Configuration de l'application React**

- Installer create-react-app globalement et créer une nouvelle application React :

```bash
npm install -g create-react-app
create-react-app reactapp
cd reactapp
npm start 0.0.0.0
```

- Accéder à l'application React à `http://192.168.136.130:3000`.

6. **Configuration d'Express**

- Installer le générateur Express globalement et créer une nouvelle application Express :

```bash
npm install -g express-generator
express mernapp
cd mernapp
npm install
npm start
```

- Accéder au serveur Express à `localhost:3000`.

## Accès à la Base de Données MongoDB

- Pour accéder à la base de données MongoDB, entrer dans le shell MongoDB avec `mongo` et lister les bases de données avec `show dbs`.
