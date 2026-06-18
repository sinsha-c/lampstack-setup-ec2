# MINI PROJECT

# LAMP Stack Setup and Configuration for Dynamic Web Applications

## Overview

A LAMP stack is a group of four open-source software components that form the foundation of building and running high-performance dynamic websites and web applications. It is an acronym for Linux, Apache, MySQL or MariaDB, and PHP.

The LAMP stack is simple, stable, and powerful. The first layer is the Linux operating system, which offers flexible configuration options and strong security. Apache is the second layer and provides compiled module support for major scripting languages such as PHP, Perl, Python, Node.js, and others. This HTTP web server delivers web content over the internet.

For the database layer, you can use either MySQL or MariaDB as the relational database management system for your web applications. The last package in the stack is PHP, a scripting language that can be embedded in HTML documents to create static or dynamic websites.

Most modern content management systems such as WordPress, Joomla, Drupal, and Magento require a LAMP stack. The same setup also works for custom web applications built with LAMP technology.

## Components of the LAMP Stack

1. Linux: Linux is the foundation of the LAMP stack. It is a secure and reliable operating system widely used for servers and cloud environments.
2. Apache: Apache is a widely used web server that delivers web content to users over the internet. It handles HTTP requests and serves websites efficiently.
3. MySQL or MariaDB: The database layer stores and manages application data.
   1. MySQL is one of the most popular relational database management systems.
   2. MariaDB is a community-developed fork of MySQL that offers improved performance and compatibility.
4. PHP: PHP is a server-side scripting language used to create dynamic web applications. PHP code can be embedded directly into HTML files.

## How the LAMP Stack Works

The components of the LAMP stack work together in the following way:

1. A user sends a request through a web browser.
2. Apache receives the request.
3. PHP processes the application logic.
4. MySQL or MariaDB stores or retrieves data.
5. Apache sends the final web page back to the user.

This process allows websites to generate dynamic content based on user interactions.

## Installation and Configuration

### 1. Launch an EC2 instance

Launch an EC2 instance with Ubuntu 20.04 or above. In the PDF example, Ubuntu 26.04 is used.

### 2. Install Apache Web Server

SSH to your Ubuntu server as a non-root user, then update the package information index and upgrade your packages.

```bash
sudo apt update && sudo apt -y upgrade
```

Install the Apache web server.

```bash
sudo apt install -y apache2
```

Visit `http://<public IP address of your server or domain name>` in a web browser. You should see the default Apache web page.

### 3. Install a Database Server

You can install either MySQL or MariaDB when deploying a LAMP stack. The PDF uses MariaDB.

```bash
sudo apt install -y mariadb-server mariadb-client
```

Secure the database server.

```bash
sudo mariadb-secure-installation
```

Enter the following choices and press Enter for each prompt:

```text
Enter current password for root (enter for none):
Set root password? [Y/n]: Y
New password: EXAMPLE_PASSWORD
Re-enter new password: EXAMPLE_PASSWORD
Remove anonymous users? [Y/n] Y
Disallow root login remotely? [Y/n] Y
Remove test database and access to it? [Y/n] Y
Reload privilege tables now? [Y/n] Y
```

Log in to MariaDB as root.

```bash
sudo mariadb -u root -p
```

Create a test database.

```sql
CREATE DATABASE test_database;
```

Create a test user and grant full privileges to the database.

```sql
CREATE USER 'test_user'@'localhost' IDENTIFIED BY 'EXAMPLE_PASSWORD';
GRANT ALL PRIVILEGES ON test_database.* TO 'test_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Your database server is now ready.

### 4. Install PHP

Install the PHP package.

```bash
sudo apt install -y php
sudo apt install -y php-{common,mysql,xml,xmlrpc,curl,gd,imagick,cli,dev,imap,mbstring,opcache,soap,zip,intl}
```

Restart Apache to load PHP.

```bash
sudo systemctl restart apache2
```

### 5. Test PHP

Create an `info.php` file in the root directory of your web server.

```bash
sudo nano /var/www/html/info.php
```

Add the following content:

```php
<?php
phpinfo();
```

Visit `http://<public IP address>/info.php` in a web browser. You should see a detailed PHP page.

## Test PHP Connectivity with the Database

Create a database test file.

```bash
sudo nano /var/www/html/database_test.php
```

Add the following content:

```php
<?php
$conn = new mysqli('localhost', 'test_user', 'EXAMPLE_PASSWORD', 'test_database');
if ($conn->connect_error) {
    die("Database connection failed: " . $conn->connect_error);
}
```

Save and close the file, then visit:

```text
http://<public ip of your instance>/database_test.php
```

You should get output showing that the script successfully connected to the database.

## Conclusion

Your LAMP stack is now ready on Ubuntu, and you can begin building a dynamic website or cloud-based web application.

The LAMP stack remains one of the most powerful and widely used web development platforms in the world. By combining Linux, Apache, MySQL or MariaDB, and PHP, developers can create secure, scalable, and dynamic web applications efficiently.

---

## 🛠️ Tech Stack

![AWS](https://img.shields.io/badge/AWS-EC2-orange?logo=amazonaws)
![Nginx](https://img.shields.io/badge/Nginx-Web%20Server-green?logo=nginx)
![Linux](https://img.shields.io/badge/Linux-Amazon%20%7C%20Ubuntu%20%7C%20RHEL-blue?logo=linux)
![Bash](https://img.shields.io/badge/Bash-Scripting-black?logo=gnubash)

---

##  Author

**Sinsha C** — [GitHub](https://github.com/sinsha-c)

---