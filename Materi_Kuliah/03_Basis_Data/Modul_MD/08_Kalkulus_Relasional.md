# 8. Kalkulus Relasional (Relational Calculus)

Navigasi: Modul Sebelumnya: [[07_Aljabar_Relasional]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[09_SQL_DDL_dan_DML]]

---

Berbeda dengan Aljabar Relasional yang bersifat **Prosedural** (menentukan *bagaimana* data diambil langkah demi langkah), **Kalkulus Relasional** bersifat **Deklaratif / Non-Prosedural** (menentukan *apa* data yang diinginkan tanpa menentukan alur prosedurnya).

 BAHASA SQL modern dirancang berdasarkan prinsip **Kalkulus Relasional**.

---

## 8.1. Tuple Relational Calculus (TRC)

Dalam **Tuple Relational Calculus (TRC)**, variabel kalkulus merepresentasikan **seluruh baris/tupel** dari sebuah relasi.

### Bentuk Umum Ekspresi TRC:
$$\{ t \mid P(t) \}$$
*Artinya: Himpunan seluruh tupel $t$ sedemikian rupa sehingga predikat $P(t)$ bernilai TRUE.*

- **Contoh**: Mencari seluruh data karyawan yang memiliki gaji lebih dari Rp5.000.000:
$$\{ t \mid t \in \text{Karyawan} \land t.\text{Gaji} > 5000000 \}$$

---

## 8.2. Domain Relational Calculus (DRC)

Dalam **Domain Relational Calculus (DRC)**, variabel kalkulus merepresentasikan **domain nilai individu dari kolom/atribut**, bukan seluruh baris.

### Bentuk Umum Ekspresi DRC:
$$\{ \langle x_1, x_2, \dots, x_n \rangle \mid P(x_1, x_2, \dots, x_n) \}$$

- **Contoh**: Mencari `Nama` dan `Gaji` dari karyawan yang bekerja di Departemen `D01`:
$$\{ \langle \text{nama}, \text{gaji} \rangle \mid \exists \text{nip}, \text{dept} (\langle \text{nip}, \text{nama}, \text{gaji}, \text{dept} \rangle \in \text{Karyawan} \land \text{dept} = \text{'D01'}) \}$$

---

## 8.3. Kuantifier Logika ($\exists$ dan $\forall$)

Kalkulus Relasional menggunakan dua kuantifier logika predikat:

1. **Existential Quantifier ($\exists$)**: 
   - Menyatakan "Ada setidaknya satu..." (*There Exists*).
   - Pada SQL diimplementasikan melalui klausa `EXISTS` atau Subquery.
2. **Universal Quantifier ($\forall$)**: 
   - Menyatakan "Untuk semua..." (*For All*).
   - Pada SQL diimplementasikan menggunakan kombinasi klausa `NOT EXISTS` bertingkat.

---

## 8.4. Ekspresi Aman (Safe Expressions)

Sebuah ekspresi kalkulus dikatakan **Safe Expression** jika menjamin bahwa hasil kueri yang dikembalikan bersifat **Terhingga (*Finite*)**.

- *Contoh Ekspresi Tidak Aman (Unsafe)*: $\{ t \mid \neg(t \in \text{Karyawan}) \}$.
- *Penjelasan*: Kueri ini meminta seluruh data di dunia yang *bukan* karyawan, sehingga menghasilkan jumlah data tak terhingga.

---

Navigasi: Modul Sebelumnya: [[07_Aljabar_Relasional]] | [[Konsep_Basis_Data]] | Modul Berikutnya: [[09_SQL_DDL_dan_DML]]
