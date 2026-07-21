# Nextcloud Calendar Event Properties Reference

## Supported Properties

| Property | Type | Description | Example |
|----------|------|-------------|---------|
| `title` | string | Event title (required) | `"Team Meeting"` |
| `description` | string | Event description | `"Weekly sync"` |
| `start` | datetime | Start date/time | `"2025-01-20T09:00:00"` |
| `end` | datetime | End date/time | `"2025-01-20T10:00:00"` |
| `allDay` | boolean | All-day event | `true` |
| `location` | string | Event location | `"Conference Room A"` |
| `status` | string | Event status | `tentative`, `confirmed`, `cancelled` |
| `transparency` | string | Time blocking | `busy` (blocks time) or `free` |
| `priority` | number | Priority (1-9) | `1` (highest), `5` (normal), `9` (lowest) |
| `categories` | array/string | Tags/categories | `["meetings", "work"]` |
| `calendar` | string | Calendar name | `"default"`, `"work"` |
| `recurrence` | string | Recurrence rule | `"FREQ=WEEKLY;BYDAY=MO"` |
| `exceptions` | array | Dates to exclude | `["2025-02-03T09:00:00"]` |
| `attendees` | array | List of attendees | See Attendees section |
| `organizer` | object | Event organizer | `{ "email": "...", "name": "..." }` |
| `reminders` | array | Reminders/alarms | See Reminders section |
| `url` | string | Link URL | `"https://meet.google.com/xxx"` |
| `sequence` | number | Event version | `0` (increment on update) |

## Status Values

| Value | Description |
|-------|-------------|
| `tentative` or `TENTATIVE` | Event is tentative |
| `confirmed` or `CONFIRMED` | Event is confirmed (default) |
| `cancelled` or `CANCELLED` | Event was cancelled |

## Transparency Values

| Value | Description |
|-------|-------------|
| `busy` or `OPAQUE` | Blocks time on calendar (default) |
| `free` or `TRANSPARENT` | Shows as free time |

## Date Formats

- ISO 8601: `"2025-01-20T09:00:00"`
- With timezone: `"2025-01-20T09:00:00Z"` (UTC)
- With offset: `"2025-01-20T09:00:00+02:00"`

## Attendees Object

```json
{
  "email": "colleague@example.com",
  "name": "John Doe",
  "status": "ACCEPTED"
}
```

### Attendee Status Values

| Value | Description |
|-------|-------------|
| `NEEDS-ACTION` | Awaiting response (default) |
| `ACCEPTED` | Accepted |
| `DECLINED` | Declined |
| `TENTATIVE` | Tentatively accepted |

## Reminders Object

```json
{
  "minutes": 30,
  "action": "DISPLAY",
  "description": "Meeting in 30 minutes"
}
```

### Reminder Actions

| Value | Description |
|-------|-------------|
| `DISPLAY` | Show notification (default) |
| `EMAIL` | Send email reminder |
| `AUDIO` | Play sound |

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
| Interval | `FREQ=MONTHLY;INTERVAL=3` | Every 3 months |

### Common Recurrence Examples

```
Every weekday: FREQ=WEEKLY;BYDAY=MO,TU,WE,TH,FR
Every Monday: FREQ=WEEKLY;BYDAY=MO
First Monday of month: FREQ=MONTHLY;BYDAY=1MO
Last Friday of month: FREQ=MONTHLY;BYDAY=-1FR
Every 2 weeks: FREQ=WEEKLY;INTERVAL=2
```

## Example Requests

### Simple Event
```json
{
  "title": "Lunch with team",
  "start": "2025-01-20T12:00:00",
  "end": "2025-01-20T13:00:00",
  "location": "Restaurant"
}
```

### All-Day Event
```json
{
  "title": "Company Holiday",
  "start": "2025-12-25",
  "end": "2025-12-26",
  "allDay": true
}
```

### Full Event with All Properties
```json
{
  "title": "Project Kickoff",
  "description": "Initial meeting for Q1 project",
  "start": "2025-01-20T09:00:00",
  "end": "2025-01-20T11:00:00",
  "location": "Conference Room A",
  "status": "confirmed",
  "transparency": "busy",
  "priority": 1,
  "categories": ["meetings", "important"],
  "calendar": "work",
  "organizer": {
    "email": "you@example.com",
    "name": "Your Name"
  },
  "attendees": [
    {
      "email": "colleague@example.com",
      "name": "John Doe",
      "status": "ACCEPTED"
    },
    {
      "email": "boss@example.com",
      "name": "Jane Smith",
      "status": "NEEDS-ACTION"
    }
  ],
  "reminders": [
    { "minutes": 60, "action": "EMAIL", "description": "Meeting in 1 hour" },
    { "minutes": 15, "action": "DISPLAY" }
  ]
}
```

### Recurring Meeting
```json
{
  "title": "Weekly Team Standup",
  "description": "Quick weekly sync",
  "start": "2025-01-20T09:30:00",
  "end": "2025-01-20T09:45:00",
  "recurrence": "FREQ=WEEKLY;BYDAY=MO,WE,FR",
  "location": "Zoom",
  "categories": ["meetings", "recurring"]
}
```

### Monthly Report Reminder
```json
{
  "title": "Submit Monthly Report",
  "start": "2025-01-31T17:00:00",
  "allDay": true,
  "recurrence": "FREQ=MONTHLY;BYMONTHDAY=-1",
  "reminders": [
    { "minutes": 1440, "action": "EMAIL", "description": "Report due tomorrow!" }
  ],
  "categories": ["work", "deadlines"]
}
```
