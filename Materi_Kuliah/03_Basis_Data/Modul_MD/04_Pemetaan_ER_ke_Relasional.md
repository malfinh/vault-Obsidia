# 4. Pemetaan Model ER/EER ke Model Relasional

Navigasi: Modul Sebelumnya: [[03_EER_Model]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[05_Model_Relasional_dan_Integritas]]

---

Perancangan basis data konseptual (ERD/EER) harus dikonversi menjadi skema relasional (tabel, kolom, dan kunci) sebelum diimplementasikan ke dalam DBMS.

---

## 4.1. Algoritma Pemetaan Pemodelan ER (7 Langkah)

### Langkah 1: Entitas Reguler (*Strong Entity*)
Untuk setiap entitas kuat $E$, buat sebuah tabel relasi $R$. Masukkan seluruh atribut simpel dari $E$. Pilih salah satu atribut unik sebagai **Primary Key (PK)**.

### Langkah 2: Entitas Lemah (*Weak Entity*)
Untuk setiap entitas lemah $W$, buat sebuah tabel $R$. Masukkan Primary Key dari entitas pemilik (*owner entity*) sebagai **Foreign Key (FK)** di $R$. Primary Key dari $R$ adalah gabungan dari FK Owner + Partial Key entitas lemah.

### Langkah 3: Relasi Biner 1:1
- Masukkan Primary Key salah satu entitas sebagai Foreign Key pada entitas yang memiliki partisipasi **Total** (wajib).
- *Contoh*: Relasi antara `Pegawai` dan `Departemen` (Pegawai Memimpin Departemen 1:1). PK Pegawai dimasukkan sebagai FK `NIP_Pimpinan` pada tabel `Departemen`.

### Langkah 4: Relasi Biner 1:N
Masukkan Primary Key dari entitas di sisi **"1"** sebagai **Foreign Key** ke dalam tabel di sisi **"N"**.
- *Contoh*: Satu `Departemen` memiliki banyak `Pegawai` (1:N). PK `Kode_Prodi` dari Departemen dimasukkan sebagai FK pada tabel `Pegawai`.

### Langkah 5: Relasi Biner M:N (*Many-to-Many*)
Wajib membuat tabel relasi baru (*Junction Table* / *Bridge Table*). Primary Key tabel baru tersebut merupakan **gabungan dari Primary Key kedua entitas yang berelasi**.

```sql
-- Pemetaan Relasi M:N antara Mahasiswa dan MataKuliah
CREATE TABLE KRS (
    NIM VARCHAR(10),
    Kode_MK VARCHAR(10),
    Nilai CHAR(2),
    PRIMARY KEY (NIM, Kode_MK),
    FOREIGN KEY (NIM) REFERENCES Mahasiswa(NIM),
    FOREIGN KEY (Kode_MK) REFERENCES MataKuliah(Kode_MK)
);
```

### Langkah 6: Atribut Multivalued
Buat tabel terpisah untuk atribut bernilai banyak (*multivalued*). Isi tabel baru tersebut dengan atribut tersebut ditambah **Foreign Key** dari Primary Key entitas induknya.

### Langkah 7: Relasi N-ary (Derajat > 2)
Buat tabel relasi baru $R$. Masukkan Primary Key dari seluruh entitas partisipan sebagai Foreign Key di $R$.

---

## 4.2. Pemetaan EER Specialization / Generalization (Langkah 8)

Terdapat **4 Opsi Strategi** untuk memetakan Subclass dan Superclass EER ke skema relasional:

| Opsi | Pendekatan Skema | Syarat & Kondisi Penggunaan | Kelebihan & Kekurangan |
| :--- | :--- | :--- | :--- |
| **8A** | **Tabel Superclass + Tabel Subclass Terpisah** | Berlaku untuk semua jenis batasan EER. |  Rapi; ❌ Membutuhkan `JOIN` untuk membaca data subclass lengkap. |
| **8B** | **Tabel Subclass Sahaja (Tanpa Tabel Superclass)** | Hanya untuk batasan **`d, Total`**. |  Query subclass tanpa `JOIN`; ❌ Tidak bisa menyimpan data superclass murni. |
| **8C** | **Satu Tabel Tunggal + Kolom `Tipe_Anggota`** | Hanya untuk batasan **Disjoint (`d`)**. |  Sangat cepat tanpa `JOIN`; ❌ Banyak kolom bernilai `NULL`. |
| **8D** | **Satu Tabel Tunggal + Flag Boolean (`Is_Dosen`, `Is_Staf`)** | Untuk batasan **Overlapping (`o`)**. |  Mendukung multiple role tanpa `JOIN`; ❌ Banyak kolom `NULL`. |

---

Navigasi: Modul Sebelumnya: [[03_EER_Model]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[05_Model_Relasional_dan_Integritas]]
