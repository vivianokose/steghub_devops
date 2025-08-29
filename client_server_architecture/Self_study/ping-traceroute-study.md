
# Ping and Traceroute – Network Diagnostic Utilities

## Introduction
When working with networks or servers, it is important to understand how to check connectivity and diagnose routing issues. Two essential tools for this are **ping** and **traceroute**.

---

## Ping
The `ping` command is used to test if a host (computer, server, or website) is reachable. It sends **ICMP Echo Requests** to a target host and waits for a reply.

### Usage
```bash
ping google.com
````

### Example Output

```
PING google.com (142.250.190.14): 56 data bytes
64 bytes from 142.250.190.14: icmp_seq=0 ttl=118 time=12.3 ms
64 bytes from 142.250.190.14: icmp_seq=1 ttl=118 time=11.8 ms
```

* **icmp\_seq** → Sequence number of the packet
* **ttl (Time To Live)** → How many hops the packet can make before being discarded
* **time** → Round-trip time in milliseconds

### What it Tells You

* Confirms if the target is reachable
* Gives you the latency (delay)
* Shows packet loss if the host is unreachable

---

## Traceroute

The `traceroute` (or `tracert` in Windows) command shows the **path packets take** to reach a destination. It lists all the routers (hops) along the way.

### Usage

```bash
traceroute google.com   # Linux/macOS
tracert google.com      # Windows
```

### Example Output

```
1  192.168.1.1 (Router)
2  10.0.0.1
3  142.250.190.14 (google.com)
```

### What it Tells You

* The exact route packets take
* Where delays or failures happen (helps in troubleshooting)

---

## Summary

* **Ping** = Tests reachability and measures delay.
* **Traceroute** = Maps the journey of packets.

Both are vital tools for **network troubleshooting and diagnostics**.

---

---

# Basic SQL Commands Refresher

## Introduction
SQL (Structured Query Language) is used to interact with relational databases like MySQL. This refresher covers essential commands to help you practice simple database operations.

---

## 1. SHOW
Displays information about databases or tables.

```sql
SHOW DATABASES;
SHOW TABLES;
````

---

## 2. CREATE

Used to create new databases or tables.

```sql
CREATE DATABASE school;

USE school;

CREATE TABLE students (
    id INT AUTO_INCREMENT PRIMARY KEY,
    name VARCHAR(50),
    age INT,
    grade VARCHAR(5)
);
```

---

## 3. DROP

Deletes databases or tables permanently.

```sql
DROP DATABASE school;
DROP TABLE students;
```

---

## 4. SELECT

Retrieves data from a table.

```sql
SELECT * FROM students;
SELECT name, age FROM students WHERE grade = 'A';
```

---

## 5. INSERT

Adds new records into a table.

```sql
INSERT INTO students (name, age, grade) 
VALUES ('John Doe', 15, 'A');
```

---

## Example Workflow

```sql
-- Create and use a database
CREATE DATABASE testdb;
USE testdb;

-- Create a table
CREATE TABLE users (
    id INT AUTO_INCREMENT PRIMARY KEY,
    username VARCHAR(50),
    email VARCHAR(100)
);

-- Insert data
INSERT INTO users (username, email) VALUES ('Alice', 'alice@example.com');

-- Retrieve data
SELECT * FROM users;

-- Drop the table
DROP TABLE users;
```

---

## Summary

* **SHOW** → See databases/tables
* **CREATE** → Make new databases/tables
* **DROP** → Delete databases/tables
* **SELECT** → Retrieve data
* **INSERT** → Add new records
