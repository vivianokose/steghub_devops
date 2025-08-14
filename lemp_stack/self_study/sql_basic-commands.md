---

## ** Basic SQL Syntax & Common Commands**

````markdown
# 📚 Self Study: Basic SQL Syntax & Most Commonly Used Commands

## 🗂 Overview
Structured Query Language (SQL) is the language we use to communicate with databases.  
In this self-study session, I focused on learning the core SQL syntax and the most commonly used commands for creating, reading, updating, and deleting data.

---

## 🚀 Journey & Backstory
When I first opened my SQL terminal, it felt like I had stepped into a secret room full of data treasures. The problem? I didn’t speak the language.  
I knew that learning SQL wasn’t just about typing commands — it was about *thinking* in terms of data tables, relationships, and queries.

I started with simple commands, but let’s be honest — forgetting a `;` at the end of a command became my number one frustration. 😅

---

## 🔑 Basic SQL Syntax

SQL statements usually follow this pattern:

```sql
COMMAND column_name(s)
FROM table_name
WHERE condition;
```

---

## 🛠 Most Common SQL Commands

### 1️⃣ Create a Database

```sql
CREATE DATABASE school;
```

### 2️⃣ Select a Database

```sql
USE school;
```

### 3️⃣ Create a Table

```sql
CREATE TABLE students (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    age INT
);
```

### 4️⃣ Insert Data

```sql
INSERT INTO students (name, age) VALUES ('John Doe', 20);
```

### 5️⃣ View Data

```sql
SELECT * FROM students;
```

### 6️⃣ Update Data

```sql
UPDATE students SET age = 21 WHERE id = 1;
```

### 7️⃣ Delete Data

```sql
DELETE FROM students WHERE id = 1;
```

---

## ⚠ Common Frustrations & Fixes

* **Forgetting the semicolon (`;`)** → SQL won’t execute until you end the statement.
* **Misspelling table or column names** → Use `SHOW TABLES;` and `DESCRIBE table_name;` to check.
* **Case sensitivity confusion** → SQL keywords aren’t case sensitive, but table and column names might be (depends on the database system).

---

## 📚 Resources

* [W3Schools SQL Tutorial](https://www.w3schools.com/sql/)
* [MySQL Documentation](https://dev.mysql.com/doc/)

````
