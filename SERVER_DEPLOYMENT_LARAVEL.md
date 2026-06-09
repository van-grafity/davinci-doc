# SOP Deployment Server Laravel Production

## Ubuntu 22.04 + Nginx + PHP + SSL

Dokumen ini menjelaskan langkah lengkap melakukan deployment Laravel ke server production menggunakan **Nginx dan Let's Encrypt SSL**.

Environment server:

| Komponen   | Value                   |
| ---------- | ----------------------- |
| OS         | Ubuntu 22.04            |
| Web Server | Nginx                   |
| PHP        | PHP 8.1                 |
| Framework  | Laravel                 |
| Domain     | kencanaglobalsystem.com |
| VPS        | BiznetGio               |
| IP Server  | 202.74.74.129           |

---

# 1. Persiapan Server

Login ke server menggunakan SSH.

```bash
ssh kencanagroup@202.74.74.129
```

Update server.

```bash
sudo apt update && sudo apt upgrade -y
```

Install basic tools.

```bash
sudo apt install -y git unzip curl software-properties-common
```

Set timezone.

```bash
sudo timedatectl set-timezone Asia/Jakarta
```

---

# 2. Install Nginx

Install nginx.

```bash
sudo apt install -y nginx
```

Enable nginx.

```bash
sudo systemctl enable nginx
sudo systemctl start nginx
```

Cek status nginx.

```bash
sudo systemctl status nginx
```

---

# 3. Install PHP 8.1

Tambahkan repository PHP.

```bash
sudo add-apt-repository ppa:ondrej/php -y
sudo apt update
```

Install PHP dan extension yang dibutuhkan Laravel.

```bash
sudo apt install -y \
php8.1-fpm \
php8.1-cli \
php8.1-common \
php8.1-mysql \
php8.1-xml \
php8.1-mbstring \
php8.1-curl \
php8.1-zip \
php8.1-bcmath \
php8.1-gd \
php8.1-intl
```

Cek versi PHP.

```bash
php -v
php -m | grep gd
```

# Untuk queue & cache Laravel:

```bash
sudo apt install redis-server -y
sudo systemctl enable redis
```

---

# 4. Install Composer

Download composer.

```bash
cd ~
curl -sS https://getcomposer.org/installer | php
```

Pindahkan ke global path.

```bash
sudo mv composer.phar /usr/local/bin/composer
```

Cek composer.

```bash
composer --version
```

---

# 5. Install NodeJS

NodeJS digunakan untuk build asset Laravel (Vite).

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Cek versi.

```bash
node -v
npm -v
```

---

# 6. Setup Folder Project

Buat folder project.

```bash
sudo mkdir -p /var/www/kencana-group-system
```

Set permission.

```bash
sudo chown -R $USER:$USER /var/www/kencana-group-system
```

Masuk folder project.

```bash
cd /var/www/kencana-group-system
```

---

# 7. Upload Source Code Laravel

Gunakan git.

```bash
git clone https://repository-project.git .
```

## Option B — Upload manual

Upload via:

* WinSCP
* VSCode Remote SSH
* SFTP

Install dependency Laravel.

```bash
composer install --no-dev --optimize-autoloader
```

Copy environment.

```bash
cp .env.example .env
```

Generate key.

```bash
php artisan key:generate
```

---

# 8. Build Asset Frontend

Install dependency Node.

```bash
npm install
```

Build production asset.

```bash
npm run build
```

---

## Siapkan cron Laravel (WAJIB production)

```bash
crontab -e
```

Tambah:

```bash
* * * * * cd /var/www/kencana-group-system && php artisan schedule:run >> /dev/null 2>&1
```
---

# 9. Setup Permission Laravel

Laravel membutuhkan akses write pada folder tertentu.

```bash
sudo chown -R www-data:www-data /var/www/kencana-group-system
```

Set permission.

```bash
sudo chmod -R 775 storage bootstrap/cache
```

---

# 10. Konfigurasi Nginx

Masuk folder konfigurasi nginx.

```bash
cd /etc/nginx/sites-available
```

Buat file virtual host.

```bash
sudo nano kencana-group-system
```

Isi konfigurasi nginx.

