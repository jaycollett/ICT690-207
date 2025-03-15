# Module 4: Portfolio Assignment
**Jeremy Collett**  
**LIS690-207**  

- **A brief introduction to what a LAMP stack is and why it is commonly used.**  
  1. LAMP stands for (Linux, Apache, MySQL, and PHP). It’s a software stack that supports websites with database needs and the PHP part of LAMP is the primary language and interpreter used in a LAMP stack to allow developers to write code to produce data-driven dynamic websites. It’s been the holy-grail for ‘nix web developers for a very long time because it’s solid, well matured, and typically very secure. I would say that it’s still very common, even in enterprise service offerings, although it’s surely given up ground to Python (I do love Python)!  

- **Installation steps for Apache, PHP, and MySQL, with necessary commands.**  
  1. Update the apt database with:  
     ```bash
     sudo apt update
     ```  
     and upgrade any required packages with:  
     ```bash
     sudo apt upgrade
     ```  
     By having Linux already, you have the L complete and are ready for the A.  
  2. Install Apache2 with:  
     ```bash
     sudo apt install apache2
     ```  
     Ensure it’s running with:  
     ```bash
     sudo systemctl status apache2
     ```  
     Then verify the "It works" HTML page by accessing the server’s loopback address or public IP.  
  3. Now for the M, MySQL. Install it with:  
     ```bash
     sudo apt install mysql-server
     ```  
     Secure it post-install with:  
     ```bash
     sudo mysql_secure_installation
     ```  
     You can continue by creating databases, users, etc. from the CLI.  
  4. Finally, the P, PHP. Since we want PHP to be able to work with Apache and MySQL, we need to install a few things. First, install PHP and the Apache module with:  
     ```bash
     sudo apt install php libapache2-mod-php
     ```  
     Restart Apache with:  
     ```bash
     sudo systemctl restart apache2
     ```  

- **Configuration changes you made to each service (e.g., editing configuration files).**  
  1. Apache2 required some basic config changes to work with our default `index.php` page.  
     - Update `dir.conf` in `/etc/apache2/mods-enabled` so that `index.php` is first and is served by default when you go to the “root” of the website address:  
       ```apache
       DirectoryIndex index.php index.html index.cgi index.pl index.xhtml index.htm
       ```  
  2. Update our bash shell to give us a nicer MySQL prompt. Adding:  
     ```bash
     export MYSQL_PS1="[\d]> "
     ```  
     to our `.bashrc` shell configuration results in the database name being added to our MySQL prompt!  

- **How you verified that each component was working properly and together.**  
  1. This was iterative. First, I followed the directions to make sure that Apache was up and running and would serve the default "It Worked!" HTML page. Once I was able to view that from the localhost, I checked it from my public IP.  
  2. Second, I validated I had installed MySQL properly. For this, I followed the assignment directions and logged into the MySQL service locally as root, created a database and user with a password, then logged out and tried to log back in as that user and modify the database schema.  
  3. Third, I validated PHP initially with a simple:  
     ```bash
     php -v
     ```  
     Then I checked to see if PHP was working with Apache using the `phpinfo()` method in an `info.php` file on the server. That was accessible by localhost and public IP (I remembered to remove it!).  
  4. Lastly, I used the demo code to create a simple PHP page that would log into the MySQL server with the user I created, fetch some data, and display that data to the user on the website using PHP code.  

- **Any challenges you encountered and how you resolved them.**  
  1. I’ve installed LAMP quite a few times over the years, but it’s been some time since I did it by hand. I usually use a script to automate it or select to have most of it installed when I install the server if I’m doing something with headless Linux.  
  2. I did get stumped for a bit on the instructions to access the `opac.php` page. I thought I missed a step where it was supposed to be served up as the default document and couldn’t understand why it wouldn’t load when I visited the public IP of my VM. I re-read the assignment a few times and may have still missed that, but it does work fine when accessed directly.  

## Links to my assignment sites:
- [OPAC Page](http://34.148.65.51/opac.php)  
- [Index Page](http://34.148.65.51/index.html)  
