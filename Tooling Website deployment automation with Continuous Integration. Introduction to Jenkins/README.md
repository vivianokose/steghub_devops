
---

## 🌍 PROJECT OVERVIEW (Beginner Edition)

**Goal:** Automate your website code deployment using Jenkins and GitHub.
**Flow:**
GitHub → Jenkins → NFS Server → (used by your web servers in future projects).

---

## ⚙️ PHASE 1: REBUILD ENVIRONMENT

### 🧩 Step 1 — Create Jenkins Server

#### 1️⃣ Go to AWS → EC2 → Launch Instance

* **Name:** `Jenkins-Server`
* **AMI:** Ubuntu Server 20.04 LTS
* **Instance type:** t2.micro (Free tier)
* **Key pair:** Select your existing `.pem` key or create new one (save it!)
* **Security Group:**

  * SSH (port 22) → Your IP
  * Custom TCP (port 8080) → Anywhere (for Jenkins UI)
  * HTTP (port 80) → Anywhere

**Jenkins Launch Instance**

![EC2 summary page showing your Jenkins server’s details before you click](./images/jenkin%20instance%20running.png)

---

#### 2️⃣ Connect to your Jenkins Server

Once it’s running, click the instance → **Connect → SSH client tab**
Copy the command and paste it into your terminal (replace `<key>` and `<public-ip>`):

```bash
ssh -i "your-key.pem" ubuntu@<jenkins-public-ip>
```

💡 *Tip:* If you see “permission denied”, run:

```bash
chmod 400 your-key.pem
```

---

#### 3️⃣ Update and install Java

Jenkins needs Java to run.

```bash
sudo apt update
sudo apt install default-jdk-headless -y
java -version
```

You should see something like:
`openjdk version "11.0.x" ...`

---

#### 4️⃣ Install Jenkins

Add Jenkins key and repo:

```bash
wget -q -O - https://pkg.jenkins.io/debian-stable/jenkins.io.key | sudo apt-key add -
sudo sh -c 'echo deb https://pkg.jenkins.io/debian-stable binary/ > /etc/apt/sources.list.d/jenkins.list'
sudo apt update
```

Then install:

```bash
sudo apt install jenkins -y
```

Start and enable the service:

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

✅ You should see “active (running)”.

**Jenkins running (green “active” status)**

![Jenkins running (green “active” status)](./images/jenkins%20active%20and%20running.png)

---

#### 5️⃣ Access Jenkins Dashboard

In your browser, visit:

```
http://<Jenkins-Public-IP>:8080
```

You’ll see a **“Unlock Jenkins”** screen.

To get the password:

```bash
sudo cat /var/lib/jenkins/secrets/initialAdminPassword
```

Copy and paste it into the unlock field.

Choose **Install Suggested Plugins**, then create your first admin user (e.g. `admin`, password of your choice).

👉 **Screenshot Spot #4:** Jenkins setup wizard page showing “Install Suggested Plugins”.

👉 **Screenshot Spot #5:** Jenkins dashboard home after successful login.

---

## ⚙️ PHASE 2: SET UP GITHUB CONNECTION (Continuous Integration)

### 🧩 Step 2 — Prepare GitHub Repository

#### 1️⃣ Go to GitHub

* Create a new repository named **tooling**
  Example URL: `https://github.com/yourusername/tooling`

Add a simple file:

* `README.md` (you can just write “Tooling CI Project”)

**GitHub repo main page after creating `tooling` repo.**

![GitHub repo main page after creating `tooling` repo](./images/tooling%20repo.png)

---

### 🧩 Step 3 — Create Jenkins Job

#### 1️⃣ In Jenkins Dashboard → click “New Item”

* Name: `tooling_github`
* Choose: **Freestyle project**
* Click **OK**

---

#### 2️⃣ Connect to your GitHub repo

Inside the configuration page:

* Scroll to **Source Code Management**
* Select **Git**
* Paste your GitHub repo HTTPS URL (e.g., `https://github.com/yourusername/tooling`)
* If private → add GitHub credentials:

  * Click **Add → Username/Password**
  * Use GitHub username and **personal access token**

**Jenkins project config showing Git repo URL added.**

![Jenkins project config showing Git repo URL added.](./images/Jenkins%20project%20config%20showing%20Git%20repo%20URL%20added.png)

---

#### 3️⃣ Test Manual Build

