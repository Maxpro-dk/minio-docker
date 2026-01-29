# Installation et Configuration de MinIO avec Docker

## Table des matières
- [Prérequis - Configuration de Docker](#prérequis---configuration-de-docker)
- [Installation de MinIO](#installation-de-minio)
- [Accès à l'interface web MinIO](#accès-à-linterface-web-minio)
- [Commandes utiles](#commandes-utiles)
- [À propos de MinIO](#à-propos-de-minio)

---

## Prérequis - Configuration de Docker

### Créer le groupe Docker
Cette commande crée un nouveau groupe système appelé "docker" qui permettra aux utilisateurs d'exécuter des commandes Docker sans sudo.
```bash
sudo groupadd docker
```

### Ajouter l'utilisateur actuel au groupe Docker
Ajoute votre utilisateur ($USER) au groupe docker pour lui donner les permissions nécessaires.
```bash
sudo usermod -aG docker $USER
```

### Activer le nouveau groupe
Active le groupe docker pour la session actuelle sans avoir besoin de se déconnecter.
```bash
newgrp docker
exec su -l $USER
```

---

## Installation de MinIO

### Télécharger l'image Docker de MinIO
Cette commande télécharge l'image officielle MinIO depuis le registre JumpServer (version spécifique du 11 juin 2024).
```bash
docker pull jumpserver/minio:RELEASE.2024-06-11T03-13-30Z
```

### Démarrer le conteneur MinIO
Lance un conteneur MinIO en mode détaché avec les configurations suivantes :
- **-d** : Mode détaché (arrière-plan)
- **--name minio-server** : Nom du conteneur
- **-p 9000:9000** : Port API MinIO
- **-p 9001:9001** : Port console web MinIO
- **-e MINIO_ROOT_USER** : Nom d'utilisateur administrateur
- **-e MINIO_ROOT_PASSWORD** : Mot de passe administrateur
- **-v minio_data:/data** : Volume persistant pour stocker les données
- **server /data** : Démarre le serveur avec le répertoire de données
- **--console-address ":9001"** : Adresse de la console web
```bash
docker run -d --name minio-server \
-p 9000:9000 -p 9001:9001 \
-e MINIO_ROOT_USER=minioadmin \
-e MINIO_ROOT_PASSWORD=minioadmin \
-v minio_data:/data \
c71d3ceb9899 \
server /data --console-address ":9001"
```

---

## Accès à l'interface web MinIO

### URL de connexion
Accédez à la console web MinIO via votre navigateur :
```
http://127.0.0.1:9001
```

### Identifiants de connexion par défaut
- **Username:** minioadmin
- **Password:** minioadmin

> ⚠️ **Attention** : Changez ces identifiants par défaut en production pour des raisons de sécurité !

---

## Commandes utiles

### Vérifier que le conteneur fonctionne
```bash
docker ps | grep minio-server
```

### Voir les logs du conteneur
```bash
docker logs minio-server
```

### Suivre les logs en temps réel
```bash
docker logs -f minio-server
```

### Arrêter le conteneur
```bash
docker stop minio-server
```

### Redémarrer le conteneur
```bash
docker start minio-server
```

### Supprimer le conteneur
```bash
docker rm -f minio-server
```

### Supprimer le volume de données
```bash
docker volume rm minio_data
```

### Inspecter le conteneur
```bash
docker inspect minio-server
```

---

## À propos de MinIO

MinIO est un serveur de stockage d'objets haute performance compatible avec l'API Amazon S3. Il est parfait pour :
- Stockage de fichiers et d'objets
- Backup et archivage
- Stockage de données pour applications cloud-native
- Data lakes et analytics

---

## Licence

MinIO est distribué sous licence GNU AGPLv3.