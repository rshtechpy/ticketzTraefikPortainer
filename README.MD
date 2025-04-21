# Ticketz Docker Stack (Traefik)

This guide documents how to deploy the **Ticketz** CRM/helpdesk system using Docker Swarm, Traefik, and external PostgreSQL and Redis services.

## 📦 Stack Components

- **ticketz_backend** – Main backend service
- **ticketz_frontend** – Frontend interface served over Traefik
- **ticketz_watchtower** – Auto-updater for Docker containers

All services connect through a shared external network (`my_stack_net`) and use persistent volumes for data.

---

## 🚀 Quick Start

### 1. Requirements
- A public server (VPS or dedicated) with Docker and Docker Swarm initialized
- Domain (e.g., `app.example.com`) with DNS pointing to your server
- Open ports: 80, 443
- A valid email for SSL certificates

### 2. Create External Resources
Before deploying the stack, manually create the following:

```bash
# Create Docker volumes:
docker volume create chat1_backend_public
docker volume create chat1_backend_private

# Create external network:
docker network create -d overlay --attachable my_stack_net
```

### 3. Stack Deployment
Save the modified `docker-compose.yaml` file (see this repo) and deploy with:

```bash
docker stack deploy -c docker-compose.yaml ticketz
```

> 📌 Make sure to replace all `example.com`, credentials, and token values with your actual configuration.

---

## 🔧 Configuration Highlights

### Backend Environment:
```env
FRONTEND_HOST=app.example.com
BACKEND_PATH=/backend
EMAIL_ADDRESS=admin@example.com
DB_DIALECT=postgres
DB_HOST=postgres
DB_USER=example_user
DB_NAME=example_db
DB_PASS=example_password
REDIS_URI=redis://redis:6379/4
```

### Traefik Routing:
- Frontend: `https://app.example.com`
- Backend: `https://app.example.com/backend`

### Watchtower:
Automatically checks and updates container images every 30 minutes.

---

## 🔑 Default Credentials (Initial Setup)
- **Username**: `admin@example.com`
- **Password**: `123456`

> 🔒 You should immediately change the default password after logging in.

---

## 🛠 Maintenance

### Logs:
```bash
docker service logs ticketz_ticketz_backend -f
```

### Update Stack:
```bash
docker service update --image ghcr.io/ticketz-oss/ticketz-backend:latest ticketz_ticketz_backend
```

---

## 📜 License
This setup is based on the AGPL-licensed Ticketz project. Please follow its redistribution terms and retain the source code link in the UI.

---

## 🙌 Credits
Original project: https://github.com/allgood/ticketz

