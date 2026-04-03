# Churchmedia Infrastructure
This repository contains the infrastructure code for Churchmedia, a platform dedicated to providing media resources for churches. The infrastructure is built using modern DevOps practices to ensure scalability, reliability, and ease of maintenance.

This repository contains the docker configurations and scripts necessary to deploy and manage the Churchmedia platform.

## Docker
- Dockerfiles for building container images

### Configuration
docker-compose.yml configures the following applications to run in the Churchmedia infrastructure:

| Image ID    | External Port Number | Internal Port Number   |
|:------------|---------------------:|-----------------------:|
| Keycloak    |                 8080 |                   8080 |
| File-Server |                 8085 |                   8085 |
| MariaDB     |                 3306 |                   3306 |

### Starting the Infrastructure
To start the Churchmedia infrastructure, use the following command:

```bash
cd churchmedia-infra
```

```bash
docker-compose up -d
```