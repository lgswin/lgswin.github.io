---
layout: post
title: "WordPress installation on GCP"
date: 2025-1-10
categories: [WordPress]
tags: [WordPress, Homepage, GCP]
---

## 1. Create a VM Instance on GCP

#### 1) **Log in to the GCP Console**, navigate to **Compute Engine → VM Instances**.
#### 2) Click **Create Instance**.
#### 3) Configure the instance with the following settings:

- **Name**: Enter your preferred name.
- **Region/Zone**: Choose your desired location.
- **Machine Type**: `e2-micro` (suitable for low-traffic websites).
- **Boot Disk**:
    - Image: `Debian 12 (Bookworm)`
    - Size: Minimum 20GB recommended.
- **Firewall**: Check both **Allow HTTP traffic** and **Allow HTTPS traffic**.

### 4) Click **Create** to launch the VM instance.

---

## 2. Access the Instance via SSH

### 1) Open the GCP Console and locate the created instance.
### 2) Click the **SSH** button to connect to your instance.

---

## 3. Update the System

Once connected, update and upgrade the system packages using the following command:

```bash
sudo apt update && sudo apt upgrade -y
```

---

## 4. Install the LAMP Stack

WordPress requires a LAMP stack (Linux, Apache, MySQL, PHP) to operate. Install the necessary components as follows:

### 1) Install Apache

```bash
sudo apt install apache2 -y
sudo systemctl enable apache2
sudo systemctl start apache2
```

### 2) Install MariaDB

```bash
sudo apt install mariadb-server -y
sudo systemctl enable mariadb
sudo systemctl start mariadb
```

Secure the installation:

```bash
sudo mysql_secure_installation
```

Follow the prompts to set a root password and secure your database.

### 3) Install PHP

Install PHP and required extensions:

```bash
sudo apt install php php-mysql libapache2-mod-php -y
sudo apt install php-curl php-gd php-mbstring php-xml php-zip -y
sudo systemctl restart apache2
```

---

## 5. Install WordPress

### 1) Download and Extract WordPress

```bash
cd /tmp
wget https://wordpress.org/latest.tar.gz
tar -xvzf latest.tar.gz
sudo mv wordpress /var/www/html/
```

### 2) Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/html/wordpress
sudo chmod -R 755 /var/www/html/wordpress
```

### 3) Configure Apache for WordPress

Create and edit a new configuration file:

```bash
sudo nano /etc/apache2/sites-available/wordpress.conf
```

Add the following content:

```
<VirtualHost *:80>
    ServerAdmin admin@example.com
    DocumentRoot /var/www/html/wordpress
    ServerName example.com
    ServerAlias www.example.com

    <Directory /var/www/html/wordpress>
        AllowOverride All
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
```

Activate the configuration:

```bash
sudo a2ensite wordpress
sudo a2enmod rewrite
sudo systemctl reload apache
```

### 4) Create a WordPress Database

Log in to MariaDB:

```bash
sudo mysql -u root -p
```

Execute the following SQL commands to create a database and user:

```sql
CREATE DATABASE wordpress DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;
CREATE USER 'wpuser'@'localhost' IDENTIFIED BY 'your_password';
GRANT ALL PRIVILEGES ON wordpress.* TO 'wpuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

---

## 6. Update Apache Default Document Root

Edit the default Apache configuration file:

```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```

Change the `DocumentRoot` to:

```
DocumentRoot /var/www/html/wordpres
```

Restart Apache:

```bash
sudo systemctl restart apache
```

---

## 7. Run the WordPress Installation Wizard

### 1) Open your browser and navigate to:
    
```
http://<your-instance-external-ip>
```
    
### 2) Follow the WordPress installation wizard:
- Enter the database details:
    - Database Name: `wordpress`
    - Username: `wpuser`
    - Password: `your_password`
- Set up your site title, admin username, and password.
  
### 3) Complete the installation and log in to the WordPress admin dashboard.

- If wordpress pages doesn’t be called properly? It needs to set to read `.htaccess` file right.

### Apache setting file

### 1) open Apache setting file
    
```bash
sudo nano /etc/apache2/sites-available/000-default.conf
```
    
### 2) **`<Directory>` section**
add this script in the bottom.

```bash
<Directory /var/www/html>
    AllowOverride All
</Directory>
```
    
### 3) save and restart apache service.
    
```bash
sudo systemctl restart apache2
```