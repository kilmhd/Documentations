# Tutorial Docker Complet - Connaissances de Base

## 1. Introduction à Docker

### Qu'est-ce que Docker ?
Docker est une plateforme de conteneurisation qui permet d'empaqueter une application et ses dépendances dans un conteneur léger et portable. Contrairement aux machines virtuelles, Docker partage le noyau de l'OS hôte, ce qui le rend plus efficace.

### Concepts clés
- **Image** : Template en lecture seule contenant le code, les dépendances et la configuration
- **Conteneur** : Instance en cours d'exécution d'une image
- **Dockerfile** : Fichier de configuration pour créer une image
- **Registry** : Dépôt d'images (Docker Hub par exemple)

## 2. Installation de Docker

### Windows et Mac
1. Téléchargez Docker Desktop depuis le site officiel
2. Suivez l'assistant d'installation
3. Redémarrez votre machine
4. Vérifiez avec : `docker --version`

### Linux (Ubuntu/Debian)
```bash
# Mise à jour des paquets
sudo apt update

# Installation des dépendances
sudo apt install apt-transport-https ca-certificates curl gnupg lsb-release

# Ajout de la clé GPG Docker
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

# Ajout du dépôt Docker
echo "deb [arch=amd64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

# Installation de Docker
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io

# Ajout de l'utilisateur au groupe docker
sudo usermod -aG docker $USER
```

## 3. Commandes Docker Essentielles

### Gestion des images
```bash
# Lister les images locales
docker images

# Télécharger une image
docker pull nginx:latest

# Supprimer une image
docker rmi nginx:latest

# Construire une image depuis un Dockerfile
docker build -t mon-app:1.0 .
```

### Gestion des conteneurs
```bash
# Lancer un conteneur
docker run -d --name mon-nginx -p 8080:80 nginx

# Lister les conteneurs en cours
docker ps

# Lister tous les conteneurs (actifs et arrêtés)
docker ps -a

# Arrêter un conteneur
docker stop mon-nginx

# Démarrer un conteneur arrêté
docker start mon-nginx

# Supprimer un conteneur
docker rm mon-nginx

# Exécuter une commande dans un conteneur
docker exec -it mon-nginx bash
```

### Commandes d'inspection et de logs
```bash
# Voir les logs d'un conteneur
docker logs mon-nginx

# Inspecter un conteneur
docker inspect mon-nginx

# Statistiques en temps réel
docker stats

# Voir les processus dans un conteneur
docker top mon-nginx
```

## 4. Création d'un Dockerfile

### Structure basique
```dockerfile
# Image de base
FROM node:16-alpine

# Définir le répertoire de travail
WORKDIR /app

# Copier le package.json
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier le code source
COPY . .

# Exposer le port
EXPOSE 3000

# Commande de démarrage
CMD ["npm", "start"]
```

### Instructions Dockerfile importantes
- **FROM** : Image de base
- **WORKDIR** : Répertoire de travail
- **COPY/ADD** : Copier des fichiers
- **RUN** : Exécuter des commandes pendant la construction
- **CMD/ENTRYPOINT** : Commande par défaut du conteneur
- **EXPOSE** : Déclarer les ports utilisés
- **ENV** : Variables d'environnement

### Exemple complet : Application Node.js
```dockerfile
FROM node:16-alpine

# Créer un utilisateur non-root
RUN addgroup -g 1001 -S nodejs
RUN adduser -S nodejs -u 1001

# Définir le répertoire de travail
WORKDIR /app

# Copier les fichiers de dépendances
COPY package*.json ./

# Installer les dépendances
RUN npm ci --only=production

# Copier le code source
COPY --chown=nodejs:nodejs . .

# Changer d'utilisateur
USER nodejs

# Exposer le port
EXPOSE 3000

# Commande de démarrage
CMD ["node", "server.js"]
```

## 5. Docker Compose

### Qu'est-ce que Docker Compose ?
Docker Compose permet de définir et gérer des applications multi-conteneurs avec un fichier YAML.

### Fichier docker-compose.yml basique
```yaml
version: '3.8'

services:
  web:
    build: .
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=development
    volumes:
      - .:/app
      - /app/node_modules
    depends_on:
      - database

  database:
    image: postgres:13
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  postgres_data:
```

