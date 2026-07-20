# Nextcloud Task Properties Reference

## Supported Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `title` | string | Task title (required) | `"Buy groceries"` |
| `description` | string | Task description | `"Milk, eggs, bread"` |
| `due` | datetime | Due date | `"2025-12-31T18:00:00"` |
| `startDate` | datetime | Start date | `"2025-01-20T09:00:00"` |
| `completed` | datetime | Completion date | `"2025-01-20T15:30:00"` |
| `status` | string | Task status | See Status Values |
| `priority` | number | Priority (1-9) | `1` (highest), `5` (normal), `9` (lowest) |
| `percentComplete` | number | Progress (0-100) | `50` |
| `categories` | array/string | Tags/categories | `["work", "urgent"]` or `"work,urgent"` |
| `parentTaskUid` | string | Parent task UID | `"n8n-task-123456"` |
| `duration` | string | Duration (ISO 8601) | `"PT1H"` (1 hour), `"P1D"` (1 day) |
| `recurrence` | string | Recurrence rule (RFC 5545) | `"FREQ=WEEKLY;BYDAY=MO"` |
| `url` | string | Link URL | `"https://example.com"` |
| `location` | string | Location | `"Office"` |
| `note` | string | Additional notes | `"Don't forget receipt"` |

## Status Values

| Value | Description |
|-------|-------------|
| `todo` or `NEEDS-ACTION` | Task needs to be done |
| `in-progress` or `IN-PROCESS` | Task is in progress |
| `completed` or `COMPLETED` | Task is done |
| `cancelled` or `CANCELLED` | Task was cancelled |

## Date Formats

- ISO 8601: `"2025-12-31T18:00:00"`
- With timezone: `"2025-12-31T18:00:00Z"` (UTC)
- With offset: `"2025-12-31T18:00:00+02:00"`

## Recurrence Rules (RRULE)

| Pattern | Example | Description |
|---------|---------|-------------|
| Daily | `FREQ=DAILY` | Every day |
| Weekly | `FREQ=WEEKLY;BYDAY=MO,WE,FR` | Mon, Wed, Fri |
| Monthly | `FREQ=MONTHLY;BYMONTHDAY=1` | 1st of each month |
| Monthly | `FREQ=MONTHLY;BYDAY=1FR` | First Friday |
| Yearly | `FREQ=YEARLY` | Every year |
| With count | `FREQ=WEEKLY;COUNT=10` | 10 weeks |
| Until date | `FREQ=DAILY;UNTIL=20251231T235959Z` | Until Dec 31 |

## Duration Format (ISO 8601)

| Duration | Description |
|----------|-------------|
| `PT30M` | 30 minutes |
| `PT1H` | 1 hour |
| `PT2H30M` | 2 hours 30 minutes |
| `P1D` | 1 day |
| `P1W` | 1 week |
| `P1M` | 1 month |
| `P1Y` | 1 year |

## Example Requests

### Simple Task
```json
{
  "title": "Buy groceries"
}
```

### Full Task
```json
{
  "title": "Project deadline",
  "description": "Complete Q1 report and submit to manager",
  "due": "2025-03-31T17:00:00",
  "startDate": "2025-01-15T09:00:00",
  "status": "in-progress",
  "priority": 1,
  "percentComplete": 25,
  "categories": ["work", "important"],
  "location": "Office",
  "note": "Check with finance team first"
}
```

### Recurring Task
```json
{
  "title": "Weekly team meeting",
  "description": "Sync with team on progress",
  "due": "2025-01-20T10:00:00",
  "recurrence": "FREQ=WEEKLY;BYDAY=MO",
  "categories": ["meetings"]
}
```

### Subtask
```json
{
  "title": "Review documentation",
  "parentTaskUid": "n8n-task-1234567890",
  "priority": 3
}
```
