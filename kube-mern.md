
# Configuration de MERN avec Kubernetes

Ce guide détaille les étapes pour configurer un environnement MERN (MongoDB, Express, React, Node.js) en utilisant Kubernetes.

## Configuration de Minikube avec Docker

### 1. Pousser les Images dans le Dépôt Privé

Avant de commencer, assurez-vous d'avoir tagué et poussé vos images dans un dépôt privé sur Docker Hub. Utilisez des tags distincts pour le client et le serveur.

```bash
# Tag client
docker tag compose-mern-client:latest alizeamasseplat/myprivaterepo:client-latest
docker push alizeamasseplat/myprivaterepo:client-latest

# Tag server
docker tag compose-mern-server:latest alizeamasseplat/myprivaterepo:server-latest
docker push alizeamasseplat/myprivaterepo:server-latest
```

### 2. Création de Secret pour l'Accès au Registre Privé

Créez un secret Kubernetes pour permettre l'accès à votre dépôt privé.

```bash
kubectl create secret docker-registry myregistrykey --docker-server=https://index.docker.io/v1/ --docker-username=alizeamasseplat --docker-password=myrealpassword --docker-email=alizea.masse@laplateforme.io
```

### 3. Création des Déploiements Kubernetes

Déployez vos applications en utilisant les images de votre dépôt privé. N'oubliez pas d'associer le secret pour l'accès au dépôt.

```bash
# Pour le client
kubectl create deployment client --image=alizeamasseplat/myprivaterepo:client-latest
kubectl patch deployment client -p "{"spec":{"template":{"spec":{"imagePullSecrets":[{"name":"myregistrykey"}]}}}}"
kubectl expose deployment client --type=NodePort --port=3000 --target-port=3000

# Pour le serveur
kubectl create deployment server --image=alizeamasseplat/myprivaterepo:server-latest
kubectl patch deployment server -p "{"spec":{"template":{"spec":{"imagePullSecrets":[{"name":"myregistrykey"}]}}}}"
kubectl expose deployment server --type=NodePort --port=5000 --target-port=5000
```

### MongoDB

Utilisez l'image publique de MongoDB pour le déploiement.

```bash
kubectl create deployment mongo --image=mongo
kubectl expose deployment mongo --type=NodePort --port=27017 --target-port=27017
```

### Gestion des Ressources et Montée en Charge

Augmenter la capacité de charge après avoir vérifier les ressources du node dans le dashboard : 

```bash
 kubectl set resources deployment client --containers="myprivaterepo" --requests=cpu=5,memory=2Gi --limits=cpu=5,memory=2Gi
```

# Recréer et lancer le test de monté en charge après avoir configuré le fichier yaml

```bash
kubectl delete configmap vegeta-targets
kubectl create configmap vegeta-targets --from-file=targets.txt 
kubectl delete job vegeta-attack 
kubectl apply -f vegeta-job.yaml
```

# Modifier le nombre de replicas pour permettre une utilisation plus efficace des ressources disponibles en les répartissant plus uniformément sur les différents nœuds du cluster : 

```bash
kubectl scale deployment client --replicas=20
```

### Connexions

- MongoDB Compass : `mongodb://127.0.0.1:27017/`
- Client : `http://127.0.0.1:3000/`
- Serveur : `http://127.0.0.1:5000/test-mongodb`
