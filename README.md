# ERPNext Coolify Deployment

Deploy ERPNext v15 (with Frappe Framework) on Coolify with Cloudflare Tunnel and Zero Trust protection.

## 🚀 Features

- **ERPNext v15** - Full-featured open-source ERP + Pqyments App
- **Frappe Framework** - Included as the base framework
- **Coolify Ready** - Optimized docker-compose for Coolify deployment
- **Secure** - No hardcoded passwords, all credentials via environment variables
- **Cloudflare Compatible** - Works with Cloudflare Tunnel and Zero Trust

## 📦 Services

| Service | Description | Port |
|---------|-------------|------|
| erpnext-frontend | Nginx reverse proxy | 8085:8080 |
| erpnext-backend | Gunicorn application server | - |
| erpnext-websocket | Socket.io for real-time updates | - |
| erpnext-queue-short | Background jobs (short queue) | - |
| erpnext-queue-long | Background jobs (long queue) | - |
| erpnext-scheduler | Scheduled tasks (cron) | - |
| erpnext-db | MariaDB 10.6 database | - |
| erpnext-redis-cache | Redis for caching | - |
| erpnext-redis-queue | Redis for job queues | - |

## 🛠️ Deployment on Coolify

### Step 1: Add Resource

1. Go to **Projects** → Your Project → **+ Add** → **Docker Compose**
2. **Git Repository**: `https://github.com/piktpikt/erpnext-coolify-deployment`
3. **Branch**: `main`

### Step 2: Configure Environment Variables

In Coolify → **Configuration** → **Environment Variables**, add:

```env
ERPNEXT_VERSION=v15
ERPNEXT_ADMIN_PASSWORD=YourSecureAdminPassword123!
DB_ROOT_PASSWORD=YourSecureDBRootPassword456!
DB_USER_PASSWORD=YourSecureDBUserPassword789!
```

### Step 3: Deploy

Click **Deploy** and wait 5-10 minutes for the site to be created.

### Step 4: Monitor Progress

Check the logs of `erpnext-create-site` container:

```bash
docker logs -f $(docker ps -q --filter "name=erpnext-create-site")
```

Wait until you see:
```
Site frontend has been created
```

## 🔐 Environment Variables

| Variable | Description | Required |
|----------|-------------|----------|
| `ERPNEXT_VERSION` | ERPNext version (default: v15) | No |
| `ERPNEXT_ADMIN_PASSWORD` | Administrator password | **Yes** |
| `DB_ROOT_PASSWORD` | MariaDB root password | **Yes** |
| `DB_USER_PASSWORD` | MariaDB erpnext user password | **Yes** |

## 🌐 Cloudflare Tunnel Configuration

### Public Hostname

| Setting | Value |
|---------|-------|
| Hostname | `erp.yourdomain.com` |
| Service | `http://erpnext-frontend-XXXXX:8080` |

> **Note**: Replace `XXXXX` with the actual container suffix from `docker ps --filter "name=erpnext-frontend"`.

### Zero Trust Application (Optional)

1. Go to **Cloudflare Zero Trust** → **Access** → **Applications**
2. **Add Application** → **Self-hosted**
3. **Application domain**: `erp.yourdomain.com`
4. **Policy**: Allow specific emails or groups

## 🔑 Default Credentials

| Type | Value |
|------|-------|
| **Username** | `Administrator` |
| **Password** | Value of `ERPNEXT_ADMIN_PASSWORD` |

## 🔧 Troubleshooting

### Check all containers status

```bash
docker ps -a --filter "name=erpnext" --format "table {{.Names}}\t{{.Status}}"
```

### Site creation failed

```bash
docker logs $(docker ps -aq --filter "name=erpnext-create-site")
```

### Database connection issues

```bash
docker logs $(docker ps -q --filter "name=erpnext-db")
```

### Reset and redeploy (clean install)

```bash
# Stop and remove all containers
docker stop $(docker ps -q --filter "name=erpnext")
docker rm $(docker ps -aq --filter "name=erpnext")

# Remove all volumes
docker volume ls | grep erpnext | awk '{print $2}' | xargs docker volume rm

# Then Redeploy in Coolify
```

### Container name changes after redeploy

Coolify generates new container names on each deployment. Update your Cloudflare Tunnel configuration with the new container name:

```bash
docker ps --filter "name=erpnext-frontend" --format "{{.Names}}"
```

## 📚 What's Included

The `frappe/erpnext:v15` image includes:

- ✅ **Frappe Framework** - Low-code web framework
- ✅ **ERPNext** - Full ERP suite (Accounting, HR, CRM, Manufacturing, etc.)
- ✅ **Payments** - Payment gateway integrations

## 🔗 Useful Links

- [ERPNext Documentation](https://docs.erpnext.com/)
- [Frappe Framework Documentation](https://frappeframework.com/docs)
- [Frappe Docker Repository](https://github.com/frappe/frappe_docker)
- [Coolify Documentation](https://coolify.io/docs)

## 📄 License

This deployment configuration is open source. ERPNext and Frappe are licensed under GNU GPLv3.

## 🙏 Credits

- [Frappe Technologies](https://frappe.io/) - ERPNext & Frappe Framework
- [Coolify](https://coolify.io/) - Self-hosting platform
- Based on [frappe_docker](https://github.com/frappe/frappe_docker)
