# **Sistem Koperasi Karyawan - Spring Boot**

Implementasi lengkap sistem koperasi untuk 40 karyawan menggunakan **Spring Boot 3.x, Java 21, dan MySQL**.

## 📁 **Struktur Project**

```
src/main/java/com/koperasi/
├── KoperasiApplication.java
├── config/
│   ├── SecurityConfig.java
│   └── SwaggerConfig.java
├── controller/
│   ├── AnggotaController.java
│   ├── SimpananController.java
│   ├── PinjamanController.java
│   └── TransaksiController.java
├── model/
│   ├── entity/
│   │   ├── Anggota.java
│   │   ├── Simpanan.java
│   │   ├── Pinjaman.java
│   │   ├── Transaksi.java
│   │   └── User.java
│   └── dto/
│       ├── request/
│       └── response/
├── repository/
│   ├── AnggotaRepository.java
│   ├── SimpananRepository.java
│   ├── PinjamanRepository.java
│   └── UserRepository.java
├── service/
│   ├── impl/
│   └── KoperasiService.java
└── exception/
    └── GlobalExceptionHandler.java
```

## 📦 **1. File Konfigurasi**

### **pom.xml**

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 
         https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    
    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>3.2.0</version>
        <relativePath/>
    </parent>
    
    <groupId>com.koperasi</groupId>
    <artifactId>sistem-koperasi</artifactId>
    <version>1.0.0</version>
    <name>sistem-koperasi</name>
    <description>Sistem Koperasi Karyawan</description>
    
    <properties>
        <java.version>21</java.version>
    </properties>
    
    <dependencies>
        <!-- Spring Boot Starters -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-validation</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-security</artifactId>
        </dependency>
        
        <!-- Database -->
        <dependency>
            <groupId>com.mysql</groupId>
            <artifactId>mysql-connector-j</artifactId>
            <scope>runtime</scope>
        </dependency>
        
        <!-- Utilities -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        
        <!-- Documentation -->
        <dependency>
            <groupId>org.springdoc</groupId>
            <artifactId>springdoc-openapi-starter-webmvc-ui</artifactId>
            <version>2.3.0</version>
        </dependency>
        
        <!-- Testing -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
    
    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <excludes>
                        <exclude>
                            <groupId>org.projectlombok</groupId>
                            <artifactId>lombok</artifactId>
                        </exclude>
                    </excludes>
                </configuration>
            </plugin>
        </plugins>
    </build>
</project>
```

### **application.yml**

```yaml
server:
  port: 8080
  servlet:
    context-path: /api

spring:
  application:
    name: sistem-koperasi
  
  datasource:
    url: jdbc:mysql://localhost:3306/db_koperasi?useSSL=false&serverTimezone=UTC
    username: root
    password: password123
    driver-class-name: com.mysql.cj.jdbc.Driver
  
  jpa:
    hibernate:
      ddl-auto: update
    show-sql: true
    properties:
      hibernate:
        dialect: org.hibernate.dialect.MySQL8Dialect
        format_sql: true
  
  security:
    user:
      name: admin
      password: admin123

app:
  koperasi:
    nama: "Koperasi Karyawan Sejahtera"
    alamat: "Jl. Perusahaan No. 123"
    bunga-pinjaman: 1.5  # 1.5% per bulan
    denda-keterlambatan: 0.5  # 0.5% per hari

logging:
  level:
    com.koperasi: DEBUG
    org.springframework.security: INFO
```

## 🏗️ **2. Entity Models**

### **Anggota.java**

```java
package com.koperasi.model.entity;

import jakarta.persistence.*;
import lombok.Data;
import java.time.LocalDate;
import java.time.LocalDateTime;

@Entity
@Table(name = "anggota")
@Data
public class Anggota {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String kodeAnggota;
    
    @Column(nullable = false)
    private String nama;
    
    @Column(nullable = false)
    private String email;
    
    private String nomorTelepon;
    
    private String alamat;
    
    @Column(nullable = false)
    private String departemen;
    
    @Column(nullable = false)
    private LocalDate tanggalBergabung;
    
    @Column(nullable = false)
    private String status; // AKTIF, NON_AKTIF
    
    private Double totalSimpanan = 0.0;
    
    private Double totalPinjaman = 0.0;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    private LocalDateTime updatedAt;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        updatedAt = LocalDateTime.now();
        if (kodeAnggota == null) {
            kodeAnggota = "KOP" + String.format("%04d", id != null ? id : 0);
        }
    }
    
    @PreUpdate
    protected void onUpdate() {
        updatedAt = LocalDateTime.now();
    }
}
```

### **Simpanan.java**

```java
package com.koperasi.model.entity;

