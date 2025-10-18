# WordPress Deployment on AWS EC2 with LVM Storage

This project demonstrates the deployment and configuration of **WordPress** on an **AWS EC2 Ubuntu server** using **Logical Volume Management (LVM)** for flexible, scalable, and production-grade storage.  

It’s part of a **DevOps foundational learning project** to understand Linux system administration, volume management, and web application hosting on AWS infrastructure.

---

## 🌍 Project Overview

| Category | Details |
|-----------|----------|
| **Cloud Provider** | AWS |
| **Instance Type** | Ubuntu Server (t2.micro / 20.04 or 22.04 LTS) |
| **Storage** | Three (3) x 10GB EBS Volumes configured with LVM |
| **Web Server** | Apache2 |
| **Database** | MySQL |
| **CMS** | WordPress |
| **Scripting Language** | PHP |
| **Goal** | Host WordPress with dynamic and persistent storage using LVM |

---

## 📁 Folder Structure

```

wordpress-on-aws/
│
├── README.md
├── screenshots/
│   ├── ec2-instance.png
│   ├── ebs-volumes.png
│   ├── lvm-setup.png
│   ├── mount-points.png
│   ├── apache-installation.png
│   ├── php-mysql-setup.png
│   ├── wordpress-files.png
│   ├── wp-setup-page.png
│   ├── lsblk-output.png
│   └── architecture-diagram.png
└── commands.txt

````

---

## 🧰 Prerequisites

Before you begin:
- An **AWS Account** with permissions to launch EC2 and create EBS volumes.
- Basic knowledge of **Linux CLI**.
- Security group configured to allow **HTTP (port 80)** and **SSH (port 22)**.

---

## 🚀 Step-by-Step Implementation

### 🟩 STEP 1: Launch EC2 Instance

1. Log in to your **AWS Management Console**.
2. Navigate to **EC2 → Launch Instance**.
3. Choose **Ubuntu Server 22.04 LTS (Free Tier Eligible)**.
4. Configure instance details and create a **Key Pair**.
5. Allow inbound HTTP (port 80) and SSH (port 22) traffic.
6. Launch the instance.

![EC2 Instance Running](/images/01_instances_launched_ubuntu.png)

---

### 🟩 STEP 2: Connect via SSH

```bash
ssh -i "your-key.pem" ubuntu@<EC2-Public-IP>
````

Update and upgrade your system packages:

```bash
sudo apt update -y && sudo apt upgrade -y
```

---

### 🟩 STEP 3: Create and Attach EBS Volumes

Create **3 additional 10GB EBS volumes** and attach them to your instance.

![EBS Volumes Created](/images/volumes%20created%20and%20attached.png)

Verify they are attached:

```bash
lsblk
```

![lsblk Output](/images/08_lsblk_after_partition.png)

---

### 🟩 STEP 4: Configure LVM

1. Identify new disks: `/dev/nvme1n1`, `/dev/nvme2n1`, `/dev/nvme3n1`

2. Create **Physical Volumes (PVs)**:

   ```bash
   sudo pvcreate /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1
   ```

3. Create **Volume Group (VG)**:

   ```bash
   sudo vgcreate webdata-vg /dev/nvme1n1 /dev/nvme2n1 /dev/nvme3n1
   ```

4. Create **Logical Volumes (LVs)**:

   ```bash
   sudo lvcreate -n apps-lv -L 14G webdata-vg
   sudo lvcreate -n logs-lv -L 14G webdata-vg
   ```

![LVM Setup](/images/07_gdisk_partition_nvme1n1.png)

---

### 🟩 STEP 5: Format and Mount Logical Volumes

1. Format the LVs to ext4:

   ```bash
   sudo mkfs.ext4 /dev/webdata-vg/apps-lv
   sudo mkfs.ext4 /dev/webdata-vg/logs-lv
   ```

2. Mount to the appropriate directories:

   ```bash
   sudo mount /dev/webdata-vg/apps-lv /var/www/html
   sudo mount /dev/webdata-vg/logs-lv /var/log
   ```

3. Confirm mount points:

   ```bash
   df -h
   ```

![Mount Points](/images/07_gdisk_partition_nvme1n1.png)

