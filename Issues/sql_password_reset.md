# MySQL Password Reset & User Access Guide (Ubuntu + MySQL 8.0)

## 🛠️ Scenario
You need to reset the MySQL root password (or another user's password) when you've lost access or want to modify credentials without a working grant table.

---

## 📋 Environment
- **OS**: Ubuntu 24.04
- **MySQL**: 8.0.41

---

## 🚨 WARNING
These steps should only be performed **on non-production** or **when maintenance window is scheduled**, as it involves starting MySQL **without security checks** (`--skip-grant-tables`).

---

## ✅ Steps

### 1. Stop MySQL if running
```bash
sudo systemctl stop mysql
```

### 2. Create socket directory if it doesn't exist
```bash
sudo mkdir -p /var/run/mysqld
sudo chown mysql:mysql /var/run/mysqld
```
> MySQL uses `/var/run/mysqld/mysqld.sock` as a Unix socket file to communicate with clients. If the directory doesn't exist, MySQL won't be able to create the socket and will fail to start.

### 3. Start MySQL with grant tables skipped
```bash
sudo mysqld_safe --skip-grant-tables &
```
Wait a few seconds for it to start.

### 4. Login without password
```bash
mysql -u root
```

### 5. List existing users
```sql
SELECT User, Host FROM mysql.user;
```

### 6. Reset Password
#### Option A: Clear existing authentication string (use with `--skip-grant-tables`)
```sql
UPDATE mysql.user
SET authentication_string = ''
WHERE User = 'root' AND Host = 'localhost';
FLUSH PRIVILEGES;
```
> This clears the password but doesn't set a new one. Use it to regain access temporarily.

#### Option B: Proper reset (after restarting normally)
```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY 'YourNewPassword';
```
> This only works **after you restart MySQL without** `--skip-grant-tables`.

### 7. Exit and Restart MySQL Normally
```bash
exit
sudo systemctl restart mysql
```

### 8. Login with new password
```bash
mysql -u root -p
```

---

## 🧪 Extra: List Databases and Tables
```sql
SHOW DATABASES;
USE your_db_name;
SHOW TABLES;
```

---

## 🧑‍💻 Who Are Clients? Why Unix Socket?
- **Clients** are tools/apps connecting to MySQL (e.g., command-line, PHP, Django, scripts).
- **Unix socket** is a special file used for fast communication on the same system (faster than TCP).
- By default, MySQL client connects via **socket** (`/var/run/mysqld/mysqld.sock`) when `localhost` is used.
- If the directory/socket is missing, MySQL startup will fail, especially in `--skip-grant-tables` safe mode which depends on minimal startup configuration.

---

## 📦 Relevance for DevOps / SRE / Infra Roles
- MySQL troubleshooting on local/VM/cloud
- Resetting DB credentials for broken apps or integrations
- Required for recovery tasks (like AWS RDS, Docker volumes)
- Needed in CI/CD scripts and playbooks (e.g., Ansible)
- General infra automation and maintenance

---

## 💡 Concepts: Host Values in MySQL
- `localhost`: Connects via Unix socket; limited to local machine.
- `127.0.0.1`: Connects over TCP/IP; also local, but bypasses socket.
- `0.0.0.0`: Binds MySQL to all available network interfaces; used for remote access.

> Choose host bindings carefully to avoid security vulnerabilities.

---

## ✅ Best Practices
- Use `ALTER USER` over directly updating the `mysql.user` table.
- Keep `--skip-grant-tables` usage minimal.
- Secure credentials using vaults, `.env` files, or secret managers.

---

## 🧾 References
- [MySQL 8.0 Docs - ALTER USER](https://dev.mysql.com/doc/refman/8.0/en/alter-user.html)
- [Ubuntu MySQL Troubleshooting](https://help.ubuntu.com/community/MySQL)

---

_Updated: April 9, 2025_

_Authored by: Manoj (with ChatGPT assistance)_

