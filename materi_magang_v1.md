## 1️⃣ Persiapan Awal & Link Download

Sebelum memulai, kamu perlu menginstal "mesin" utama untuk menjalankan Java.

### 📥 Link Download JDK (Java Development Kit)

Sangat disarankan menggunakan versi **LTS (Long Term Support)** seperti Java 17 atau 21 untuk stabilitas.

| Komponen | Link Download Resmi |
| --- | --- |
| **JDK 17/21 (Adoptium/Temurin)** | [Klik di Sini (Recommended)](https://adoptium.net/temurin/releases/) |
| **JDK 17/21 (Oracle)** | [Klik di Sini](https://www.oracle.com/java/technologies/downloads/) |
| **Visual Studio Code** | [Download VS Code](https://code.visualstudio.com/) |

### ✅ Verifikasi Instalasi

Setelah instalasi selesai, buka Terminal/CMD dan pastikan Java sudah terbaca oleh sistem:

```bash
java -version
javac -version

```

---

## 2️⃣ Konfigurasi Extension VS Code

VS Code secara bawaan adalah *text editor* ringan. Agar menjadi IDE Java yang mumpuni, kamu **wajib** menginstal paket ekstensi berikut melalui menu **Extensions (Ctrl+Shift+X)**:

1. **Extension Pack for Java** (Oleh Microsoft): Paket lengkap untuk dukungan bahasa, *debugger*, dan manajemen Maven/Gradle.
2. **Spring Boot Extension Pack** (Oleh VMware): Memberikan fitur *auto-complete* khusus Spring Boot dan dashboard untuk menjalankan aplikasi.
3. **Lombok Device** (Opsional): Sangat berguna jika kamu menggunakan library Lombok untuk menyingkat kode (Getter/Setter).

---

## 3️⃣ Membuat Project Spring Boot Baru

Ikuti langkah-langkah presisi ini untuk membuat struktur project yang benar:

### 🔹 Langkah-langkah (Via Command Palette)

1. Tekan **`Ctrl + Shift + P`** pada keyboard.
2. Ketik dan pilih: **`Spring Initializr: Create a Maven Project`**.
3. Tentukan versi Spring Boot: Pilih versi terbaru yang stabil (hindari yang berlabel *Snapshot*).
4. Pilih Bahasa: **`Java`**.
5. Input Group Id: Contoh `com.namaanda` (seperti nama paket/domain).
6. Input Artifact Id: Contoh `belajar-spring` (ini akan jadi nama folder project).
7. Packaging Type: **`Jar`**.
8. Java Version: Pilih **`17`** atau **`21`** (sesuai JDK yang kamu install).
9. **Dependencies (PENTING):** Tambahkan minimal **`Spring Web`**. (Kamu bisa menambah `Spring Data JPA` atau `Lombok` nanti).
10. Pilih folder penyimpanan, lalu klik **`Generate into this folder`**.

---

## 4️⃣ Menjalankan Aplikasi Pertama Kamu

Setelah project terbuka, VS Code akan melakukan sinkronisasi Maven (tunggu hingga bar loading di pojok kanan bawah selesai).

1. Buka file `src/main/java/com/example/demo/DemoApplication.java`.
2. Klik tombol **Run** atau **Debug** yang muncul di atas baris `public static void main`.
3. Atau gunakan **Spring Boot Dashboard** di sidebar kiri (icon daun) untuk kontrol yang lebih mudah.
4. Buka browser dan akses: `http://localhost:8080`.

> [!TIP]
> **Tips Tambahan:** Jika perintah `mvn` tidak ditemukan di terminal, jangan khawatir. VS Code sudah menyertakan `mvnw` (Maven Wrapper) di dalam folder project kamu. Kamu bisa menjalankan perintah dengan `./mvnw spring-boot:run`.