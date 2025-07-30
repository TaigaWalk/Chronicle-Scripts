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
- Self-contained with embedded common modules

---

## 🔗 Supported Integrations

| Integration        | Log Type         | Description                                                             |
|--------------------|------------------|-------------------------------------------------------------------------|
| Microsoft Entra ID | `AZURE_AD`       | Captures non-interactive sign-in events via Microsoft Graph API (Beta)  |
| 1Password          | `ONEPASSWORD`    | Pulls audit events using 1Password Events API                           |
| GitHub             | `GITHUB`         | Collects GitHub org audit logs using the REST API                       |
| Snowflake          | `SNOWFLAKE`      | Gathers usage logs from ACCOUNT_USAGE views in Snowflake                |
| Thinkst Canary     | `THINKST_CANARY` | Ingests audit trail logs from the Canary console                        |

---

## 🧱 Folder Structure

```
chronicle-scripts/
├── common/                                   # Shared modules for Chronicle ingestion
│   ├── ingest.py                            # Core ingestion functionality
│   ├── utils.py                             # Utility functions
│   ├── env_constants.py                     # Environment variable constants
│   └── __init__.py                          # Module initialization
├── 1password-chronicle-ingestor/            # 1Password Events API ingestion
├── entra-noninteractive-chronicle-ingestor/ # Microsoft Entra non-interactive sign-ins
├── github-chronicle-ingestor/               # GitHub audit log ingestion
├── snowflake-chronicle-ingestor/            # Snowflake ACCOUNT_USAGE logs
└── thinkst-canary-chronicle-ingestor/       # Thinkst Canary audit trail ingestion
```

---

## 🚀 Setup & Installation

### Prerequisites

- Python 3.8+
- Google Cloud credentials (service account key)
- Chronicle instance configured

### Dependencies

Each connector requires specific dependencies. Install them using:

```bash
# For GitHub connector
cd github-chronicle-ingestor
pip install -r requirements.txt

# For other connectors
cd <connector-name>
pip install -r requirements.txt
```

### Environment Variables

Set the following environment variables for your Chronicle instance:

- `CHRONICLE_CUSTOMER_ID`: Your Chronicle customer ID
- `CHRONICLE_REGION`: Chronicle region (default: "us")
- `CHRONICLE_SERVICE_ACCOUNT`: Path to your service account JSON file
- `CHRONICLE_NAMESPACE`: Your Chronicle namespace

---

## 🧠 Configuration Notes

- All secrets (API tokens, credentials, org URLs) are securely managed via **GitHub Actions Secrets**
- Each `main.py` script handles:
  - Authentication to source platform
  - API communication and log retrieval
  - Pushing logs to Chronicle via the Unstructured Ingestion API
- The `common/` module provides shared functionality for Chronicle ingestion

---

## 🕒 Scheduling & Execution

Each connector includes a GitHub Actions workflow that can:

- Be triggered **manually**
- Run on a **cron schedule** (e.g., every 15 or 30 minutes)

This model gives users flexibility while avoiding the costs associated with always-on cloud infrastructure.

---

## 👥 Contributions

We welcome community contributions! To propose an enhancement or new connector, please open an issue or submit a pull request.