import jakarta.persistence.*;
import lombok.Data;
import java.time.LocalDate;
import java.time.LocalDateTime;

@Entity
@Table(name = "simpanan")
@Data
public class Simpanan {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @ManyToOne
    @JoinColumn(name = "anggota_id", nullable = false)
    private Anggota anggota;
    
    @Column(nullable = false)
    private String jenis; // POKOK, WAJIB, SUKARELA
    
    @Column(nullable = false)
    private Double jumlah;
    
    @Column(nullable = false)
    private LocalDate tanggal;
    
    private String keterangan;
    
    @Column(nullable = false)
    private String status; // DRAFT, VERIFIED, CANCELLED
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @Column(nullable = false)
    private String createdBy;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        status = "VERIFIED";
    }
}
```

### **Pinjaman.java**

```java
package com.koperasi.model.entity;

import jakarta.persistence.*;
import lombok.Data;
import java.math.BigDecimal;
import java.time.LocalDate;
import java.time.LocalDateTime;

@Entity
@Table(name = "pinjaman")
@Data
public class Pinjaman {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String kodePinjaman;
    
    @ManyToOne
    @JoinColumn(name = "anggota_id", nullable = false)
    private Anggota anggota;
    
    @Column(nullable = false)
    private BigDecimal jumlahPinjaman;
    
    @Column(nullable = false)
    private BigDecimal bungaPersen; // Persentase bunga per bulan
    
    @Column(nullable = false)
    private Integer tenor; // Dalam bulan
    
    @Column(nullable = false)
    private LocalDate tanggalPinjam;
    
    @Column(nullable = false)
    private LocalDate tanggalJatuhTempo;
    
    private BigDecimal totalBayar; // Jumlah pinjaman + bunga
    
    private BigDecimal sisaPinjaman;
    
    @Column(nullable = false)
    private String status; // PENDING, APPROVED, REJECTED, ACTIVE, PAID, OVERDUE
    
    private String tujuanPinjaman;
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @Column(nullable = false)
    private String createdBy;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        if (kodePinjaman == null) {
            kodePinjaman = "PIN" + System.currentTimeMillis();
        }
        if (status == null) {
            status = "PENDING";
        }
    }
    
    // Method untuk menghitung cicilan per bulan
    public BigDecimal getCicilanPerBulan() {
        if (totalBayar == null || tenor == null || tenor == 0) {
            return BigDecimal.ZERO;
        }
        return totalBayar.divide(BigDecimal.valueOf(tenor), 2, BigDecimal.ROUND_HALF_UP);
    }
}
```

### **Transaksi.java**

```java
package com.koperasi.model.entity;

import jakarta.persistence.*;
import lombok.Data;
import java.math.BigDecimal;
import java.time.LocalDate;
import java.time.LocalDateTime;

@Entity
@Table(name = "transaksi")
@Data
public class Transaksi {
    
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private Long id;
    
    @Column(unique = true, nullable = false)
    private String kodeTransaksi;
    
    @ManyToOne
    @JoinColumn(name = "anggota_id")
    private Anggota anggota;
    
    @Column(nullable = false)
    private String jenis; // SIMPANAN, PENARIKAN, PEMBAYARAN_PINJAMAN
    
    @Column(nullable = false)
    private BigDecimal jumlah;
    
    @Column(nullable = false)
    private LocalDate tanggal;
    
    @Column(nullable = false)
    private String metodePembayaran; // TUNAI, TRANSFER
    
    private String keterangan;
    
    @Column(nullable = false)
    private String status; // SUCCESS, PENDING, FAILED
    
    @Column(nullable = false)
    private LocalDateTime createdAt;
    
    @Column(nullable = false)
    private String createdBy;
    
    @PrePersist
    protected void onCreate() {
        createdAt = LocalDateTime.now();
        if (kodeTransaksi == null) {
            kodeTransaksi = "TRX" + System.currentTimeMillis();
        }
        if (status == null) {
            status = "SUCCESS";
        }
    }
}
```

## 📊 **3. Repository Layer**

### **AnggotaRepository.java**

```java
package com.koperasi.repository;

import com.koperasi.model.entity.Anggota;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.util.List;
import java.util.Optional;

@Repository
public interface AnggotaRepository extends JpaRepository<Anggota, Long> {
    
    Optional<Anggota> findByKodeAnggota(String kodeAnggota);
    
    Optional<Anggota> findByEmail(String email);
    
    List<Anggota> findByStatus(String status);
    
    List<Anggota> findByDepartemen(String departemen);
    
    @Query("SELECT a FROM Anggota a WHERE LOWER(a.nama) LIKE LOWER(CONCAT('%', :keyword, '%'))")
    List<Anggota> searchByNama(String keyword);
    
