Berikut **praktik terbaik (best practice)** untuk pemula DaVinci Resolve, terutama soal **shortcut fundamental**, **workflow dasar**, dan **menambahkan node untuk color grading**.

---

# ✅ **1. FUNDAMENTAL SHORTCUTS DAVINCI RESOLVE (PEMULA WAJIB TAHU)**

## 🎬 **Timeline & Playback**

| Fungsi             | Shortcut  |
| ------------------ | --------- |
| Play / Pause       | **Space** |
| Play Reverse       | **J**     |
| Play Forward       | **L**     |
| Stop               | **K**     |
| One frame backward | **←**     |
| One frame forward  | **→**     |
| Go to beginning    | **Home**  |
| Go to end          | **End**   |

---

## ✂️ **Editing**

| Fungsi         | Shortcut                 |
| -------------- | ------------------------ |
| Cut (blade)    | **B**                    |
| Select mode    | **A**                    |
| Trim edit mode | **T**                    |
| Split clip     | **Ctrl + \ (Backslash)** |
| Ripple delete  | **Shift + Backspace**    |
| Undo           | **Ctrl + Z**             |
| Redo           | **Ctrl + Shift + Z**     |

---

## 🖼️ **View & UI**

| Fungsi               | Shortcut      |
| -------------------- | ------------- |
| Full screen viewer   | **Ctrl + F**  |
| Zoom timeline in     | **Ctrl + +**  |
| Zoom out             | **Ctrl + -**  |
| Switch to Cut page   | **Shift + 2** |
| Switch to Edit page  | **Shift + 4** |
| Switch to Color page | **Shift + 6** |

---

# 🎨 **2. BEST PRACTICES COLOR GRADING UNTUK PEMULA**

Saat membuka **Color Page**, lakukan grading dengan workflow node berikut:

---

# 🌿 **3. CARA MENAMBAHKAN NODE (PEMULA WAJIB KUASAI)**

## ➕ **Tambah Node Baru**

| Node               | Shortcut            |
| ------------------ | ------------------- |
| Add Serial Node    | **Alt + S**         |
| Add Parallel Node  | **Alt + P**         |
| Add Layer Mixer    | **Alt + L**         |
| Add Corrector Node | **Alt + Shift + S** |
| Delete Node        | **Backspace**       |

---

# 🎨 **4. WORKFLOW NODE GRADING STANDAR (PEMULA)**

Gunakan struktur seperti ini:

### **1. Node 1 – Primary Balance (warna dasar & exposure)**

* Lift / Gamma / Gain
* White balance
* Fix underexpose / overexpose
  Shortcut cepat:
  ➡️ Alt + S untuk Node pertama

### **2. Node 2 – Contrast & Pivot**

* Tambah kontras
* Atur pivot supaya midtone tidak hancur

### **3. Node 3 – Skin Tone / Color Boost (jika ada wajah)**

* Gunakan qualifier (HSL)
* Haluskan dengan blur radius kecil

### **4. Node 4 – Look / Creative (Film look)**

* Tambah Film Curve
* Saturation
* Highlight rolloff

### **5. Node 5 – Sharpen / Noise Reduction**

(Selalu taruh paling belakang)

---

# 🎬 **Node Tree Contoh (SIMPLE)**

```
Node 1 = Primary Balance
Node 2 = Contrast
Node 3 = Skin Tone
Node 4 = Creative Look
Node 5 = Sharpen
```

Untuk pemula, ini sudah profesional banget.

---

# 🎞️ **5. BEST PRACTICES DALAM COLOR GRADING**

### ✔ Gunakan scopes

Tekan **Ctrl + Shift + W** untuk menampilkan scopes.
Gunakan waveform untuk exposure, vectorscope untuk warna kulit.

### ✔ Jangan grading di node yang sama

Pisahkan agar hasil rapi dan mudah diubah.

### ✔ Kerjakan dari “global → detail”

1. Exposure
2. White balance
3. Kontras
4. Warna kulit
5. Look

### ✔ Grading **setelah** editing selesai

Jangan grading dulu, nanti berubah layoutnya.

---

# 🎥 Jika Ingin Saya Buatkan **Preset Node Tree Siap Pakai**, bilang saja.

Atau ingin **gaya color grading drama komedi** → bisa saya buatkan template node + settingnya.

Baik, kita mulai **langkah demi langkah BEST PRACTICE color grading** seperti yang kamu minta, dari awal sampai menghasilkan warna yang bersih, natural, dan profesional.
Aku sesuaikan dengan footage kamu yang tampak **flat dan agak overexposed**.

---

# 🎯 **GOAL:**

