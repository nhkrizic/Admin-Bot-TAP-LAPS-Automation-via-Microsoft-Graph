# Architecture

## Execution Model & Identity Context

### Execution Type

- **Delegated permissions**
- Flow runs in the **context of the requesting user**
- No application secrets or client credentials

### Required Entra Roles (ACTIVE)

| Role                              | Purpose                       |
| --------------------------------- | ----------------------------- |
| Authentication Administrator      | Required for TAP creation     |
| Cloud Device Administrator        | Required for LAPS LIST + READ |
| (Optional) Helpdesk Administrator | Not sufficient for LAPS LIST  |

> ⚠️ **Important:**
> PIM roles must be **activated**.
> "Eligible" roles alone will cause **403 Forbidden**.

---

## Architecture Flow

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

## Component Boundaries

The system spans three layers:

1. **Presentation:** Microsoft Teams group chat
2. **Orchestration:** Power Automate cloud flow with delegated connector
3. **Data plane:** Microsoft Graph v1.0 REST API
