# Materi Kuliah: Arsitektur Komputer dan Sistem Operasi

Dokumen ini merupakan rangkuman berkesinambungan yang menjembatani konsep perangkat keras (Arsitektur Komputer) dengan perangkat lunak pengelolanya (Sistem Operasi). Materi disajikan dengan kedalaman yang pas—tidak terlalu dangkal, namun juga tidak terlalu rumit—sehingga cocok sebagai pegangan pemahaman fundamental.

---

## 1. Pengantar: Arsitektur Komputer vs Sistem Operasi
*   **Arsitektur Komputer:** Membahas atribut sistem yang tampak bagi seorang pemrogram, seperti set instruksi, mekanisme I/O, dan teknik pengalamatan memori. Ini adalah tentang *bagaimana komponen perangkat keras dirancang dan dihubungkan*.
*   **Sistem Operasi (OS):** Perangkat lunak sistem yang bertugas mengelola sumber daya perangkat keras dan menyediakan antarmuka bagi program aplikasi. OS adalah "manajer" yang memastikan CPU, Memori, dan I/O bekerja secara harmonis.

## 2. Struktur dan Fungsi CPU
CPU (*Central Processing Unit*) adalah otak dari komputer. CPU memiliki tiga komponen utama:
*   **Control Unit (CU):** Bertugas mengambil, mendekode, dan mengeksekusi instruksi. CU mengontrol aliran data antar komponen.
*   **ALU (Arithmetic Logic Unit):** Bagian yang melakukan semua operasi perhitungan matematis (tambah, kurang) dan operasi logika (AND, OR, NOT).
*   **Register:** Lokasi memori internal di dalam CPU yang sangat cepat, digunakan untuk menyimpan data dan instruksi yang sedang diproses saat itu juga.

## 3. Komunikasi Perangkat: BUS dan I/O
Komponen CPU, Memori, dan I/O tidak berdiri sendiri; mereka dihubungkan oleh jalur komunikasi yang disebut **System BUS**. Terdapat tiga jenis Bus utama:
1.  **Data Bus:** Jalur perpindahan data antar modul.
2.  **Address Bus:** Menentukan lokasi sumber atau tujuan data (memori atau modul I/O).
3.  **Control Bus:** Mengontrol akses dan penggunaan data serta jalur alamat (contoh: sinyal *read/write*).

**Input/Output (I/O):** Modul I/O bukan sekadar *port* colokan kabel, melainkan sebuah pengontrol (*controller*) yang menjembatani CPU dengan perangkat periferal (seperti *keyboard*, *mouse*, atau monitor). OS mengelola I/O ini menggunakan *driver* agar CPU tidak perlu tahu detail mekanis setiap perangkat keras.

## 4. Hierarki Memori dan Memori Eksternal
Memori disusun secara hierarki berdasarkan kecepatan, kapasitas, dan harga.
*   **Memori Internal (Utama):** Terdiri dari Register, Cache Memori, dan RAM (*Random Access Memory*). RAM adalah tempat OS dan aplikasi dimuat saat sedang berjalan. Semakin dekat memori ke CPU, semakin cepat namun kapasitasnya semakin kecil.
*   **Memori Eksternal (Sekunder):** Penyimpanan permanen seperti Hard Disk Drive (HDD), Solid State Drive (SSD), atau Flashdisk. Saat RAM penuh, OS menggunakan sebagian memori eksternal ini sebagai *Virtual Memory* untuk mencegah sistem *crash*.

## 5. Set Instruksi dan Mode Pengalamatan
CPU berkomunikasi menggunakan instruksi mesin (*Machine Instruction*). Sebuah instruksi terdiri dari dua bagian utama: **Opcode** (apa yang harus dilakukan) dan **Operand** (data yang diproses).

**Mode Pengalamatan (*Addressing Modes*):** Cara CPU mencari letak data (operand) tersebut. Contohnya:
*   *Immediate Addressing:* Data langsung ada di dalam instruksi.
*   *Direct Addressing:* Instruksi menyebutkan alamat memori tempat data berada secara spesifik.
*   *Indirect Addressing:* Instruksi menunjuk ke sebuah alamat, yang mana alamat tersebut menunjuk ke alamat lain tempat data sesungguhnya berada.

## 6. Filosofi Desain CPU: RISC vs CISC dan Superscalar
Seiring berkembangnya zaman, ada dua pendekatan dalam membuat set instruksi CPU:
*   **CISC (*Complex Instruction Set Computer*):** Memiliki instruksi yang sangat banyak dan kompleks. Satu instruksi bisa melakukan operasi rumit sekaligus. Contoh: Arsitektur x86 pada prosesor Intel dan AMD lawas.
*   **RISC (*Reduced Instruction Set Computer*):** Memiliki instruksi yang sedikit dan sederhana, namun dieksekusi dengan sangat cepat (biasanya 1 siklus per instruksi). Contoh: Arsitektur ARM (digunakan pada *smartphone* dan Apple Silicon M-Series).

**Superscalar:**
Ini adalah teknik di dalam CPU yang memungkinkannya mengeksekusi *lebih dari satu instruksi dalam satu siklus clock*. CPU memiliki beberapa jalur eksekusi (ALU) yang bekerja berbarengan, membuat pemrosesan jauh lebih cepat.

## 7. Parallel Processing (Pemrosesan Paralel)
Jika Superscalar bermain di level instruksi dalam satu CPU, *Parallel Processing* membagi tugas yang besar ke beberapa prosesor fisik sekaligus.
*   **Symmetric Multiprocessing (SMP):** Beberapa prosesor berbagi satu memori utama dan satu sistem operasi (contoh: prosesor *multi-core* pada laptop saat ini).
*   **Cluster:** Beberapa komputer utuh (node) digabungkan melalui jaringan berkecepatan tinggi untuk bekerja bersama menyelesaikan satu tugas besar (contoh: Superkomputer).
Sistem Operasi modern dirancang agar mampu membagi dan mendistribusikan *thread* aplikasi ke prosesor-prosesor ini secara seimbang (*load balancing*).