# Symfony Docker Development Environment

[![Docker](https://img.shields.io/badge/Docker-24-blue?logo=docker)](https://www.docker.com/)
[![Symfony](https://img.shields.io/badge/Symfony-6-black?logo=symfony)](https://symfony.com/)
[![PHP](https://img.shields.io/badge/PHP-8.2-blue?logo=php)](https://www.php.net/)

Ce projet fournit un environnement de développement complet pour **Symfony**, basé sur **Docker**.  
Il inclut PHP 8.2, Composer, Symfony CLI, Xdebug, Nginx, MariaDB, Adminer et Mailhog.

---

## 📂 Structure du projet

symfony-docker/
├─ app/ # Projet Symfony
├─ docker/
│ ├─ php/
│ │ ├─ Dockerfile
│ │ └─ conf.d/xdebug.ini
│ └─ nginx/
│   └─ default.conf
└─ docker-compose.yml

---

## 🐳 Conteneurs inclus

| Service | Conteneur       | Ports                   | Description                                    |
|---------|-----------------|-------------------------|------------------------------------------------|
| PHP     | symfony-php     | -                       | PHP 8.2 FPM avec Composer, Symfony CLI, Xdebug |
| Nginx   | symfony-nginx   | 8080                    | Sert l’application Symfony                     |
| MariaDB | symfony-db      | 3306                    | Base de données                                |
| Adminer | symfony-adminer | 8081                    | Interface web pour gérer la BDD                |
| Mailhog | symfony-mailhog | 8025 (web), 1025 (SMTP) | Test des mails en local                        |

---

## ⚡ Prérequis

- Docker ≥ 24
- Docker Compose ≥ 2

---

## 🚀 Démarrage de l’environnement

1. Construire et démarrer les conteneurs :

```bash
docker compose build
docker compose up -d

Vérifier que les conteneurs tournent :

docker compose ps
```

Accéder à l’application Symfony : http://localhost:8080

Adminer : http://localhost:8081

Serveur : db

Utilisateur : symfony

Mot de passe : symfony

Base: symfony

Mailhog: http://localhost:8025

## 🛠 Commandes utiles

Symfony CLI et Composer dans le conteneur PHP

```bash
docker compose exec php bash
docker compose exec php symfony -v
docker compose exec php composer -v
docker compose exec php php bin/console
```

Xdebug Port : 9003

Configurable via docker/php/conf.d/xdebug.ini

Active le débogage à chaque requête.

Arrêter / redémarrer la stack

```bash
docker compose down
docker compose up -d
docker compose restart
```

## ✉ Mailer Symfony

Dans .env ou .env.local :


MAILER_DSN=smtp://mailhog:1025

Mailhog capture tous les mails envoyés en dev, accessibles via l’interface web sur http://localhost:8025.

## 🗂 Volumes

./app:/var/www/html → code Symfony

./docker/php/conf.d/xdebug.ini:/usr/local/etc/php/conf.d/xdebug.ini → configuration Xdebug

./docker/nginx/default.conf:/etc/nginx/conf.d/default.conf → configuration Nginx

## 🔧 Notes
Nginx pointe vers /var/www/html/public

MariaDB exposé sur le port 3306 pour usage local (Adminer ou IDE)

Symfony CLI et Composer installés dans le conteneur PHP

Xdebug configuré pour host.docker.internal (Linux : remplacer par l’IP de l’hôte)
