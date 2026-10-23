## Setup Question: How do you install and configure Apache, Python, and MariaDB on Ubuntu?
**Answer:**
```bash
sudo apt update
sudo apt install -y apache2 mariadb-server python3 python3-venv libapache2-mod-wsgi-py3
sudo a2enmod cgi
sudo systemctl restart apache2

# Install Python packages in a virtual environment (recent Ubuntu versions restrict system-wide pip installs).
sudo python3 -m venv /var/www/venv
sudo /var/www/venv/bin/pip install pymysql mysql-connector-python sqlalchemy
```
Create the database and application user once:
```sql
-- sudo mysql
CREATE DATABASE IF NOT EXISTS python_web_db;
CREATE USER IF NOT EXISTS 'pyuser'@'localhost' IDENTIFIED BY 'pypass123';
GRANT ALL PRIVILEGES ON python_web_db.* TO 'pyuser'@'localhost';
FLUSH PRIVILEGES;
```
Create the shared configuration file at `/usr/lib/cgi-bin/dbconfig.py`:
```python
DB = dict(host="localhost", user="pyuser", password="pypass123", database="python_web_db")
```
Place CGI scripts in `/usr/lib/cgi-bin/`, make each script executable with `sudo chmod +x file.py`, and open it at `http://localhost/cgi-bin/file.py`. Use `#!/var/www/venv/bin/python` as the first line of scripts that use the virtual environment.

Python 3.13 and later no longer include the `cgi` module. The CGI examples below use `urllib.parse` to read form data and do not depend on that module. Older Python versions also support `cgi.FieldStorage()`.

---