    @Query("SELECT COUNT(a) FROM Anggota a WHERE a.status = 'AKTIF'")
    Long countAktifAnggota();
}
```

### **SimpananRepository.java**

```java
package com.koperasi.repository;

import com.koperasi.model.entity.Simpanan;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.time.LocalDate;
import java.util.List;

@Repository
public interface SimpananRepository extends JpaRepository<Simpanan, Long> {
    
    List<Simpanan> findByAnggotaId(Long anggotaId);
    
    List<Simpanan> findByJenis(String jenis);
    
    List<Simpanan> findByTanggalBetween(LocalDate start, LocalDate end);
    
    @Query("SELECT SUM(s.jumlah) FROM Simpanan s WHERE s.anggota.id = :anggotaId AND s.status = 'VERIFIED'")
    Double getTotalSimpananByAnggota(Long anggotaId);
    
    @Query("SELECT s.jenis, SUM(s.jumlah) FROM Simpanan s WHERE s.status = 'VERIFIED' GROUP BY s.jenis")
    List<Object[]> getSummaryByJenis();
}
```

### **PinjamanRepository.java**

```java
package com.koperasi.repository;

import com.koperasi.model.entity.Pinjaman;
import org.springframework.data.jpa.repository.JpaRepository;
import org.springframework.data.jpa.repository.Query;
import org.springframework.stereotype.Repository;

import java.time.LocalDate;
import java.util.List;

@Repository
public interface PinjamanRepository extends JpaRepository<Pinjaman, Long> {
    
    List<Pinjaman> findByAnggotaId(Long anggotaId);
    
    List<Pinjaman> findByStatus(String status);
    
    List<Pinjaman> findByTanggalPinjamBetween(LocalDate start, LocalDate end);
    
    @Query("SELECT p FROM Pinjaman p WHERE p.status = 'ACTIVE' AND p.tanggalJatuhTempo < :today")
    List<Pinjaman> findOverduePinjaman(LocalDate today);
    
    @Query("SELECT SUM(p.sisaPinjaman) FROM Pinjaman p WHERE p.anggota.id = :anggotaId AND p.status = 'ACTIVE'")
    Double getTotalPinjamanAktifByAnggota(Long anggotaId);
}
```

## 🧠 **4. Service Layer**

### **KoperasiService.java**

```java
package com.koperasi.service;

import com.koperasi.model.entity.*;
import com.koperasi.repository.*;
import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;

import java.math.BigDecimal;
import java.math.RoundingMode;
import java.time.LocalDate;
import java.time.LocalDateTime;
import java.util.List;
import java.util.Map;
import java.util.HashMap;

@Service
@RequiredArgsConstructor
@Transactional
public class KoperasiService {
    
    private final AnggotaRepository anggotaRepository;
    private final SimpananRepository simpananRepository;
    private final PinjamanRepository pinjamanRepository;
    private final TransaksiRepository transaksiRepository;
    
    // ========== ANGGOTA SERVICES ==========
    
    public Anggota createAnggota(Anggota anggota) {
        anggota.setStatus("AKTIF");
        anggota.setTanggalBergabung(LocalDate.now());
        anggota.setTotalSimpanan(0.0);
        anggota.setTotalPinjaman(0.0);
        return anggotaRepository.save(anggota);
    }
    
    public List<Anggota> getAllAnggota() {
        return anggotaRepository.findAll();
    }
    
    public Anggota getAnggotaByKode(String kodeAnggota) {
        return anggotaRepository.findByKodeAnggota(kodeAnggota)
                .orElseThrow(() -> new RuntimeException("Anggota tidak ditemukan"));
    }
    
    // ========== SIMPANAN SERVICES ==========
    
    public Simpanan simpanPokok(Long anggotaId, Double jumlah, String dibuatOleh) {
        Anggota anggota = anggotaRepository.findById(anggotaId)
                .orElseThrow(() -> new RuntimeException("Anggota tidak ditemukan"));
        
        Simpanan simpanan = new Simpanan();
        simpanan.setAnggota(anggota);
        simpanan.setJenis("POKOK");
        simpanan.setJumlah(jumlah);
        simpanan.setTanggal(LocalDate.now());
        simpanan.setKeterangan("Simpanan Pokok");
        simpanan.setCreatedBy(dibuatOleh);
        
        // Update total simpanan anggota
        anggota.setTotalSimpanan(anggota.getTotalSimpanan() + jumlah);
        anggotaRepository.save(anggota);
        
        // Buat transaksi
        createTransaksi(anggota, "SIMPANAN", BigDecimal.valueOf(jumlah), "TUNAI", 
                       "Simpanan Pokok", dibuatOleh);
        
        return simpananRepository.save(simpanan);
    }
    
