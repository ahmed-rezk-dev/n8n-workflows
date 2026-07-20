# Nextcloud Tasks Integration for n8n

## Overview

This integration allows you to manage Nextcloud Tasks/Reminders directly from n8n workflows.

## Files Created

| File | Description |
|------|-------------|
| `nextcloud-simple-workflow.json` | Fetch all tasks |
| `nextcloud-create-task-workflow.json` | Create new tasks |
| `nextcloud-setup-guide.md` | Detailed setup guide |

## Quick Setup

### 1. Get Nextcloud App Password

1. Log into your Nextcloud instance
2. Go to **Settings** → **Security**
3. Under **App passwords**, create a new one named "n8n"
4. Copy the generated credentials

### 2. Configure n8n Environment

Edit `/root/n8n-setup/.env`:

```bash
# Add these lines
NEXTCLOUD_URL=https://your-nextcloud-domain.com
NEXTCLOUD_USER=your_username
```

Restart n8n:
```bash
cd /root/n8n-setup
docker compose restart
```

### 3. Add Credentials in n8n

1. Open n8n: http://localhost:5678
2. Go to **Credentials** → **Add Credential**
3. Search and select **HTTP Basic Auth**
4. Configure:
   - **Name**: Nextcloud API
   - **User**: Your Nextcloud username
   - **Password**: The app password from step 1
5. Save

### 4. Import Workflows

1. Go to **Workflows** → **Import from File**
2. Import `nextcloud-simple-workflow.json`
3. Click on the HTTP Request node
4. Select the "Nextcloud API" credential you created
5. Save and activate the workflow

## API Usage

### Get All Tasks

```bash
curl http://localhost:5678/webhook/nextcloud-tasks
```

Response:
```json
{
  "tasks": [
    {
      "uid": "task-123",
      "title": "Buy groceries",
      "status": "NEEDS-ACTION",
      "due": "20250120T180000Z",
      "description": "Milk, eggs, bread",
      "priority": 1
    }
  ],
  "count": 1
}
```

### Create a Task

```bash
curl -X POST http://localhost:5678/webhook/nextcloud-create-task \
  -H "Content-Type: application/json" \
  -d '{
    "title": "Meeting with team",
    "description": "Discuss Q1 goals",
    "due": "20250120T140000Z",
    "priority": 1
  }'
```

## Workflow Examples

### Daily Task Summary (Email)

```
Schedule Trigger (9am) → Get Tasks → Filter (NEEDS-ACTION) → Send Email
```

### Slack Reminder for Due Tasks

```
Schedule Trigger → Get Tasks → Filter (due today) → Slack Message
```

### Add Task from Google Form

```
Google Forms Trigger → Create Task in Nextcloud
```

## Troubleshooting

**Check n8n logs:**
```bash
cd /root/n8n-setup
docker compose logs -f n8n
```

**Common issues:**
- 401 Unauthorized: Check app password
- 404 Not Found: Verify NEXTCLOUD_URL
- Connection refused: Ensure Nextcloud is accessible from n8n container

## Nextcloud Tasks App

Make sure the **Tasks** app is enabled in your Nextcloud:
- Go to **Apps** → **Disabled apps**
- Find **Tasks** and enable it
