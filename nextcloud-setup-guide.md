# Nextcloud Tasks Integration for n8n

## Prerequisites

1. **Nextcloud instance** running
2. **App Password** generated in Nextcloud

## Step 1: Generate Nextcloud App Password

1. Log into Nextcloud at https://cloud.tal.ahmed-rezk.com
2. Go to **Settings** → **Security**
3. Scroll to **App passwords**
4. Enter a name like "n8n"
5. Click **Create new app password**
6. **Save the credentials** - you'll need them!

## Step 2: Import Workflow into n8n

1. Open n8n at http://localhost:5678
2. Go to **Workflows** → **Import from File**
3. Select `nextcloud-simple-workflow.json`
4. The workflow will appear with a credential error (expected)

## Step 3: Configure Authentication

In the imported workflow:

1. Click on the **"Get Tasks from Nextcloud"** node
2. Under **Authentication**, select **Generic Credential Type**
3. Under **Generic Auth Type**, select **Basic Auth**
4. Click **Create New Credential**
5. Enter:
   - **User**: `admin`
   - **Password**: *(the app password from Step 1)*
6. Click **Save**
7. The credential will be named "Nextcloud API" automatically

## Step 4: Save and Activate

1. Click **Save** to save the workflow
2. Toggle the workflow to **Active**

## Step 5: Test

```bash
curl http://localhost:5678/webhook/nextcloud-tasks
```

## Creating Tasks

Import `nextcloud-create-task-workflow.json` and configure the credential the same way.

```bash
curl -X POST http://localhost:5678/webhook/nextcloud-create-task \
  -H "Content-Type: application/json" \
  -d '{"title": "Buy groceries", "due": "20250120T180000Z"}'
```

## Troubleshooting

- **401 Unauthorized**: Check app password is correct
- **404 Not Found**: Verify NEXTCLOUD_URL in `.env`
- **View logs**: `cd /root/n8n-setup && docker compose logs -f n8n`
