
  

  

# Module 5: Full OPAC and cataloging module setup

  

  

## Introduction

  

  

An OPAC or Online Public Access Catalog is effectively a modern-day take on the card catalog system of days long gone. It’s a new digital way for users to search and find the resources available within the library. The tool usually allows the user to search by the various key data fields like title, author, subject, etc. and will help them find the physical location for the resource they choose. Typically the database used by these tools are also leveraged by other library tools so the resources can be more easily managed by the librarians, doing things like cataloging the resources, handling the circulation, managing the patrons accounts, etc.

  

  

## Relational Database Structure

  

  

For this module we created a very basic relational database system and OPAC front-end to store our records. It consisted of just one table called books which stored the various bibliography data fields within it. Fields such as author, title, publisher, copyright, etc. This allows the front-end web application to easily insert, edit, and find books based on the data the user enters.

  

  

In a more robust database we might find multiple tables with foreign keys that link records. A books table might have a foreign key called book_id that is also found in an authors table such that the OPAC could easily connect all the books an author wrote, allowing users more robust functionality. The primary benefit of normalizing a relational database like this would be to eliminate duplicated data, like have a book appear multiple times in the books table with the author’s name in a column over and over.

  

  

## Step-by-Step Setup

  

  

### 1. LAMP Stack setup

  

  

The OPAC and cataloging system relies on a LAMP stack: Linux, Apache, MySQL, and PHP. Here's the setup:

  

  

#### Always Update the System

  

```bash
sudo  apt  update
sudo  apt  -y  upgrade
sudo  apt  -y  autoremove
sudo  apt  clean
```

  

  

#### Get Apache Installed

  

```bash
sudo  apt  install  -y  apache2
```

  

Apache will be the webserver we use to serve our OPAC HTML/PHP pages.

  

  

#### Installing the MySQL Server

  

```bash
sudo  apt  install  -y  mysql-server
sudo  mysql_secure_installation
```

  

  

#### Now PHP and Required Modules

  

```bash
sudo  apt  install  -y  php  libapache2-mod-php  php-mysql
```

  

  

#### Restart Apache

```bash
sudo  systemctl  restart  apache2
```

  

  

### 2. MySQL User & Database Setup

```sql

create  database  opacdb  default  character  set utf8mb4 collate utf8mb4_unicode_ci;
grant all privileges on opacdb.* to  'opacuser'@'localhost'  with  grant  option;
  

CREATE  TABLE  books (

id INT UNSIGNED NOT NULL AUTO_INCREMENT,

author VARCHAR(150) NOT NULL,

title VARCHAR(150) NOT NULL,

publisher VARCHAR(150) NOT NULL,

copyright_year YEAR(4) NOT NULL,

PRIMARY KEY (id)

);

  
```

  

  

### 3. Upload PHP Files to Server

  

  

Files to upload to `/var/www/html/`:

  

  

-  `insert.php` – Handles catalog record insertion.
-  `cataloging.html` – A basic form to input catalog data.
-  `search.php` – Enables basic search via OPAC.

```bash
sudo  chown  www-data:www-data  /var/www/html/*.php
```

  

  

### 4. How the Provided Code Establishes a Connection to the Database

  

The OPAC uses PHP and the PHP MySQL libraries to connect to the MySQL database. Here's the snippit where it loads credentials from a separate login file and connects:

  

  

```php

  

require_once  '/var/www/login.php';

mysqli_report(MYSQLI_REPORT_ERROR | MYSQLI_REPORT_STRICT);

$conn = new  mysqli($db_hostname, $db_username, $db_password, $db_database);

if ($conn->connect_error) {

  die("Connection failed: "  .  $conn->connect_error);

}

  
```

  

  

### 5. The Cataloging Module

  

  

The HTML form posts to `insert.php`. This code inserts form data (author, title, publisher, copyright) from the previous page.

  

  

```php

  

<?php

$mysqli = new  mysqli("localhost", "opacuser", "super_secret_pw", "opacdb");

if ($mysqli->connect_error) {

  die("Connection failed: "  .  $mysqli->connect_error);

}

$author = $_POST['author'];
$title = $_POST['title'];
$publisher = $_POST['publisher'];
$copyright = $_POST['copyright'];

$sql = "INSERT INTO books (author, title, publisher, copyright_year)
VALUES ('$author', '$title', '$publisher', '$copyright')";

$mysqli->query($sql);
$mysqli->close();
  
?>

  

```

  

  

### 6. Search and retrieval in our bare bones OPAC

  

  

The search form lets our users search the fields we added to the HTML page. The generic "search" field searches multiple fields. Here's :

  

  

```php

  

<?php

$mysqli = new  mysqli("localhost", "opacuser", "super_secret_pw", "opacdb");


$search = $_GET['query'];

$sql = "SELECT  *  FROM books WHERE title LIKE '%$search%' OR author LIKE '%$search%' OR publisher LIKE '%$search%'";

$result = $mysqli->query($sql);


while($row = $result->fetch_assoc()) {

  echo  "Title: "  .  $row["title"] .  "<br>";
  echo  "Author: "  .  $row["author"] .  "<br><hr>";
}


$mysqli->close();

?>

```

  

  

### 7. Additional Steps for a Real-World System
- Better security: Prepared statements, authentication
- More intuitive UI
- More fields and advanced search
- More robust and normalized database
- Probably functionality for patron management, circulation, etc.

  

  

### 8. Final Configuration Notes


Enable services and allow HTTP traffic:

```bash

sudo  systemctl  enable  apache2
sudo  systemctl  enable  mysql
sudo  ufw  allow  'Apache Full'

```

  
## Reflection on the Experience


The OPAC and cataloging module chapters were helpful in understanding the basic flow of how to set up a  database system. I have some expirence with LAMP stacks but not enough to be able to do this from start to finish on my own. I found the step-by-step directions we were given to be straightforward once I followed each step carefully. Apache permissions and where to configure the .htpasswd file would have required some Google-assist for sure.

Attention to detail obviously matters a lot. A single typo or misnamed file caused my webserver not to serve pages, or my PHP to error out and I spent some time fixing my stupid mistakes. 

This assignment helped me better understand the basics of what powers an ILS and how important it is to document processes clearly. Even though we built a very basic system, I can now see the building blocks of what larger systems like Koha or Evergreen use. It's also a great example of how much effort goes into creating or even just installing and maintaining systems like these would be. 