### Commandes Docker Compose
```bash
# Lancer tous les services
docker-compose up -d

# Arrêter tous les services
docker-compose down

# Voir les logs de tous les services
docker-compose logs

# Reconstruire et relancer
docker-compose up --build

# Exécuter une commande dans un service
docker-compose exec web bash
```

## 6. Volumes et Persistance des Données

### Types de volumes
1. **Volumes nommés** : Gérés par Docker
2. **Bind mounts** : Liaison avec le système de fichiers hôte
3. **tmpfs mounts** : Stockage en mémoire

### Exemples d'utilisation
```bash
# Volume nommé
docker run -v mon-volume:/data nginx

# Bind mount
docker run -v /chemin/host:/chemin/container nginx

# Volume temporaire
docker run --tmpfs /tmp nginx
```

## 7. Réseaux Docker

### Types de réseaux
- **bridge** : Réseau par défaut
- **host** : Utilise le réseau de l'hôte
- **none** : Aucune connectivité réseau

### Gestion des réseaux
```bash
# Créer un réseau
docker network create mon-reseau

# Lister les réseaux
docker network ls

# Connecter un conteneur à un réseau
docker network connect mon-reseau mon-conteneur

# Inspecter un réseau
docker network inspect mon-reseau
```

## 8. Bonnes Pratiques

### Optimisation des images
1. **Utilisez des images de base légères** (alpine)
2. **Multi-stage builds** pour réduire la taille
3. **Minimisez le nombre de layers**
4. **Utilisez .dockerignore**

### Exemple de multi-stage build
```dockerfile
# Stage de build
FROM node:16-alpine AS builder
WORKDIR /app
COPY package*.json ./
RUN npm ci
COPY . .
RUN npm run build

# Stage de production
FROM nginx:alpine
COPY --from=builder /app/dist /usr/share/nginx/html
EXPOSE 80
CMD ["nginx", "-g", "daemon off;"]
```

### Fichier .dockerignore
```
node_modules
npm-debug.log
.git
.gitignore
README.md
.env
coverage
.nyc_output
```

### Sécurité
1. **N'utilisez pas l'utilisateur root**
2. **Scannez vos images** pour les vulnérabilités
3. **Utilisez des images officielles**
4. **Gardez vos images à jour**

## 9. Débogage et Monitoring

### Techniques de débogage
```bash
# Exécuter un shell dans un conteneur
docker exec -it conteneur-id /bin/bash

# Copier des fichiers
docker cp conteneur-id:/chemin/fichier ./fichier

# Surveiller les ressources
docker stats conteneur-id

# Inspecter les logs en temps réel
docker logs -f conteneur-id
```

### Health checks
```dockerfile
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD curl -f http://localhost:3000/health || exit 1
```

## 10. Exercices Pratiques

### Exercice 1 : Application Web Simple
1. Créez un Dockerfile pour une application Node.js
2. Construisez l'image
3. Lancez le conteneur avec port mapping
4. Testez l'application

### Exercice 2 : Stack complète avec Docker Compose
1. Créez un docker-compose.yml avec :
   - Application web (Node.js/Python)
   - Base de données (PostgreSQL/MySQL)
   - Reverse proxy (Nginx)
2. Configurez les volumes pour la persistance
3. Testez la communication entre services

### Exercice 3 : Optimisation d'image
1. Créez une image basique
2. Optimisez-la avec multi-stage build
3. Comparez les tailles avant/après

## Ressources Utiles

- **Documentation officielle** : https://docs.docker.com/
- **Docker Hub** : https://hub.docker.com/
- **Awesome Docker** : Collection de ressources Docker
- **Play with Docker** : Environnement en ligne pour pratiquer

## Conclusion

Ce tutorial couvre les bases essentielles de Docker. La maîtrise de ces concepts vous permettra de conteneuriser vos applications efficacement. La pratique régulière est la clé pour devenir à l'aise avec Docker.

N'hésitez pas à expérimenter avec différents types d'applications et configurations pour approfondir vos connaissances !