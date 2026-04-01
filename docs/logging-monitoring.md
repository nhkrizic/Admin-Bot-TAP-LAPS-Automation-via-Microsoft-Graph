# Logging & Monitoring

## 1. Log Sources

All actions are logged via:

- Entra ID Sign-In Logs
- Directory Audit Logs
- Microsoft Graph Audit Logs

---

## 2. Captured Information

| Field              | Description                            |
| ------------------ | -------------------------------------- |
| Requesting user    | Identity that triggered the command    |
| Executed command   | `!tap` or `!laps` with parameters      |
| Target user/device | UPN or device name                     |
| Timestamp          | UTC time of execution                  |

---

## 3. Forensic Traceability

This logging model enables **full forensic traceability** for any privileged operation performed through the bot.

✅ Fully audit-ready
✅ Forensic traceability