4. Update `/etc/fstab` for persistent mounting:

   ```bash
   sudo blkid
   sudo vi /etc/fstab
   ```

   Add lines:

   ```
   /dev/webdata-vg/apps-lv /var/www/html ext4 defaults 0 0
   /dev/webdata-vg/logs-lv /var/log ext4 defaults 0 0
   ```

---

### 🟩 STEP 6: Install Apache, PHP, and MySQL

Install Apache:

```bash
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

Install PHP and MySQL:

```bash
sudo apt install php libapache2-mod-php php-mysql mysql-server -y
```

---

### 🟩 STEP 7: Install and Configure WordPress

1. Download and extract WordPress:

   ```bash
   wget https://wordpress.org/latest.tar.gz
   tar -xzf latest.tar.gz
   ```

2. Move WordPress files to the web root:

   ```bash
   sudo mv wordpress/* /var/www/html/
   ```

3. Set correct ownership and permissions:

   ```bash
   sudo chown -R www-data:www-data /var/www/html/
   sudo chmod -R 755 /var/www/html/
   ```

4. Restart Apache:

   ```bash
   sudo systemctl restart apache2
   ```

![WordPress Files in HTML Directory](/images/15_wordpress_files_copied.png)

---

### 🟩 STEP 8: Configure WordPress Database

1. Log into MySQL:

   ```bash
   sudo mysql
   ```

2. Create the database and user:

   ```sql
   CREATE DATABASE wordpress_db;
   CREATE USER 'wp_user'@'localhost' IDENTIFIED BY 'yourpassword';
   GRANT ALL PRIVILEGES ON wordpress_db.* TO 'wp_user'@'localhost';
   FLUSH PRIVILEGES;
   EXIT;
   ```

3. Configure `wp-config.php`:

   ```bash
   sudo mv /var/www/html/wp-config-sample.php /var/www/html/wp-config.php
   sudo nano /var/www/html/wp-config.php
   ```

   Update:

   ```php
   define('DB_NAME', 'wordpress_db');
   define('DB_USER', 'wp_user');
   define('DB_PASSWORD', 'yourpassword');
   define('DB_HOST', 'localhost');
   ```

---

### �� STEP 9: Access WordPress in Browser

```
http://54.167.111.208/wordpress
```

![WordPress Setup Page](/images/word_press%20main%20page.png)

---

## 🏗️ Architecture Overview

![Architecture Diagram](/images/architecture%20design.png)

```
+------------------------------------------------------+
|                    AWS EC2 Instance                  |
|------------------------------------------------------|
| OS: Ubuntu 22.04                                     |
| Web Server: Apache2                                  |
| Database: MySQL                                      |
| CMS: WordPress                                       |
|------------------------------------------------------|
| Storage via LVM:                                     |
|   /var/www/html → apps-lv (14GB)                     |
|   /var/log → logs-lv (14GB)                          |
+------------------------------------------------------+
```

---

## ✅ Validation and Testing

* Verified WordPress site is reachable via public IP.
* Confirmed `/var/www/html` and `/var/log` mounted on separate LVM volumes.
* Checked Apache and MySQL services are enabled on boot:

  ```bash
  sudo systemctl is-enabled apache2
  sudo systemctl is-enabled mysql
  ```

---

## 🧠 Key Learnings

* Hands-on experience configuring **LVM** (pvcreate, vgcreate, lvcreate).
* Improved understanding of **persistent storage** with EBS.
* Strengthened Linux administration and **Apache/MySQL** configuration skills.
* Learned **WordPress manual deployment** process.
* Practical exposure to **AWS EC2 networking and permissions**.

---

## 💡 Future Improvements

* Automate setup using **Ansible** or **Terraform**.
* Add **SSL certificate (Let's Encrypt)** for HTTPS support.
* Integrate **CI/CD pipeline** for WordPress updates.

---

## 👩‍💻 Author

**Vivian Chiamaka Okose**
💼 DevOps Enthusiast | AWS | Linux | Automation

🔗 [LinkedIn](https://linkedin.com/in/okosechiamaka)
📧 [Email](vivianokose@gmail.com)