* Click **Build Now**
* Wait for the build to finish (blue dot = success)
* Click the build (#1) → **Console Output**

You should see:

```
Cloning the remote Git repository
Finished: SUCCESS
```

**Jenkins showing Build #1 success.**

![Jenkins showing Build #1 success](./images/build%20success.png)

---

### 🧩 Step 4 — Enable Webhooks (Automation)

#### 1️⃣ On GitHub

* Go to your repo → **Settings → Webhooks → Add Webhook**
* **Payload URL:**
  `http://<Jenkins-Public-IP>:8080/github-webhook/`
* **Content type:** `application/json`
* **Event:** “Just the push event”
* Click **Add Webhook**

It should show a green check ✅ under “Active”.

**Webhook added and active on GitHub.**

![Webhook added and active on GitHub.](./images/add%20webhook.png)

![](./images/webhook%20active.png)

---

#### 2️⃣ In Jenkins

Go to your job → **Configure**

* Scroll to **Build Triggers**
* Check ✅ “GitHub hook trigger for GITScm polling”
* Scroll down → “Post-build Actions” → Add **Archive the artifacts**
* In **Files to archive**, type:

```
**/*
```

Click **Save**.

---

#### 3️⃣ Test the Webhook

Make a small change to your GitHub repo (edit `README.md`, commit, push).

Go back to Jenkins → it should automatically start a new build (#2).

👉 **Screenshot Spot #10:** Jenkins showing build triggered automatically after GitHub change.

![](./images/webhook%20active.png)

---

## ⚙️ PHASE 3: ADD NFS SERVER & DEPLOY ARTIFACTS

### 🧩 Step 5 — Create NFS Server

#### 1️⃣ Launch another EC2 instance

* **Name:** `NFS-Server`
* **AMI:** RHEL 8
* **Type:** t2.micro
* **Security Group:**

  * SSH (22) → Your IP
  * NFS (2049) → Anywhere
  * Custom TCP (22) → Jenkins Server’s IP

**EC2 summary page for NFS-Server.** 
![](./images/NFS-Server%20running.png)

---

#### 2️⃣ Connect to NFS Server

```bash
ssh -i "your-key.pem" ec2-user@<nfs-public-ip>
```

#### 3️⃣ Install and configure NFS utilities

```bash
sudo yum update -y
sudo yum install nfs-utils -y
sudo mkdir -p /mnt/apps
sudo chown -R ec2-user:ec2-user /mnt/apps
```

---

### 🧩 Step 6 — Configure Jenkins to Deploy via SSH

#### 1️⃣ Back in Jenkins Dashboard

Go to:
**Manage Jenkins → Manage Plugins → Available tab → search “Publish Over SSH”**

Install and restart Jenkins.

**Plugin installation progress.**
![](./images/pluggin%20installation%20progress.png)

![](./images/pluggin%20installation%20progress%20II.png)

---

#### 2️⃣ Configure SSH Settings

**Manage Jenkins → Configure System → Publish over SSH section**

Fill in:

* **Name:** NFS_Server
* **Hostname:** `<NFS-Server-Private-IP>`
* **Username:** `ec2-user`
* **Remote Directory:** `/mnt/apps`
* **Key:** Paste your `.pem` key content

Click **Test Configuration** → should say “Success”.

**Successful SSH test connection.**
![](./images/successful%20SSH%20test%20connection.png)

---

#### 3️⃣ Update Jenkins Job to Deploy

Go to your job → **Configure → Post-build Actions → Add → Send build artifacts over SSH**

Set:

* **Name:** NFS_Server
* **Source files:** `**/*`
* **Remove prefix:** (leave blank)
* **Remote directory:** `/mnt/apps`

Click **Save**.

---

#### 4️⃣ Test Deployment

Edit your GitHub repo’s README.md → Commit → Push.

Jenkins should:

1. Detect the webhook
2. Run build automatically
3. Transfer files to NFS

Check in Jenkins **Console Output**, you should see:

```
SSH: Transferred X file(s)
Finished: SUCCESS
```

**Jenkins console showing “Transferred files” message.**
![Jenkins console showing “Transferred files” message.](./images/jenkin%20console%20output%20.png)

---

#### 5️⃣ Confirm on NFS Server

SSH into your NFS server:

```bash
cat /mnt/apps/README.md
```

You should see your latest GitHub content!

---

## 🎉 YOU DID IT!

You’ve built a **Continuous Integration (CI)** pipeline where:

* Code changes on GitHub → trigger Jenkins automatically (webhook)
* Jenkins builds → sends files to NFS server

This is the **foundation** of real-world DevOps pipelines.

---