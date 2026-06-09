# Dokumentasi Migrasi dari XAMPP ke MySQL Server Resmi + MySQL Workbench

## 1. Kondisi Awal

- Sistem Operasi: Windows
- PHP CLI sudah terpasang global:

```text
PHP 8.1.25 (cli)
```

- Sebelumnya menggunakan **XAMPP**
- Sering mengalami error:
  - `MySQL shutdown unexpectedly`
  - Corrupt `ibdata1`
  - Perlu recovery manual dari folder `backup`

Tujuan:
> Mengganti XAMPP MySQL dengan **MySQL Server resmi + MySQL Workbench** agar lebih stabil dan production-like.

---

## 2. Masalah Utama yang Ditemui

### ❌ MySQL tidak bisa start
Pesan error:
```
The specified port already in use
```

### Penyebab:
- MySQL lama (XAMPP / MariaDB) masih berjalan
- Port default MySQL **3306** masih dipakai proses lain

---

## 3. Investigasi Port MySQL

Cek port 3306:

```bat
netstat -ano | findstr :3306
```

Output:
```text
TCP    0.0.0.0:3306     LISTENING   17572
TCP    [::]:3306        LISTENING   17572
```

Artinya:
- Ada proses aktif dengan **PID 17572** menggunakan port 3306

---

## 4. Menghentikan Proses Lama

```bat
taskkill /PID 17572 /F
```

Hasil:
```text
SUCCESS: The process with PID 17572 has been terminated.
```

Setelah ini:
- Port 3306 bebas
- MySQL Installer bisa dilanjutkan

---

## 5. Instalasi MySQL Server Resmi

### Installer
- Menggunakan **MySQL Installer (Community)**
- Versi: **MySQL Server 8.0.44 (x64)**

### Konfigurasi Penting
- Config Type: **Development Computer**
- Port: **3306**
- Authentication:
  - `mysql_native_password` (legacy)
- Service:
  - Service name: `MySQL80`
  - Start at system startup

Hasil:
> MySQL Server berhasil ter-install dan service berjalan

---

## 6. Verifikasi MySQL Server

```bat
mysql -u root -p
```

Hasil:
```text
Welcome to the MariaDB monitor.
```

Catatan:
- Ini menandakan client `mysql` lama (MariaDB) masih ada di PATH
- Namun server yang berjalan adalah **MySQL Server resmi**
- Disarankan ke depan memastikan PATH mengarah ke MySQL resmi

---

## 7. Instalasi MySQL Workbench

### Kondisi Awal
- MySQL Installer hanya menampilkan:
  - MySQL Server
- MySQL Workbench belum terpasang

### Langkah
1. Buka **MySQL Installer**
2. Klik **Add**
3. Pilih:
   - Applications → **MySQL Workbench 8.0**
4. Pindahkan ke kolom **Products to be installed** (panah kanan)
5. Next → Execute → Finish

---

## 8. Verifikasi MySQL Workbench

- Buka **MySQL Workbench 8.0**
- Muncul koneksi otomatis:
  - `Local instance MySQL80`
- Berhasil login menggunakan user `root`

Workbench siap digunakan untuk:
- Create schema (database)
- Create table
- Query
- Backup & restore

---

## 9. Arsitektur Akhir (Stabil)

```text
PHP 8.1 (CLI)
MySQL Server 8.0 (Official)
MySQL Workbench (GUI)
Laravel (php artisan serve)
```

Tidak lagi menggunakan:
- XAMPP MySQL
- phpMyAdmin

---

## 10. Best Practice yang Diputuskan

- MySQL Server berdiri sendiri (tidak satu paket XAMPP)
- Gunakan MySQL Workbench sebagai GUI
- Gunakan user khusus aplikasi (bukan root) untuk Laravel
- Port tetap **3306** (default & standar)
- Backup rutin via Workbench

---

## 11. Pelajaran Penting

- XAMPP MySQL rawan corrupt untuk pemakaian jangka panjang
- Port conflict adalah penyebab utama kegagalan install
- MySQL Workbench **bukan pengganti server**, hanya GUI
- Setup ini lebih mendekati lingkungan production

---

## 12. Status Akhir

✅ PHP CLI aktif
✅ MySQL Server resmi aktif
✅ MySQL Workbench ter-install
✅ Port conflict terselesaikan
✅ Siap dipakai untuk Laravel & development profesional

---

**Dokumentasi ini dibuat sebagai catatan migrasi dan troubleshooting untuk referensi ke depan.**

