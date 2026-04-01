# Flow Design

## 1. Flow Trigger & Message Intake

### Trigger

**Trigger:** `When a new message is added to a chat or channel`

**Configuration:**

- Message type: **Group chat**
- Conversation ID: **TheGroupchat**

✅ Ensures the bot only reacts in a controlled environment.

---

## 2. Message & User Resolution

### Get Message Details

**Action:** `Get message details`

- Message ID: dynamic from trigger
- Message type: Group chat
- Groupchat: TheGroupchat

Used to extract:

- Message text
- Sender ID

### Get User Profile (V2)

**Action:** `Get user profile (V2)`

**Input (User ID):**

```text
outputs('Get_message_details')?['body/from/user/id']
```

This user is:

- The **requesting admin**
- Used for authorization, logging and notification

---

## 3. Command Parsing

### Supported Commands

- `!tap <userUPN>`
- `!laps <deviceName>`

### Command Detection Condition

```json
{
  "or": [
    {
      "startsWith": [
        "@outputs('Get_message_details')?['body/body/plainTextContent']",
        "!tap"
      ]
    },
    {
      "startsWith": [
        "@outputs('Get_message_details')?['body/body/plainTextContent']",
        "!laps"
      ]
    }
  ]
}
```

### False Branch

Post message:

> "Command not recognized."

---

## 4. TAP Flow (TRUE Branch – !tap)

### 4.1 Extract Target User

**Compose:**

```text
trim(
  last(
    split(
      outputs('Get_message_details')?['body/body/plainTextContent'],
      ' '
    )
  )
)
```

### 4.2 Create TAP

**HTTP POST**

```http
https://graph.microsoft.com/v1.0/users/{UPN}/authentication/temporaryAccessPassMethods
```

**Body:**

```json
{
  "lifetimeInMinutes": 120,
  "isUsableOnce": true
}
```

### 4.3 Output Handling

- Decode HTML encoding if required
- Post TAP value in controlled chat

### 4.4 User Notification (Mandatory)

📧 **Email to target user**

Contents:

- TAP created
- Timestamp
- Security notice

✅ User awareness
✅ Audit traceability

---

## 5. LAPS Flow (FALSE Branch – !laps)

### 5.1 Extract Device Name

**Compose:**

```text
trim(
  last(
    split(
      outputs('Get_message_details')?['body/body/plainTextContent'],
      ' '
    )
  )
)
```

### 5.2 LAPS Record Lookup (LIST)

> ⚠️ **Critical design rule:**
> LAPS does **not** use `/devices` IDs.

**HTTP GET**

```http
https://graph.microsoft.com/v1.0/directory/deviceLocalCredentials
?$filter=deviceName eq '{DEVICE_NAME}'
```

✅ Requires **Cloud Device Administrator (ACTIVE)**

### 5.3 Extract LAPS Record ID

**Compose:**

```text
first(body('Invoke_an_HTTP_request_1')?['value'])?['id']
```

### 5.4 Retrieve LAPS Credentials

**HTTP GET**

```http
https://graph.microsoft.com/v1.0/directory/deviceLocalCredentials/{LAPS_RECORD_ID}
?$select=credentials
```

### 5.5 Decode Password

```text
base64ToString(
  first(body('Invoke_an_HTTP_request_2')?['credentials'])?['passwordBase64']
)
```

✅ Plaintext local admin password

### 5.6 Chat Output

Password returned **only inside the authorized group chat**.