Membuat grading rapi, terstruktur, dan mudah diperbaiki dengan **Node Workflow 5 Langkah**.

Node yang kita akan pakai:

```
Node 1 – Primary Correction (Exposure + WB)
Node 2 – Contrast & Pivot
Node 3 – Skin Tone
Node 4 – Creative Look
Node 5 – Sharpen / NR
```

---

# 🟦 **LANGKAH 1 – Buat Node Structure (Wajib)**

Tekan shortcut:

### ➕ Tambah node:

* **Alt + S** = Add Serial Node
  Klik sampai kamu punya 5 node seperti ini:

```
[01] → [02] → [03] → [04] → [05]
```

---

# 🟧 **LANGKAH 2 – Node 1: PRIMARY BALANCE**

(Exposure + White Balance)

### ✔ Lakukan ini dulu:

1. Buka **Waveform** (Ctrl + Shift + W)
2. Naikkan / turunkan:

   * **Lift** → gelapkan bagian hitam
   * **Gamma** → perbaiki midtone (terpenting!)
   * **Gain** → highlight
3. Sesuaikan **Temperature** dan **Tint** untuk WB

   * Terlalu kuning → naikkan Tint sedikit
   * Terlalu hijau → tambahkan Magenta
4. Pertahankan highlight di **700–800** (jangan clipping)

👉 Footage kamu terlihat agak **washout dan overexposed** → turunkan **Gain** dan **Gamma** sedikit.

---

# 🟩 **LANGKAH 3 – Node 2: KONTRAS & PIVOT**

Jangan tambah contrast di Node 1.
Tambahkan di Node 2:

* Contrast: **1.10 – 1.20**
* Pivot: sesuaikan untuk menarik midtone
  (Kalau gambar terlalu pecah, turunkan pivot)

⚠ Jangan pakai contrast terlalu tinggi; footage akan terlihat murah.

---

# 🟫 **LANGKAH 4 – Node 3: SKIN TONE (Opsional tapi Penting)**

Gunakan ini untuk memperbaiki warna kulit:

1. Klik ikon **Qualifier (eyedropper)**
2. Select kulit di pipi
3. Buka **Vectorscope (Skin Tone line)**
   → pastikan warna kulit berada di garis diagonal
4. Softening:

   * Blur Radius: **5–10**
   * Clean black & clean white sedikit

**Tujuan:** warna kulit natural, tidak merah atau hijau.

---

# 🟪 **LANGKAH 5 – Node 4: CREATIVE LOOK**

Di sini kamu bisa:

### ✔ Tambah Saturation

* Sat: **45–55**

### ✔ Tambah Look halus

* Curve → turunkan highlight sedikit
* Tambahkan Color Boost **5–15**
* Sedikit highlight roll-off (kurva S tipis)

🎨 Kalau mau look drama komedi (cocok untuk footage kamu):

* Turunkan highlight
* Naikkan shadow sedikit
* Saturation ringan
* Warm highlights (temperature + sedikit magenta)

Aku bisa buat preset khusus kalau mau.

---

# 🟦 **LANGKAH 6 – Node 5: SHARPEN / NR**

### Jika kamera noise (ISO tinggi)

* Enable **Noise Reduction** (kalau Studio)

### Jika mau sharpness natural

* Sharpen: **0.40 – 0.60**

Jangan pakai lebih dari itu, nanti gambar terasa "digital" dan murah.

---

# 📌 RINGKASAN WORKFLOW

Ini urutan yang benar dan profesional:

```
Node 1 = Exposure + WB
Node 2 = Contrast
Node 3 = Skin Tone
Node 4 = Creative Look (S-Curve / Saturation / Warm tone)
Node 5 = Sharpen
```

---

# 🔥 Kalau mau, saya bisa kasih:

### ✔ Template node tree otomatis

### ✔ Color grading preset “Drama Komedi”

### ✔ Setting exposure spesifik berdasarkan foto kamu

### ✔ Cara membuat LUT pribadi dari grading kamu

Cukup bilang: **"buatkan preset drama komedi"** atau **“lanjut node 1”**, nanti aku buatkan step-by-step dengan nilai yang pas untuk footage kamu.

**Ringkas spesifikasi:**

* CPU: **Intel i7-1165G7 (11th Gen)** ✔️
* RAM: **16 GB** ✔️ (cukup ideal)
* GPU: **Intel Iris Xe (shared, ±2 GB)** ⚠️ (ini bottleneck utama)
* Storage: **SSD 477 GB** ✔️
* OS: **Windows 11 Pro 64-bit**

