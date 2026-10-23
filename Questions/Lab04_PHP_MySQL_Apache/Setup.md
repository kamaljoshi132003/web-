## Setup Question: How do you install and start Apache, PHP, and MariaDB on Ubuntu?
**Answer:**
```bash
sudo apt update
sudo apt install -y apache2 php libapache2-mod-php php-mysql php-mbstring php-curl mariadb-server
sudo systemctl start apache2 mariadb
```
Project folder: `/var/www/html/lab4` (for XAMPP: `C:\xampp\htdocs\lab4`). Test URL: `http://localhost/lab4/`.

The shared `db.php` file can be included in other scripts with `require 'db.php';`:
```php
<?php
$dsn = "mysql:host=localhost;dbname=college_db;charset=utf8mb4";
try {
    $pdo = new PDO($dsn, "root", "", [
        PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
        PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
    ]);
} catch (PDOException $e) {
    die("Connection failed: " . $e->getMessage());
}
```
(On Ubuntu, the root account may require a password. You can also create a separate database user; see Q14.)
