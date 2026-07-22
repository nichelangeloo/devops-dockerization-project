# TodoApp - Dockerized Django + MySQL (DevOps case)
 
## Short description
 
Portfolio DevOps project demonstrating containerization of a Django REST API application with a MySQL database using Docker and Docker Compose. Focuses on environment externalization, database configuration via environment variables, container orchestration (local), semantic-versioned image tagging, and automated migrations.
 
## Key highlights / stack
 
* Django REST API (TodoApp)
* MySQL database container
* Docker Compose orchestration (local)
* Fully configurable database connection via environment variables (`ENGINE`, `NAME`, `USER`, `PASSWORD`, `HOST`, `PORT`)
* Semantic versioning for the application image tag in `docker-compose.yml`
* Environment configuration via `.env`
* Automated database migrations on startup
## Repository structure (high-level)
 
```
.
├── docker-compose.yml         # Orchestration (app + DB)
├── Dockerfile                 # Django app container
├── .env.example               # Example environment variables
├── requirements.txt           # Python dependencies
├── manage.py                  # Django management script
├── accounts/                  # App code (not DevOps focus)
├── api/                       # App code (not DevOps focus)
├── lists/                     # App code (not DevOps focus)
└── todolist/                  # App code (not DevOps focus) - settings.py reads DB env vars
```
 
## How it works
 
1. Docker Compose orchestrates two services:
   * `pythonapp` - Django application
   * `mysql` - MySQL DB with persistent volume
2. Database connection settings are fully externalized via environment variables and consumed by `todolist/settings.py`:
   * `ENGINE` - Django database backend (e.g. `django.db.backends.mysql`)
   * `NAME` - database name
   * `USER` - database user
   * `PASSWORD` - database password
   * `HOST` - database host (service name in Compose network)
   * `PORT` - database port
3. The application image in `docker-compose.yml` is tagged using semantic versioning (e.g. `todoapp:1.0.0`), so releases are traceable and reproducible instead of relying on `latest`.
4. On startup, the Django container:
   * installs dependencies,
   * runs migrations automatically,
   * starts the dev server on port `8080`.
5. Application becomes available at http://localhost:8080.
## Run locally
 
1. Copy the environment file:
```
cp .env.example .env
```
 
2. Fill in the database variables in `.env`:
```
ENGINE=mysql.connector.django
NAME=todoapp
USER=todoapp_user
PASSWORD=change_me
HOST=mysql
PORT=3306
```
 
3. Build and start containers:
```
docker compose up --build
```
 
4. Access the application:
```
http://localhost:8080
```
 
## Features
 
* Clear separation of services (Django app + MySQL DB)
* Fully configurable database connection via `.env` (engine, name, user, password, host, port)
* Semantic-versioned application image tag in Docker Compose
* Persistent DB volume (`todoapp-db-data`)
* Automated Django DB migrations
* No change to application behavior - TodoApp functions exactly as before, just fully containerized and configurable
## Next steps / improvements
 
* Push Docker images to Docker Hub with matching semantic version tags
* Add GitHub Actions pipeline for CI/CD (lint, build, push)
* Extend Docker Compose for production-like setup:
   * nginx reverse proxy
   * gunicorn app server
   * health checks
* Add monitoring/logging stack (Prometheus/Grafana, ELK)
## Contacts / Author
 
Project prepared as portfolio DevOps case, demonstrating environment externalization, database configuration via environment variables, semantic versioning, and containerized Django + MySQL orchestration.