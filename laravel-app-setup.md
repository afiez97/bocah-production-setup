# Laravel App Server Setup

- [ ] `sudo apt update && upgrade`
- [ ] Install: `php`, `composer`
- [ ] `composer install`
- [ ] Set `.env`:
  ```
  DB_HOST=REMOTE_IP
  DB_PORT=3306
  ```
- [ ] Set Laravel root to `/public`
- [ ] `php artisan key:generate`
- [ ] `php artisan migrate`
- [ ] Set file permissions:
  ```
  sudo chown -R www-data:www-data /path/to/project
  ```