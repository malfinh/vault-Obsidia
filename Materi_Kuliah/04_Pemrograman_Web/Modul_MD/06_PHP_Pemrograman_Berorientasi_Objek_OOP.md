# 6. Pemrograman Berorientasi Objek di PHP (PHP OOP)

Navigasi: Modul Sebelumnya: [[05_Dasar_PHP_dan_Pemrosesan_Form]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[07_Koneksi_Database_PHP_dan_SQL]]

---

**Object-Oriented Programming (OOP)** pada PHP adalah paradigma pemrograman yang menyusun kode dalam bentuk **Class** (cetak biru) dan **Object** (instansi nyata) yang mengombinasikan data (*properti*) dan perilaku (*method*).

---

## 6.1. Konsep Class, Object, dan Constructor

```php
<?php
class Produk {
    // 1. Properti (Atribut)
    public string $nama;
    public float $harga;

    // 2. Magic Method Constructor (Otomatis dipanggil saat objek dibuat)
    public function __construct(string $nama, float $harga) {
        $this->nama = $nama;
        $this->harga = $harga;
    }

    // 3. Method (Fungsi Perilaku)
    public function getInfo(): string {
        return "Produk: " . $this->nama . " | Harga: Rp" . number_format($this->harga, 0, ',', '.');
    }
}

// Instansiasi Objek (Menggunakan kata kunci 'new')
$laptop = new Produk("Laptop Gaming", 15000000);
echo $laptop->getInfo();
?>
```

---

## 6.2. Modifier Akses (Access Modifiers)

PHP menyediakan 3 tingkat hak akses enkapsulasi data:

- **`public`**: Properti/Method dapat diakses dari mana saja (luar kelas, kelas turunan, maupun objek).
- **`protected`**: Properti/Method hanya dapat diakses oleh kelas itu sendiri dan **kelas turunannya (*subclass*)**.
- **`private`**: Properti/Method **HANYA** dapat diakses di dalam kelas asal itu sendiri.

```php
<?php
class RekeningBank {
    private float $saldo; // Tidak bisa diakses langsung dari luar

    public function __construct(float $saldoAwal) {
        $this->saldo = $saldoAwal;
    }

    // Getter untuk membaca nilai private secara aman
    public function getSaldo(): float {
        return $this->saldo;
    }

    // Setter dengan validasi aturan bisnis
    public function setoran(float $jumlah): void {
        if ($jumlah > 0) {
            $this->saldo += $jumlah;
        }
    }
}
?>
```

---

## 6.3. Pewarisan (Inheritance)

Subclass dapat mewarisi seluruh properti dan method ber-modifier `public` atau `protected` dari Superclass menggunakan kata kunci **`extends`**.

```php
<?php
// Superclass Induk
class Pengguna {
    protected string $nama;
    protected string $email;

    public function __construct(string $nama, string $email) {
        $this->nama = $nama;
        $this->email = $email;
    }

    public function getProfil(): string {
        return "Nama: " . $this->nama . " (" . $this->email . ")";
    }
}

// Subclass Turunan
class Admin extends Pengguna {
    private int $levelAkses;

    public function __construct(string $nama, string $email, int $level) {
        parent::__construct($nama, $email); // Memanggil constructor induk
        $this->levelAkses = $level;
    }

    public function getRole(): string {
        return "Admin Level: " . $this->levelAkses;
    }
}

$admin1 = new Admin("Siti", "siti@admin.com", 1);
echo $admin1->getProfil(); // Inherited dari Pengguna
echo $admin1->getRole();
?>
```

---

## 6.4. Interface dan Traits

- **Interface (`interface` / `implements`)**: Kontrak kerja yang mewajibkan kelas turunan untuk mengimplementasikan seluruh fungsi method yang dideklarasikan.
- **Trait (`trait` / `use`)**: Mekanisme untuk menggunakan kembali kode (*code reuse*) pada beberapa kelas independen secara fleksibel tanpa keterbatasan pewarisan tunggal.

```php
<?php
interface NotifikasiInterface {
    public function kirimPesan(string $pesan): bool;
}

class EmailNotifikasi implements NotifikasiInterface {
    public function kirimPesan(string $pesan): bool {
        // Logika pengiriman email...
        return true;
    }
}
?>
```

---

Navigasi: Modul Sebelumnya: [[05_Dasar_PHP_dan_Pemrosesan_Form]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[07_Koneksi_Database_PHP_dan_SQL]]
