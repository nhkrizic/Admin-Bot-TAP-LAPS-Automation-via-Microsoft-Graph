# Security Model

## 1. Access Control Model

### Security Group

Only members of the following Entra ID security group are authorized to execute bot commands:

```text
SG_IT-Bot-Requesters
```

The flow explicitly validates group membership **before** any privileged action is executed.

---

## 2. Authorization Logic

### Check Group Membership (V2)

**Action:** `Check group membership (V2)`

**Input (User ID):**

```text
outputs('Get_user_profile_(V2)')?['body/id']
```

### Condition – Membership Validation (IMPORTANT)

**Condition:**

```text
equals(
  outputs('Check_group_membership_(V2)')?['body/value'],
  false
)
```

### Logic (CORRECT – Inverted):

| Result | Meaning                | Action         |
| ------ | ---------------------- | -------------- |
| true   | User is **NOT** member | **STOP FLOW**  |
| false  | User **IS** member     | **CONTINUE**   |

This inverted logic is required because `Check group membership (V2)` returns `false` when the user **is a member**.

### Failure Action

Post message:

> "You are not authorized to use this admin command."

✅ **Audit relevance:** Explicit access boundary enforced.

---

## 3. Security Controls

| Control             | Implemented |
| ------------------- | ----------- |
| Least Privilege     | ✅          |
| RBAC                | ✅          |
| Security Group Gate | ✅          |
| No Secrets in Code  | ✅          |
| User Notification   | ✅          |
| Central Logging     | ✅          |

---

## 4. Reporting Security Issues

Security issues must **not** be disclosed publicly. Contact the internal IT security team directly.