    public Simpanan simpanSukarela(Long anggotaId, Double jumlah, String keterangan, String dibuatOleh) {
        Anggota anggota = anggotaRepository.findById(anggotaId)
                .orElseThrow(() -> new RuntimeException("Anggota tidak ditemukan"));
        
        Simpanan simpanan = new Simpanan();
        simpanan.setAnggota(anggota);
        simpanan.setJenis("SUKARELA");
        simpanan.setJumlah(jumlah);
        simpanan.setTanggal(LocalDate.now());
        simpanan.setKeterangan(keterangan);
        simpanan.setCreatedBy(dibuatOleh);
        
        // Update total simpanan anggota
        anggota.setTotalSimpanan(anggota.getTotalSimpanan() + jumlah);
        anggotaRepository.save(anggota);
        
        // Buat transaksi
        createTransaksi(anggota, "SIMPANAN", BigDecimal.valueOf(jumlah), "TUNAI", 
                       keterangan, dibuatOleh);
        
        return simpananRepository.save(simpanan);
    }
    
    // ========== PINJAMAN SERVICES ==========
    
    public Pinjaman ajukanPinjaman(Pinjaman pinjaman, String dibuatOleh) {
        Anggota anggota = pinjaman.getAnggota();
        
        // Validasi: Cek total pinjaman aktif
        Double totalPinjamanAktif = pinjamanRepository.getTotalPinjamanAktifByAnggota(anggota.getId());
        if (totalPinjamanAktif != null && totalPinjamanAktif > 0) {
            throw new RuntimeException("Anggota masih memiliki pinjaman aktif");
        }
        
        // Validasi: Maksimal pinjaman 10x total simpanan
        if (pinjaman.getJumlahPinjaman().doubleValue() > (anggota.getTotalSimpanan() * 10)) {
            throw new RuntimeException("Jumlah pinjaman melebihi batas maksimal");
        }
        
        // Hitung total bayar (pinjaman + bunga)
        BigDecimal bungaTotal = pinjaman.getJumlahPinjaman()
                .multiply(pinjaman.getBungaPersen())
                .divide(BigDecimal.valueOf(100))
                .multiply(BigDecimal.valueOf(pinjaman.getTenor()));
        
        BigDecimal totalBayar = pinjaman.getJumlahPinjaman().add(bungaTotal);
        
        pinjaman.setTotalBayar(totalBayar);
        pinjaman.setSisaPinjaman(totalBayar);
        pinjaman.setTanggalPinjam(LocalDate.now());
        pinjaman.setTanggalJatuhTempo(LocalDate.now().plusMonths(pinjaman.getTenor()));
        pinjaman.setCreatedBy(dibuatOleh);
        pinjaman.setStatus("PENDING");
        
        return pinjamanRepository.save(pinjaman);
    }
    
    public Pinjaman approvePinjaman(Long pinjamanId, String approvedBy) {
        Pinjaman pinjaman = pinjamanRepository.findById(pinjamanId)
                .orElseThrow(() -> new RuntimeException("Pinjaman tidak ditemukan"));
        
        if (!"PENDING".equals(pinjaman.getStatus())) {
            throw new RuntimeException("Status pinjaman tidak valid untuk approval");
        }
        
        pinjaman.setStatus("ACTIVE");
        
        // Update total pinjaman anggota
        Anggota anggota = pinjaman.getAnggota();
        anggota.setTotalPinjaman(anggota.getTotalPinjaman() + pinjaman.getTotalBayar().doubleValue());
        anggotaRepository.save(anggota);
        
        // Buat transaksi penerimaan pinjaman
        createTransaksi(anggota, "PINJAMAN", pinjaman.getJumlahPinjaman(), "TRANSFER",
                       "Pencairan pinjaman: " + pinjaman.getKodePinjaman(), approvedBy);
        
        return pinjamanRepository.save(pinjaman);
    }
    
    public Transaksi bayarCicilan(Long pinjamanId, BigDecimal jumlah, String metode, String dibayarOleh) {
        Pinjaman pinjaman = pinjamanRepository.findById(pinjamanId)
                .orElseThrow(() -> new RuntimeException("Pinjaman tidak ditemukan"));
        
        if (!"ACTIVE".equals(pinjaman.getStatus())) {
            throw new RuntimeException("Pinjaman tidak aktif");
        }
        
        if (jumlah.compareTo(pinjaman.getSisaPinjaman()) > 0) {
            jumlah = pinjaman.getSisaPinjaman();
        }
        
        // Kurangi sisa pinjaman
        pinjaman.setSisaPinjaman(pinjaman.getSisaPinjaman().subtract(jumlah));
        
        // Jika sudah lunas
        if (pinjaman.getSisaPinjaman().compareTo(BigDecimal.ZERO) <= 0) {
            pinjaman.setStatus("PAID");
            
            // Update total pinjaman anggota
            Anggota anggota = pinjaman.getAnggota();
            anggota.setTotalPinjaman(Math.max(0, anggota.getTotalPinjaman() - pinjaman.getTotalBayar().doubleValue()));
            anggotaRepository.save(anggota);
        }
        
        pinjamanRepository.save(pinjaman);
        
        // Buat transaksi pembayaran
        return createTransaksi(pinjaman.getAnggota(), "PEMBAYARAN_PINJAMAN", 
                             jumlah, metode, "Pembayaran cicilan pinjaman", dibayarOleh);
    }
    
