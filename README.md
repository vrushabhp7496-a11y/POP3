

---

## 📋 POP3 Commands Cheatsheet

| Command | Purpose | Example |
|---------|---------|---------|
| `USER` | Send username | `USER john` |
| `PASS` | Send password | `PASS secret123` |
| `STAT` | Mailbox status (count + size) | `STAT` → `+OK 2 12345` |
| `LIST` | List all emails (ID + size) | `LIST` |
| `LIST n` | List specific email | `LIST 1` |
| `RETR n` | Retrieve email number n | `RETR 1` |
| `DELE n` | Mark email n for deletion | `DELE 1` |
| `RSET` | Reset all deletion marks | `RSET` |
| `NOOP` | No operation (keep alive) | `NOOP` |
| `TOP n lines` | Headers + first n lines | `TOP 1 10` |
| `UIDL` | Unique ID listing | `UIDL` |
| `QUIT` | End session, apply deletions | `QUIT` |

---

## 🔢 POP3 Response Codes

| Response | Meaning | Security Insight |
|----------|---------|------------------|
| `+OK` | Command successful | User exists / login success |
| `-ERR` | Command failed | Wrong password or invalid command |
| `+OK User name accepted` | Username is valid | **User exists (enumeration possible)** |
| `-ERR Invalid user` | Username invalid | User does not exist |
| `+OK Mailbox locked` | Login successful | Valid credentials |
| `-ERR Authentication failed` | Wrong password | Invalid password |

---

## ⚠️ POP3 Security Issues

| Issue | Description | Risk |
|-------|-------------|------|
| **Plain text authentication** | Username/password sent in clear (port 110) | Credential sniffing |
| **User enumeration** | `USER` command reveals if user exists | Attacker builds valid user list |
| **Brute force** | No account lockout by default | Account compromise |
| **No encryption** | Entire session can be sniffed | Email content leakage |
| **Missing rate limiting** | Unlimited login attempts | Easy brute force |

---

## 🛠️ POP3 Testing Commands

```bash
# Manual connection (plain)
telnet <target> 110

# Manual connection (SSL)
openssl s_client -connect <target>:995

# Nmap service detection
nmap -sV -p 110,995 <target>

# User enumeration (manual)
telnet <target> 110
USER root
# +OK = exists  |  -ERR = no such user

# Hydra brute force
hydra -L users.txt -P pass.txt <target> pop3 -s 110
hydra -L users.txt -P pass.txt <target> pop3s -s 995 -t 4

# Nmap brute force
nmap -p 110 --script pop3-brute --script-args userdb=users.txt,passdb=pass.txt <target>

# Nmap POP3 scripts
nmap -p 110 --script pop3-capabilities <target>
nmap -p 110 --script pop3-ntlm-info <target>
