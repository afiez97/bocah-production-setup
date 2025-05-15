# Web Server Setup (NGINX & APACHE)

## NGINX Setup

- [ ] Install Nginx:
  ```
  sudo apt install nginx
  ```
- [ ] Create config `/etc/nginx/sites-available/your-app`
- [ ] Sample Nginx config:
  ```nginx
  # Konfigurasi Nginx untuk Laravel Application (Produksi)

server {
    listen 80;  # Port HTTP standard
    server_name your_domain.com;  # Tukar kepada domain sebenar atau IP pelayan

    # Folder root aplikasi Laravel - pastikan ia menunjuk ke 'public'
    root /var/www/your-app/public;
    index index.php index.html;

    # Semua request akan dilayan oleh Laravel melalui index.php
    location / {
        try_files $uri $uri/ /index.php?$query_string;  # Routing Laravel
    }

    # Konfigurasi untuk fail PHP
    location ~ \.php$ {
        include snippets/fastcgi-php.conf;  # Guna konfigurasi FastCGI standard

        # Tukar ikut versi PHP server, contoh:
        # PHP 7.4  : /var/run/php/php7.4-fpm.sock
        # PHP 8.1  : /var/run/php/php8.1-fpm.sock
        fastcgi_pass unix:/var/run/php/php8.1-fpm.sock;
        
        # Jika guna TCP (bukan socket):
        # fastcgi_pass 127.0.0.1:9000;
    }

    # Sekat akses kepada fail tersembunyi seperti .htaccess
    location ~ /\.ht {
        deny all;  # Sekuriti tambahan
    }

    # (Optional) Sekat akses ke folder tersembunyi
    location ~ /\. {
        deny all;
        access_log off;
        log_not_found off;
    }

    # (Optional) Tambah log access dan error
    # access_log /var/log/nginx/your-app_access.log;
    # error_log /var/log/nginx/your-app_error.log;
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
