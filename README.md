# Traefik

Reverse proxy avec TLS automatique via Let's Encrypt.

## Prérequis

- Docker + Docker Compose installés
- Ports 80, 443 et 8080 ouverts sur le routeur
- Réseau Docker `frontend` existant

## Installation

### 1. Créer le réseau Docker

```bash
sudo docker network create frontend
```

### 2. Cloner le repo

```bash
git clone https://github.com/XPouPouille/traefik.git
cd traefik
```

### 3. Créer le fichier acme.json

```bash
touch acme.json
chmod 600 acme.json
```

### 4. Lancer le container

```bash
sudo docker compose up -d
```

## Accès

- **Dashboard** : http://[ip-serveur]:8080
- **HTTP** : port 80 (redirigé automatiquement vers HTTPS)
- **HTTPS** : port 443

## Exposer un service

Ajouter ces labels sur le container à exposer :

```yaml
labels:
  - "traefik.enable=true"
  - "traefik.docker.network=frontend"
  - "traefik.http.routers.<nom>.rule=Host(`sous-domaine.xavierchapouille.ddns.net`)"
  - "traefik.http.routers.<nom>.entrypoints=websecure"
  - "traefik.http.routers.<nom>.tls.certresolver=letsencrypt"
  - "traefik.http.services.<nom>.loadbalancer.server.port=<port-interne>"
```

Le container doit aussi être connecté au réseau `frontend`.
