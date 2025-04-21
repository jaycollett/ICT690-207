
# Omeka Installation Guide

## Prerequisites

### 1. Always Update The System

Ensure the system is up to date:

```bash
sudo apt update && sudo apt upgrade
```

### 2. Verify PHP and MySQL Versions

Check installed versions to ensure they meet Omeka's requirements:

```bash
php -v
mysql --version
apache2 -v
```

### 3. Install Required PHP Extensions

Install the required PHP extensions for Omeka, including the DOM extension (that DOM error tripped me up):

```bash
sudo apt install php8.1-xml php8.1-mysql php8.1-cli php8.1-gd php8.1-curl php8.1-mbstring
```

### 4. Install ImageMagick

Omeka uses ImageMagick to create thumbnails of uploaded images:

```bash
sudo apt install imagemagick
```

### 5. Enable Apache mod_rewrite

Omeka requires the `mod_rewrite` module for clean URLs:

```bash
sudo a2enmod rewrite
sudo systemctl restart apache2
```

## Installation Steps

### 1. Create a New MySQL Database and User

Log into MySQL:

```bash
sudo mysql -u root -p
```

Within the MySQL prompt, run the following commands (replace `PUT_REAL_PW_HERE`):

```sql
CREATE DATABASE omeka_db CHARACTER SET utf8 COLLATE utf8_unicode_ci;
CREATE USER 'omeka_user'@'localhost' IDENTIFIED BY 'PUT_REAL_PW_HERE';
GRANT ALL PRIVILEGES ON omeka_db.* TO 'omeka_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

### 2. Download and Extract Omeka

Navigate to the web root directory:

```bash
cd /var/www/html
```

Download the latest Omeka Classic release:

```bash
wget https://github.com/omeka/Omeka/releases/download/v3.1.2/omeka-3.1.2.zip
```

Install `unzip` if it's not already installed:

```bash
sudo apt install unzip
```

Extract the downloaded archive:

```bash
unzip omeka-3.1.2.zip
```

Rename the extracted directory:

```bash
mv omeka-3.1.2 digital_library
```

### 3. Configure Database Settings

Navigate to the Omeka directory:

```bash
cd digital_library
```

Edit the `db.ini` file:

```bash
nano db.ini
```

Replace the placeholder values with my database credentials:

```ini
[database]
host     = "localhost"
username = "omeka_user"
password = "PUT_REAL_PW_HERE"
dbname   = "omeka_db"
prefix   = "omeka_"
charset  = "utf8"
;port     = ""
```

### 4. Set Directory Permissions

Ensure the `files` directory is writable by the web server:

```bash
sudo chown -R www-data:www-data files
sudo chmod -R 755 files
```

### 5. Complete the installation via a web browser



