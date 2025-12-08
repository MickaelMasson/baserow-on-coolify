# Baserow sur Coolify (Production Ready)

Ce dépôt contient une configuration `docker-compose.yml` optimisée pour déployer **Baserow** sur **Coolify**.

Contrairement aux templates par défaut qui peuvent poser problème, cette configuration résout spécifiquement les erreurs de déconnexion ("Connection lost", "Reconnecting...") liées aux **WebSockets** et au proxy **Traefik**.

## ✨ Fonctionnalités

* **Architecture Hybride :** Utilise l'image `baserow/baserow` (tout-en-un) pour une gestion interne simplifiée des processus, mais connecte une **Base de données (PostgreSQL)** et **Redis** externes pour la persistance et la sécurité.
* **WebSockets fonctionnels :** Configuration Traefik spécifique pour gérer correctement les connexions persistantes.
* **Support S3 :** Prêt à être connecté à MinIO ou AWS S3 pour le stockage de fichiers.
* **Zero-Config Routing :** Les labels Traefik sont gérés directement dans le fichier, évitant les conflits avec l'interface de Coolify.

## 🚀 Installation sur Coolify

### 1. Créer le service
Dans Coolify, créez un nouveau service en sélectionnant **"Source: Git Repository"** et utilisez l'URL de ce dépôt.

### 2. ⚠️ IMPORTANT : Configuration du Domaine
C'est l'étape critique pour éviter les erreurs "Bad Gateway" ou les boucles de reconnexion.

1.  Allez dans l'onglet **Settings** (ou General) du service.
2.  **VIDEZ COMPLÈTEMENT** le champ **Domains (FQDN)**. Ne mettez rien dedans.
3.  Coolify va essayer de le remplir automatiquement, **effacez-le**.
4.  Le domaine sera géré via la variable d'environnement `BASEROW_DOMAIN`.

### 3. Variables d'Environnement
Copiez-collez le contenu du fichier `.env.example` dans l'onglet **Environment Variables** de Coolify et modifiez les valeurs :

* `BASEROW_DOMAIN` : Votre domaine final (ex: `baserow.mondomaine.com`). Ne mettez pas `https://`, juste le domaine.
* `SECRET_KEY` & `BASEROW_JWT_SIGNING_KEY` : Générez des chaînes aléatoires longues et sécurisées.
* `DATABASE_PASSWORD` & `REDIS_PASSWORD` : Choisissez des mots de passe forts.

### 4. Déploiement
Cliquez sur **Deploy**.

## 🛠️ Dépannage

* **Erreur "No Available Server" :** Traefik ne trouve pas le conteneur. Vérifiez que le déploiement est vert (Healthy) et que la variable `BASEROW_DOMAIN` est correcte.
* **Message "Reconnexion au serveur..." permanent :** C'est un problème de WebSocket. Assurez-vous d'avoir bien **VIDÉ** le champ "Domains" dans l'interface Coolify pour laisser le `docker-compose.yml` gérer le routage prioritaire.
