
  

# WordPress install


## Step-by-Step Instructions (Starting from Step 6)

  

### 1. Create the WordPress database and user

  

```bash
sudo  mysql  -u  root  -p
```
Inside MySQL prompt:

```sql
CREATE  DATABASE  wordpress;
CREATE  USER 'wordpressuser'@'localhost' IDENTIFIED BY  'super_secret_pw';
GRANT ALL PRIVILEGES ON wordpress.* TO  'wordpressuser'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

- Created a new database named `wordpress`
- Created a user with full privileges on that DB

### 2. Download and Set Up WordPress
```bash
cd  /tmp
curl  -O  https://wordpress.org/latest.tar.gz
tar  xzvf  latest.tar.gz
sudo  cp  -a  wordpress/.  /var/www/html/
```

- Download the latest and greatest WP and put it in the wordpress subfolder in our root web folder.

### 3. Set correct permissions

  

```bash
sudo  chown  -R  www-data:www-data  /var/www/html/
sudo  find  /var/www/html/  -type  d  -exec  chmod  755  {}  \;
sudo  find  /var/www/html/  -type  f  -exec  chmod  644  {}  \;
```

### 4. Configure WordPress database

```bash
cd  /var/www/html
cp  wp-config-sample.php  wp-config.php
nano  wp-config.php
```
- Edited the following lines to match the settings I used above:
```php
define( 'DB_NAME', 'wordpress' );
define( 'DB_USER', 'wordpressuser' );
define( 'DB_PASSWORD', 'password' );
define( 'DB_HOST', 'localhost' );
```

### 5. Restart Apache

```bash
sudo  systemctl  restart  apache2
```

### 11. Final Installation (use browser, 34.148.65.51)

- Navigated to the VM’s external IP in a web browser.
- Completed the WordPress installation wizard:
- Set site title
- Chose admin username and password
- Provided email address

## Final Test
- After setup, logged into the WordPress admin panel successfully.
- Site was visible to the public at the external IP address of the VM.
