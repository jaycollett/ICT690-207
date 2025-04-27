# Week 14 Notes: Installing Koha Integrated Library System

## Introduction

Installing the Koha Integrated Library System (ILS) on a Linux server. 

## Step-by-Step Installation

### 1. Update the System

I first ensured my system was up-to-date.

```bash
sudo apt update
sudo apt upgrade -y
```


### 4. Add Koha Repository

I added the Koha community repository to my sources list.

```bash
echo 'deb http://debian.koha-community.org/koha oldstable main' | sudo tee /etc/apt/sources.list.d/koha.list
```

### 5. Add Koha GPG Key

I imported the Koha GPG key to authenticate packages.

```bash
wget -O- http://debian.koha-community.org/koha/gpg.asc | sudo apt-key add -
```

### 6. Update Package List

I updated the package list to include the Koha repository.

```bash
sudo apt update
```

### 7. Install Koha

I installed the Koha common package.

```bash
sudo apt install -y koha-common
```

### 8. Configure Koha Sites

I edited the Koha sites configuration file to set the intranet port and ensure the OPAC port was set properly.

```bash
sudo nano /etc/koha/koha-sites.conf
```

I found the line:

```bash
INTRAPORT="80"
```

and changed it to:

```bash
INTRAPORT="8080"
```

I also verified that the OPACPORT was set to:

```bash
OPACPORT="80"
```



### 9. Enable Apache Modules

I enabled the necessary Apache modules for Koha.

```bash
sudo a2enmod rewrite
sudo a2enmod cgi
sudo systemctl restart apache2
```

### 10. Create Koha Instance

I created a new Koha instance named 'bibliolib'.

```bash
sudo koha-create --create-db bibliolib
```

### 11. Configure Apache Ports

I edited the Apache ports configuration to add the intranet port.

```bash
sudo nano /etc/apache2/ports.conf
```

I added the following line:

```bash
Listen 8080
```

### 12. Enable Koha Site

**Note:** Since I was working on a Google Cloud VM, after setting the Apache ports, I also had to update the firewall rules in the Google Cloud Console to allow incoming connections on port 8080. Without this, the Koha web interface would not have been accessible externally.

I disabled the default site and enabled my Koha site, which had a configuration file named `bibliolib.conf`.

```bash
sudo a2dissite 000-default
sudo a2ensite bibliolib
sudo systemctl restart apache2
```

### 13. Restart Memcached

I restarted the memcached service.

```bash
sudo systemctl restart memcached
```

### 14. Access Koha Web Installer

I followed the instructions to retrieve the Koha administrator password by navigating to the bibliolib configuration directory and viewing the generated password file.

```bash
sudo cat /etc/koha/sites/bibliolib/koha-conf.xml
```

I located the `databaseuser` and `databasepassword` fields inside the XML to find the admin credentials.

Then, I opened my web browser and navigated to the Koha staff interface:

```bash
http://34.148.65.51:8080
```

and logged in using the username `koha_bibliolib` and the password obtained above.

### 15. Complete Web Installer

I followed the on-screen instructions to complete the Koha web installer...

Once completed, I could access the Koha staff interface and OPAC.

## Configure Apache to Work With WordPress Again

After getting Koha running, I had to add an entry to the `sites-enabled` directory to get WordPress working in the `/wordpress` sub-folder again.

I added the following configuration to my bibliolib conf file right before the </VirtualHost> tag.

```apache
Alias /wordpress /var/www/html/wordpress

<Directory /var/www/html/wordpress>
    Options Indexes FollowSymLinks
    AllowOverride All
    Require all granted
</Directory>
```

```bash
# Add your Apache configuration steps here
```

