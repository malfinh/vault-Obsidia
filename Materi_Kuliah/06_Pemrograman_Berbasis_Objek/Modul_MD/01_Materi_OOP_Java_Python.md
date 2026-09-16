# Panduan Lengkap Pemrograman Berorientasi Objek (OOP): Java & Python

Navigasi: [[../Konsep_Pemrograman_Berbasis_Objek|Pemrograman Berbasis Objek (Hub Utama)]] | Prasyarat: [[../../01_Konsep_Pemrograman/Konsep_Pemrograman|Konsep Pemrograman]] | Penerapan: [[../../04_Pemrograman_Web/Modul_MD/06_PHP_Pemrograman_Berorientasi_Objek_OOP|PHP OOP]] & [[../../05_Pengembangan_Aplikasi_Bergerak/Modul_MD/01_Pengenalan_Kotlin_dan_OOP|Kotlin OOP]]

---



## 1. OOP Introduction (Pengenalan OOP)
OOP dirancang agar kode lebih terstruktur, mudah dikelola (*maintainable*), dan dapat digunakan kembali (*reusable*). Objek merupakan entitas yang memiliki:
*   **Data:** Disebut atribut atau variabel.
*   **Perilaku:** Disebut *method* atau fungsi.

## 2. Class & Object
*   **Class:** Cetak biru (*blueprint*) atau *template* untuk membuat objek.
*   **Object:** Wujud nyata (instansiasi) dari sebuah *Class*.

### ☕ Java:
```java
class Mobil {
    String merk; // Atribut
    
    void nyalakanMesin() { // Method
        System.out.println("Mesin menyala");
    }
}

public class Main {
    public static void main(String[] args) {
        Mobil mobilku = new Mobil(); // Membuat Object
        mobilku.merk = "Toyota";
        mobilku.nyalakanMesin();
    }
}
```

### 🐍 Python:
```python
class Mobil:
    def __init__(self):
        self.merk = "" # Atribut

    def nyalakan_mesin(self): # Method
        print("Mesin menyala")

# Membuat Object
mobilku = Mobil()
mobilku.merk = "Toyota"
mobilku.nyalakan_mesin()
```

## 3. Variabel (Java vs Python)
Dalam Java, variabel diklasifikasikan dengan ketat berdasarkan ruang lingkupnya (*scope*).
1.  **Local Variable:** Dideklarasikan di dalam *method*, hanya hidup di dalam *method* tersebut.
2.  **Instance Variable:** Dideklarasikan di dalam *class* (luar *method*). Tiap objek punya salinannya masing-masing. Di Python, ini setara dengan variabel yang menempel pada `self`.
3.  **Static/Class Variable:** Dimiliki oleh *class* itu sendiri, bukan oleh objek. Nilainya sama untuk semua objek.

### ☕ Java:
```java
class Akun {
    static String namaBank = "Bank Sentral"; // Static/Class variable
    int saldo; // Instance variable

    void setSaldo(int jumlah) {
        int bonus = 10; // Local variable
        this.saldo = jumlah + bonus;
    }
}
```

### 🐍 Python:
```python
class Akun:
    nama_bank = "Bank Sentral" # Class variable (mirip static)

    def __init__(self):
        self.saldo = 0 # Instance variable

    def set_saldo(self, jumlah):
        bonus = 10 # Local variable
        self.saldo = jumlah + bonus
```

## 4. Inheritance (Pewarisan)
Mekanisme di mana sebuah *Class* (Anak) mewarisi atribut dan *method* dari *Class* lain (Induk) untuk mencegah duplikasi kode.

### ☕ Java (menggunakan `extends`):
```java
class Hewan {
    void makan() { System.out.println("Sedang makan"); }
}

class Kucing extends Hewan {
    void meong() { System.out.println("Meong!"); }
}
```

### 🐍 Python:
```python
class Hewan:
    def makan(self):
        print("Sedang makan")

class Kucing(Hewan): # Pewarisan ditulis dalam kurung
    def meong(self):
        print("Meong!")
```

## 5. Encapsulation (Pengkapsulan)
Menyembunyikan detail data internal dari dunia luar untuk keamanan. Di Java menggunakan *Access Modifier* (`private`, `protected`, `public`) dan *Getter & Setter*. Di Python, variabel *private* ditandai dengan *underscore* ganda (`__`).

