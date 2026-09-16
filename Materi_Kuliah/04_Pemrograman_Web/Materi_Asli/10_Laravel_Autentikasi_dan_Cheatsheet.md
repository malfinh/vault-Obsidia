# 10. Laravel Autentikasi, Middleware, dan Artisan Cheatsheet

Navigasi: Modul Sebelumnya: [[09_Framework_Laravel_dan_Arsitektur_MVC]] | [[Konsep_Pemrograman_Web]]

---

## 10.1. Autentikasi dan Auth Scaffolding di Laravel

Laravel menyediakan paket perancah autentikasi bawaan yang siap pakai (seperti **Laravel Breeze** atau **Laravel Jetstream**) untuk menangani pendaftaran pengguna (*registration*), login, verifikasi email, dan pemulihan kata sandi.

### 🚀 Cara Penginstalan Auth Scaffolding (Breeze):
```bash
# 1. Install Laravel Breeze via Composer
composer require laravel/breeze --dev

# 2. Install Perancah Breeze (Blade / Vue / React)
php artisan breeze:install blade

# 3. Jalankan Migrasi Tabel User
php artisan migrate

# 4. Install & Build Asset Frontend
npm install
npm run dev
```

---

## 10.2. Middleware di Laravel

**Middleware** bertindak sebagai penyaring (*filter*) HTTP yang mengevaluasi permintaan yang masuk sebelum mencapai Controller.

- *Penggunaan Populer*:
  - **`auth`**: Memastikan pengguna sudah login sebelum dapat mengakses rute terproteksi (seperti `/dashboard`).
  - **`guest`**: Memastikan pengguna yang sudah login tidak dapat mengakses halaman login lagi.

```php
use App\Http\Controllers\DashboardController;

// Rute yang Hanya Boleh Diakses Pengguna Terautentikasi (Sudah Login)
Route::middleware(['auth'])->group(function () {
    Route::get('/dashboard', [DashboardController::class, 'index'])->name('dashboard');
    Route::get('/profile', [DashboardController::class, 'profile'])->name('profile');
});
```

---

## 10.3. Panduan Ringkas Command Artisan (Artisan CLI Cheatsheet)

Artisan adalah antarmuka baris perintah (*command-line interface*) bawaan Laravel.

### 🛠️ Perintah Umum & Pengelolaan Aplikasi:
```bash
# Jalankan Local Server Laravel (Standard Port 8000)
php artisan serve

# Cek Versi Framework Laravel
php artisan --version

# Aktifkan Mode Perbaikan (Under Maintenance)
php artisan down

# Mengaktifkan Kembali Aplikasi (Online)
php artisan up

# Bersihkan Cache Aplikasi & Konfigurasi
php artisan optimize:clear
```

### 📦 Perintah Pembuatan Komponen (Generator Commands):
```bash
# Membuat Model sekaligus Migration, Controller, dan Resource
php artisan make:model Produk -mcr

# Membuat Controller Terpisah
php artisan make:controller ProdukController

# Membuat Migration Terpisah
php artisan make:migration create_produks_table

# Membuat Middleware Baru
php artisan make:middleware CheckRole

# Membuat Seeder Data Dummy
php artisan make:seeder MahasiswaSeeder
```

### 🗄️ Perintah Database & Migrasi:
```bash
# Jalankan Seluruh Migration Baru
php artisan migrate

# Rollback (Batalkan) Migration Terakhir
php artisan migrate:rollback

# Reset dan Ulangi Seluruh Migration dari Awal
php artisan migrate:fresh

# Jalankan Seeder untuk Mengisi Data Dummy
php artisan db:seed
```

---

Navigasi: Modul Sebelumnya: [[09_Framework_Laravel_dan_Arsitektur_MVC]] | [[Konsep_Pemrograman_Web]]