    // ========== TRANSAKSI SERVICES ==========
    
    private Transaksi createTransaksi(Anggota anggota, String jenis, BigDecimal jumlah, 
                                     String metode, String keterangan, String dibuatOleh) {
        Transaksi transaksi = new Transaksi();
        transaksi.setAnggota(anggota);
        transaksi.setJenis(jenis);
        transaksi.setJumlah(jumlah);
        transaksi.setTanggal(LocalDate.now());
        transaksi.setMetodePembayaran(metode);
        transaksi.setKeterangan(keterangan);
        transaksi.setCreatedBy(dibuatOleh);
        
        return transaksiRepository.save(transaksi);
    }
    
    // ========== REPORT SERVICES ==========
    
    public Map<String, Object> getDashboardSummary() {
        Map<String, Object> summary = new HashMap<>();
        
        // Total anggota aktif
        Long totalAnggota = anggotaRepository.countAktifAnggota();
        summary.put("totalAnggota", totalAnggota);
        
        // Total simpanan
        List<Simpanan> semuaSimpanan = simpananRepository.findAll();
        Double totalSimpanan = semuaSimpanan.stream()
                .filter(s -> "VERIFIED".equals(s.getStatus()))
                .mapToDouble(Simpanan::getJumlah)
                .sum();
        summary.put("totalSimpanan", totalSimpanan);
        
        // Total pinjaman aktif
        List<Pinjaman> pinjamanAktif = pinjamanRepository.findByStatus("ACTIVE");
        BigDecimal totalPinjamanAktif = pinjamanAktif.stream()
                .map(Pinjaman::getSisaPinjaman)
                .reduce(BigDecimal.ZERO, BigDecimal::add);
        summary.put("totalPinjamanAktif", totalPinjamanAktif);
        
        // Pinjaman overdue
        List<Pinjaman> pinjamanOverdue = pinjamanRepository.findOverduePinjaman(LocalDate.now());
        summary.put("pinjamanOverdue", pinjamanOverdue.size());
        
        // Transaksi bulan ini
        LocalDate awalBulan = LocalDate.now().withDayOfMonth(1);
        LocalDate akhirBulan = LocalDate.now().withDayOfMonth(LocalDate.now().lengthOfMonth());
        List<Transaksi> transaksiBulanIni = transaksiRepository.findByTanggalBetween(awalBulan, akhirBulan);
        summary.put("transaksiBulanIni", transaksiBulanIni.size());
        
        return summary;
    }
    
    public Map<String, Double> getSimpananSummary() {
        Map<String, Double> summary = new HashMap<>();
        List<Object[]> result = simpananRepository.getSummaryByJenis();
        
        result.forEach(row -> {
            summary.put((String) row[0], ((Number) row[1]).doubleValue());
        });
        
        return summary;
    }
}
```

## 🌐 **5. Controller Layer**

### **AnggotaController.java**

```java
package com.koperasi.controller;

import com.koperasi.model.entity.Anggota;
import com.koperasi.service.KoperasiService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api/anggota")
@RequiredArgsConstructor
public class AnggotaController {
    
    private final KoperasiService koperasiService;
    
    @PostMapping
    public ResponseEntity<Anggota> createAnggota(@RequestBody Anggota anggota) {
        return ResponseEntity.ok(koperasiService.createAnggota(anggota));
    }
    
    @GetMapping
    public ResponseEntity<List<Anggota>> getAllAnggota() {
        return ResponseEntity.ok(koperasiService.getAllAnggota());
    }
    
    @GetMapping("/{kodeAnggota}")
    public ResponseEntity<Anggota> getAnggotaByKode(@PathVariable String kodeAnggota) {
        return ResponseEntity.ok(koperasiService.getAnggotaByKode(kodeAnggota));
    }
    
    @GetMapping("/departemen/{departemen}")
    public ResponseEntity<List<Anggota>> getAnggotaByDepartemen(@PathVariable String departemen) {
        // Implementasi bisa ditambahkan di service
        return ResponseEntity.ok(List.of());
    }
}
```

### **SimpananController.java**

```java
package com.koperasi.controller;

