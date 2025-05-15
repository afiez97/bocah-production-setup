# Web Server Setup (NGINX & APACHE)

## NGINX Setup

- [ ] Install Nginx:
  ```
  sudo apt install nginx
  ```
- [ ] Create config `/etc/nginx/sites-available/your-app`
- [ ] Sample Nginx config:
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
  }
  ```
- [ ] Enable site:
  ```
  sudo ln -s /etc/nginx/sites-available/your-app /etc/nginx/sites-enabled/
  ```
- [ ] Test config:
  ```
  sudo nginx -t
  ```
- [ ] Restart Nginx:
  ```
  sudo systemctl restart nginx
  ```

---

## APACHE Setup (Optional)

- [ ] Install Apache:
  ```
  sudo apt install apache2 libapache2-mod-php
  ```
- [ ] Enable rewrite:
  ```
  sudo a2enmod rewrite
  ```
- [ ] Create config `/etc/apache2/sites-available/your-app.conf`
- [ ] Sample Apache config:
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
- [ ] Enable site:
  ```
  sudo a2ensite your-app.conf
  ```
- [ ] Reload Apache:
  ```
  sudo systemctl reload apache2
  ```