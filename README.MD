# Ticketz Docker Stack (Traefik)

Esta guía documenta cómo desplegar el sistema **Ticketz** CRM/helpdesk utilizando Docker Swarm, Traefik y servicios externos de PostgreSQL y Redis.

## 📦 Componentes de la Stack

- **ticketz_backend** – Servicio principal del backend
- **ticketz_frontend** – Interfaz del frontend servida a través de Traefik
- **ticketz_watchtower** – Autoactualizador de contenedores Docker

Todos los servicios se conectan a través de una red externa compartida (`my_stack_net`) y utilizan volúmenes persistentes para los datos.

---

## 🚀 Inicio Rápido

### 1. Requisitos
- Un servidor público (VPS o dedicado) con Docker y Docker Swarm inicializado
- Dominio (por ejemplo, `app.example.com`) apuntando por DNS a tu servidor
- Puertos 80 y 443 abiertos
- Un correo electrónico válido para los certificados SSL

### 2. Crear Recursos Externos
Antes de desplegar la stack, crea manualmente lo siguiente:

```bash
# Crear volúmenes de Docker:
docker volume create chat1_backend_public
docker volume create chat1_backend_private

# Crear red externa:
docker network create -d overlay --attachable my_stack_net
```

### 3. Desplegar la Stack
Guarda el archivo `docker-compose.yaml` modificado (ver este repositorio) y ejecútalo con:

```bash
docker stack deploy -c docker-compose.yaml ticketz
```

> 📌 Asegúrate de reemplazar todos los valores `example.com`, credenciales y tokens por tu configuración real.

---

## 🔧 Configuración Destacada

### Variables del Backend:
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

### Ruteo con Traefik:
- Frontend: `https://app.example.com`
- Backend: `https://app.example.com/backend`

### Watchtower:
Verifica y actualiza automáticamente las imágenes de los contenedores cada 30 minutos.

---

## 🔑 Credenciales por Defecto (Instalación Inicial)
- **Usuario**: `admin@example.com`
- **Contraseña**: `123456`

> 🔒 Se recomienda cambiar la contraseña por defecto inmediatamente después de iniciar sesión.

---

## 🛠 Mantenimiento

### Ver Logs:
```bash
docker service logs ticketz_ticketz_backend -f
```

### Actualizar la Stack:
```bash
docker service update --image ghcr.io/ticketz-oss/ticketz-backend:latest ticketz_ticketz_backend
```

---

## 📜 Licencia
Esta configuración se basa en el proyecto Ticketz licenciado bajo AGPL. Por favor, respeta los términos de redistribución y conserva el enlace al código fuente en la interfaz del sistema.

---

## 🙌 Créditos
Proyecto original: https://github.com/rshtechpy

---

## ⭐ ¡Apóyanos!

Si esta stack te resultó útil, considera dejarnos una estrella ⭐ en [GitHub](https://github.com/rshtechpy) y hacer un fork para seguir mejorándola o adaptarla a tus necesidades.

¡Tu apoyo motiva y mantiene vivo este proyecto! 💪

