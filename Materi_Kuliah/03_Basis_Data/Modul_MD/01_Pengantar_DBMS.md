# 1. Konsep Dasar DBMS (Database Management System)

Navigasi: [[Konsep_Basis_Data]] | Modul Berikutnya: [[02_Arsitektur_DBMS]]

---

**Database Management System (DBMS)** adalah perangkat lunak yang dirancang khusus untuk mendefinisikan, menyimpan, mengelola, dan memanipulasi data di dalam sebuah basis data secara terorganisir, aman, dan efisien.

---

## 1.1. Data, Informasi, dan Pengetahuan

Dalam sistem informasi, terdapat perbedaan mendasar antara data, informasi, dan pengetahuan (*knowledge*):

- **Data**: Fakta mentah yang terekam namun belum memiliki arti khusus bagi pengguna.
  - *Contoh*: Angka `50000`, teks `"LUNAS"`.
- **Informasi**: Data yang telah diolah, diberi konteks, dan memiliki makna bagi penerima.
  - *Contoh*: `"Pesanan ID 102 senilai Rp50.000 berstatus LUNAS"`.
- **Pengetahuan (Knowledge)**: Pemahaman atau wawasan yang didapatkan dari analisis pola informasi untuk pengambilan keputusan.
  - *Contoh*: `"80% transaksi bernilai di atas Rp50.000 terjadi pada hari akhir pekan, sehingga stok barang harus ditambah setiap Sabtu"`.

---

## 1.2. Klasifikasi Tipe Data

Data yang disimpan dalam basis data umumnya terbagi menjadi tiga kategori:

| Tipe Data | Deskripsi | Contoh |
| :--- | :--- | :--- |
| **Structured Data** | Data yang memiliki format dan skema yang sangat kaku (baris dan kolom). | Tabel SQL, Spreadsheet Excel. |
| **Semi-Structured Data** | Data yang tidak memiliki skema tabel kaku, tetapi memiliki tag atau penanda struktur internal. | Berkas JSON, XML, CSV. |
| **Unstructured Data** | Data mentah tanpa bentuk terstruktur tunggal, biasanya diolah oleh sistem pencarian informasi. | Dokumen PDF, Gambar, Audio, Video. |

---

## 1.3. Fungsi Utama DBMS

Sebuah DBMS modern menyediakan tiga fungsi inti:

1. **Defining (Mendefinisikan)**: Menentukan tipe data, struktur tabel, dan batasan (*constraints*) yang harus dipenuhi oleh data.
2. **Constructing (Membangun)**: Menyimpan data fisik ke dalam media penyimpanan eksternal yang dikelola oleh DBMS.
3. **Manipulating (Memanipulasi)**: Memproses data melalui kueri (*query*) seperti pencarian data, pembaruan, serta pembuatan laporan.

```sql
-- Contoh Manipulasi Data Sederhana pada DBMS (SQL DML)
SELECT Nama, Email, Status 
FROM Pengguna 
WHERE Status = 'AKTIF';
```

---

## 1.4. Aktor Pengguna Basis Data

Pengguna sistem basis data dikategorikan berdasarkan peran dan tanggung jawabnya:

- **Database Administrator (DBA)**: Mengelola keamanan sistem, memberikan hak akses pengguna, memantau kinerja DBMS, serta melakukan cadangan (*backup*) dan pemulihan data (*recovery*).
- **Database Designer**: Merancang skema konseptual basis data, menentukan entitas, relasi, dan aturan integritas sebelum tabel dibuat.
- **System Analyst & Programmer**: Membangun aplikasi yang terhubung ke DBMS menggunakan kueri SQL.
- **End-User (Pengguna Akhir)**: Pengguna yang mengakses data melalui antarmuka aplikasi untuk keperluan operasional harian (misal: kasir, staf administrasi).

---

## 1.5. Keuntungan dan Kapan DBMS Tidak Diperlukan

### Keuntungan Menggunakan DBMS:
- **Mengurangi Redundansi Data**: Mencegah duplikasi data yang sama di banyak tempat sehingga menghemat memori.
- **Pengendalian Akses Bersama (*Concurrency Control*)**: Memungkinkan banyak pengguna mengakses data secara serentak tanpa saling merusak.
- **Integritas Data Terjamin**: Menerapkan aturan secara otomatis (misal: NIM harus unik, harga tidak boleh bernilai negatif).
- **Pemulihan Otomatis (*Backup & Recovery*)**: Menyediakan mekanisme pemulihan data saat terjadi kesalahan sistem (*crash*).

### Kapan DBMS Tidak Diperlukan?
DBMS tidak selalu harus digunakan jika:
1. Aplikasi sangat sederhana, data berukuran kecil, dan tidak pernah mengalami pembaruan.
2. Diperlukan performa real-time mikrodetik yang tidak toleran terhadap *overhead* pemrosesan transaksi pada DBMS.
3. Data hanya diakses oleh satu pengguna tunggal tanpa kebutuhan berbagi (*sharing*).

---

Navigasi: [[Konsep_Basis_Data]] | Modul Berikutnya: [[02_Arsitektur_DBMS]]