```nginx
server {
    listen 80;
    server_name kencanaglobalsystem.com www.kencanaglobalsystem.com;

    root /var/www/kencana-group-system/public;
    index index.php index.html;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.1-fpm.sock;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

Aktifkan site.

```bash
sudo ln -s /etc/nginx/sites-available/kencana-group-system /etc/nginx/sites-enabled/
```

Test konfigurasi.

```bash
sudo nginx -t
```

Reload nginx.

```bash
sudo systemctl reload nginx
```

---

# 11. Install SSL (Let's Encrypt)

Install certbot.

```bash
sudo apt install -y certbot python3-certbot-nginx
```

Generate SSL certificate.

```bash
sudo certbot --nginx -d kencanaglobalsystem.com -d www.kencanaglobalsystem.com
```

Masukkan email admin dan setujui terms.

Certbot akan otomatis:

* membuat SSL certificate
* konfigurasi nginx
* redirect HTTP ke HTTPS

---

# 12. Lokasi SSL Certificate

Certificate disimpan pada:

```
/etc/letsencrypt/live/kencanaglobalsystem.com/
```

File penting:

```
fullchain.pem
privkey.pem
```

---

# 13. Auto Renewal SSL

Certbot membuat timer otomatis.

Cek status:

```bash
sudo systemctl status certbot.timer
```

Server akan melakukan pengecekan renewal **dua kali sehari**.

---

## Siapkan Cloudflare (optional tapi powerful)

Manfaat:

* DDoS protection
* CDN
* free SSL edge
* bot protection

---

# 14. Update Laravel Environment

Edit `.env`.

```bash
nano /var/www/kencana-group-system/.env
```

Update APP_URL.

```
APP_ENV=production
APP_DEBUG=false
APP_URL=https://kencanaglobalsystem.com
```

Clear cache Laravel.

```bash
php artisan config:clear
php artisan route:clear
```

---

# 15. Testing Website

Akses website melalui browser.

```
https://kencanaglobalsystem.com
```

Jika berhasil maka:

* domain sudah pointing
* nginx berjalan
* SSL aktif
* Laravel berjalan

---

# Restart Services

```bash
sudo systemctl restart php8.1-fpm
sudo systemctl restart nginx
```

---

# Firewall Check (Optional)

```bash
sudo ufw status
```

Jika aktif:

```bash
sudo ufw allow OpenSSH
sudo ufw allow 'Nginx Full'
sudo ufw reload
```

# 16. Debugging Server

Cek error nginx.

```bash
sudo tail -f /var/log/nginx/error.log
```

Cek log Laravel.

```
/var/www/kencana-group-storage/logs/laravel.log
```

PHP-FPM error

```bash
sudo journalctl -u php8.1-fpm -n 100 --no-pager
```

---

# 17. Struktur Server

```
/var/www/
│
├── kencana-group-system
│   ├── app
│   ├── bootstrap
│   ├── public
│   ├── resources
│   └── vendor
│
└── kencana-group-storage
    ├── logs
    └── framework
```

---

# 18. Checklist Deployment

| Item                | Status |
| ------------------- | ------ |
| VPS Running         | ✅      |
| Domain pointing     | ✅      |
| Nginx running       | ✅      |
| Laravel installed   | ✅      |
| Composer dependency | ✅      |
| Node build          | ✅      |
| Permission fixed    | ✅      |
| SSL active          | ✅      |
| HTTPS redirect      | ✅      |

---

# Kesimpulan

Server Laravel production telah berhasil dikonfigurasi dengan:

* Nginx Web Server
* PHP 8.1
* Composer
* NodeJS
* Domain pointing
* SSL HTTPS Let's Encrypt
* Auto renewal certificate

Website dapat diakses melalui:

```
https://kencanaglobalsystem.com
```

---

Kalau kamu mau, saya juga bisa bantu buat **3 hal yang akan membuat server kamu jauh lebih profesional (seperti server perusahaan besar)**:

1️⃣ **Auto deploy dari Git (CI/CD sederhana)**
2️⃣ **Laravel queue worker (Supervisor)**
3️⃣ **Auto backup database + storage setiap hari**

Ini akan membuat server kamu **level production DevOps** 🚀.
