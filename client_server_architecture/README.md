# Client-Server Architecture with MySQL – 101

This project demonstrates how to implement a **basic client-server architecture** using **MySQL Database Management System (DBMS)** on AWS EC2 instances.

We set up:

* **Server A (mysql-server)** → Runs MySQL server
* **Server B (mysql-client)** → Runs MySQL client and connects to the server remotely

This exercise reinforced the **Client-Server model**, where the **client sends requests** and the **server responds**, in this case with database operations.

---

## 🏗️ Architecture

* **mysql-client** connects to **mysql-server** over TCP (port `3306`)
* Both instances run in the same VPC (local network)
* Security Group rules restrict access for safety

![Security group](./images/security%20group.png)

---

## ⚙️ Setup Steps

### 1. Provision EC2 Instances

* Launch **two Ubuntu EC2 instances**:

  * `mysql-server`
  * `mysql-client`

### 2. Install MySQL Server (on mysql-server)

```bash
sudo apt update
sudo apt install mysql-server -y
```

Verify installation:

```bash
sudo systemctl status mysql
```

### 3. Install MySQL Client (on mysql-client)

```bash
sudo apt update
sudo apt install mysql-client -y
```

---

### 4. Configure MySQL for Remote Access

On **mysql-server**:

1. Create a remote user:

   ```sql
   CREATE USER 'remote_user'@'%' IDENTIFIED BY 'Password123!';
   GRANT ALL PRIVILEGES ON *.* TO 'remote_user'@'%' WITH GRANT OPTION;
   FLUSH PRIVILEGES;
   ```

2. Update MySQL config:

   ```bash
   sudo nano /etc/mysql/mysql.conf.d/mysqld.cnf
   ```

   Change:

   ```ini
   bind-address = 0.0.0.0
   mysqlx-bind-address = 127.0.0.1
   ```

![config file](./images/mysql_server%20config%20file%20edit.png)

3. Restart MySQL:

   ```bash
   sudo systemctl restart mysql
   ```

4. Verify MySQL is listening on the network:

   ```bash
   sudo ss -tulnp | grep 3306
   ```

   ✅ Expected: `0.0.0.0:3306`

![MySQL restart and verification](./images/mysql%20restart%20and%20verification.png)

---

### 5. Security Group Configuration (AWS)

On **mysql-server Security Group**:

* Add inbound rule:

  * **Type**: MySQL/Aurora
  * **Port**: 3306
  * **Source**: `mysql-client` **private IP** (e.g., `172.31.xx.yy/32`)

![Security Group](./images/security%20group.png)

---

### 6. Test Connection

On **mysql-client**:

1. Check port accessibility:

   ```bash
   nc -zv 172.31.45.193 3306
   ```

   ✅ Success means the client can reach the server

![port accessibilty check](./images/port%20accessibility%20check.png)

2. Connect using MySQL:

   ```bash
   mysql -h 172.31.45.193 -u remote_user -p
   ```

3. Run query:

   ```sql
   SHOW DATABASES;
   ```

✅ If databases are listed, the setup is complete!

![MySQL connectionand Query running](./images/mysql%20connection%20and%20query%20running.png)

---

## 🐞 Issues & Fixes

### 1. **Error 2003: Can't connect to MySQL server on 'IP:3306'**

* Cause: MySQL was only listening on `127.0.0.1` (localhost)
* Fix: Updated `mysqld.cnf` → `bind-address=0.0.0.0`, restarted MySQL

---

### 2. **MySQL user not restricted to client**

* Initially created user with `'%'` (all hosts) → less secure
* Fix: Created user with specific host:

  ```sql
  CREATE USER 'remote_user'@'172.31.xx.yy' IDENTIFIED BY 'Password123!';
  ```

---

### 3. **Security group blocked connection**

* Cause: Port `3306` not open to client
* Fix: Allowed `mysql-client` private IP in inbound rules


## 🎯 Key Learnings

* Client-Server architecture basics
* MySQL user management (`CREATE USER`, `GRANT`)
* Configuring MySQL to accept remote connections
* AWS Security Group fine-tuning
* Debugging connectivity issues (`ss`, `nc`, error codes)

---

✅ With this, we successfully implemented and troubleshooted a **MySQL client-server architecture on AWS**.

---