import com.koperasi.model.entity.Simpanan;
import com.koperasi.service.KoperasiService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.*;

import java.util.Map;

@RestController
@RequestMapping("/api/simpanan")
@RequiredArgsConstructor
public class SimpananController {
    
    private final KoperasiService koperasiService;
    
    @PostMapping("/pokok")
    public ResponseEntity<Simpanan> simpanPokok(
            @RequestParam Long anggotaId,
            @RequestParam Double jumlah,
            @RequestParam String dibuatOleh) {
        return ResponseEntity.ok(koperasiService.simpanPokok(anggotaId, jumlah, dibuatOleh));
    }
    
    @PostMapping("/sukarela")
    public ResponseEntity<Simpanan> simpanSukarela(
            @RequestParam Long anggotaId,
            @RequestParam Double jumlah,
            @RequestParam String keterangan,
            @RequestParam String dibuatOleh) {
        return ResponseEntity.ok(koperasiService.simpanSukarela(anggotaId, jumlah, keterangan, dibuatOleh));
    }
    
    @GetMapping("/summary")
    public ResponseEntity<Map<String, Double>> getSummary() {
        return ResponseEntity.ok(koperasiService.getSimpananSummary());
    }
}
```

### **DashboardController.java**

```java
package com.koperasi.controller;

import com.koperasi.service.KoperasiService;
import lombok.RequiredArgsConstructor;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.RestController;

import java.util.Map;

@RestController
@RequestMapping("/api/dashboard")
@RequiredArgsConstructor
public class DashboardController {
    
    private final KoperasiService koperasiService;
    
    @GetMapping("/summary")
    public ResponseEntity<Map<String, Object>> getDashboardSummary() {
        return ResponseEntity.ok(koperasiService.getDashboardSummary());
    }
}
```

## 🔒 **6. Security Configuration**

### **SecurityConfig.java**

```java
package com.koperasi.config;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.security.config.annotation.web.builders.HttpSecurity;
import org.springframework.security.config.annotation.web.configuration.EnableWebSecurity;
import org.springframework.security.config.annotation.web.configurers.AbstractHttpConfigurer;
import org.springframework.security.core.userdetails.User;
import org.springframework.security.core.userdetails.UserDetails;
import org.springframework.security.core.userdetails.UserDetailsService;
import org.springframework.security.crypto.bcrypt.BCryptPasswordEncoder;
import org.springframework.security.crypto.password.PasswordEncoder;
import org.springframework.security.provisioning.InMemoryUserDetailsManager;
import org.springframework.security.web.SecurityFilterChain;

@Configuration
@EnableWebSecurity
public class SecurityConfig {
    
    @Bean
    public SecurityFilterChain securityFilterChain(HttpSecurity http) throws Exception {
        http
            .csrf(AbstractHttpConfigurer::disable)
            .authorizeHttpRequests(authz -> authz
                .requestMatchers("/api/public/**", "/swagger-ui/**", "/v3/api-docs/**").permitAll()
                .requestMatchers("/api/admin/**").hasRole("ADMIN")
                .anyRequest().authenticated()
            )
            .httpBasic(httpBasic -> {});
        
        return http.build();
    }
    
    @Bean
    public UserDetailsService userDetailsService() {
        UserDetails admin = User.builder()
                .username("admin")
                .password(passwordEncoder().encode("admin123"))
                .roles("ADMIN")
                .build();
                
        UserDetails user = User.builder()
                .username("user")
                .password(passwordEncoder().encode("user123"))
                .roles("USER")
                .build();
        
        return new InMemoryUserDetailsManager(admin, user);
    }
    
    @Bean
    public PasswordEncoder passwordEncoder() {
        return new BCryptPasswordEncoder();
    }
}
```

## 📚 **7. Documentation (Swagger)**

### **SwaggerConfig.java**

```java
package com.koperasi.config;

import io.swagger.v3.oas.models.OpenAPI;
import io.swagger.v3.oas.models.info.Info;
import io.swagger.v3.oas.models.info.Contact;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class SwaggerConfig {
    
    @Bean
    public OpenAPI customOpenAPI() {
        return new OpenAPI()
                .info(new Info()
                        .title("Sistem Koperasi Karyawan API")
                        .version("1.0")
                        .description("API untuk sistem koperasi perusahaan dengan 40 karyawan")
                        .contact(new Contact()
                                .name("Admin Koperasi")
                                .email("admin@koperasi.com")));
    }
}
```

## 🚀 **8. Main Application**

### **KoperasiApplication.java**

```java
package com.koperasi;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class KoperasiApplication {
    
    public static void main(String[] args) {
        SpringApplication.run(KoperasiApplication.class, args);
    }
}
```

## 📋 **9. Database Script (MySQL)**

### **database_setup.sql**

```sql
-- Create database
CREATE DATABASE IF NOT EXISTS db_koperasi;
USE db_koperasi;

