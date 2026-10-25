# HRFlow AI — Production Deployment & Operations Runbook

This guide outlines the production deployment, database provisioning, migration management, and monitoring standards for **HRFlow AI**.

---

## 1. Production Architecture Overview

The production architecture consists of three interconnected containerized services orchestrated via Docker or Kubernetes:

1. **`hrflow-frontend`**: High-performance Nginx web server serving compiled React 19 static assets, enforcing security headers, gzip compression, and reverse-proxying `/api/` traffic to the backend.
2. **`hrflow-backend`**: Multi-worker asynchronous FastAPI application running on Uvicorn, handling workflow execution, RBAC, AI Copilot, file validation, and event handling.
3. **`hrflow-postgres`**: PostgreSQL 16 database with persistent storage, connection pooling, and strict ACID transaction boundaries.

---

## 2. Infrastructure Sizing Requirements

| Tier | Workload / Concurrency | Recommended Hardware | Database Specs |
| :--- | :--- | :--- | :--- |
| **Small / Pilot** | < 100 Employees / < 1k Workflows | 2 vCPU, 4 GB RAM | 10 GB SSD, PostgreSQL 14+ |
| **Medium / Production** | 100 - 1,000 Employees / < 50k Workflows | 4 vCPU, 8 GB RAM | 50 GB SSD, PostgreSQL 15+ |
| **Enterprise / High SLA** | 1,000+ Employees / High Concurrency | 8+ vCPU, 16+ GB RAM (Multi-node) | 200+ GB SSD, Managed Aurora/RDS |

---

## 3. Environment Variable Configuration

Copy `.env.example` to `.env` and fill in your production values.

```bash
cp .env.example .env
chmod 600 .env
```

### Critical Production Security Checks
1. **`JWT_SECRET`**: Generate a strong cryptographic secret:
   ```bash
   openssl rand -hex 32
   ```
2. **`ENVIRONMENT`**: Set to `production` to activate strict CORS, disable debug tracebacks, and enforce rate limits.
3. **`EMAIL_SEND_REAL`**: Keep `false` until SMTP or API credentials are fully tested.
4. **`DATABASE_URL`**: Ensure SSL mode is enabled if connecting to managed databases (e.g., `?sslmode=require`).

---

## 4. Docker Compose Production Deployment

### Step 1: Clone and Configure
```bash
git clone https://github.com/your-org/hrflow-ai.git /opt/hrflow-ai
cd /opt/hrflow-ai
cp .env.example .env
nano .env
```

### Step 2: Build and Launch
```bash
docker-compose up --build -d
```

### Step 3: Verify Container Health
```bash
docker-compose ps
```
All containers (`hrflow-postgres`, `hrflow-backend`, `hrflow-frontend`) should display `Up (healthy)`.

### Step 4: Run Database Migrations
Migrations are automatically applied during backend container initialization via `lifespan`. To manually verify or apply migration revisions:
```bash
docker-compose exec backend python -m alembic upgrade head
```

### Step 5: Seed Demo or Enterprise Baseline
```bash
# Seed initial enterprise roles, departments, and admin accounts:
docker-compose exec backend python -m app.seed

# Or seed complete realistic demo environment:
docker-compose exec backend python -m app.seed_demo
```

---

## 5. Database Migration Management

HRFlow AI uses **Alembic** alongside SQLAlchemy 2.0.

### Check Current Migration Status
```bash
docker-compose exec backend python -m alembic current
```

### Create a New Schema Migration
When modifying ORM models in `backend/app/models/`:
```bash
docker-compose exec backend python -m alembic revision --autogenerate -m "Add custom field"
```

### Apply Migrations
```bash
docker-compose exec backend python -m alembic upgrade head
```

### Rollback Previous Migration
```bash
docker-compose exec backend python -m alembic downgrade -1
```

---

## 6. SSL / TLS Reverse Proxy Setup (Certbot & Nginx)

For public internet deployment, place Nginx or Caddy with Let's Encrypt SSL in front of the application:

```nginx
server {
    listen 80;
    server_name hrflow.yourcompany.com;
    return 301 https://$host$request_uri;
}

server {
    listen 443 ssl http2;
    server_name hrflow.yourcompany.com;

    ssl_certificate /etc/letsencrypt/live/hrflow.yourcompany.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/hrflow.yourcompany.com/privkey.pem;

    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    location / {
        proxy_pass http://127.0.0.1:80;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto https;
    }
}
```

---

## 7. Backup and Disaster Recovery

### Automated PostgreSQL Backup
Create a daily cron job script `/opt/hrflow-ai/backup.sh`:
```bash
#!/bin/bash
BACKUP_DIR="/var/backups/hrflow"
TIMESTAMP=$(date +"%Y%m%d_%H%M%S")
mkdir -p "$BACKUP_DIR"

docker-compose exec -T postgres pg_dump -U hrflow_user hrflow_db | gzip > "$BACKUP_DIR/hrflow_backup_$TIMESTAMP.sql.gz"

# Retain backups for 30 days
find "$BACKUP_DIR" -type f -name "*.sql.gz" -mtime +30 -delete
```

### Database Restoration
```bash
gunzip < /var/backups/hrflow/hrflow_backup_TIMESTAMP.sql.gz | docker-compose exec -T postgres psql -U hrflow_user -d hrflow_db
```

---

## 8. Health Checking & Telemetry Endpoints

- **Frontend Liveness:** `GET /healthz` (Returns HTTP 200 `healthy`)
- **Backend Telemetry:** `GET /api/v1/modules/dashboard-stats` (Requires JWT Bearer header)
- **OpenAPI Schema:** `GET /openapi.json`
- **Interactive API Documentation:** `GET /docs`
