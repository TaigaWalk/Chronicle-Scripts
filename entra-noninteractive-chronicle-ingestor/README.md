# Microsoft Entra Non-Interactive Sign-In Ingestor

This module is a custom Python-based ingestion script designed to forward non-interactive sign-in events from **Microsoft Entra ID** (formerly Azure AD) into **Google Chronicle** using the Unstructured Ingestion API.

---

## 🔍 What It Does

- Authenticates to Microsoft Graph API (Beta)
- Fetches **non-interactive sign-in events** (e.g., service principal or daemon access)
- Parses, normalizes, and forwards logs to Chronicle
- Executes via GitHub Actions on a scheduled or manual basis

> 🔎 **Note:** Chronicle currently provides native ingestion only for **interactive** Entra sign-ins. This script fills a critical visibility gap for service-based or daemon-based sign-ins, improving coverage and auditability.

---

## 📂 Included Script

### `main.py`

This script:
- Retrieves an OAuth2 token using Microsoft client credentials
- Queries Microsoft Graph Beta API for sign-ins filtered by `signInEventTypes eq 'nonInteractiveUser'`
- Uses `common/ingest.py` to send the data to Chronicle’s Unstructured Ingestion API

---

## 🛠 Requirements

- Python 3.7+
- GitHub repository with GitHub Actions enabled
- Microsoft Graph API access via:
  - Tenant ID
  - Client ID
  - Client Secret
- Chronicle access:
  - Customer ID
  - Region
  - Namespace (optional)
  - Base64-encoded service account JSON key

---

## 🔐 GitHub Secrets (Required)

Store these values in your GitHub repository’s **Secrets**:

- `GRAPH_CLIENT_ID`
- `GRAPH_CLIENT_SECRET`
- `GRAPH_TENANT_ID`
- `CHRONICLE_CUSTOMER_ID`
- `CHRONICLE_REGION`
- `CHRONICLE_NAMESPACE`
- `CHRONICLE_CREDENTIALS_JSON` (Base64-encoded Chronicle service account JSON)

> You can rename the secrets to your preference but must update both the workflow and script accordingly.

---

## ⚙ GitHub Actions Workflow

This repo includes a prebuilt GitHub Actions workflow at:

```
.github/workflows/entra.yml
```

It runs:
- **Every 15 minutes** (`cron`)
- **Manually** via the GitHub UI (`workflow_dispatch`)

### Key steps:
- Sets up Python environment
- Installs script dependencies
- Writes Chronicle service account JSON to disk
- Runs the Entra ingestion script with all required secrets

---

## 🧪 Running Locally

You can run the script manually for testing:

```bash
export GRAPH_CLIENT_ID=...
export GRAPH_CLIENT_SECRET=...
export GRAPH_TENANT_ID=...
export CHRONICLE_CUSTOMER_ID=...
export CHRONICLE_REGION=...
export GOOGLE_APPLICATION_CREDENTIALS=/path/to/chronicle_sa.json

python main.py --creds-file /path/to/chronicle_sa.json
```

---

## 📄 Notes

- Uses the **Microsoft Graph Beta API** — subject to schema changes
- The default filter fetches only **non-interactive** sign-in events
- Useful for organizations that require **fine-grained service principal monitoring**

---

## 🤝 Contributions

Want to enhance this module or add support for other Entra sign-in types? Open a PR or submit an issue!
