# 8. Manajemen Session, Cookies, dan Keamanan Web

Navigasi: Modul Sebelumnya: [[07_Koneksi_Database_PHP_dan_SQL]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[09_Framework_Laravel_dan_Arsitektur_MVC]]

---

Protokol HTTP bersifat **Stateless**, artinya setiap permintaan (*request*) dari peramban ke server diperlakukan sebagai permintaan mandiri tanpa mengingat siapa pengguna pada permintaan sebelumnya. Untuk mengingat status login atau keranjang belanja pengguna, digunakan **Cookies** dan **Sessions**.

---

## 8.1. Perbandingan Cookies vs Sessions

| Karakteristik | Cookies | Sessions |
| :--- | :--- | :--- |
| **Lokasi Penyimpanan** | Disimpan di **Client / Browser Peramban** | Disimpan di **Server Web** |
| **Kapasitas Penyimpanan** | Sangat Terbatas ($\approx 4\text{ KB}$) | Besar (Tergantung kapasitas memori server) |
| **Keamanan** | **Kurang Aman** (Dapat dimanipulasi pengguna di peramban) | **Lebih Aman** (Nilai rahasia tidak terlihat di client) |
| **Waktu Kedaluwarsa** | Tetap tersimpan hingga batas waktu `expire` habis | Otomatis terhapus saat browser ditutup / timeout |
| **Penggunaan Ideal** | Mengingat preferensi tema, bahasa, "Remember Me" | Mengelola status **Login Autentikasi** & Keranjang |

---

## 8.2. Pengelolaan Cookies di PHP

Cookie dibuat menggunakan fungsi `setcookie()` **sebelum ada output HTML apa pun dikirimkan**.

```php
<?php
// 1. Membuat Cookie (Nama, Nilai, Waktu Kadaluwarsa, Path, Domain, Secure, HttpOnly)
$nama_cookie = "user_theme";
$nilai_cookie = "dark_mode";
$kadaluwarsa = time() + (86400 * 30); // Berlaku selama 30 hari

setcookie($nama_cookie, $nilai_cookie, $kadaluwarsa, "/", "", false, true);

// 2. Membaca Cookie
if (isset($_COOKIE["user_theme"])) {
    echo "Tema Pilihan Pengguna: " . $_COOKIE["user_theme"];
} else {
    echo "Cookie tema belum dibuat.";
}

// 3. Menghapus Cookie (Set waktu expire ke masa lalu)
setcookie("user_theme", "", time() - 3600, "/");
?>
```

> [!TIP]
> Selalu aktifkan parameter **`HttpOnly = true`** pada `setcookie()` untuk mencegah skrip malicious JavaScript membaca cookie melalui serangan **XSS (Cross-Site Scripting)**.

---

## 8.3. Pengelolaan Sessions di PHP

Setiap berkas PHP yang ingin menggunakan session wajib memanggil `session_start()` di baris paling atas.

```php
<?php
// 1. Wajib Panggil session_start() di awal file
session_start();

// 2. Menyimpan Data ke dalam Session (Saat Login Berhasil)
$_SESSION["user_id"] = 101;
$_SESSION["username"] = "budi_santoso";
$_SESSION["role"] = "admin";
$_SESSION["is_logged_in"] = true;

// 3. Mencegah Session Hijacking (Regenerasi ID Session secara berkala)
session_regenerate_id(true);
?>
```

### Implementasi Pemeriksaan Halaman Terproteksi (Auth Guard):

```php
<?php
session_start();

// Cek apakah pengguna sudah login
if (!isset($_SESSION["is_logged_in"]) || $_SESSION["is_logged_in"] !== true) {
    // Alihkan ke halaman login jika belum tersertifikasi
    header("Location: login.php");
    exit;
}
?>
<h1>Selamat Datang di Dashboard Rahasia, <?php echo $_SESSION["username"]; ?>!</h1>
```

### Implementasi Logout:

```php
<?php
session_start();

// 1. Kosongkan variabel array session
$_SESSION = array();

// 2. Hapus Cookie Session ID jika ada
if (ini_get("session.use_cookies")) {
    $params = session_get_cookie_params();
    setcookie(session_name(), '', time() - 42000,
        $params["path"], $params["domain"],
        $params["secure"], $params["httponly"]
    );
}

// 3. Hancurkan session di server
session_destroy();

// 4. Redirect ke halaman login
header("Location: login.php");
exit;
?>
```

---

## 8.4. Keamanan Web Utama: CSRF dan XSS

1. **Cross-Site Scripting (XSS)**: Serangan di mana peretas memasukkan kode JavaScript berbahaya ke dalam situs web.
   - *Pencegahan*: Sanitasi seluruh output teks menggunakan `htmlspecialchars($input, ENT_QUOTES, 'UTF-8')`.
2. **Cross-Site Request Forgery (CSRF)**: Serangan yang memaksa pengguna yang sudah ter-autentikasi untuk mengirimkan permintaan tak diinginkan ke aplikasi web.
   - *Pencegahan*: Gunakan **CSRF Token** acak rahasia pada setiap form POST yang diverifikasi server saat form dikirimkan.

---

Navigasi: Modul Sebelumnya: [[07_Koneksi_Database_PHP_dan_SQL]] | [[Konsep_Pemrograman_Web]] | Modul Berikutnya: [[09_Framework_Laravel_dan_Arsitektur_MVC]]
