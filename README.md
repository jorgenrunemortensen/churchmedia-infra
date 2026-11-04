# Churchmedia Infrastructure
This repository contains the infrastructure code for Churchmedia, a platform dedicated to providing media resources for churches. The infrastructure is built using modern DevOps practices to ensure scalability, reliability, and ease of maintenance.

This repository contains the docker configurations and scripts necessary to deploy and manage the Churchmedia platform.

## Docker
- Dockerfiles for building container images

### Configuration
docker-compose.yml configures the following applications to run in the Churchmedia infrastructure:

| Service       | Image                                                  | Port(s)       |
|:--------------|:-------------------------------------------------------|:--------------|
| Keycloak      | quay.io/keycloak/keycloak                              | 8080          |
| Content-App   | ghcr.io/jorgenrunemortensen/churchmedia-content-app    | 8081          |
| File-Server   | ghcr.io/jorgenrunemortensen/file-server                | 8085          |
| Indexing-App  | ghcr.io/jorgenrunemortensen/churchmedia-index-app      | 8086          |
| MariaDB       | mariadb                                                | 3306, 3316    |
| Elasticsearch | docker.elastic.co/elasticsearch/elasticsearch          | 9200          |
| RabbitMQ      | rabbitmq:3-management                                  | 5672, 15672   |

The application images are built and published to GitHub Container Registry by GitHub Actions
in each application repository whenever `master` is pushed.

### Starting the Infrastructure
Only Docker is required:

```bash
git clone https://github.com/jorgenrunemortensen/churchmedia-infra.git
cd churchmedia-infra
cp .env.example .env    # then change the passwords in .env
docker compose up -d
```

`docker compose up -d --wait` waits until every service reports healthy. The services are started
in dependency order using health checks, so the applications only start when MariaDB, Keycloak,
RabbitMQ and Elasticsearch are ready.

On the first start:
- Keycloak imports the realm `Alive` from `keycloak/alive-realm.json`, including the demo users
  `admin`, `editor`, `viewer` and `tester` with the password `KEYCLOAK_DEMO_PASSWORD` from `.env`.
  The admin console is at http://localhost:8080 (`KEYCLOAK_ADMIN_USERNAME` / `KEYCLOAK_ADMIN_PASSWORD`).
- MariaDB creates the database `churchmedia` and the application user; Content-App creates the tables.

Data is kept in named Docker volumes. Media files for File-Server are read from `./media`,
or from the folder given by `MEDIA_ROOT` in `.env`.

### Building from source
To build the applications from source instead of pulling the published images, check out the
repositories side by side as described in `docker-compose.dev.yml`, and run:

```bash
docker compose -f docker-compose.yml -f docker-compose.dev.yml up -d --build
```
