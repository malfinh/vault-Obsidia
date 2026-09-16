# 4. Konversi Sistem Bilangan: Biner, Desimal, dan Heksadesimal

Navigasi: Modul Sebelumnya: [[03_Lapisan_Fisik_dan_Data_Link_Ethernet]] | [[Konsep_Jaringan_Komputer]] | Modul Berikutnya: [[05_Lapisan_Jaringan_Pengalamatan_IPv4_dan_Subnetting]]

---

Pemahaman sistem bilangan sangat penting dalam jaringan komputer:
- **Desimal (Basis 10)**: Digunakan oleh manusia untuk membaca alamat IP.
- **Biner (Basis 2)**: Digunakan oleh komputer dan router untuk memproses alamat **IPv4** (32-bit).
- **Heksadesimal (Basis 16)**: Digunakan untuk menulis alamat **MAC** (48-bit) dan **IPv6** (128-bit).

---

## 4.1. Bobot Nilai Oktet Biner (8-bit)

Satu oktet alamat IPv4 terdiri dari 8 bit biner. Setiap posisi bit memiliki bobot nilai basis 2 ($2^n$):

| Posisi Bit | Bit 7 | Bit 6 | Bit 5 | Bit 4 | Bit 3 | Bit 2 | Bit 1 | Bit 0 |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Nilai Bobot ($2^n$)** | **128** | **64** | **32** | **16** | **8** | **4** | **2** | **1** |

---

## 4.2. Konversi Biner ke Desimal dan Desimal ke Biner

### 💡 Contoh 1: Konversi Biner `11000000` ke Desimal
$$128 \times 1 + 64 \times 1 + 32 \times 0 + 16 \times 0 + 8 \times 0 + 4 \times 0 + 2 \times 0 + 1 \times 0 = 128 + 64 = \mathbf{192}$$

### 💡 Contoh 2: Konversi Desimal `168` ke Biner
- $168 \ge 128 \rightarrow$ Bit 1 (Sisa: $168 - 128 = 40$)
- $40 < 64 \rightarrow$ Bit 0
- $40 \ge 32 \rightarrow$ Bit 1 (Sisa: $40 - 32 = 8$)
- $8 < 16 \rightarrow$ Bit 0
- $8 \ge 8 \rightarrow$ Bit 1 (Sisa: $8 - 8 = 0$)
- Hasil Biner: **`10101000`**

---

## 4.3. Sistem Bilangan Heksadesimal (Basis 16)

Heksadesimal menggunakan 16 digit: `0` hingga `9`, dan `A` hingga `F` (di mana `A=10, B=11, C=12, D=13, E=14, F=15`).

Setiap 1 digit Heksadesimal direpresentasikan oleh 4 bit biner (*nibble*):

| Heksadesimal | Biner (4-bit) | Desimal | Heksadesimal | Biner (4-bit) | Desimal |
| :---: | :---: | :---: | :---: | :---: | :---: |
| `0` | `0000` | 0 | `8` | `1000` | 8 |
| `1` | `0001` | 1 | `9` | `1001` | 9 |
| `2` | `0010` | 2 | `A` | `1010` | 10 |
| `3` | `0011` | 3 | `B` | `1011` | 11 |
| `4` | `0100` | 4 | `C` | `1100` | 12 |
| `5` | `0101` | 5 | `D` | `1101` | 13 |
| `6` | `0110` | 6 | `E` | `1110` | 14 |
| `7` | `0111` | 7 | `F` | `1111` | 15 |

---

Navigasi: Modul Sebelumnya: [[03_Lapisan_Fisik_dan_Data_Link_Ethernet]] | [[Konsep_Jaringan_Komputer]] | Modul Berikutnya: [[05_Lapisan_Jaringan_Pengalamatan_IPv4_dan_Subnetting]]