### ☕ Java:
```java
class Pengguna {
    private String password; // Data disembunyikan

    public void setPassword(String pass) {
        this.password = pass; // Bisa ditambahkan validasi
    }
}
```

### 🐍 Python:
```python
class Pengguna:
    def __init__(self):
        self.__password = None # Private variable

    def set_password(self, passw):
        self.__password = passw
```

## 6. Polymorphism (Polimorfisme)
Kemampuan sebuah *method* untuk memiliki perilaku berbeda.
*   **Overriding:** *Class* anak mengubah implementasi *method* dari *class* induk (Berlaku di Java & Python).
*   **Overloading:** Beberapa *method* dengan nama sama tapi parameter berbeda (Ada di Java, tidak didukung secara bawaan di Python).

### ☕ Java (Overriding):
```java
class Burung {
    void suara() { System.out.println("Cuit"); }
}
class Bebek extends Burung {
    @Override
    void suara() { System.out.println("Kwek kwek"); } // Implementasi ulang
}
```

## 7. Abstraction (Abstraksi)
Menyembunyikan detail implementasi rumit dan hanya menampilkan fitur-fitur penting. Di Java menggunakan `abstract class` atau `interface`. Di Python menggunakan modul `abc`.

### ☕ Java:
```java
abstract class Kendaraan {
    abstract void bergerak(); // Wajib diimplementasikan oleh class anak
}

class Motor extends Kendaraan {
    void bergerak() { System.out.println("Roda dua melaju"); }
}
```

### 🐍 Python:
```python
from abc import ABC, abstractmethod

class Kendaraan(ABC):
    @abstractmethod
    def bergerak(self):
        pass

class Motor(Kendaraan):
    def bergerak(self):
        print("Roda dua melaju")
```

## 8. String
String di Java dan Python sama-sama bersifat **Immutable** (tidak bisa diubah setelah diciptakan di memori).
*   **Java:** `String teks = "Halo";` (Gunakan `StringBuilder` untuk manipulasi berat).
*   **Python:** `teks = "Halo"` (Lebih fleksibel dengan *slicing* seperti `teks[0:2]`).

## 9. Generics & Collections
Koleksi data dinamis.
*   **Java:** Memiliki *Collections Framework* (`ArrayList`, `HashMap`) dan menggunakan **Generics** (`<T>`) untuk tipe data yang seragam.
*   **Python:** Struktur datanya (`list`, `dict`) bawaannya sudah dinamis dan bisa menampung berbagai tipe data sekaligus.

### ☕ Java:
```java
import java.util.ArrayList;

ArrayList<String> daftarNama = new ArrayList<>();
daftarNama.add("Budi");
```

### 🐍 Python:
```python
daftar_nama: list[str] = [] # Type hinting untuk meniru kedisiplinan Java
daftar_nama.append("Budi")
```

## 10. Exceptions & Assertions
Menangani eror eksekusi agar program tidak *crash*.

### ☕ Java:
```java
try {
    int hasil = 10 / 0;
} catch (ArithmeticException e) {
    System.out.println("Dibagi nol!");
} finally {
    System.out.println("Selalu dieksekusi");
}

// Assertion (aktif dengan flag -ea)
assert saldo > 0 : "Saldo tidak boleh minus";
```

### 🐍 Python:
```python
try:
    hasil = 10 / 0
except ZeroDivisionError:
    print("Dibagi nol!")
finally:
    print("Selalu dieksekusi")

# Assertion
assert saldo > 0, "Saldo tidak boleh minus"
```

## 11. Java IO (Input/Output)
Mekanisme membaca dan menulis data (*file*).

### ☕ Java:
```java
import java.io.FileWriter;
import java.io.IOException;

try (FileWriter writer = new FileWriter("data.txt")) {
    writer.write("Hello World");
} catch (IOException e) {
    e.printStackTrace();
}
```

### 🐍 Python:
```python
with open("data.txt", "w") as writer:
    writer.write("Hello World")
```

## 12. Multithreading
Menjalankan beberapa proses secara paralel. Java sangat andal dalam fitur ini.

### ☕ Java:
```java
class Tugas extends Thread {
    public void run() {
        System.out.println("Thread berjalan");
    }
}

public class Main {
    public static void main(String[] args) {
        Tugas t1 = new Tugas();
        t1.start();
    }
}
```

### 🐍 Python:
```python
import threading

def tugas():
    print("Thread berjalan")

t1 = threading.Thread(target=tugas)
t1.start()
```
