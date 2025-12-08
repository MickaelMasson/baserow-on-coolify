# Baserow on Coolify (Production Ready)

This repository contains an optimized `docker-compose.yml` configuration for deploying **Baserow** on **Coolify**.

Unlike default templates which may struggle with reverse proxies, this configuration specifically resolves **WebSocket** disconnection errors ("Connection lost", "Reconnecting...") related to **Traefik**.

## ✨ Features

* **Hybrid Architecture:** Uses the `baserow/baserow` (all-in-one) image for simplified internal process management, but connects to external **PostgreSQL** and **Redis** containers for data persistence and security.
* **Working WebSockets:** Specific Traefik configuration to correctly handle persistent connections.
* **S3 Ready:** Pre-configured environment variables for MinIO or AWS S3 file storage.
* **Zero-Config Routing:** Traefik labels are managed directly within the compose file to avoid conflicts with Coolify's UI logic.

## 🚀 Installation on Coolify

### 1. Create Service
In Coolify, create a new service by selecting **"Source: Git Repository"** and use the URL of this repository.

### 2. ⚠️ CRITICAL: Domain Configuration
This is the most important step to avoid "Bad Gateway" errors or reconnection loops.

1.  Go to the **Settings** (or General) tab of your service stack.
2.  **COMPLETELY CLEAR** the **Domains (FQDN)** field. Leave it empty.
3.  Coolify might try to autofill it; **delete it**.
4.  The domain will be handled solely by the `BASEROW_DOMAIN` environment variable.

### 3. Environment Variables
Copy the content of `.env.example` into the **Environment Variables** tab in Coolify and update the values:

* `BASEROW_DOMAIN`: Your actual domain (e.g., `baserow.mydomain.com`). Do not include `https://`.
* `SECRET_KEY` & `BASEROW_JWT_SIGNING_KEY`: Generate long, random, secure strings.
* `DATABASE_PASSWORD` & `REDIS_PASSWORD`: Set strong passwords.

### 4. Deploy
Click **Deploy**.

## 🛠️ Troubleshooting

* **"No Available Server" Error:** Traefik cannot find the container. Ensure the deployment status is "Healthy" and the `BASEROW_DOMAIN` variable is set correctly.
* **Permanent "Reconnecting to server..." message:** This is a WebSocket issue. Double-check that you have **CLEARED** the "Domains" field in the Coolify UI to let the `docker-compose.yml` handle routing priorities.
