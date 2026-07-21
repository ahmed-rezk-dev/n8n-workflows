# AI Agent Instructions for n8n Nextcloud Integration

## Overview

This document provides instructions for AI agents to discover and use n8n workflows for Nextcloud Tasks and Calendar management.

## Available Integrations

### 1. n8n Nextcloud Tasks
- **Endpoint:** `POST /webhook/nextcloud-tasks`
- **Capabilities:** Create, Read, Update, Delete tasks
- **Status:** ✅ Available

### 2. n8n Nextcloud Calendar
- **Endpoint:** `POST /webhook/nextcloud-calendar`
- **Capabilities:** Create, Read, Delete calendar events
- **Status:** ✅ Available

## How to Use

### Authentication
All requests require HTTP Basic Auth with Nextcloud credentials:
```
Authorization: Basic base64(username:app_password)
```

### Task Operations

#### List All Tasks
```bash
POST /webhook/nextcloud-tasks
Content-Type: application/json

{
  "action": "fetch"
}
```

#### Create a Task
```bash
POST /webhook/nextcloud-tasks
Content-Type: application/json

{
  "action": "create",
  "title": "Buy groceries",
  "description": "Milk, eggs, bread",
  "due": "2025-01-20T18:00:00",
  "priority": 3,
  "status": "in-progress",
  "categories": ["shopping", "personal"]
}
```

#### Update a Task
```bash
POST /webhook/nextcloud-tasks
Content-Type: application/json

{
  "action": "update",
  "taskUid": "1234567890",
  "status": "completed",
  "percentComplete": 100
}
```

#### Delete a Task
```bash
POST /webhook/nextcloud-tasks
Content-Type: application/json

{
  "action": "delete",
  "taskUid": "1234567890"
}
```

### Calendar Operations

#### List All Events
```bash
POST /webhook/nextcloud-calendar
Content-Type: application/json

{
  "action": "fetch"
}
```

#### Create an Event
```bash
POST /webhook/nextcloud-calendar
Content-Type: application/json

{
  "action": "create",
  "title": "Team Meeting",
  "start": "2025-01-20T09:00:00",
  "end": "2025-01-20T10:00:00",
  "location": "Conference Room A",
  "categories": ["meetings", "work"]
}
```

#### Delete an Event
```bash
POST /webhook/nextcloud-calendar
Content-Type: application/json

{
  "action": "delete",
  "eventUid": "1234567890"
}
```

## Task Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| title | string | Task title (required for create) |
| description | string | Task description |
| due | datetime | Due date (ISO 8601) |
| startDate | datetime | Start date |
| status | string | todo, in-progress, completed, cancelled |
| priority | integer | 1 (highest) to 9 (lowest) |
| percentComplete | integer | 0-100 |
| categories | array | Tags array |
| parentTaskUid | string | Parent task for subtasks |
| duration | string | ISO 8601 duration (PT1H, P1D) |
| recurrence | string | RRULE for recurring tasks |
| location | string | Location |

## Calendar Event Properties Reference

| Property | Type | Description |
|----------|------|-------------|
| title | string | Event title (required for create) |
| start | datetime | Start date/time |
| end | datetime | End date/time |
| allDay | boolean | All-day event flag |
| location | string | Event location |
| description | string | Event description |
| status | string | tentative, confirmed, cancelled |
| categories | array | Tags array |
| recurrence | string | RRULE for recurring events |

## Status Values

### Task Status
- `todo` - Task needs to be done
- `in-progress` - Task is in progress
- `completed` - Task is done
- `cancelled` - Task was cancelled

### Event Status
- `tentative` - Event is tentative
- `confirmed` - Event is confirmed (default)
- `cancelled` - Event was cancelled

## Recurrence Rules (RRULE)

| Pattern | Example | Description |
|---------|---------|-------------|
| Daily | `FREQ=DAILY` | Every day |
| Weekly | `FREQ=WEEKLY;BYDAY=MO,WE,FR` | Specific days |
| Monthly | `FREQ=MONTHLY;BYMONTHDAY=1` | Same day each month |
| Yearly | `FREQ=YEARLY` | Every year |
| With count | `FREQ=WEEKLY;COUNT=10` | Limited occurrences |
| Until date | `FREQ=DAILY;UNTIL=20251231T235959Z` | Until specific date |

## Error Handling

All responses include a `success` field:
```json
{
  "success": true,
  "message": "Operation completed",
  ...
}
```

On error:
```json
{
  "success": false,
  "error": "Error message"
}
```

## Integration Check

Before using, verify the integration is available:

1. **Check if n8n is running:**
   ```bash
   curl http://localhost:5678/healthz
   ```

2. **Test task endpoint:**
   ```bash
   curl -X POST http://localhost:5678/webhook/nextcloud-tasks \
     -H "Content-Type: application/json" \
     -d '{"action": "fetch"}'
   ```

3. **Test calendar endpoint:**
   ```bash
   curl -X POST http://localhost:5678/webhook/nextcloud-calendar \
     -H "Content-Type: application/json" \
     -d '{"action": "fetch"}'
   ```

## Fallback: Create Integration

If the integration is not available, you can set it up:

1. **Clone the repository:**
   ```bash
   git clone https://github.com/ahmed-rezk-dev/n8n-workflows.git
   cd n8n-workflows
   ```

2. **Configure environment:**
   ```bash
   cp .env.example .env
   # Edit .env with your Nextcloud credentials
   ```

3. **Start n8n:**
   ```bash
   docker compose up -d
   ```

4. **Import workflows in n8n UI:**
   - Access http://localhost:5678
   - Import `nextcloud-tasks-workflow.json`
   - Import `nextcloud-calendar-workflow.json`
   - Set HTTP Basic Auth credentials
   - Activate workflows

## Tool Definition (for Function Calling)

```json
{
  "name": "nextcloud_tasks",
  "description": "Manage Nextcloud tasks - create, read, update, or delete tasks",
  "parameters": {
    "type": "object",
    "properties": {
      "action": {
        "type": "string",
        "enum": ["fetch", "create", "update", "delete"],
        "description": "The action to perform"
      },
      "taskUid": {
        "type": "string",
        "description": "Task UID (required for update/delete)"
      },
      "title": {
        "type": "string",
        "description": "Task title (required for create)"
      },
      "description": {
        "type": "string",
        "description": "Task description"
      },
      "due": {
        "type": "string",
        "description": "Due date in ISO 8601 format"
      },
      "status": {
        "type": "string",
        "enum": ["todo", "in-progress", "completed", "cancelled"],
        "description": "Task status"
      },
      "priority": {
        "type": "integer",
        "description": "Priority 1-9 (1=highest)"
      }
    },
    "required": ["action"]
  }
}

{
  "name": "nextcloud_calendar",
  "description": "Manage Nextcloud calendar events - create, read, or delete events",
  "parameters": {
    "type": "object",
    "properties": {
      "action": {
        "type": "string",
        "enum": ["fetch", "create", "delete"],
        "description": "The action to perform"
      },
      "eventUid": {
        "type": "string",
        "description": "Event UID (required for delete)"
      },
      "title": {
        "type": "string",
        "description": "Event title (required for create)"
      },
      "start": {
        "type": "string",
        "description": "Start date/time in ISO 8601 format"
      },
      "end": {
        "type": "string",
        "description": "End date/time in ISO 8601 format"
      },
      "location": {
        "type": "string",
        "description": "Event location"
      }
    },
    "required": ["action"]
  }
}
```
