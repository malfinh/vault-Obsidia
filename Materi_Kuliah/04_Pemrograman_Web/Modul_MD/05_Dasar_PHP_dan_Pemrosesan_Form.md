# 5. Dasar PHP dan Pemrosesan Formulir (Form Handling)

Navigasi: Modul Sebelumnya: [[04_jQuery_dan_AJAX]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[06_PHP_Pemrograman_Berorientasi_Objek_OOP]]

---

**PHP (Hypertext Preprocessor)** adalah bahasa pemrograman skrip sisi server (*server-side scripting language*) yang dirancang khusus untuk memproses logika bisnis dinamis, mengakses basis data, dan menghasilkan halaman web HTML secara dinamis.

---

## 5.1. Sintaksis Dasar PHP

Kode PHP disisipkan di dalam berkas HTML menggunakan tag khusus `<?php ... ?>`.

```php
<?php
// 1. Deklarasi Variabel (Diawali tanda $)
$nama_aplikasi = "Sistem Informasi Akademik";
$versi = 2.0;
$is_maintenance = false;

// 2. Array Terindeks dan Asosiatif
$matakuliah = ["Pemrograman Web", "Struktur Data", "Basis Data"];
$mahasiswa = [
    "nim" => "M0520001",
    "nama" => "Siti Rahma",
    "ipk" => 3.85
];

// 3. Output ke HTML
echo "<h1>Selamat Datang di " . $nama_aplikasi . "</h1>";
echo "<p>Mahasiswa: " . $mahasiswa["nama"] . " (IPK: " . $mahasiswa["ipk"] . ")</p>";
?>
```

---

## 5.2. Variabel Superglobal (Superglobals)

PHP menyediakan variabel bawaan yang selalu dapat diakses di seluruh cakupan script (*global scope*):

| Variabel Superglobal | Fungsi dan Deskripsi |
| :--- | :--- |
| **`$_GET`** | Memuat data yang dikirim melalui formulir dengan metode `GET` (terlihat di URL browser). |
| **`$_POST`** | Memuat data yang dikirim melalui formulir dengan metode `POST` (tersembunyi dalam body HTTP). |
| **`$_SERVER`** | Informasi header server, jalur script, dan lokasi file pengeksekusi. |
| **`$_FILES`** | Informasi dan berkas yang diunggah pengguna melalui form `type="file"`. |
| **`$_SESSION`** | Memuat data sesi pengguna yang tersimpan di server. |

---

## 5.3. Pemrosesan Form: Metode GET vs POST

```mermaid
flowchart LR
    FormHTML["Formulir HTML (form action='proses.php')"]
    FormHTML -->|Metode GET: Parameter di URL| ServerGET["proses.php ($_GET['keyword'])"]
    FormHTML -->|Metode POST: Body Request HTTP (Aman)| ServerPOST["proses.php ($_POST['username'])"]
```

### Perbandingan Metode Form:
- **`GET`**: Data dikirim via URL (`proses.php?nama=Budi&kategori=umum`). Sesuai untuk formulir **pencarian / pencocokan** yang dapat di-*bookmark*. Tidak boleh digunakan untuk password!
- **`POST`**: Data dikirim di dalam *HTTP Body Request*. Sesuai untuk pendaftaran, login, transaksi, dan data rahasia.

---

## 5.4. Validasi dan Sanitasi Input (Pencegahan Vulnerability XSS)

Input pengguna tidak boleh dipercaya secara langsung (*Never Trust User Input*). Menampilkan masukan mentah pengguna ke HTML tanpa sanitasi berisiko menimbulkan kerentanan **Cross-Site Scripting (XSS)**.

```php
<?php
// Fungsi Sanitasi Input
function bersihkanInput($data) {
    $data = trim($data);                  // Menghapus spasi di awal/akhir
    $data = stripslashes($data);           // Menghapus backslashes (\)
    $data = htmlspecialchars($data, ENT_QUOTES, 'UTF-8'); // Mengubah karakter HTML khusus (<, >, &)
    return $data;
}

// Pemrosesan Formulir POST
if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $username = bersihkanInput($_POST["username"]);
    $email = filter_var($_POST["email"], FILTER_SANITIZE_EMAIL);

    // Validasi Format Email
    if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
        echo "Format email tidak valid!";
    } else {
        echo "Selamat datang, " . $username;
    }
}
?>
```

---

Navigasi: Modul Sebelumnya: [[04_jQuery_dan_AJAX]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[06_PHP_Pemrograman_Berorientasi_Objek_OOP]]
