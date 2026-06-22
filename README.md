# Laravel Docker Template

> A ready-to-use Dockerized Laravel starter template — clone, configure, and `docker compose up`.

[![Laravel](https://img.shields.io/badge/Laravel-FF2D20?logo=laravel&logoColor=white)](https://laravel.com)
[![PHP 8.3](https://img.shields.io/badge/PHP-8.3-777BB4?logo=php&logoColor=white)](https://www.php.net)
[![Docker](https://img.shields.io/badge/Docker-2496ED?logo=docker&logoColor=white)](https://www.docker.com)
[![MySQL 8](https://img.shields.io/badge/MySQL-8.0-4479A1?logo=mysql&logoColor=white)](https://www.mysql.com)
[![Nginx](https://img.shields.io/badge/Nginx-009639?logo=nginx&logoColor=white)](https://nginx.org)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

A ready-to-use Dockerized Laravel starter template — clone, configure, and `docker compose up`. It bundles PHP 8.3-FPM and Nginx into a single application image alongside a MySQL 8 service, and runs database migrations automatically when the container starts, so you can go from clone to running app in minutes.

## Features

- **PHP 8.3-FPM + Nginx in one image** — the application container serves the app on port 80 with no extra web-server container to wire up.
- **MySQL 8 service** — a dedicated `db` service backed by a persistent volume.
- **Automatic migrations** — the `app` service runs `php artisan migrate --force` on start.
- **Preinstalled PHP extensions** — `gd` (with freetype/jpeg/webp), `pdo_mysql`, `mysqli`, and `zip` are built into the image.
- **Persistent named volumes** — separate volumes for the application code (`app_data`) and the database (`db_data`).
- **Configurable container prefix and DB port** — set `CONTAINER_NAME_PREFIX` and `DB_PORT` to avoid clashes across projects.

## Tech Stack

- **Laravel** — PHP `^8.2` (image `php:8.3-fpm`)
- **Nginx** — web server (configured via `docker-nginx.conf`)
- **MySQL 8** — database
- **Docker Compose** — orchestration

## Prerequisites

- [Docker](https://docs.docker.com/get-docker/)
- [Docker Compose](https://docs.docker.com/compose/) (the bundled `docker compose` plugin)

## Getting Started

1. **Copy the environment file:**

   ```bash
   cp .env.example .env
   ```

2. **Configure the environment.** Set the Compose-specific variables and align the Laravel `DB_*` values with the `db` service in `docker-compose.yml`:

   ```env
   # Compose settings
   CONTAINER_NAME_PREFIX=myapp
   DB_PORT=3306

   # Laravel DB connection (DB_HOST is the Compose service name)
   DB_CONNECTION=mysql
   DB_HOST=db
   DB_DATABASE=docker_db
   DB_USERNAME=dev
   DB_PASSWORD=dev
   ```

3. **Build and start the containers:**

   ```bash
   docker compose up -d --build
   ```

4. **Generate the application key:**

   ```bash
   docker compose exec app php artisan key:generate
   ```

The app is now served on port 80 of the `app` container, and the database is reachable on the host at `${DB_PORT}`.

## Configuration

| Variable | Description | Default |
| -------- | ----------- | ------- |
| `CONTAINER_NAME_PREFIX` | Prefix for the `app` and `db` container names | _(set in `.env`)_ |
| `DB_PORT` | Host port mapped to the MySQL container's `3306` | _(set in `.env`)_ |
| `MYSQL_DATABASE` | Database created on first run | `docker_db` |
| `MYSQL_USER` | Application database user | `dev` |
| `MYSQL_PASSWORD` | Application database password | `dev` |
| `MYSQL_ROOT_PASSWORD` | MySQL root password | `rootadmin` |

> The MySQL credentials above are development defaults defined in `docker-compose.yml`. Change them before any shared or production use.

## Services

| Service | Image / Build | Description | Ports |
| ------- | ------------- | ----------- | ----- |
| `app` | Built from `Dockerfile` | PHP 8.3-FPM + Nginx serving the Laravel app; runs migrations on start | `80` (container) |
| `db` | `mysql:8.0` | MySQL 8 database with a persistent volume | `${DB_PORT}:3306` |

## Common Commands

```bash
# Run any artisan command inside the app container
docker compose exec app php artisan <command>

# Follow the app container logs
docker compose logs -f app

# Stop and remove the containers
docker compose down

# Stop and remove the containers AND their volumes (drops the database)
docker compose down -v
```

## Security

See [SECURITY.md](SECURITY.md) for the security policy and how to report vulnerabilities.

This template is intended for **local development**. It ships with default MySQL credentials (`dev`/`dev`, root `rootadmin`) and runs `chmod -R 777` on `storage`, `resources`, and `bootstrap/cache` for convenience. Change the credentials and tighten file permissions before any shared or production deployment.

## Contributing

Contributions are welcome. Please open an issue to discuss significant changes first, then submit a pull request against the `master` branch.

## License

This project is licensed under the [MIT License](LICENSE).
