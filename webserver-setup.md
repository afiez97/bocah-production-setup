# ✅ Web Server Setup (NGINX & APACHE)

---

## 📌 NGINX Setup

- [ ] **Install Nginx**  
  ```bash
  sudo apt install nginx
  ```

- [ ] **Create config file**  
  Location:  
  ```bash
  /etc/nginx/sites-available/your-app
  ```

- [ ] **Sample Nginx config (HTTP Only)**  
  ```nginx
  server {
      listen 80;
      server_name your_domain.com;

      root /var/www/your-app/public;
      index index.php index.html;

      location / {
          try_files $uri $uri/ /index.php?$query_string;
      }

      location ~ \.php$ {
          include snippets/fastcgi-php.conf;
          fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
      }

      location ~ /\.ht {
          deny all;
      }

      location ~ /\. {
          deny all;
          access_log off;
          log_not_found off;
      }
  }
  ```

- [ ] **Sample Nginx config (HTTPS + SSL)**  
  ```nginx
  server {
      listen 443 ssl http2;
      server_name your_domain.com;

      ssl_certificate     /etc/ssl/certs/your_domain.crt;
      ssl_certificate_key /etc/ssl/private/your_domain.key;

      root /var/www/your-app/public;
      index index.php index.html;

      location / {
          try_files $uri $uri/ /index.php?$query_string;
      }

      location ~ \.php$ {
          include snippets/fastcgi-php.conf;
          fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
      }

      location ~ /\.ht {
          deny all;
      }

      ssl_protocols TLSv1.2 TLSv1.3;
      ssl_ciphers HIGH:!aNULL:!MD5;
  }

  server {
      listen 80;
      server_name your_domain.com;

      return 301 https://$host$request_uri;
  }
  ```

- [ ] **Enable site**
  ```bash
  sudo ln -s /etc/nginx/sites-available/your-app /etc/nginx/sites-enabled/
  ```

- [ ] **Test config**
  ```bash
  sudo nginx -t
  ```

- [ ] **Restart Nginx**
  ```bash
  sudo systemctl restart nginx
  ```

---

## 📌 APACHE Setup (Optional)

- [ ] **Install Apache**
  ```bash
  sudo apt install apache2 libapache2-mod-php
  ```

- [ ] **Enable rewrite module**
  ```bash
  sudo a2enmod rewrite
  ```

- [ ] **Create config file**  
  Location:  
  ```bash
  /etc/apache2/sites-available/your-app.conf
  ```

- [ ] **Sample Apache config**
  ```apache
  <VirtualHost *:80>
      ServerName your_domain.com
      DocumentRoot /var/www/your-app/public

      <Directory /var/www/your-app/public>
          AllowOverride All
          Require all granted
      </Directory>

      ErrorLog ${APACHE_LOG_DIR}/error.log
      CustomLog ${APACHE_LOG_DIR}/access.log combined
  </VirtualHost>
  ```

- [ ] **Enable site**
  ```bash
  sudo a2ensite your-app.conf
  ```

- [ ] **Reload Apache**
  ```bash
  sudo systemctl reload apache2
  ```

---

📝 **Nota Tambahan**  
- Pastikan folder `storage` dan `bootstrap/cache` diberi permission `www-data` jika gunakan Laravel.
- Gunakan `ufw` untuk buka port 80 dan 443 jika server ada firewall.
