Berikut **setup standar production** untuk:

* **1 VPS**
* **2 Domain**
* **2 Project Laravel berbeda**

Domain:

* `kencanaglobalsystem.com`
* `ivansuhendra.net`

Stack yang umum dipakai perusahaan:

* **Ubuntu 22.04**
* **Nginx**
* **PHP-FPM**
* **MySQL**
* **Let's Encrypt SSL**
* **Folder terpisah per project**

---

# 1️⃣ Struktur Folder Production (Best Practice)

Biasanya perusahaan memakai struktur seperti ini:

```
/var/www/
│
├── kencanaglobalsystem
│   ├── current
│   ├── releases
│   └── shared
│
└── ivansuhendra
    ├── current
    ├── releases
    └── shared
```

Tetapi untuk simple production bisa:

```
/var/www/
│
├── kencanaglobalsystem
│   └── project
│       └── public
│
└── ivansuhendra
    └── project
        └── public
```

Contoh:

```
/var/www/kencanaglobalsystem/project
/var/www/ivansuhendra/project
```

---

# 2️⃣ Clone Project Laravel

Masuk VPS:

```bash
cd /var/www
```

Clone project pertama

```bash
git clone https://repo-project1.git kencanaglobalsystem
```

Clone project kedua

```bash
git clone https://repo-project2.git ivansuhendra
```

---

# 3️⃣ Permission Laravel

Ini penting untuk production.

```
sudo chown -R www-data:www-data /var/www
sudo chmod -R 755 /var/www
```

Laravel storage:

```
sudo chmod -R 775 storage
sudo chmod -R 775 bootstrap/cache
```

---

# 4️⃣ Nginx Configuration

Lokasi config:

```
/etc/nginx/sites-available/
```

---

# Config Domain 1

```
sudo nano /etc/nginx/sites-available/kencanaglobalsystem
```

```
server {
    listen 80;
    server_name kencanaglobalsystem.com www.kencanaglobalsystem.com;

    root /var/www/kencanaglobalsystem/public;
    index index.php index.html;

    add_header X-Frame-Options "SAMEORIGIN";
    add_header X-Content-Type-Options "nosniff";

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /favicon.ico { access_log off; log_not_found off; }
    location = /robots.txt  { access_log off; log_not_found off; }

    error_page 404 /index.php;

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
        fastcgi_param SCRIPT_FILENAME $realpath_root$fastcgi_script_name;
        include fastcgi_params;
    }

    location ~ /\.(?!well-known).* {
        deny all;
    }
}
```

---

# Config Domain 2

```
sudo nano /etc/nginx/sites-available/ivansuhendra
```

```
server {
    listen 80;
    server_name ivansuhendra.net www.ivansuhendra.net;

    root /var/www/ivansuhendra/public;
    index index.php index.html;

    charset utf-8;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location ~ \.php$ {
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
        include fastcgi_params;
    }
}
```

---

# 5️⃣ Aktifkan Website

```
sudo ln -s /etc/nginx/sites-available/kencanaglobalsystem /etc/nginx/sites-enabled/
sudo ln -s /etc/nginx/sites-available/ivansuhendra /etc/nginx/sites-enabled/
```

Test nginx

```
sudo nginx -t
```

Reload

```
sudo systemctl reload nginx
```

---

# 6️⃣ DNS Setup

Di **NeoDNS** buat record berikut.

## Domain 1

```
A     @       IP_VPS
CNAME www     kencanaglobalsystem.com
```

---

## Domain 2

```
A     @       IP_VPS
CNAME www     ivansuhendra.net
```

Contoh:

```
A     @       103.xxx.xxx.xxx
```

---

# 7️⃣ SSL (WAJIB PRODUCTION)

Install certbot:

```
sudo apt install certbot python3-certbot-nginx
```

Install SSL domain 1:

```
sudo certbot --nginx -d kencanaglobalsystem.com -d www.kencanaglobalsystem.com
```

Domain 2:

```
sudo certbot --nginx -d ivansuhendra.net -d www.ivansuhendra.net
```

SSL akan otomatis renew.

---

# 8️⃣ Laravel Optimization

Setiap project jalankan:

```
php artisan config:cache
php artisan route:cache
php artisan view:cache
```

---

# 9️⃣ Setup Queue & Scheduler (Production)

Scheduler:

```
crontab -e
```

Tambahkan:

```
* * * * * php /var/www/kencanaglobalsystem/artisan schedule:run >> /dev/null 2>&1
```

---

# 🔟 Struktur Final Server

Server production biasanya seperti ini:

```
/var/www
│
├── kencanaglobalsystem
│   ├── app
│   ├── bootstrap
│   ├── public
│   ├── storage
│   └── vendor
│
└── ivansuhendra
    ├── app
    ├── bootstrap
    ├── public
    ├── storage
    └── vendor
```

---

# 💡 Bonus Best Practice Perusahaan

Biasanya production server juga pakai:

### Firewall

```
ufw allow OpenSSH
ufw allow 'Nginx Full'
ufw enable
```

---

### Swap Memory

```
sudo fallocate -l 2G /swapfile
```

---

### Log monitoring

```
/var/log/nginx/access.log
/var/log/nginx/error.log
```

---

# 🚀 Arsitektur Final

```
Internet
     │
     │
DNS (NeoDNS Biznet)
     │
     │
VPS (Nginx)
     │
 ┌───────────────┐
 │               │
 │               │
Laravel 1       Laravel 2
kencanaglobalsystem
ivansuhendra
```

---

💡 Jika kamu mau, saya bisa buatkan **setup yang lebih advance seperti yang dipakai perusahaan besar**:

* **Auto deploy dari Git (CI/CD)**
* **Zero downtime deploy**
* **Supervisor untuk queue**
* **Redis cache**
* **Laravel Horizon**
* **Server bisa handle 10k+ traffic**

Kalau mau saya juga bisa buatkan **panduan lengkap step by step dari VPS kosong sampai live production**. Ini biasanya setup yang dipakai **startup dan SaaS production server**.
