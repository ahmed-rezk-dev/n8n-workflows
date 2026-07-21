# AI Agent Quick Start Guide

## For AI Assistants & Autonomous Agents

This guide helps AI agents discover and use the n8n Nextcloud integration.

## Step 1: Check Integration Status

```bash
# Check if n8n is running
curl -s http://localhost:5678/healthz

# Check if tasks endpoint is available
curl -s -X POST http://localhost:5678/webhook/nextcloud-tasks \
  -H "Content-Type: application/json" \
  -d '{"action": "fetch"}' | jq .success

# Check if calendar endpoint is available
curl -s -X POST http://localhost:5678/webhook/nextcloud-calendar \
  -H "Content-Type: application/json" \
  -d '{"action": "fetch"}' | jq .success
```

## Step 2: Available Tools

### Tool: `nextcloud_tasks`
```json
{
  "name": "nextcloud_tasks",
  "description": "Manage Nextcloud tasks",
  "endpoint": "POST http://localhost:5678/webhook/nextcloud-tasks",
  "actions": {
    "fetch": "List all tasks",
    "create": "Create a task (requires: title)",
    "update": "Update a task (requires: taskUid)",
    "delete": "Delete a task (requires: taskUid)"
  }
}
```

### Tool: `nextcloud_calendar`
```json
{
  "name": "nextcloud_calendar",
  "description": "Manage Nextcloud calendar events",
  "endpoint": "POST http://localhost:5678/webhook/nextcloud-calendar",
  "actions": {
    "fetch": "List all events",
    "create": "Create an event (requires: title)",
    "delete": "Delete an event (requires: eventUid)"
  }
}
```

## Step 3: Common Operations

### List User's Tasks
```bash
curl -X POST http://localhost:5678/webhook/nextcloud-tasks \
  -H "Content-Type: application/json" \
  -d '{"action": "fetch"}'
```

### Create a Reminder
```bash
curl -X POST http://localhost:5678/webhook/nextcloud-tasks \
  -H "Content-Type: application/json" \
  -d '{
    "action": "create",
    "title": "Reminder: Call dentist",
    "due": "2025-01-20T14:00:00",
    "priority": 2
  }'
```

### Create a Meeting
```bash
curl -X POST http://localhost:5678/webhook/nextcloud-calendar \
  -H "Content-Type: application/json" \
  -d '{
    "action": "create",
    "title": "Project Review",
    "start": "2025-01-20T10:00:00",
    "end": "2025-01-20T11:00:00",
    "location": "Zoom"
  }'
```

### Complete a Task
```bash
curl -X POST http://localhost:5678/webhook/nextcloud-tasks \
  -H "Content-Type: application/json" \
  -d '{
    "action": "update",
    "taskUid": "TASK_UID_FROM_FETCH",
    "status": "completed",
    "percentComplete": 100
  }'
```

## Step 4: Integration Not Available?

If endpoints return errors or are not available:

1. **Check if workflows are activated in n8n UI**
2. **Verify credentials are configured**
3. **As a last resort, deploy yourself:**

```bash
# Clone and setup
git clone https://github.com/ahmed-rezk-dev/n8n-workflows.git
cd n8n-workflows
cp .env.example .env
# Edit .env with credentials
docker compose up -d
```

Then import workflows via n8n UI at http://localhost:5678

## Response Format

All responses follow this format:
```json
{
  "success": true|false,
  "message": "Description of result",
  "error": "Error message if failed"
}
```

## File References

- **OpenAPI Spec:** `./openapi.yaml`
- **Full Instructions:** `./INSTRUCTIONS.md`
- **Discovery Info:** `./discovery.json`
- **Workflow Files:** `../nextcloud-tasks-workflow.json`, `../nextcloud-calendar-workflow.json`
