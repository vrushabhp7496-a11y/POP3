# 📬 POP3 (Post Office Protocol version 3) – Complete Guide

## 📌 Table of Contents

1. What is POP3?
2. POP3 vs IMAP
3. POP3 Commands
4. POP3 Response Codes
5. POP3 Security Issues
6. POP3 Security Testing (Enumeration, Brute Force, Sniffing)
7. POP3 Hardening (Defense)
8. Commands Cheatsheet
9. Hands-on Labs

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

### POP3 Session Example

```text
$ telnet mail.example.com 110
Trying 192.168.1.10...
Connected to mail.example.com.
+OK POP3 server ready
USER john
+OK User name accepted
PASS secret123
+OK 3 messages (12345 octets)
STAT
+OK 3 12345
LIST
+OK 3 messages
1 1024
2 2048
3 9273
RETR 1
+OK 1024 octets
Received: from ...
From: sender@example.com
Subject: Hello

Message body...
.
DELE 1
+OK Message 1 deleted
QUIT
+OK POP3 server signing off


---
# +OK = exists  |  -ERR = no such user

# Hydra brute force
hydra -L users.txt -P pass.txt <target> pop3 -s 110
hydra -L users.txt -P pass.txt <target> pop3s -s 995 -t 4
