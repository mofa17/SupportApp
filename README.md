# Inova IT Support Ticketing System

A lightweight, fully self-contained IT support ticketing app built as a single HTML file — deployable to Azure Static Web Apps in minutes.

## Features

- **Role-based views** — Manager (Mohamed Abdelsamei) and Agents (Mohamed Alfarra, Mohamed Abdelhamid)
- **SLA timers** — per-priority countdowns (Critical: 4h, High: 8h, Medium: 24h, Low: 72h)
- **Manager assignment panel** — assign/reassign tickets to agents with one click
- **Team workload dashboard** — per-agent active ticket count and SLA breach tracking
- **Full ticket lifecycle** — Open → In Progress → Resolved → Closed
- **Comments** — threaded comments per ticket with user attribution
- **Filters** — by status, priority, SLA breach; plus full-text search
- **Export CSV** — all tickets exported to a downloadable CSV file
- **Export PDF** — per-ticket print-ready PDF via browser print dialog
- **Dark mode** — automatic via `prefers-color-scheme`
- **Responsive** — collapses sidebar on narrow screens

---

## Deployment to Azure Static Web Apps

### Option A — Azure Portal (quickest, no CLI needed)

1. **Push this repo to GitHub**
   ```bash
   git init
   git add .
   git commit -m "Initial commit — IT Support App"
   git remote add origin https://github.com/<your-org>/it-support-app.git
   git push -u origin main
   ```

2. **Create the Static Web App in Azure Portal**
   - Go to [portal.azure.com](https://portal.azure.com)
   - Search for **Static Web Apps** → click **Create**
   - Fill in:
     | Field | Value |
     |---|---|
     | Subscription | Your Azure subscription |
     | Resource Group | Create new: `rg-it-support` |
     | Name | `it-support-inova` |
     | Plan type | **Free** (sufficient for internal use) |
     | Region | UAE North (closest to Egypt) |
     | Source | GitHub |
   - Authorize GitHub, select your repo and `main` branch
   - Build presets: **Custom**
     - App location: `/`
     - Api location: *(leave blank)*
     - Output location: *(leave blank)*
   - Click **Review + Create** → **Create**

3. **Azure auto-creates the GitHub Actions workflow** and deploys automatically on every push to `main`.

4. Your app will be live at:
   ```
   https://it-support-inova.azurestaticapps.net
   ```
   (or the auto-generated URL shown in the Azure Portal overview)

---

### Option B — Azure CLI

```bash
# Login
az login

# Create resource group
az group create --name rg-it-support --location uaenorth

# Create Static Web App (free tier)
az staticwebapp create \
  --name it-support-inova \
  --resource-group rg-it-support \
  --source https://github.com/<your-org>/it-support-app \
  --location uaenorth \
  --branch main \
  --app-location "/" \
  --login-with-github
```

---

### Option C — Deploy directly without GitHub (SWA CLI)

```bash
# Install the SWA CLI
npm install -g @azure/static-web-apps-cli

# Login to Azure
swa login

# Deploy
swa deploy ./  --deployment-token <YOUR_DEPLOYMENT_TOKEN> --env production
```
Get your deployment token from:
**Azure Portal → Static Web App → Manage deployment token**

---

## Custom Domain (optional)

1. In Azure Portal → your Static Web App → **Custom domains** → **Add**
2. Enter your domain (e.g. `support.inova-sysco.com`)
3. Add the CNAME record in your DNS provider pointing to your `azurestaticapps.net` URL
4. Azure provisions a free TLS certificate automatically

---

## Future Upgrades (Phase 2)

When you're ready to add persistence and authentication, these Azure services slot in cleanly:

| Feature | Azure Service |
|---|---|
| Persistent ticket storage | **Cosmos DB** (NoSQL, serverless) |
| Backend API | **Azure Functions** (Node.js or Python) |
| SSO login (Entra ID) | **Azure Static Web Apps built-in auth** with Entra ID provider |
| Email notifications | **Azure Communication Services** or **Logic Apps** |
| File attachments | **Azure Blob Storage** |

The Static Web App tier can be upgraded to **Standard** (~$9/month) to unlock custom auth providers, private endpoints, and staging environments.

---

## Project Structure

```
it-support-app/
├── index.html                          # Entire application (self-contained)
├── staticwebapp.config.json            # SWA routing + security headers
├── README.md                           # This file
└── .github/
    └── workflows/
        └── azure-static-web-apps.yml  # Auto-deploy on push to main
```

---

## Team

| Name | Role |
|---|---|
| Mohamed Abdelsamei | Manager |
| Mohamed Alfarra | Agent |
| Mohamed Abdelhamid | Agent |

SLA policy: Critical 4h · High 8h · Medium 24h · Low 72h
