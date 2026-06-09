Berikut **draft dokumentasi** untuk penggunaan *Office Deployment Tool* (ODT) dengan perintah:

```
cd C:\msoffice
setup.exe /download Configuration.xml
setup.exe /configure Configuration.xml
```

yang sesuai praktik resmi Microsoft dan mudah dipakai sebagai panduan dokumentasi teknis.

---

## 📄 **Draft Dokumentasi: Office Deployment Tool (ODT)**

### 1️⃣ Tujuan

Dokumentasi ini menjelaskan cara menggunakan **Office Deployment Tool (ODT)** untuk:
✔ mendownload installer Microsoft Office secara tersentral,
✔ menginstal Office sesuai konfigurasi yang ditentukan di file XML.

ODT sangat berguna untuk deployment di banyak PC, instalasi offline, atau instalasi ter-custom sesuai kebutuhan organisasi. ([Microsoft Learn][1])

---

### 2️⃣ Komponen dan Persiapan

#### 🧰 Yang dibutuhkan

* **Office Deployment Tool (ODT)** – unduh dari Microsoft Download Center. ([Microsoft Learn][1])
  Setelah di-extract, akan ada:

  ```
  setup.exe
  sample Configuration.xml
  ```
* **File konfigurasi XML** (misal: Configuration.xml) – berisi pengaturan instalasi. ([Microsoft Learn][1])

#### 📂 Struktur folder contoh

```
C:\msoffice
│── setup.exe
│── Configuration.xml
```

---

### 3️⃣ Membuat File Konfigurasi (Configuration.xml)

File ini menentukan:
✔ produk Office yang diinstal
✔ bahasa
✔ arsitektur (32/64 bit)
✔ channel update
✔ aplikasi apa saja yang di-include atau exclude

Contoh minimal:

```xml
<Configuration>
  <Add OfficeClientEdition="64" Channel="Current">
    <Product ID="O365ProPlusRetail">
      <Language ID="en-us" />
    </Product>
  </Add>
  <Display Level="None" AcceptEULA="TRUE" />
</Configuration>
```

📌 Anda bisa mengedit manual atau memakai **Office Customization Tool** untuk generate XML lebih mudah. ([Microsoft Learn][1])

---

### 4️⃣ Download File Instalasi Office

Perintah berikut **mengunduh installer Office** sesuai yang ditentukan di XML:

```
cd C:\msoffice
setup.exe /download Configuration.xml
```

✔ ODT akan mengambil file dari Microsoft CDN
✔ Proses ini akan menghasilkan folder `Office` berisi source installer untuk deployment lanjutan ([Microsoft Learn][1])

---

### 5️⃣ Menginstal Office

Setelah selesai mendownload, jalankan:

```
setup.exe /configure Configuration.xml
```

Ini akan:
✔ memulai instalasi Office
✔ mengaplikasikan setting dari XML
✔ menginstal Office ke mesin yang sedang dijalankan ([Microsoft Learn][1])

---

### 6️⃣ Opsional: Mode Lain

ODT juga mendukung:

* **/customize** – hanya apply preferensi aplikasi tanpa reinstall
* **/packager** – bikin paket App-V dari source Office ([Microsoft Learn][1])

---

### 7️⃣ Catatan Penting

* Pastikan **Configuration.xml valid** dan sesuai format yang didukung Microsoft. ([Microsoft Learn][2])
* Anda tetap memerlukan **lisensi Office resmi** untuk aktivasi setelah instalasi.
* Jika ingin offline installation asli, file hasil download dapat dipakai di banyak mesin. ([Microsoft Learn][1])

---

### 📌 Referensi Video (Tutorial Visual)

Berikut video tutorial *cara menggunakan ODT* sebagai referensi visual:

[How To Use The Office Deployment Tool (YouTube tutorial)](https://www.youtube.com/watch?v=0oC-QtTI1sc&utm_source=chatgpt.com)

---

Kalau perlu, aku bisa bantu buatkan **contoh file Configuration.xml** yang sesuai kebutuhan Anda (misalnya Microsoft 365, Office 2021/2024, pilih aplikasi tertentu, patokan bahasa, atau instalasi offline). Cukup sebutkan versi Office dan pengaturan yang Anda mau!

[1]: https://learn.microsoft.com/en-us/microsoft-365-apps/deploy/overview-office-deployment-tool?utm_source=chatgpt.com "Overview of the Office Deployment Tool - Microsoft 365 Apps | Microsoft Learn"
[2]: https://learn.microsoft.com/en-us/microsoft-365-apps/deploy/office-deployment-tool-configuration-options?utm_source=chatgpt.com "Configuration options for the Office Deployment Tool - Microsoft 365 Apps | Microsoft Learn"

```
https://www.youtube.com/watch?v=JpOAeTlNjWw

office deployment tool

custom configuration tool

cd C:\msoffice
setup.exe /download Configuration.xml
setup.exe /configure Configuration.xml
```