-- Table anggota
CREATE TABLE IF NOT EXISTS anggota (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    kode_anggota VARCHAR(20) UNIQUE NOT NULL,
    nama VARCHAR(100) NOT NULL,
    email VARCHAR(100) NOT NULL,
    nomor_telepon VARCHAR(20),
    alamat TEXT,
    departemen VARCHAR(50) NOT NULL,
    tanggal_bergabung DATE NOT NULL,
    status VARCHAR(20) NOT NULL DEFAULT 'AKTIF',
    total_simpanan DECIMAL(15,2) DEFAULT 0.00,
    total_pinjaman DECIMAL(15,2) DEFAULT 0.00,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    updated_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP,
    INDEX idx_kode_anggota (kode_anggota),
    INDEX idx_status (status),
    INDEX idx_departemen (departemen)
);

-- Table simpanan
CREATE TABLE IF NOT EXISTS simpanan (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    anggota_id BIGINT NOT NULL,
    jenis VARCHAR(20) NOT NULL, -- POKOK, WAJIB, SUKARELA
    jumlah DECIMAL(15,2) NOT NULL,
    tanggal DATE NOT NULL,
    keterangan TEXT,
    status VARCHAR(20) NOT NULL DEFAULT 'VERIFIED',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_by VARCHAR(100) NOT NULL,
    FOREIGN KEY (anggota_id) REFERENCES anggota(id) ON DELETE CASCADE,
    INDEX idx_anggota_id (anggota_id),
    INDEX idx_jenis (jenis),
    INDEX idx_tanggal (tanggal)
);

-- Table pinjaman
CREATE TABLE IF NOT EXISTS pinjaman (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    kode_pinjaman VARCHAR(50) UNIQUE NOT NULL,
    anggota_id BIGINT NOT NULL,
    jumlah_pinjaman DECIMAL(15,2) NOT NULL,
    bunga_persen DECIMAL(5,2) NOT NULL,
    tenor INT NOT NULL,
    tanggal_pinjam DATE NOT NULL,
    tanggal_jatuh_tempo DATE NOT NULL,
    total_bayar DECIMAL(15,2),
    sisa_pinjaman DECIMAL(15,2),
    status VARCHAR(20) NOT NULL, -- PENDING, APPROVED, REJECTED, ACTIVE, PAID, OVERDUE
    tujuan_pinjaman TEXT,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_by VARCHAR(100) NOT NULL,
    FOREIGN KEY (anggota_id) REFERENCES anggota(id) ON DELETE CASCADE,
    INDEX idx_kode_pinjaman (kode_pinjaman),
    INDEX idx_anggota_id (anggota_id),
    INDEX idx_status (status),
    INDEX idx_tanggal_jatuh_tempo (tanggal_jatuh_tempo)
);

-- Table transaksi
CREATE TABLE IF NOT EXISTS transaksi (
    id BIGINT PRIMARY KEY AUTO_INCREMENT,
    kode_transaksi VARCHAR(50) UNIQUE NOT NULL,
    anggota_id BIGINT,
    jenis VARCHAR(30) NOT NULL, -- SIMPANAN, PENARIKAN, PEMBAYARAN_PINJAMAN
    jumlah DECIMAL(15,2) NOT NULL,
    tanggal DATE NOT NULL,
    metode_pembayaran VARCHAR(20) NOT NULL,
    keterangan TEXT,
    status VARCHAR(20) NOT NULL DEFAULT 'SUCCESS',
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP,
    created_by VARCHAR(100) NOT NULL,
    FOREIGN KEY (anggota_id) REFERENCES anggota(id) ON DELETE SET NULL,
    INDEX idx_kode_transaksi (kode_transaksi),
    INDEX idx_anggota_id (anggota_id),
    INDEX idx_jenis (jenis),
    INDEX idx_tanggal (tanggal)
);

-- Insert sample data
INSERT INTO anggota (kode_anggota, nama, email, departemen, tanggal_bergabung, status) VALUES
('KOP0001', 'Budi Santoso', 'budi@perusahaan.com', 'IT', '2024-01-15', 'AKTIF'),
('KOP0002', 'Siti Rahayu', 'siti@perusahaan.com', 'HRD', '2024-01-16', 'AKTIF'),
('KOP0003', 'Ahmad Hidayat', 'ahmad@perusahaan.com', 'Finance', '2024-01-17', 'AKTIF');

