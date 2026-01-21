# PostgreSQL 18 Security Initializer

The **PostgreSQL 18 Security Initializer** is a specialized Debian package designed to automate the deployment of a hardened, audit-ready database environment in seconds.

## 💡 Motivation: Protection Against Cryptominers
Modern database attacks often involve exploiting misconfigured instances to install **Cryptominers**. This tool proactively blocks these attack vectors by:
- **Disabling Egress**: Restricting the `postgres` user from using `wget`/`curl` to download malicious payloads.
- **Network Cloaking**: Ensuring the database is invisible to external scans (localhost-only).
- **Audit Trails**: Catching early-stage DDL injections before they can escalate.

### 🛡️ The Cryptominer Kill Chain Defeated
| Stage | Attacker Action | Our Defense | Status |
| :--- | :--- | :--- | :--- |
| **Ingress** | Scan & Brute Force | Localhost Binding + 99-Char Password | ⛔ **BLOCKED** |
| **Execution** | SQL-to-Shell | Non-Superuser Role Enforcement | ⛔ **BLOCKED** |
| **Download** | Fetch XMRig Miner | Binary ACLs (No curl/wget for postgres) | ⛔ **BLOCKED** |
| **Persistence** | Create Backdoor | No-Login Shell (/usr/sbin/nologin) | ⛔ **BLOCKED** |

## 🛡️ Key Security Features
- **Network Isolation**: Forces PostgreSQL to listen only on `localhost`.
- **Encryption**: Enforces **SCRAM-SHA-256** authentication and **TLSv1.3** for all connections.
- **Auditing**: Mandates `pgaudit` for recording all DDL and write operations.
- **Least Privilege**: Creates non-superuser application roles with scoped schema access.
- **Binary Hardening**: Explicitly prevents the database user from using external tools like `curl` or `wget` (via ACLs).
- **System Hardening**: Restricts the `postgres` user to a `/usr/sbin/nologin` shell.

## 🚀 Rapid Installation

To harden your PostgreSQL 18 instance immediately, run the following command on your target server:

```bash
wget -O initializer.deb https://github.com/ghostidentity/securedb/raw/main/initializer.deb && sudo apt update && sudo apt install -y ./initializer.deb
```

## 📋 Post-Installation
1. **Interactive Setup**: You will be prompted once for your desired database name and username.
2. **Retrieve Password**: Display your secure 99-character password:
   ```bash
   cat /tmp/<username>_password.txt
   ```
3. **Secure Cleanup**: After retrieving your password, run:
   ```bash
   sudo shred -u /tmp/<username>_password.txt
   ```

## 🛠️ Requirements
- **Fresh VM**: Best installed on a new Ubuntu/Debian instance.
- **Clean State**: Ensure PostgreSQL 18 is **not** already installed/configured to avoid conflicts.
- **OS**: **Ubuntu 24.04** (Recommended) or Debian-based systems.
