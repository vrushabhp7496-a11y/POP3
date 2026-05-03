# 📬 POP3 (Post Office Protocol version 3) – Complete Guide

## 📌 Table of Contents

1. What is POP3?
2. POP3 vs IMAP
3. POP3 Commands
4. POP3 Response Codes
5. POP3 Security Issues
   
---

## 1. What is POP3?

**POP3 (Post Office Protocol version 3)** is a protocol used by email clients to **retrieve emails** from a mail server.

- **Port 110** – Default POP3 (plain text)
- **Port 995** – POP3 over SSL (POP3S – encrypted)
- **Download & Delete** model – emails are downloaded to the client and **usually deleted** from the server
- **Stateless** – once emails are downloaded, server does not track them

> POP3 is a **pull** protocol – it fetches emails from the server to the client. It does NOT send emails (SMTP is used for sending).

---

## 2. POP3 vs IMAP

| Feature | POP3 | IMAP |
|---------|------|------|
| Primary purpose | Download emails | Sync emails across devices |
| Ports | 110 (plain), 995 (SSL) | 143 (plain), 993 (SSL) |
| Email storage | Downloaded locally, removed from server | Remains on server |
| Multi-device access | ❌ Difficult (emails scattered) | ✅ Seamless sync |
| Server space required | ❌ No (after download) | ✅ Yes |
| Offline access | ✅ Yes | ✅ Yes |
| Use case | Single device user | Multiple devices (phone, laptop, tablet) |

---

## 3. POP3 Commands

| Command | Purpose | Example |
|---------|---------|---------|
| `USER` | Send username | `USER john` |
| `PASS` | Send password | `PASS secret123` |
| `STAT` | Get mailbox status (number of emails, total size) | `STAT` → `+OK 2 12345` |
| `LIST` | List all emails (ID + size) | `LIST` |
| `LIST n` | List specific email | `LIST 1` |
| `RETR n` | Retrieve email number n | `RETR 1` |
| `DELE n` | Mark email n for deletion | `DELE 1` |
| `RSET` | Reset all deletion marks | `RSET` |
| `NOOP` | No operation (keep alive) | `NOOP` |
| `TOP n lines` | Retrieve headers + specified lines of email | `TOP 1 10` |
| `UIDL` | Get unique ID listing for all emails | `UIDL` |
| `QUIT` | End session and apply deletions | `QUIT` |

---

## 4. POP3 Response Codes

| Response | Meaning | Security Insight |
|----------|---------|------------------|
| `+OK` | Command successful | User exists / login success |
| `-ERR` | Command failed | Wrong password or invalid command |
| `+OK User name accepted` | Username is valid | **User exists (enumeration possible)** |
| `-ERR Invalid user` | Username invalid | User does not exist |
| `+OK Mailbox locked` | Login successful | Valid credentials |
| `-ERR Authentication failed` | Wrong password | Invalid password |

---

## 5. POP3 Security Issues

| Issue | Description | Risk |
|-------|-------------|------|
| **Plain text authentication** | Username/password sent in clear (port 110) | Credential sniffing |
| **User enumeration** | `USER` command reveals if user exists | Attacker builds valid user list |
| **Brute force** | No account lockout by default | Account compromise |
| **No encryption** | Entire session can be sniffed | Email content leakage |
| **Missing rate limiting** | Unlimited login attempts | Easy brute force |

---