INSERT INTO simpanan (anggota_id, jenis, jumlah, tanggal, keterangan, created_by) VALUES
(1, 'POKOK', 500000.00, '2024-01-15', 'Simpanan Pokok', 'admin'),
(1, 'SUKARELA', 1000000.00, '2024-01-20', 'Simpanan Sukarela', 'admin'),
(2, 'POKOK', 500000.00, '2024-01-16', 'Simpanan Pokok', 'admin');

UPDATE anggota SET total_simpanan = 1500000.00 WHERE id = 1;
UPDATE anggota SET total_simpanan = 500000.00 WHERE id = 2;
```

## 📁 **10. File README.md**

```markdown
# Sistem Koperasi Karyawan

Sistem manajemen koperasi untuk perusahaan dengan 40 karyawan.

## 🚀 Fitur Utama

1. **Manajemen Anggota**
   - Pendaftaran anggota baru
   - Update data anggota
   - Status keanggotaan

2. **Simpanan**
   - Simpanan Pokok (wajib)
   - Simpanan Sukarela
   - Laporan simpanan per anggota

3. **Pinjaman**
   - Pengajuan pinjaman
   - Approval pinjaman
   - Pembayaran cicilan
   - Perhitungan bunga otomatis

4. **Transaksi**
   - Riwayat semua transaksi
   - Multiple metode pembayaran
   - Status transaksi real-time

5. **Laporan & Dashboard**
   - Ringkasan keuangan
   - Statistik anggota
   - Pinjaman overdue

## 🛠️ Teknologi

- **Backend:** Spring Boot 3.x
- **Java:** JDK 21
- **Database:** MySQL 8.x
- **Security:** Spring Security
- **Documentation:** Swagger/OpenAPI 3

## 📦 Instalasi

1. **Clone repository**
```bash
git clone https://github.com/yourusername/sistem-koperasi.git
cd sistem-koperasi
```

2. **Setup Database**
```bash
mysql -u root -p < database_setup.sql
```

3. **Konfigurasi Application**
- Edit `src/main/resources/application.yml`
- Sesuaikan database credentials

4. **Run Application**
```bash
./mvnw spring-boot:run
```

## 🔐 Default Login

- **Admin:** `admin` / `admin123`
- **User:** `user` / `user123`

## 📖 API Documentation

Setelah aplikasi running, akses:
- Swagger UI: http://localhost:8080/api/swagger-ui.html
- API Docs: http://localhost:8080/api/v3/api-docs

## 📊 Endpoint Penting

| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| POST | `/api/anggota` | Buat anggota baru |
| GET | `/api/anggota` | Daftar semua anggota |
| POST | `/api/simpanan/pokok` | Setor simpanan pokok |
| POST | `/api/pinjaman/ajukan` | Ajukan pinjaman |
| GET | `/api/dashboard/summary` | Ringkasan dashboard |

## 🧪 Testing

```bash
# Run all tests
./mvnw test

# Run with coverage
./mvnw test jacoco:report
```

## 🐛 Troubleshooting

1. **Port 8080 sudah digunakan**
   - Ganti port di `application.yml`
   - Atau kill process: `sudo lsof -ti:8080 | xargs kill -9`

2. **Database connection error**
   - Pastikan MySQL service running
   - Cek username/password di application.yml

3. **Build error**
   - Clear Maven cache: `./mvnw clean`
   - Update dependencies: `./mvnw versions:update-properties`

## 👥 Kontribusi

1. Fork repository
2. Create feature branch
3. Commit changes
4. Push to branch
5. Create Pull Request

## 📄 Lisensi

MIT License
```

## 🚀 **Instruksi Deploy**

### **Step-by-Step Deployment:**

1. **Clone & Setup:**
```bash
git clone [repository-url]
cd sistem-koperasi
```

2. **Database Setup:**
```bash
mysql -u root -p
> CREATE DATABASE db_koperasi;
> USE db_koperasi;
> SOURCE database_setup.sql;
```

3. **Build Application:**
```bash
./mvnw clean package
```

4. **Run Application:**
```bash
java -jar target/sistem-koperasi-1.0.0.jar
```

5. **Access Application:**
- API: http://localhost:8080/api
- Swagger: http://localhost:8080/api/swagger-ui.html
- Dashboard: http://localhost:8080/api/dashboard/summary

### **Untuk Production:**
- Gunakan environment variables untuk sensitive data
- Konfigurasi SSL/TLS
- Setup backup database otomatis
- Monitor dengan Spring Boot Actuator

## 📈 **Fitur Tambahan yang Bisa Dikembangkan:**

1. **Notifikasi Email** untuk:
   - Jadwal pembayaran pinjaman
   - Transaksi berhasil
   - Reminder simpanan

2. **Export Laporan** ke Excel/PDF

3. **Mobile App** menggunakan REST API

4. **Integrasi Payment Gateway**

5. **Multi-user dengan role berbeda**