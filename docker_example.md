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

## 11. Cas Pratique : Projet Vue.js avec Docker

### Approche 1 : Dockerfile seul (Application simple)

Pour un projet Vue.js simple, un Dockerfile suffit amplement. Voici comment procéder :

**1. À la racine de votre projet Vue.js, créez un Dockerfile :**

```dockerfile
FROM node:18-alpine

WORKDIR /app

# Copier package.json et package-lock.json
COPY package*.json ./

# Installer les dépendances
RUN npm install

# Copier le code source
COPY . .

# Exposer le port de développement
EXPOSE 5173

# Commande pour lancer le serveur de dev
CMD ["npm", "run", "dev", "--", "--host", "0.0.0.0"]
```

**2. Commandes pour construire et lancer :**

```bash
# Construire l'image
docker build -t mon-vue-app .

# Lancer le conteneur avec volume pour le développement
docker run -d --name vue-dev -p 5173:5173 -v $(pwd):/app -v /app/node_modules mon-vue-app
```

### Approche 2 : Docker Compose (Application complexe)

Docker Compose devient utile quand vous avez :
- **Plusieurs services** (frontend + backend + base de données)
- **Variables d'environnement complexes**
- **Plusieurs volumes à gérer**
- **Configuration réseau spécifique**

**Exemple : Vue.js + API Node.js + PostgreSQL + Migrate**

```yaml
# docker-compose.yml
version: '3.8'

services:
  # Frontend Vue.js
  frontend:
    build: 
      context: ./frontend
      dockerfile: Dockerfile
    ports:
      - "5173:5173"
    volumes:
      - ./frontend:/app
      - /app/node_modules
    environment:
      - VITE_API_URL=http://localhost:3000
    depends_on:
      - backend

  # Backend API Node.js
  backend:
    build:
      context: ./backend
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    volumes:
      - ./backend:/app
      - /app/node_modules
    environment:
      - NODE_ENV=development
      - DATABASE_URL=postgresql://user:password@database:5432/myapp
    depends_on:
      database:
        condition: service_healthy
      migrate:
        condition: service_completed_successfully

  # Base de données PostgreSQL
  database:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: myapp
      POSTGRES_USER: user
      POSTGRES_PASSWORD: password
    volumes:
      - postgres_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"
    healthcheck:
      test: ["CMD-SHELL", "pg_isready -U user -d myapp"]
      interval: 5s
      timeout: 5s
      retries: 5
      start_period: 10s

  # Service de migration avec migrate
  migrate:
    image: migrate/migrate:v4.16.2
    volumes:
      - ./migrations:/migrations
    environment:
      - DATABASE_URL=postgresql://user:password@database:5432/myapp?sslmode=disable
    command: [
      "-path", "/migrations",
      "-database", "postgresql://user:password@database:5432/myapp?sslmode=disable",
      "up"
    ]
    depends_on:
      database:
        condition: service_healthy
    restart: "no"

volumes:
  postgres_data:
```

**Dockerfile pour le backend :**

```dockerfile
# backend/Dockerfile
FROM node:18-alpine

WORKDIR /app

COPY package*.json ./
RUN npm install

COPY . .

EXPOSE 3000

CMD ["npm", "run", "dev"]
```

**Commandes Docker Compose :**

```bash
# Lancer toute la stack
docker-compose up -d

# Voir les logs
docker-compose logs -f

# Arrêter la stack
docker-compose down

# Reconstruire et relancer
docker-compose up --build
```

### Quand utiliser chaque approche ?

| Dockerfile seul | Docker Compose |
|----------------|----------------|
| ✅ Application simple | ✅ Plusieurs services |
| ✅ Développement rapide | ✅ Environnement complet |
| ✅ Un seul conteneur | ✅ Base de données |
| ✅ Prototype | ✅ Production |

### Gestion des migrations avec Migrate

**Structure du projet avec migrations :**