Laptop ini **cukup untuk belajar fundamental DaVinci Resolve**, **asal disetting dengan benar**. Jika default → memang bisa **lag / freeze**.

---

## 1️⃣ Saran Saat Installer (PILIH YANG AMAN & RINGAN)

Di installer DaVinci Resolve yang Anda tampilkan:

### ✅ Centang (WAJIB / AMAN)

✔ Visual C++ 2013 x64
✔ Visual C++ 2013 x86
✔ **DaVinci Resolve (utama)**
✔ Fairlight Audio Accelerator Utility

### ⚠️ Boleh Dicentang (opsional)

✔ Blackmagic RAW Player (tidak berat, aman)

### ❌ Tidak perlu khawatir

* **DaVinci Control Panels** → tidak berat (driver saja)
* **Pre-Installation Cleanup** → biarkan aktif (bagus)

👉 **Kesimpulan installer:**
✔ **Biarkan semua default seperti gambar Anda** → **AMAN**
❌ Tidak ada yang bikin berat di tahap installer

---

## 2️⃣ SETTING PENTING AGAR TIDAK LAG (INI KUNCI 🔑)

Setelah install, **JANGAN LANGSUNG EDIT**
Masuk dulu ke **Preferences**

### 🔧 A. Preferences → Memory & GPU

**GPU Configuration**

* ❌ Auto
* ✅ **Manual**
* GPU processing mode: **OpenCL**
* GPU Selection: **Intel Iris Xe**

📌 Ini penting karena Resolve sering salah pilih GPU di laptop.

---

### 🔧 B. Preferences → Memory

* **Limit Resolve memory usage**: `10–12 GB`
* **Fusion memory cache**: `2 GB`

👉 Jangan pakai full 16 GB → biar Windows tidak kehabisan RAM.

---

## 3️⃣ PROJECT SETTING (WAJIB DIUBAH)

Masuk **Project Settings (⚙️)**

### 🎬 Master Settings

* Timeline Resolution:
  👉 **1280×720 (HD)** ❗ (BUKAN 1080p dulu)
* Timeline Frame Rate: **24 fps**
* Playback Frame Rate: **24 fps**

> Belajar fundamental **TIDAK BUTUH 1080p / 4K**

---

### ⚡ Optimized Media

* Optimized Media Format: **DNxHR LB**
* Optimized Media Resolution: **Quarter**

---

## 4️⃣ SETTING PLAYBACK AGAR HALUS

Di menu atas:

✔ **Playback → Use Optimized Media**
✔ **Playback → Render Cache → Smart**
✔ **Playback → Proxy Mode → Half Resolution**
(atau Quarter kalau masih berat)

---

## 5️⃣ JENIS LATIHAN YANG AMAN UNTUK SPEK ANDA

### ✅ Fokus belajar:

* Cut Page
* Edit Page (cut, trim, ripple)
* Basic Color (Lift, Gamma, Gain)
* Simple transition
* Simple title (Text, bukan Fusion Title)

### ❌ Hindari dulu:

* Fusion heavy effect
* Noise Reduction
* Motion Blur
* Film Grain
* AI tools (Magic Mask, dll)

---

## 6️⃣ FORMAT VIDEO LATIHAN (SANGAT PENTING)

❌ Jangan pakai:

* H.265 / HEVC 10-bit
* 4K footage

✅ Pakai:

* **H.264 1080p**
* Atau 720p
* Frame rate 24 / 30 fps

---

## 7️⃣ EXPORT AGAR TIDAK FREEZE

Deliver Page:

* Format: **MP4**
* Codec: **H.264**
* Resolution: **Sesuai timeline (720p)**
* Quality: **Restrict to 8.000–10.000 Kb/s**
* ❌ Disable “Use GPU” jika export error

---

## 8️⃣ REALISTIS & JUJUR

Dengan laptop Anda:

* ✅ **Belajar fundamental DaVinci → SANGAT BISA**
* ❌ **Color grading berat & cinematic complex → BELUM IDEAL**
* ❌ **Fusion kompleks → AKAN LAG**

Tapi:

> 🎯 **Editor profesional pun belajar Resolve pertama kali di spek seperti ini**

---

## 9️⃣ MAU SAYA BANTU LEBIH LANJUT?

Kalau Anda mau, saya bisa:

1. Buatkan **preset Project Setting khusus laptop Anda**
2. Rekomendasi **workflow belajar Resolve 14 hari**
3. Setting khusus **Resolve 18 vs 19/20**
4. Rekomendasi **versi Resolve paling stabil untuk Intel Iris Xe**

Tinggal bilang mau fokus ke yang mana 👍
