# Lessons Learned

## 1. Inverted Group Membership Logic

The Power Automate connector for checking Entra group membership returns inverted boolean values. A result of `false` means the user **is** a member. This must be accounted for in all condition blocks.

## 2. PIM Role Activation

Eligible PIM roles are not sufficient. Roles must be **actively activated** before the flow runs. Failure to activate produces `403 Forbidden` errors that are not descriptive.

## 3. LAPS Credential Path

LAPS credentials are **not** stored under the `/devices` endpoint. They reside under `/directory/deviceLocalCredentials`, a separate directory resource. Device object IDs from the devices endpoint **cannot** be reused for LAPS lookups.

## 4. User.Read Requirement

The `User.Read` permission is mandatory for delegated flows. Without it, the delegated context cannot resolve the calling user's identity, causing all subsequent Graph calls to fail.

## 5. Permission Grant Lifecycle

When updating Graph permissions for the Power Automate connector:

1. Old grants must be **explicitly removed**
2. The Power Automate connection must be **recreated** to pick up the new permission set
