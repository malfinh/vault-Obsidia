# 1. Dasar Pemrograman Komputer: Perbandingan Python dan Java

Navigasi: [[../Konsep_Pemrograman|Konsep Pemrograman (Hub Utama)]] | Modul Lanjutan: [[../../06_Pemrograman_Berbasis_Objek/Konsep_Pemrograman_Berbasis_Objek|Pemrograman Berorientasi Objek (PBO)]]

---

Pemrograman komputer adalah proses menulis instruksi yang dapat dieksekusi oleh komputer untuk menyelesaikan masalah tertentu. Modul ini membahas konsep dasar pemrograman menggunakan dua bahasa populer: **Python** (Interpreted, Dynamically Typed) dan **Java** (Compiled to Bytecode, Statically Typed).

---

## 1.1. Perbandingan Karakteristik Bahasa: Python vs Java

| Karakteristik | Python 🐍 | Java ☕ |
| :--- | :--- | :--- |
| **Eksekusi Kode** | Interpreted (Diinterpretasikan baris per baris) | Compiled ke Bytecode (JVM / Java Virtual Machine) |
| **Pengecekan Tipe**| Dynamically Typed (Tipe data otomatis terdeteksi) | Statically Typed (Tipe data wajib dideklarasikan) |
| **Sintaksis** | Sangat Ringkas & Bebas Tanda Kurung Kurawal `{}` | Terstruktur Ketat dengan Tanda Kurung Kurawal `{}` |
| **Titik Masuk** | Eksekusi langsung dari baris paling atas | Membutuhkan `public static void main(String[] args)` |

---

## 1.2. Variabel dan Tipe Data

```python
# 🐍 Python: Variabel tidak memerlukan deklarasi tipe eksplisit
nama = "Budi Santoso"     # str
umur = 20                 # int
ipk = 3.85                # float
is_aktif = True           # bool

print(f"Mahasiswa: {nama} | Umur: {umur} | IPK: {ipk}")
```

```java
// ☕ Java: Variabel wajib mendefinisikan tipe data
public class Main {
    public static void main(String[] args) {
        String nama = "Budi Santoso";
        int umur = 20;
        double ipk = 3.85;
        boolean isAktif = true;

        System.out.println("Mahasiswa: " + nama + " | Umur: " + umur + " | IPK: " + ipk);
    }
}
```

---

## 1.3. Struktur Kontrol Perkondisikan (Control Flow)

### A. Percabangan IF - ELSE

```python
# 🐍 Python (Menggunakan Indentasi Spasi & elif)
skor = 85

if skor >= 90:
    grade = 'A'
elif skor >= 80:
    grade = 'B'
else:
    grade = 'C'

print(f"Grade: {grade}")
```

```java
// ☕ Java (Menggunakan Kurung Kurawal & else if)
int skor = 85;
char grade;

if (skor >= 90) {
    grade = 'A';
} else if (skor >= 80) {
    grade = 'B';
} else {
    grade = 'C';
}

System.out.println("Grade: " + grade);
```

---

## 1.4. Struktur Perulangan (Looping)

### A. Perulangan FOR

```python
# 🐍 Python: Iterasi elemen dalam range / koleksi
for i in range(1, 6):
    print(f"Iterasi ke-{i}")
```

```java
// ☕ Java: Standard For Loop (inisialisasi; kondisi; increment)
for (int i = 1; i <= 5; i++) {
    System.out.println("Iterasi ke-" + i);
}
```

---

## 1.5. Fungsi / Method

Fungsi adalah blok kode terisolasi yang menerima input (*parameter*), melakukan pemrosesan, dan mengembalikan hasil (*return value*).

```python
# 🐍 Python: Deklarasi fungsi dengan kata kunci 'def'
def hitung_luas_persegi(sisi: float) -> float:
    return sisi * sisi

luas = hitung_luas_persegi(4.0)
print(f"Luas Persegi: {luas}")
```

```java
// ☕ Java: Method wajib menentukan tipe return & modifier akses
public class Kalkulator {
    public static double hitungLuasPersegi(double sisi) {
        return sisi * sisi;
    }

    public static void main(String[] args) {
        double luas = hitungLuasPersegi(4.0);
        System.out.println("Luas Persegi: " + luas);
    }
}
```

---

Navigasi: [[../Konsep_Pemrograman|Konsep Pemrograman (Hub Utama)]] | Modul Lanjutan: [[../../06_Pemrograman_Berbasis_Objek/Konsep_Pemrograman_Berbasis_Objek|Pemrograman Berorientasi Objek (PBO)]]
