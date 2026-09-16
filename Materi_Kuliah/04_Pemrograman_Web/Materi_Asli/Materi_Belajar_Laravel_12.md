# Panduan Lengkap Laravel 12: CRUD, REST API, dan Autentikasi

Dokumen ini adalah rangkuman materi belajar Laravel 12 yang disusun dengan bahasa yang mudah dipahami. Materi ini mencakup tiga pondasi utama dalam pengembangan aplikasi web menggunakan Laravel: **CRUD (Create, Read, Update, Delete)**, **RESTful API**, dan **Sistem Autentikasi (Login, Register, Logout)**.

---

## Bagian 1: Membangun CRUD di Laravel 12

CRUD adalah operasi dasar dalam pengelolaan data. Pada bagian ini, kita belajar bagaimana menampilkan data, menambah data, melihat detail, mengubah, dan menghapus data dari *database*.

### 1. Persiapan dan Konfigurasi
*   **Installasi:** Pastikan Composer sudah terinstall, lalu jalankan `composer create-project laravel/laravel nama-project` untuk menginstal Laravel 12.
*   **Konfigurasi Database:** Buka file `.env` dan atur koneksi *database* (seperti `DB_DATABASE`, `DB_USERNAME`, `DB_PASSWORD`).
*   **File System:** Jika ada fitur *upload* gambar, jalankan perintah `php artisan storage:link` agar folder penyimpanan gambar bisa diakses oleh publik.

### 2. Model dan Migration
*   **Migration:** Digunakan untuk merancang struktur tabel di *database* (misalnya tabel `posts` dengan kolom `title`, `content`, dan `image`).
*   **Model:** Merepresentasikan tabel tersebut di dalam kode Laravel. Kita bisa mengatur kolom mana saja yang boleh diisi (*Mass Assignment*) menggunakan properti `$fillable`.
*   Jalankan perintah: `php artisan make:model Post -m` untuk membuat Model beserta file Migration-nya sekaligus. Setelah kolom diatur, jalankan `php artisan migrate`.

### 3. Routing, Controller, dan Views
*   **Controller:** Dibuat dengan `php artisan make:controller PostController`. Di sinilah semua logika CRUD diletakkan:
    *   `index()`: Mengambil data dari database dan menampilkannya ke View.
    *   `create()` & `store()`: Menampilkan form tambah data dan memproses penyimpanannya ke database (termasuk *upload* gambar).
    *   `show()`: Menampilkan detail spesifik satu data berdasarkan ID.
    *   `edit()` & `update()`: Menampilkan form edit dan memproses perubahan data.
    *   `destroy()`: Menghapus data dan gambar terkait dari server.
*   **Views:** Menggunakan *Blade Templating* bawaan Laravel (file `.blade.php`) untuk mendesain tampilan form dan tabel antarmuka yang akan dilihat oleh pengguna.

---

## Bagian 2: Membangun RESTful API di Laravel 12

RESTful API memungkinkan aplikasi Laravel kita berkomunikasi dengan aplikasi lain (seperti aplikasi *mobile* Android/iOS atau *frontend* seperti React/Vue) dengan menggunakan format data JSON.

### 1. Apa itu API Resource?
Di Laravel, **API Resource** digunakan untuk mengubah (*transform*) data dari Model (atau *database*) ke dalam format JSON dengan bentuk yang seragam dan mudah dibaca.
Buat resource dengan perintah: `php artisan make:resource PostResource`.

### 2. Routing dan Controller API
*   **Routing:** Alamat rute (URL) untuk API didaftarkan di dalam file khusus yaitu `routes/api.php`. URL-nya otomatis akan diawali dengan `/api/`.
*   **Controller API:** Logika Controller untuk API mirip dengan CRUD biasa, namun nilai kembaliannya (*return*) bukan berupa View (HTML), melainkan JSON menggunakan *Resource* yang sudah kita buat.
    *   Menampilkan data menggunakan format JSON.
    *   Menyimpan data dengan merespon kode *status HTTP* (misal: `201 Created` atau `200 OK`).
    *   *Update* dan *Delete* data dengan mengembalikan status *response* JSON sukses atau gagal.

### 3. Pengujian (Testing) API
Proses API tidak dites di browser web biasa. Kita menggunakan *tools* seperti **Postman** atau **Insomnia** untuk melakukan pengujian *request* (GET, POST, PUT, DELETE) ke *endpoint* API yang sudah dibuat.

---

## Bagian 3: Autentikasi (Login, Register, Logout)

Untuk melindungi data agar tidak sembarangan diakses atau dimanipulasi orang lain, kita membutuhkan sistem autentikasi.

### 1. Konsep Dasar Autentikasi
Laravel telah menyediakan fitur bawaan untuk mengelola proses pendaftaran akun (*Register*), masuk (*Login*), dan keluar (*Logout*). Saat pengguna mendaftar, *password* mereka akan dienkripsi secara aman menggunakan *Bcrypt*.

### 2. Langkah Pembuatan
*   **Register:** 
    *   Membuat tampilan form pendaftaran.
    *   Membuat Controller yang menerima input (nama, email, *password*).
    *   Melakukan validasi (email tidak boleh kembar, panjang *password* minimal).
    *   Menyimpan data *User* ke database.
*   **Login:**
    *   Membuat tampilan form login.
    *   Mencocokkan *email* dan *password* menggunakan perintah bawaan `Auth::attempt()`.
    *   Jika cocok, *user session* dibuat dan diarahkan ke halaman *Dashboard*.
*   **Logout:**
    *   Menghapus sesi (*session*) pengguna yang sedang aktif menggunakan perintah `Auth::logout()` agar pengguna keluar dari sistem.

### 3. Melindungi Rute (Middleware)
Agar operasi CRUD hanya bisa dilakukan oleh orang yang sudah berhasil login, kita menggunakan **Middleware `auth`**.
Jika rute dibungkus oleh `auth`, maka siapapun yang mencoba mengakses URL CRUD tanpa login akan otomatis ditendang (*redirect*) kembali ke halaman Login.

---
*Materi ini dirangkum dari tutorial komprehensif SantriKoding (CRUD dan RESTful API) serta implementasi Autentikasi dasar Laravel.*
