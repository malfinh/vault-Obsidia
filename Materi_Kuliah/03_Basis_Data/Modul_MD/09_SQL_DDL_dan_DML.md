# 9. SQL Data Definition Language (DDL) dan Data Manipulation Language (DML)

Navigasi: Modul Sebelumnya: [[08_Kalkulus_Relasional]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[10_SQL_TCL_dan_Transaksi]]

---

**SQL (Structured Query Language)** adalah standar bahasa komputer deklaratif universal yang digunakan untuk mengelola data di dalam DBMS Relasional (seperti PostgreSQL, MySQL, SQL Server, dan Oracle).

---

## 9.1. SQL Data Definition Language (DDL)

DDL digunakan untuk mendefinisikan, mengubah, dan menghapus struktur fisik tabel, indeks, serta batasan integritas basis data.

```sql
-- 1. Membuat Tabel dengan Batasan (Constraints)
CREATE TABLE Mahasiswa (
    NIM VARCHAR(10) PRIMARY KEY,
    Nama VARCHAR(100) NOT NULL,
    Email VARCHAR(100) UNIQUE,
    IPK DECIMAL(3, 2) CHECK (IPK >= 0.00 AND IPK <= 4.00),
    Kode_Prodi INT,
    Status VARCHAR(20) DEFAULT 'AKTIF',
    FOREIGN KEY (Kode_Prodi) REFERENCES ProgramStudi(Kode_Prodi)
        ON DELETE CASCADE
);

-- 2. Mengubah Struktur Tabel (ALTER TABLE)
ALTER TABLE Mahasiswa ADD COLUMN No_HP VARCHAR(15);
ALTER TABLE Mahasiswa DROP COLUMN Status;

-- 3. Menghapus Tabel (DROP TABLE)
DROP TABLE Mahasiswa;
```

---

## 9.2. SQL Data Manipulation Language (DML)

DML digunakan untuk memasukkan, memperbarui, menghapus, dan mencari baris data di dalam tabel.

### A. Operasi Manipulasi Dasar (`INSERT`, `UPDATE`, `DELETE`)

```sql
-- Insert Data Baru
INSERT INTO Mahasiswa (NIM, Nama, Email, IPK, Kode_Prodi) 
VALUES ('M0520001', 'Budi Santoso', 'budi@mail.com', 3.75, 101);

-- Update Data
UPDATE Mahasiswa 
SET IPK = 3.85 
WHERE NIM = 'M0520001';

-- Delete Data
DELETE FROM Mahasiswa 
WHERE NIM = 'M0520001';
```

---

### B. Pemrosesan Kueri Pencarian (`SELECT`)

Urutan logika pemrosesan klausa `SELECT` di dalam DBMS:

1. **`FROM`**: Menentukan tabel sumber dan mengombinasikan baris (JOIN).
2. **`WHERE`**: Menyaring baris data individu berdasarkan kondisi predikat.
3. **`GROUP BY`**: Mengelompokkan baris data berdasarkan kolom tertentu.
4. **`HAVING`**: Menyaring hasil kelompok setelah operasi agregasi.
5. **`SELECT`**: Memilih kolom dan mengeksekusi fungsi agregasi.
6. **`ORDER BY`**: Mengurutkan baris hasil pencarian (*ASC / DESC*).
7. **`LIMIT / OFFSET`**: Membatasi jumlah baris data yang ditampilkan.

```sql
-- Contoh Query Kompleks
SELECT 
    p.Nama_Prodi,
    COUNT(m.NIM) AS Total_Mahasiswa,
    AVG(m.IPK) AS Rata_IPK
FROM Mahasiswa m
JOIN ProgramStudi p ON m.Kode_Prodi = p.Kode_Prodi
WHERE m.IPK > 2.00
GROUP BY p.Nama_Prodi
HAVING AVG(m.IPK) > 3.00
ORDER BY Rata_IPK DESC
LIMIT 5;
```

---

## 9.3. Jenis-Jenis JOIN pada SQL

| Jenis JOIN | Deskripsi Operasi |
| :--- | :--- |
| **`INNER JOIN`** | Menampilkan baris yang memiliki pasangan cocok di kedua tabel. |
| **`LEFT JOIN`** | Menampilkan seluruh baris dari tabel kiri + baris yang cocok dari tabel kanan. |
| **`RIGHT JOIN`** | Menampilkan seluruh baris dari tabel kanan + baris yang cocok dari tabel kiri. |
| **`FULL JOIN`** | Menampilkan seluruh baris dari kedua tabel. |

```sql
-- Contoh Inner Join
SELECT m.Nama, p.Nama_Prodi
FROM Mahasiswa m
INNER JOIN ProgramStudi p ON m.Kode_Prodi = p.Kode_Prodi;

-- Contoh Left Outer Join (Menampilkan mahasiswa meski belum memilih prodi)
SELECT m.Nama, p.Nama_Prodi
FROM Mahasiswa m
LEFT JOIN ProgramStudi p ON m.Kode_Prodi = p.Kode_Prodi;
```

---

Navigasi: Modul Sebelumnya: [[08_Kalkulus_Relasional]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[10_SQL_TCL_dan_Transaksi]]
