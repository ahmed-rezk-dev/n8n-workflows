# n8n Homelab Setup

Self-hosted workflow automation with n8n and PostgreSQL.

## Quick Start

1. **Edit the `.env` file** with your preferred credentials:
   ```bash
   nano .env
   ```

2. **Start the services**:
   ```bash
   docker compose up -d
   ```

3. **Access n8n** at: http://localhost:5678

## Configuration

### Environment Variables (`.env`)

| Variable | Description | Default |
|----------|-------------|---------|
| `POSTGRES_USER` | PostgreSQL username | `n8n` |
| `POSTGRES_PASSWORD` | PostgreSQL password | `change_me_to_a_strong_password` |
| `POSTGRES_DB` | PostgreSQL database name | `n8n` |
| `N8N_USER` | n8n login username | `admin` |
| `N8N_PASSWORD` | n8n login password | `change_me_to_a_strong_password` |
| `N8N_HOST` | n8n hostname/IP | `localhost` |
| `TZ` | Timezone | `America/New_York` |

### For Remote Access

If accessing from another device on your network:
1. Change `N8N_HOST` to your server's IP address (e.g., `192.168.1.100`)
2. Access via `http://192.168.1.100:5678`

### HTTPS Setup (Optional)

For production use, consider adding a reverse proxy (Traefik or Nginx) in front of n8n for SSL/TLS.

## Management Commands

```bash
# Start services
docker compose up -d

# Stop services
docker compose down

# View logs
docker compose logs -f

# View n8n logs only
docker compose logs -f n8n

# Restart services
docker compose restart

# Update to latest version
docker compose pull
docker compose up -d

# Backup database
docker exec n8n-postgres pg_dump -U n8n n8n > backup.sql

# Restore database
docker exec -i n8n-postgres psql -U n8n n8n < backup.sql
```

## Data Storage

- **n8n data**: Docker volume `n8n-setup_n8n_data`
- **PostgreSQL data**: Docker volume `n8n-setup_postgres_data`
- **Local files**: `./local-files` directory (for file operations in workflows)

## Troubleshooting

1. **Check container status**: `docker compose ps`
2. **View logs**: `docker compose logs`
3. **Reset everything** (deletes all data):
   ```bash
   docker compose down -v
   ```
