# n8n Nextcloud Integration

Self-hosted workflow automation with n8n, Nextcloud Tasks, and Nextcloud Calendar.

## 🚀 Quick Start

1. **Configure environment:**
   ```bash
   cp .env.example .env
   nano .env  # Add your credentials
   ```

2. **Start n8n:**
   ```bash
   docker compose up -d
   ```

3. **Access n8n:** http://localhost:5678

4. **Import workflows:**
   - `nextcloud-tasks-workflow.json` - Tasks management
   - `nextcloud-calendar-workflow.json` - Calendar management

5. **Set credentials:**
   - Go to Credentials → Add → HTTP Basic Auth
   - Enter your Nextcloud username and app password
   - Select this credential in the workflow HTTP Request nodes

## 📋 Workflows

### Tasks Manager (`/webhook/nextcloud-tasks`)

| Action | Description | Required Fields |
|--------|-------------|-----------------|
| `fetch` | List all tasks | - |
| `create` | Create a task | `title` |
| `update` | Update a task | `taskUid` |
| `delete` | Delete a task | `taskUid` |

**Create task with all properties:**
```json
{
  "action": "create",
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "due": "2025-01-20T18:00:00",
  "priority": 3,
  "status": "in-progress",
  "percentComplete": 50,
  "categories": ["personal", "shopping"]
}
```

### Calendar Manager (`/webhook/nextcloud-calendar`)

| Action | Description | Required Fields |
|--------|-------------|-----------------|
| `fetch` | List all events | - |
| `create` | Create an event | `title` |
| `delete` | Delete an event | `eventUid` |

**Create event:**
```json
{
  "action": "create",
  "title": "Team Meeting",
  "start": "2025-01-20T09:00:00",
  "end": "2025-01-20T10:00:00",
  "location": "Conference Room"
}
```

## 📚 Documentation

- [Nextcloud Task Properties](nextcloud-task-properties.md)
- [Nextcloud Calendar Properties](nextcloud-calendar-properties.md)
- [Nextcloud Setup Guide](nextcloud-setup-guide.md)

## 🔧 Management

```bash
# Start
docker compose up -d

# Stop
docker compose down

# Restart
docker compose restart

# View logs
docker compose logs -f n8n

# Update
docker compose pull && docker compose up -d
```

## 🐛 Troubleshooting

- **401 Unauthorized:** Check app password in Nextcloud
- **Webhook not found:** Ensure workflow is activated
- **Connection refused:** Check n8n is running (`docker compose ps`)

## 🔐 Security Notes

- Use strong passwords in `.env`
- Create app passwords in Nextcloud (Settings → Security)
- Consider adding HTTPS with a reverse proxy for production

## 📦 What's Included

- Docker Compose setup (n8n + PostgreSQL)
- Nextcloud Tasks workflow (CRUD)
- Nextcloud Calendar workflow (CRUD)
- Documentation and examples

---

**Repository:** https://github.com/ahmed-rezk-dev/n8n-workflows
