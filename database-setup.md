# Database Server (MySQL) Setup

- [ ] `sudo apt update && upgrade`
- [ ] Install: `mysql-server`
- [ ] `sudo mysql_secure_installation`
- [ ] Edit `/etc/mysql/mysql.conf.d/mysqld.cnf`:
  ```
  bind-address = 0.0.0.0
  ```
- [ ] Restart MySQL:
  ```
  sudo systemctl restart mysql
  ```
- [ ] Create database:
  ```
  CREATE DATABASE your_db;
  ```
- [ ] Create user:
  ```
  CREATE USER 'user'@'app_ip' IDENTIFIED BY 'pass';
  ```
- [ ] Grant privileges:
  ```
  GRANT ALL ON your_db.* TO 'user'@'app_ip';
  FLUSH PRIVILEGES;
  ```
- [ ] Open port 3306 in firewall (UFW or cloud)