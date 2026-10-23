## Setup Question: How do you organize the bookstore project files and configure a shared database connection?
**Answer:** Place all project files in `/var/www/html/bookstore/` (or `htdocs/bookstore/` in XAMPP). Open the project at `http://localhost/bookstore/`.

Project file structure:
```
bookstore/
  db.php  header.php  footer.php  style.css  validate.js
  index.php  book.php  register.php  login.php  logout.php
  cart.php  checkout.php  invoice.php  orders.php  wishlist.php  search.php
  admin/ books.php  orders.php  reports.php
  images/ (book covers)
```

Include the shared `db.php` file from pages with `require 'db.php';`:
```php
<?php
session_start();
$pdo = new PDO("mysql:host=localhost;dbname=bookstore;charset=utf8mb4", "root", "", [
    PDO::ATTR_ERRMODE => PDO::ERRMODE_EXCEPTION,
    PDO::ATTR_DEFAULT_FETCH_MODE => PDO::FETCH_ASSOC,
]);
function e($s) { return htmlspecialchars((string)$s, ENT_QUOTES, 'UTF-8'); }   // output sanitize
function require_login() { if (empty($_SESSION['uid'])) { header("Location: login.php"); exit; } }
function require_admin() { if (($_SESSION['role'] ?? '') !== 'admin') { die("Admins only"); } }
```

---
