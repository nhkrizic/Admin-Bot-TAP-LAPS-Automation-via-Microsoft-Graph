# Permissions

## 1. Microsoft Graph Permissions (Delegated)

Granted to the Microsoft-owned app:

```text
PowerPlatform-webcontentsv2-Connector
```

### Required Scopes

```text
User.Read
Device.Read.All
DeviceLocalCredential.Read.All
UserAuthMethod-TAP.ReadWrite.All
```

### Critical Notes

- `User.Read` is **mandatory** — without it, `/me` and delegated context resolution fails
- Old grants must be **deleted and recreated**
- Power Automate connection must be **recreated** after changes

---

## 2. Permission Grant Script (Audit Critical)

### Script Used

**ManagePermissionGrant.ps1** (Microsoft official sample)

See: [`scripts/ManagePermissionGrant.ps1`](../scripts/ManagePermissionGrant.ps1)

### Configuration

- **App:** `PowerPlatform-webcontentsv2-Connector`
- **Resource:** `Microsoft Graph`
- **Delegated Scopes:**
  ```text
  User.Read
  Device.Read.All
  DeviceLocalCredential.Read.All
  UserAuthMethod-TAP.ReadWrite.All
  ```
- **Consent:** `AllPrincipals = YES`

✅ This ensures consistent delegated behavior across all users.

---

## 3. Required Entra Roles

| Role                         | Purpose                       |
| ---------------------------- | ----------------------------- |
| Authentication Administrator | TAP creation                  |
| Cloud Device Administrator   | LAPS LIST + READ              |

> ⚠️ PIM roles must be **activated** — eligible-only roles produce `403 Forbidden`.
