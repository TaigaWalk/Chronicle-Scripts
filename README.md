# 🛡 Chronicle Ingestion Scripts

This repository contains custom ingestion connectors designed to forward third-party security logs into [Google Chronicle](https://cloud.google.com/chronicle).

Each connector is a lightweight, self-contained Python script that authenticates to a third-party API or platform, collects relevant logs, and pushes them into Chronicle using the **Unstructured Ingestion API**.

---

## 📌 Why This Exists

While Chronicle provides powerful detection capabilities, it does **not offer native integrations** for many widely used security tools and services. This repository was created to:

- Enable ingestion of log sources not supported by default in Chronicle
- Provide a **cost-effective alternative** to GCP Cloud Functions or Cloud Run for those seeking to reduce infrastructure spend
- Standardize and automate ingestion workflows using open-source tooling and GitHub Actions

---

## ✅ Key Features

- Python-based, modular connectors
- GitHub Actions support for **manual** and **scheduled (cron)** runs
- Secure credential injection via GitHub Secrets
- Shared utilities for common ingestion and transformation logic (`common/` directory)

---

## 🔗 Supported Integrations

| Integration        | Log Type   | Description                                                             |
|--------------------|------------|-------------------------------------------------------------------------|
| Microsoft Entra ID | `AZURE_AD` | Captures non-interactive sign-in events via Microsoft Graph API (Beta)  |

---

## 🧱 Folder Structure

```
chronicle-scripts/
├── common/                              # Shared helpers (ingest, utils, env_constants)
│   ├── ingest.py
│   ├── utils.py
│   └── env_constants.py
│
├── entra-noninteractive-chronicle-ingestor/  # Microsoft Entra sign-ins
│   ├── main.py
│   └── requirements.txt
```

---

## 🧠 Configuration Notes

- All secrets (API tokens, org IDs, credentials) are stored securely via **GitHub Actions Secrets**
- Each integration leverages shared logic in the `common/` module
- Each `main.py` handles:
  - Authentication
  - API communication and log parsing
  - Ingestion into Chronicle

---

## 🕒 Scheduling & Execution

Each integration includes a GitHub Actions workflow that can:

- Be triggered **manually**
- Run on a **scheduled cron** interval (e.g., every 15 or 30 minutes)

This approach allows users to maintain full control over ingestion timing and avoid always-on cloud billing from serverless runners.

---

## 👥 Contributions

If you’d like to contribute a new connector or improve an existing one, feel free to open a pull request or submit an issue.
