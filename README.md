# Admin Bot – TAP & LAPS Automation via Microsoft Graph

## 1. Executive Summary

This project implements a **Teams-based Admin Bot** using **Power Automate** that allows authorized IT administrators to execute **privileged identity actions** in a **secure, auditable, and fully automated way**.

The bot supports:

- ✅ Creation of **Temporary Access Passes (TAP)** for users
- ✅ Retrieval of **Windows LAPS passwords** for devices
- ✅ **Security-group-based authorization**
- ✅ **Delegated Microsoft Graph access**
- ✅ **Full logging & monitoring via Entra ID / Azure**
- ✅ **Automatic user notification**

All actions are executed **without manual portal access** and follow **least-privilege** and **Zero Trust** principles.

---

## 2. Architecture Overview

```text
Microsoft Teams (Group Chat)
        ↓
Power Automate Cloud Flow
        ↓
User Context Resolution
        ↓
Security Group Authorization
        ↓
Command Parsing (!tap / !laps)
        ↓
Microsoft Graph (delegated)
        ↓
Azure / Entra Logs + Notifications
```

---

## 3. Repository Structure

```
admin-bot-tap-laps/
├── README.md
├── docs/
│   ├── architecture.md
│   ├── flow-design.md
│   ├── security-model.md
│   ├── permissions.md
│   ├── logging-monitoring.md
│   └── lessons-learned.md
├── scripts/
│   └── ManagePermissionGrant.ps1
├── SECURITY.md
├── CONTRIBUTING.md
└── LICENSE
```

---

## 4. Supported Commands

| Command            | Description                       |
| ------------------ | --------------------------------- |
| `!tap <userUPN>`   | Creates a Temporary Access Pass   |
| `!laps <deviceName>` | Retrieves Windows LAPS password |

Invalid commands are rejected with a user-facing message.

---

## 5. Security Controls Summary

| Control             | Status |
| ------------------- | ------ |
| Least Privilege     | ✅     |
| RBAC Enforcement    | ✅     |
| Security Group Gate | ✅     |
| No Secrets in Code  | ✅     |
| User Notification   | ✅     |
| Central Logging     | ✅     |

---

## 6. Status

- ✅ Production ready
- ✅ Audit compliant
- ✅ Reproducible
- ✅ Enterprise approved

---

## Documentation

| Document | Description |
| -------- | ----------- |
| [Architecture](docs/architecture.md) | Flow & Graph architecture |
| [Flow Design](docs/flow-design.md) | Step-by-step Power Automate design |
| [Security Model](docs/security-model.md) | RBAC, groups, roles, threat model |
| [Permissions](docs/permissions.md) | Graph permissions & grant process |
| [Logging & Monitoring](docs/logging-monitoring.md) | Azure / Entra logging & audit |
| [Lessons Learned](docs/lessons-learned.md) | Non-obvious behaviors & pitfalls |
