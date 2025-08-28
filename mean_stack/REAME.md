# MEAN Stack Deployment on Ubuntu (AWS)

## Introduction

This project demonstrates the deployment of a **MEAN stack** application (MongoDB, Express, AngularJS, Node.js) on an Ubuntu server hosted in AWS. The goal was to set up a full-stack JavaScript application that allows basic CRUD operations on a collection of books.

Throughout this deployment, I encountered several challenges, learned about server configuration, package management, and the interplay between backend and frontend frameworks. This README not only documents the steps but also explains **why each step is important**, potential pitfalls, and real-world relevance.


---

## Step 0 – Preparing Prerequisites

Before diving into installation, it’s essential to ensure that the environment is ready.

**Tasks performed:**

* Verified that the Ubuntu server on AWS EC2 is accessible via SSH.
* Confirmed that I had `sudo` privileges to install packages.
* Prepared the project structure in advance to avoid path conflicts later.

**Why this matters:**
Preparing prerequisites ensures smooth installation, prevents permission issues, and lays the groundwork for a well-structured project.

![Add a screenshot of your SSH terminal showing access to the AWS Ubuntu instance.](./images/server%20update%20and%20upgrade.png)


---

## Step 1 – Install Node.js

Node.js is the runtime environment for executing JavaScript on the server.

### 1.1 Update Ubuntu

```bash
sudo apt update
```

![Server Update](./images/server%20update%20and%20upgrade.png)

* Updates the package list to make sure we install the latest versions.


![Terminal output showing `sudo apt update` executed successfully.](./images/server%20update%20and%20upgrade.png)

### 1.2 Upgrade Ubuntu

```bash
sudo apt upgrade -y
```

* Upgrades all installed packages to the latest versions.


![Server Update](./images/server%20update%20and%20upgrade.png)


### 1.3 Add Certificates

```bash
sudo apt install -y curl gnupg
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
```

![Certification addition](./images/certificate%20addition%20I.png)


* Adds Node.js repository certificates for secure installation.

![Certification addition](./images/certificate%20addition%20II.png)

### 1.4 Install Node.js

```bash
sudo apt install -y nodejs
```

* Installs Node.js and `npm`.

![Node.JS and npm installation](./images/nodejs%20install.png)

![Node.JS and npm version](./images/nodejs%20and%20npm%20version.png)

---

## Step 2 – Install MongoDB

MongoDB is the NoSQL database used to store book records.

### 2.1 Install MongoDB

```bash
sudo apt install -y mongodb
```

![MongoDB installation](./images/mongodb%20installation.png)

### 2.2 Start the MongoDB Service

```bash
sudo systemctl start mongodb
sudo systemctl enable mongodb
```

### 2.3 Verify Service

```bash
sudo systemctl status mongodb
```

### 2.4 Install `npm` and Required Packages

```bash
sudo apt install -y npm
npm install body-parser
```

![Body-parser installation](./images/body%20parser%20installation.png)

### 2.5 Project Setup

```bash
mkdir Books
cd Books
npm init -y
touch server.js
```


---

## Step 3 – Install Express and Set Up Routes

Express handles routing, middleware, and HTTP requests.

### 3.1 Project Structure

```bash
cd Books
mkdir apps
cd apps
touch routes.js
mkdir models
touch models/books.js
```

* `routes.js` defines API endpoints.
* `books.js` defines MongoDB schema.


---

## Step 4 – Access the Routes with AngularJS

AngularJS handles frontend logic and communicates with backend APIs.

### 4.1 Project Structure

```bash
cd Books
mkdir public
touch public/index.html
touch public/script.js
```

![Folder structure screenshot showing `public` folder with `index.html` and `script.js`.](./images/script.js%20public%20file.png)

### 4.2 Start the Server

```bash
cd Books
node server.js
```

![Terminal screenshot showing server started (`Listening on port 3000`).](./images/server.js%20running%20in%20command%20line.png)

### 4.3 Test in Browser

* Open the server IP in a browser (e.g., `http://<AWS_PUBLIC_IP>:3000`).
* Test CRUD operations on books.

![Browser screenshot showing the working application and CRUD functionality.](./images/server.js%20running%20in%20browser.png)

---

## Challenges & Lessons Learned

1. **Permissions issues:** Forgetting `sudo` led to installation failures.
2. **MongoDB startup problems:** The server didn’t run automatically until `enable` was used.
3. **Package version conflicts:** Installing the latest LTS versions of Node.js and npm prevented runtime errors.

**Real-life applications:**

* Deploying a MEAN stack app on a cloud server mirrors real production setups.
* Knowledge of server setup, routing, and frontend-backend communication is critical for full-stack development.
* This approach can be adapted for library management, inventory systems, or online catalogs.


---

## Conclusion

This project demonstrates the **end-to-end deployment of a MEAN stack application** on Ubuntu in AWS. It highlights server configuration, database management, API development, and frontend integration. The process strengthened my understanding of full-stack development and cloud deployment best practices.

![Final screenshot of the application fully working in the browser.](./images/testing%20functionality%20of%20the%20running%20server.png)

