# Hermes Agent + n8n Integration Guide

## Overview

Hermes Agent has a **built-in n8n MCP catalog entry** — no custom server needed. This guide shows you how to set it up.

## Prerequisites

1. **Hermes Agent installed**
   ```bash
   curl -fsSL https://hermes-agent.nousresearch.com/install.sh | bash
   ```

2. **n8n running** (your current setup at `http://localhost:5678`)

3. **n8n API Key** (different from app password)

---

## Step 1: Generate n8n API Key

1. Open n8n: http://localhost:5678
2. Go to **Settings** → **API**
3. Click **Create an API Key**
4. Copy the key (you'll need it in Step 3)

---

## Step 2: Install n8n MCP from Catalog

Run the interactive MCP picker:

```bash
hermes mcp
```

Or install directly:

```bash
hermes mcp install n8n
```

This will:
1. Clone the n8n MCP bridge from `https://github.com/CyberSamuraiX/hermes-n8n-mcp`
2. Create a Python virtual environment
3. Install dependencies
4. Prompt you for credentials

---

## Step 3: Configure Credentials

When prompted, enter:

| Prompt | Value |
|--------|-------|
| n8n instance URL | `http://127.0.0.1:5678` |
| n8n API key | *(your API key from Step 1)* |

These are saved to `~/.hermes/.env`:
```
N8N_BASE_URL=http://127.0.0.1:5678
N8N_API_KEY=your-api-key-here
```

---

## Step 4: Select Tools

Hermes will show a checklist of available tools:

```
Select tools for 'n8n' (SPACE toggle, ENTER confirm)
[x] health              Check n8n connection status
[x] list_workflows      List all workflows
[x] get_workflow        Get workflow details
[x] find_workflows      Search workflows
[x] list_executions     List execution history
[x] get_execution       Get execution details
[x] recent_failures     Show recent failed executions
[x] export_workflow     Export workflow as JSON
[ ] activate_workflow   Activate a workflow (mutation)
[ ] deactivate_workflow Deactivate a workflow (mutation)
[ ] docker_container_logs  Get n8n container logs
```

**Recommended:** Keep the defaults (read-only tools). Enable mutations only if needed.

---

## Step 5: Start Hermes

```bash
hermes chat
```

The n8n tools are now available!

---

## Usage Examples

### List All Workflows
```
hermes> List all my n8n workflows
```

### Check Workflow Details
```
hermes> Show me the details of the Nextcloud Tasks workflow
```

### Find Failed Executions
```
hermes> Show recent failed n8n executions
```

### Export a Workflow
```
hermes> Export the Nextcloud Tasks workflow to a file
```

### Check n8n Health
```
hermes> Is n8n running and accessible?
```

---

## What This Enables

With the n8n MCP connected, Hermes can:

1. **Monitor workflows** — Check status, view executions
2. **Debug failures** — Find and inspect failed runs
3. **Manage workflows** — Activate/deactivate (if enabled)
4. **Export/Import** — Backup workflows as JSON
5. **Search** — Find workflows by name or description

---

## Combining with Nextcloud Integration

Your n8n workflows expose webhooks for Tasks and Calendar. Hermes can:

1. Use the **n8n MCP** to manage the workflows themselves
2. Use **bash tool** to call the webhooks:
   ```bash
   curl -X POST http://localhost:5678/webhook/nextcloud-tasks \
     -H "Content-Type: application/json" \
     -d '{"action": "fetch"}'
   ```

---

## Troubleshooting

### "Connection refused"
- Ensure n8n is running: `docker compose ps`
- Check the URL in config: `cat ~/.hermes/.env`

### "Invalid API key"
- Regenerate in n8n: Settings → API → Create API Key
- Update `~/.hermes/.env`

### Tools not appearing
- Restart Hermes: `hermes chat`
- Check MCP status: `hermes mcp` (should show "enabled")

---

## Configuration File

The MCP config is stored in `~/.hermes/config.yaml`:

```yaml
mcp_servers:
  n8n:
    command: "/home/user/.hermes/git/hermes-n8n-mcp/.venv/bin/python"
    args:
      - "/home/user/.hermes/git/hermes-n8n-mcp/server.py"
    env:
      N8N_BASE_URL: "http://127.0.0.1:5678"
      N8N_API_KEY: "your-api-key"
    tools:
      include:
        - health
        - list_workflows
        - get_workflow
        - find_workflows
        - list_executions
        - get_execution
        - recent_failures
        - export_workflow
```

---

## Useful Commands

```bash
# View MCP catalog
hermes mcp catalog

# Reconfigure tools
hermes mcp configure n8n

# Check MCP status
hermes mcp

# View n8n MCP source
cat ~/.hermes/git/hermes-n8n-mcp/server.py
```

---

## Links

- [Hermes Agent Docs](https://hermes-agent.nousresearch.com/docs)
- [n8n MCP Source](https://github.com/CyberSamuraiX/hermes-n8n-mcp)
- [Your n8n Workflows](https://github.com/ahmed-rezk-dev/n8n-workflows)
