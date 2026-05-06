Itu **benar tampil**, tapi bukan sebagai akun `PENDAPATAN SHIP MANAGEMENT`.

Nilai `1.100.000.000` masuk ke:

```text
SYS-CP - Current Month Profit (Computed)
```

karena di controller Anda ada logic:

```php
$currentMonthProfit = $this->getNetIncome($currentMonthStart, $asOf);
```

Lalu `getNetIncome()` menghitung:

```php
$revenue = $this->sumProfitLossByPrefixes(['4-'], $from, $to);
```

Jadi akun `4-7000 PENDAPATAN SHIP MANAGEMENT` **tidak tampil langsung di Neraca**, tetapi nilainya dihitung sebagai **laba bulan berjalan** dan dimasukkan ke bagian **Equity**.

Secara accounting, ini konsepnya benar:

```text
Pendapatan - Beban = Laba/Rugi
Laba/Rugi masuk ke Ekuitas
```

Yang kurang tepat hanya label-nya. Saat ini `SYS-RE` tertulis:

```text
Current Year Profit (Computed)
```

Padahal itu dari kode Anda adalah:

```php
$retainedEarningsAmount
```

Sebaiknya ubah menjadi:

```php
'name' => 'Retained Earnings / Laba Ditahan (Computed)',
```

dan untuk `SYS-CP`:

```php
'name' => 'Current Month Profit / Laba Bulan Berjalan (Computed)',
```

Jadi kesimpulannya: **PENDAPATAN tidak lagi tampil sebagai akun 4-7000, tapi nilainya memang wajar masuk ke Current Month Profit di Equity.**
