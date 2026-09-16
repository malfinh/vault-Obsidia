# 5. Model Relasional, Kunci, dan Aturan Integritas

Navigasi: Modul Sebelumnya: [[04_Pemetaan_ER_ke_Relasional]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[06_Ketergantungan_Fungsional_dan_Normalisasi]]

---

Model Relasional merepresentasikan data dalam bentuk **Tabel Dua Dimensi** (Relasi) yang terdiri dari baris dan kolom.

---

## 5.1. Terminologi Model Relasional

| Istilah Formil | Istilah Praktis | Deskripsi |
| :--- | :--- | :--- |
| **Relation** | Tabel | Struktur data utama berisi baris dan kolom. |
| **Tuple** | Baris / Record | Satu baris entitas data individual. |
| **Attribute** | Kolom / Field | Karakteristik atau properti dari entitas. |
| **Domain** | Tipe Data & Batasan Nilai | Himpunan nilai *atomic* yang diizinkan untuk kolom tertentu. |
| **Degree** | Jumlah Kolom | Total banyaknya atribut dalam sebuah relasi. |
| **Cardinality** | Jumlah Baris | Total banyaknya baris/tupel aktif di dalam relasi. |

---

## 5.2. Hirarki Kunci Basis Data (Database Keys)

Kunci (*Keys*) digunakan untuk mengidentifikasi baris data secara unik dan menghubungkan antar tabel:

- **Super Key**: Satu atau lebih atribut yang secara bersama-sama mengidentifikasi secara unik sebuah baris (misal: `{NIM}`, `{NIM, Nama}`).
- **Candidate Key**: Super Key minimal yang tidak memiliki redundansi atribut (misal: `{NIM}` dan `{Email}`).
- **Primary Key (PK)**: Candidate Key yang dipilih secara resmi sebagai identitas utama tabel. **Primary Key TIDAK BOLEH bernilai `NULL`**.
- **Alternate Key**: Candidate Key yang tidak terpilih menjadi Primary Key.
- **Foreign Key (FK)**: Atribut pada suatu tabel yang nilainya merujuk (*referencing*) ke Primary Key pada tabel lain (*referenced table*).

---

## 5.3. Aturan Integritas Data (Integrity Constraints)

DBMS menegakkan empat aturan integritas utama untuk memastikan data konsisten:

1. **Domain Constraint**: Nilai dari setiap atribut harus bersifat *atomic* dan berada di dalam batasan tipe data domain yang terdefinisi.
2. **Key Constraint / Uniqueness**: Nilai Primary Key atau Candidate Key pada setiap baris harus unik.
3. **Entity Integrity Constraint**: Atribut yang merupakan **Primary Key TIDAK BOLEH bernilai `NULL`**.
4. **Referential Integrity Constraint**: Nilai Foreign Key pada tabel asal harus sesuai dengan salah satu nilai Primary Key pada tabel acuan, atau bernilai `NULL`.

---

## 5.4. Aksi Penanganan Pelanggaran Integritas Referensial

Saat terjadi operasi pembaruan atau penghapusan data (`UPDATE` / `DELETE`) pada tabel induk yang melanggar *Referential Integrity*, DBMS menyediakan opsi penanganan:

```sql
CREATE TABLE Pesanan (
    ID_Pesanan INT PRIMARY KEY,
    ID_Pelanggan INT,
    FOREIGN KEY (ID_Pelanggan) REFERENCES Pelanggan(ID_Pelanggan)
        ON DELETE CASCADE
        ON UPDATE SET NULL
);
```

- **`RESTRICT / REJECT`**: Menolak operasi penghapusan dan menampilkan pesan error (opsi standar).
- **`CASCADE`**: Otomatis meneruskan penghapusan ke seluruh baris anak yang merujuknya.
- **`SET NULL`**: Mengubah nilai Foreign Key di tabel anak menjadi `NULL`.
- **`SET DEFAULT`**: Mengubah nilai Foreign Key menjadi nilai standar (*default*).

---

Navigasi: Modul Sebelumnya: [[04_Pemetaan_ER_ke_Relasional]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[06_Ketergantungan_Fungsional_dan_Normalisasi]]
