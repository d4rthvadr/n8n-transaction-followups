# n8n Local Development Setup

This setup provides a local n8n instance using Docker Compose with a pre-built AI agent workflow template.

## 🎯 Goal & Motivation

This repository is designed to help you learn how **n8n can automate everyday business problems** through powerful workflow automation and AI orchestration.

### Why n8n?

Modern businesses face repetitive tasks that consume valuable time and resources. n8n empowers you to:

- **Automate routine operations**: Customer follow-ups, data entry, report generation, and more
- **Orchestrate AI capabilities**: Integrate AI models (like Google Gemini, OpenAI) to add intelligence to your workflows
- **Connect your tools**: Seamlessly integrate 400+ apps and services (Google Sheets, Slack, email, databases, APIs)
- **Build custom solutions**: Create tailored automation without complex coding

### What You'll Learn

Through this example workflow, you'll discover how to:

1. **Build AI-powered chatbots** that can answer questions and take actions
2. **Integrate multiple data sources** (APIs, spreadsheets, RSS feeds) into one intelligent agent
3. **Use AI to make decisions** and route information based on context
4. **Create scalable automations** that grow with your business needs

Whether you're automating customer support, processing transactions, generating reports, or building intelligent assistants, this starter template shows you the foundation of what's possible with n8n.

## Prerequisites

- Docker Desktop installed and running
- Docker Compose v3.8 or higher
- Google Cloud Console account (for Google Sheets integration)

## Quick Start

1. Start n8n:

   ```bash
   docker-compose up -d
   ```

2. Access n8n at: http://localhost:5678

3. Default credentials:

   - Username: `admin`
   - Password: `admin`

4. Import the workflow template (see [Using the Workflow Template](#using-the-workflow-template) below)

## Commands

- **Start n8n**: `docker-compose up -d`
- **Stop n8n**: `docker-compose down`
- **View logs**: `docker-compose logs -f n8n`
- **Restart n8n**: `docker-compose restart`
- **Remove everything** (including data): `docker-compose down -v`

## Using the Workflow Template

This repository includes a pre-built AI agent workflow (`workflow-template-001.json`) that demonstrates:

- AI-powered chatbot using Google Gemini
- Weather forecasting
- News RSS feeds
- Google Sheets integration

### Importing the Workflow

1. Log into n8n at http://localhost:5678
2. Click **"Add Workflow"** in the top menu
3. Click the **three dots (⋮)** menu → **Import from File**
4. Select `workflows/workflow-template-001.json` from this repository
5. The workflow will be imported with all nodes pre-configured

### Setting Up Credentials

The workflow requires two main credentials:

#### 1. Google Gemini API (Required)

1. Go to [Google AI Studio](https://aistudio.google.com/app/apikey)
2. Click **"Create API key in new project"**
3. Copy the API key
4. In n8n, open the **"Connect Gemini"** node
5. Click **"Credential to connect with"** → **"Create New"**
6. Paste your API key and click **"Save"**

#### 2. Google Sheets OAuth (Optional - for sheet integration)

See the [Google Sheets API Setup](#google-sheets-api-setup) section below for detailed instructions.

### Testing the Workflow

1. After setting up credentials, click the **"🗨 Open chat"** button in the workflow
2. Try asking:
   - "What's the weather in Paris?"
   - "Get me the latest tech news"
   - "Give me ideas for n8n AI agents"

## Google Sheets API Setup

To enable Google Sheets integration in the workflow, you need to create OAuth credentials in Google Cloud Console.

### Step 1: Create a Google Cloud Project

1. Go to [Google Cloud Console](https://console.cloud.google.com/)
2. Click **"Select a project"** → **"New Project"**
3. Enter a project name (e.g., "n8n-integration") and click **"Create"**
4. Wait for the project to be created and select it

### Step 2: Enable Google Sheets API

1. In the Google Cloud Console, go to **"APIs & Services"** → **"Library"**
2. Search for **"Google Sheets API"**
3. Click on it and click **"Enable"**

### Step 3: Configure OAuth Consent Screen

1. Go to **"APIs & Services"** → **"OAuth consent screen"**
2. Select **"External"** user type and click **"Create"**
3. Fill in the required fields:
   - **App name**: "n8n Local"
   - **User support email**: Your email
   - **Developer contact information**: Your email
4. Click **"Save and Continue"**
5. On the **Scopes** page, click **"Add or Remove Scopes"**
6. Add these scopes:
   - `https://www.googleapis.com/auth/spreadsheets`
   - `https://www.googleapis.com/auth/drive.file`
7. Click **"Update"** → **"Save and Continue"**
8. On **Test users**, click **"Add Users"** and add your Google email
9. Click **"Save and Continue"** → **"Back to Dashboard"**

### Step 4: Create OAuth Credentials

1. Go to **"APIs & Services"** → **"Credentials"**
2. Click **"Create Credentials"** → **"OAuth client ID"**
3. Select **"Desktop app"** as the application type
4. Enter a name (e.g., "n8n Desktop Client")
5. Click **"Create"**
6. A dialog will appear with your credentials - click **"Download JSON"** or copy:
   - **Client ID**
   - **Client Secret**

### Step 5: Configure Credentials in n8n

1. In n8n, open the **"Get row(s) in sheet in Google Sheets"** node
2. Click **"Credential to connect with"** → **"Create New"**
3. Enter the credentials:
   - **Client ID**: Paste from Google Cloud Console
   - **Client Secret**: Paste from Google Cloud Console
4. Click the **"Connect my account"** button
5. Sign in with your Google account and grant permissions
6. Click **"Save"** in n8n

### Step 6: Configure the Sheet

1. Update the **documentId** in the Google Sheets node to point to your own Google Sheet
2. Or use the example sheet URL already configured in the workflow

## Configuration

Edit the environment variables in `docker-compose.yml` or create a `.env` file based on `.env.example`.

### Using PostgreSQL (Optional)

To use PostgreSQL instead of the default SQLite:

1. Uncomment the `postgres` service in `docker-compose.yml`
2. Add these environment variables to the n8n service:
   ```yaml
   - DB_TYPE=postgresdb
   - DB_POSTGRESDB_HOST=postgres
   - DB_POSTGRESDB_PORT=5432
   - DB_POSTGRESDB_DATABASE=n8n
   - DB_POSTGRESDB_USER=n8n
   - DB_POSTGRESDB_PASSWORD=n8n
   ```

## Data Persistence

- Workflows and credentials are stored in Docker volumes
- Local directories `./workflows` and `./credentials` are mounted for easy access

## Troubleshooting

- **Port already in use**: Change the port mapping in docker-compose.yml (e.g., `"8080:5678"`)
- **Container won't start**: Check logs with `docker-compose logs n8n`
- **Reset everything**: Run `docker-compose down -v` to remove all data and start fresh
