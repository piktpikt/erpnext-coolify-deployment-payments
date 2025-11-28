# ERPNext Coolify Deployment

Deploy ERPNext v15 on Coolify with Cloudflare Tunnel.

## Services

| Service | Description |
|---------|-------------|
| erpnext-frontend | Nginx reverse proxy (port 8085) |
| erpnext-backend | Gunicorn application server |
| erpnext-websocket | Socket.io for real-time |
| erpnext-queue-short | Background jobs (short queue) |
| erpnext-queue-long | Background jobs (long queue) |
| erpnext-scheduler | Scheduled tasks |
| erpnext-db | MariaDB 10.6 database |
| erpnext-redis-cache | Redis for caching |
| erpnext-redis-queue | Redis for job queues |

## Deployment on Coolify

1. **Add new resource** → Docker Compose → Git Repository
2. **Repository**: `https://github.com/piktpikt/erpnext-coolify-deployment`
3. **Branch**: `main`
4. **Deploy**

## First Deployment

The first deployment takes **5-10 minutes** because:
- `erpnext-configurator` sets up the bench configuration
- `erpnext-create-site` creates the default site with ERPNext installed

Check the logs of `erpnext-create-site` container to monitor progress.

## Default Credentials

| Type | Value |
|------|-------|
| **URL** | https://erp.flightpawr.com |
| **Username** | Administrator |
| **Password** | admin |

⚠️ **Change the admin password immediately after first login!**

## Cloudflare Tunnel Configuration

| Setting | Value |
|---------|-------|
| Hostname | erp.flightpawr.com |
| Service | http://erpnext-frontend-XXXXX:8080 |

Note: Replace `XXXXX` with the actual container suffix from `docker ps`.

## Database Credentials

| Parameter | Value |
|-----------|-------|
| Root Password | erpnext_db_root_2024 |
| Database | erpnext |
| User | erpnext |
| User Password | erpnext_user_2024 |

⚠️ **Change these passwords for production use!**

## Customization

### Change Admin Password
Edit `docker-compose.yml`, find `erpnext-create-site` service and modify:
```
--admin-password=YOUR_SECURE_PASSWORD
```

### Change Database Passwords
Edit `docker-compose.yml`:
1. `erpnext-db` service: `MYSQL_ROOT_PASSWORD` and `MYSQL_PASSWORD`
2. `erpnext-create-site` service: `--db-root-password`

## Troubleshooting

### Site not accessible
```bash
docker logs erpnext-create-site-XXXXX
```

### Database connection issues
```bash
docker logs erpnext-db-XXXXX
```

### Check all containers
```bash
docker ps --filter "name=erpnext"
```

## Based on

- [Frappe Docker](https://github.com/frappe/frappe_docker)
- [ERPNext](https://erpnext.com/)