```
projet/
├── frontend/
│   ├── Dockerfile
│   └── ... (fichiers Vue.js)
├── backend/
│   ├── Dockerfile
│   └── ... (fichiers API)
├── migrations/
│   ├── 000001_create_users_table.up.sql
│   ├── 000001_create_users_table.down.sql
│   ├── 000002_add_posts_table.up.sql
│   └── 000002_add_posts_table.down.sql
└── docker-compose.yml
```

**Exemples de fichiers de migration :**

**migrations/000001_create_users_table.up.sql :**
```sql
CREATE TABLE IF NOT EXISTS users (
    id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(255) NOT NULL,
    password_hash VARCHAR(255) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Index sur l'email pour les performances
CREATE INDEX idx_users_email ON users(email);
```

**migrations/000001_create_users_table.down.sql :**
```sql
DROP INDEX IF EXISTS idx_users_email;
DROP TABLE IF EXISTS users;
```

**migrations/000002_add_posts_table.up.sql :**
```sql
CREATE TABLE IF NOT EXISTS posts (
    id SERIAL PRIMARY KEY,
    user_id INTEGER NOT NULL REFERENCES users(id) ON DELETE CASCADE,
    title VARCHAR(255) NOT NULL,
    content TEXT,
    published BOOLEAN DEFAULT FALSE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

-- Index sur user_id pour les performances
CREATE INDEX idx_posts_user_id ON posts(user_id);
CREATE INDEX idx_posts_published ON posts(published);
```

**migrations/000002_add_posts_table.down.sql :**
```sql
DROP INDEX IF EXISTS idx_posts_published;
DROP INDEX IF EXISTS idx_posts_user_id;
DROP TABLE IF EXISTS posts;
```

**Commandes utiles pour les migrations :**

```bash
# Lancer toute la stack (migrations incluses)
docker-compose up -d

# Voir les logs des migrations
docker-compose logs migrate

# Créer une nouvelle migration (depuis le host)
migrate create -ext sql -dir migrations -seq add_comments_table

# Appliquer manuellement les migrations
docker-compose run --rm migrate -path /migrations -database "postgresql://user:password@database:5432/myapp?sslmode=disable" up

# Revenir en arrière d'une migration
docker-compose run --rm migrate -path /migrations -database "postgresql://user:password@database:5432/myapp?sslmode=disable" down 1

# Vérifier la version actuelle
docker-compose run --rm migrate -path /migrations -database "postgresql://user:password@database:5432/myapp?sslmode=disable" version

# Forcer une version spécifique
docker-compose run --rm migrate -path /migrations -database "postgresql://user:password@database:5432/myapp?sslmode=disable" force 2
```

**Alternative : Service de migration personnalisé**

Si vous préférez plus de contrôle, vous pouvez créer votre propre service de migration :

```yaml
# Service de migration personnalisé
migrate:
  build:
    context: ./migrate-service
    dockerfile: Dockerfile
  volumes:
    - ./migrations:/migrations
  environment:
    - DATABASE_URL=postgresql://user:password@database:5432/myapp?sslmode=disable
  depends_on:
    database:
      condition: service_healthy
  restart: "no"
```

**migrate-service/Dockerfile :**
```dockerfile
FROM migrate/migrate:v4.16.2

# Installer des outils supplémentaires si nécessaire
RUN apk add --no-cache postgresql-client

# Script personnalisé de migration
COPY migrate.sh /migrate.sh
RUN chmod +x /migrate.sh

ENTRYPOINT ["/migrate.sh"]
```

**migrate-service/migrate.sh :**
```bash
#!/bin/sh
set -e

echo "🔄 Début des migrations..."

# Attendre que la base soit prête
until pg_isready -h database -p 5432 -U user; do
  echo "⏳ En attente de PostgreSQL..."
  sleep 2
done

# Appliquer les migrations
migrate -path /migrations -database "$DATABASE_URL" up

echo "✅ Migrations terminées avec succès!"
```

## Conclusion

Ce tutorial couvre les bases essentielles de Docker. La maîtrise de ces concepts vous permettra de conteneuriser vos applications efficacement. La pratique régulière est la clé pour devenir à l'aise avec Docker.

N'hésitez pas à expérimenter avec différents types d'applications et configurations pour approfondir vos connaissances !
