# LAMP Stack Setup on AWS EC2 (Ubuntu)

A practical guide to deploying a LAMP stack — **Linux, Apache, MariaDB, and PHP** — on an AWS EC2 Ubuntu instance to host dynamic web applications.

---

## Architecture Overview

![Architecture Overview](docs/architecture.png)

## What is a LAMP Stack?

| Component | Role |
|-----------|------|
| **Linux** | Operating system — the foundation |
| **Apache** | Web server — handles HTTP requests |
| **MariaDB** | Database — stores and manages app data |
| **PHP** | Scripting language — powers dynamic content |

### How It Works

```
User Request → Apache → PHP (processes logic) → MariaDB (fetches data) → Response to User
```

---

## Prerequisites

- AWS account with an EC2 instance running **Ubuntu 20.04+**
- A key pair (.pem file) for SSH access
- Security group with ports **22 (SSH)** and **80 (HTTP)** open

---

## Setup Guide

### Step 1 — Launch EC2 Instance

Launch an Ubuntu EC2 instance from the AWS Console and SSH into it:

```bash
ssh -i your-key.pem ubuntu@<your-ec2-public-ip>
```

> **[Add Screenshot]** — EC2 instance running in AWS Console

---

### Step 2 — Install Apache

Update packages and install Apache:

```bash
sudo apt update && sudo apt -y upgrade
sudo apt install -y apache2
```

Verify Apache is running by visiting `http://<your-ec2-public-ip>` in a browser.

> **[Add Screenshot]** — Apache default welcome page in browser

---

### Step 3 — Install MariaDB

Install and secure the database server:

```bash
sudo apt install -y mariadb-server mariadb-client
sudo mariadb-secure-installation
```

When prompted, respond as follows:

```
Enter current password for root: (press Enter — no password yet)
Set root password? Y  →  Enter a strong password
Remove anonymous users? Y
Disallow root login remotely? Y
Remove test database? Y
Reload privilege tables? Y
```

**Create a database and user:**

```bash
sudo mariadb -u root -p
```

```sql
CREATE DATABASE test_database;
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'YOUR_PASSWORD';
GRANT ALL PRIVILEGES ON test_database.* TO 'test_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

### Step 4 — Install PHP

```bash
sudo apt install -y php
sudo apt install -y php-{common,mysql,xml,curl,gd,cli,mbstring,opcache,zip,intl}
sudo systemctl restart apache2
```

**Verify PHP installation:**

```bash
sudo nano /var/www/html/info.php
```

Paste the following and save:

```php
<?php phpinfo(); ?>
```

Visit `http://<your-ec2-public-ip>/info.php` to confirm PHP is working.

> **[Add Screenshot]** — PHP info page in browser

---

### Step 5 — Test PHP–Database Connectivity

```bash
sudo nano /var/www/html/database_test.php
```

Paste the following:

```php
<?php
$conn = new mysqli('localhost', 'test_user', 'YOUR_PASSWORD', 'test_database');
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}
echo "Database connection successful!";
?>
```

Visit `http://<your-ec2-public-ip>/database_test.php` to confirm connectivity.

> **[Add Screenshot]** — "Database connection successful!" message in browser


---

## Cleanup

Remove test files before going to production:

```bash
sudo rm /var/www/html/info.php
sudo rm /var/www/html/database_test.php
```

---

## Tech Stack

![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![Linux](https://img.shields.io/badge/OS-Ubuntu_20.04+-blue?logo=linux)
![Apache](https://img.shields.io/badge/Web_Server-Apache2-red?logo=apache)
![MariaDB](https://img.shields.io/badge/Database-MariaDB-teal?logo=mariadb)
![PHP](https://img.shields.io/badge/Language-PHP-purple?logo=php)

---

## Author

**Sinsha C** — [GitHub](https://github.com/sinsha-c)

> *Part of my DevOps learning journey — documenting real-world setups as I get back into the field.*