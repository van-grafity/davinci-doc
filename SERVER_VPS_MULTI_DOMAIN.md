Untuk **pointing domain ke VPS Laravel di BiznetGio menggunakan NeoDNS**, kamu cukup **mengganti nameserver domain** menjadi yang diberikan oleh BiznetGio.

Pada gambar kamu saat ini nameserver masih:

* `NS1.SWITCHERCLOUD.COM`
* `NS2.SWITCHERCLOUD.COM`

Padahal BiznetGio meminta:

* `satu.neodns.id`
* `dua.neodns.id`

---

# 1️⃣ Ubah Nameserver Domain

Di halaman **Manage Nameservers** ubah menjadi:

```
Nameserver 1: satu.neodns.id
Nameserver 2: dua.neodns.id
Nameserver 3: (kosongkan)
Nameserver 4: (kosongkan)
Nameserver 5: (kosongkan)
```

Kemudian klik **Save Changes**.

⚠️ **Tidak perlu isi bagian "Register Nameservers".**
Itu hanya untuk membuat **custom nameserver seperti `ns1.domain.com`**, jadi tidak diperlukan di kasus ini.

---

# 2️⃣ Setelah Nameserver Diganti

Tunggu propagasi DNS:

⏱ Biasanya:

* 5 menit – 1 jam (cepat)
* maksimal 24 jam

---

# 3️⃣ Setting DNS di NeoDNS BiznetGio

Setelah nameserver aktif, masuk ke **NeoDNS panel BiznetGio** lalu buat record:

### A Record

```
Type: A
Name: @
IP: IP_VPS_KAMU
```

Contoh:

```
A    @      103.xxx.xxx.xxx
```

### Untuk www

```
Type: A
Name: www
IP: IP_VPS_KAMU
```

---

# 4️⃣ Konfigurasi VPS Laravel

Di VPS:

```
/var/www/project-laravel
```

Edit nginx config:

```
sudo nano /etc/nginx/sites-available/domain.conf
```

Contoh:

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
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
    }
}
```

Aktifkan:

```
sudo ln -s /etc/nginx/sites-available/domain.conf /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

# 5️⃣ Install SSL (Recommended)

Setelah domain sudah resolve:

```
sudo apt install certbot python3-certbot-nginx
sudo certbot --nginx -d kencanaglobalsystem.com -d www.kencanaglobalsystem.com
```

---

# 🔎 Cara Cek DNS

Cek apakah nameserver sudah berubah:

```
nslookup kencanaglobalsystem.com
```

atau

[https://dnschecker.org](https://dnschecker.org)

---

✅ **Kesimpulan**

Yang perlu kamu lakukan di halaman yang kamu screenshot:

Ganti menjadi

```
satu.neodns.id
dua.neodns.id
```

dan **jangan isi Register Nameservers**.

---

💡 Kalau kamu mau, saya juga bisa bantu jelaskan **arsitektur yang benar untuk VPS Laravel kamu**:

* 1 VPS
* 2 domain

  * `kencanaglobalsystem.com`
  * `ivansuhendra.net`
* masing-masing project Laravel berbeda

Sekalian saya buatkan **setup nginx + DNS + folder structure yang rapi seperti production server**. Ini biasanya yang dipakai di perusahaan.
