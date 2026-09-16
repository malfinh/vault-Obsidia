# 1. Pengenalan Bahasa Pemrograman Kotlin dan OOP

Navigasi: [[Konsep_Pengembangan_Aplikasi_Bergerak]] | Modul Berikutnya: [[02_Arsitektur_Sistem_Operasi_Mobile]]

---

**Kotlin** adalah bahasa pemrograman modern, terketik statis (*statically typed*), yang berjalan di atas Java Virtual Machine (JVM). Pada tahun 2017, Google secara resmi mengumumkan Kotlin sebagai bahasa utama (*first-class language*) untuk pengembangan aplikasi Android.

---

## 1.1. Mengapa Menggunakan Kotlin?

1. **Interoperabilitas 100% dengan Java**: Kode Kotlin dapat dipanggil dari Java dan sebaliknya tanpa hambatan.
2. **Ringkas (*Concise*)**: Mengurangi jumlah baris kode *boilerplate* hingga 40% dibandingkan Java.
3. **Null Safety**: Mencegah kesalahan klasik **`NullPointerException` (NPE)** secara otomatis di tingkat kompilasi.
4. **Fitur Modern**: Mendukung *Coroutines* untuk pemrograman asinkronus yang ringan, *Data Classes*, dan *Extension Functions*.

---

## 1.2. Sintaksis Dasar dan Variabel Kotlin

```kotlin
fun main() {
    // 1. Variabel Read-Only (Immutable) - Nilai tidak dapat diubah (Disarankan)
    val namaAplikasi: String = "My Mobile App"
    val versi: Double = 1.0

    // 2. Variabel Mutabel (Mutable) - Nilai dapat diperbarui
    var skor: Int = 100
    skor += 50

    // 3. String Template
    println("Aplikasi: $namaAplikasi | Versi: $versi | Skor: $skor")

    // 4. Struktur Kontrol (if-else sebagai Ekspresi)
    val status = if (skor >= 100) "Lulus" else "Tidak Lulus"
    println("Status: $status")
}
```

---

## 1.3. Fitur Null Safety di Kotlin

Secara *default*, variabel di Kotlin tidak boleh bernilai `null` kecuali diizinkan secara eksplisit menggunakan tanda tanya (`?`).

```kotlin
fun main() {
    // 1. Non-nullable Variable (Tidak boleh null)
    var nama: String = "Budi"
    // nama = null // Error Kompilasi!

    // 2. Nullable Variable (Boleh null)
    var email: String? = "budi@mail.com"
    email = null // Diizinkan

    // 3. Safe Call Operator (?.)
    val panjangEmail: Int? = email?.length // Bernilai null jika email null (tanpa crash NPE)

    // 4. Elvis Operator (?:) - Memberikan nilai default jika null
    val panjangPasti: Int = email?.length ?: 0
    println("Panjang Email: $panjangPasti") // Output: 0
}
```

---

## 1.4. Pemrograman Berorientasi Objek (OOP) di Kotlin

### A. Class dan Constructor

```kotlin
// Primary Constructor langsung di baris deklarasi kelas
class Mahasiswa(val nim: String, var nama: String, var ipk: Double) {

    // Method Kelas
    fun tampilkanProfil() {
        println("NIM: $nim | Nama: $nama | IPK: $ipk")
    }
}

fun main() {
    // Instansiasi Objek (Tanpa kata kunci 'new')
    val mhs1 = Mahasiswa("M0520001", "Siti Rahma", 3.85)
    mhs1.tampilkanProfil()
}
```

### B. Data Class (Otomatis Generate equals, hashCode, toString, copy)

```kotlin
// Data Class khusus untuk menampung data / DTO
data class User(val id: Int, val username: String, val role: String)

fun main() {
    val user1 = User(1, "budi123", "Admin")
    val user2 = user1.copy(username = "budi_baru") // Mengkloning dengan perubahan
    println(user1) // Output: User(id=1, username=budi123, role=Admin)
}
```

---

Navigasi: [[Konsep_Pengembangan_Aplikasi_Bergerak]] | Modul Berikutnya: [[02_Arsitektur_Sistem_Operasi_Mobile]]
