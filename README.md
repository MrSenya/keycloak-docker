# Keycloak Docker Setup

This repository provides a simple setup for running Keycloak using Docker Compose.

## Prerequisites
1. Install Docker and Docker Compose: [Docker Installation Guide](https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
2. Create a dedicated user for Keycloak:
   ```sh
   sudo useradd -m -s /bin/bash keycloak
   sudo usermod -aG docker keycloak
   ```
3. Create a directory for Keycloak and set appropriate permissions:
   ```sh
   sudo mkdir -p /opt/keycloak
   sudo chown -R keycloak:keycloak /opt/keycloak
   sudo chmod -R 700 /opt/keycloak
   ```

## Installation Steps
1. Switch to the Keycloak user:
   ```sh
   su - keycloak
   ```
2. Navigate to the working directory:
   ```sh
   cd /opt/keycloak
   ```
3. Clone this repository:
   ```sh
   git clone https://github.com/MrSenya/keycloak-docker.git
   ```
4. Change to the repository directory:
   ```sh
   cd keycloak-docker
   ```
5. Adjust environment variables at docker-compose.yml and keycloak.conf:
   ```sh
   #docker-compose.yml
    - KEYCLOAK_ADMIN=admin
    - KEYCLOAK_ADMIN_PASSWORD=<changeme>
    - PROXY_ADDRESS_FORWARDING=true
    - KC_DB_PASSWORD=<changeme>  # Must be the same as POSTGRES_PASSWORD

    - "--certificatesresolvers.myresolver.acme.email=<user@your.domain.com>"
    - "traefik.http.routers.keycloak.rule=Host(`<your.domain.com>`)"

    - POSTGRES_USER=keycloak
    - POSTGRES_PASSWORD=<changeme>
    - POSTGRES_DB=keycloak

    #keycloak.conf
    db-password=<changeme>
   ```
6. Start the Keycloak service using Docker Compose:
   ```sh
   docker compose up -d
   ```
7. Verify that the containers are running:
   ```sh
   docker ps
   ```

## Accessing Keycloak
Once the service is up and running, you can access the Keycloak admin console at:
```
https://<your.domain.com>
```

Default credentials (if configured in `docker-compose.yml`):
- Username: `admin`
- Password: `admin`

## Stopping the Service
To stop Keycloak and remove containers, run:
```sh
docker compose down
```

---
