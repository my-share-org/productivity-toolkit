# Server Access & Connection Commands

> **CRITICAL SECURITY RULE:**
> **Never read, dump, print, or commit the private key (.pem) file. Only use its file path when executing SSH or deployment commands.**

---

## 1. Main Production Server

- **Host IP / Domain:** `{{SERVER_IP_MAIN}}`
- **Username:** `ubuntu`
- **Access Command:**

```bash
ssh -i "C:\path\to\keys\{{SSH_KEY_NAME}}" ubuntu@{{SERVER_IP_MAIN}}
```

---

## 2. Staging / Test Server

- **Host IP / Domain:** `{{SERVER_IP_TEST}}`
- **Username:** `ubuntu`
- **Access Command:**

```bash
ssh -i "C:\path\to\keys\{{SSH_KEY_NAME}}" ubuntu@{{SERVER_IP_TEST}}
```

---

## 3. Auxiliary / Database Server (if applicable)

- **Host IP / Domain:** `0.0.0.0`
- **Username:** `ubuntu`
- **Access Command:**

```bash
ssh -i "C:\path\to\keys\{{SSH_KEY_NAME}}" ubuntu@0.0.0.0
```

---

## 4. Environment & Security Guidelines
1. Store private keys outside the repository in a secure directory (e.g. `~/.ssh/` or dedicated keys directory).
2. Never hardcode database passwords, API secret keys, or JWT tokens in this file. Use `.env` files located inside each specific code repository.
