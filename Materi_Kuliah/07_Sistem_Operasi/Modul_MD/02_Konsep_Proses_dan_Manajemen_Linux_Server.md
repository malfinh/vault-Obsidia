# 2. Konsep Proses dan Manajemen Proses pada Linux Server

Navigasi: Modul Sebelumnya: [[01_Pengantar_dan_Struktur_Sistem_Komputer]] | [[Konsep_Sistem_Operasi]] | Modul Berikutnya: [[03_Thread_Multithreading_dan_Arsitektur_Server]]

---

**Proses** adalah program yang sedang berada dalam status dieksekusi (*program in execution*). Berbeda dengan program yang merupakan entitas pasif (berkas di disk), proses adalah entitas aktif yang memiliki register CPU, memori alamat, dan alokasi sumber daya.

---

## 2.1. Blok Kontrol Proses (Process Control Block - PCB)

Setiap proses diwakili oleh struktur data di dalam kernel yang disebut **PCB**:

```mermaid
graph TD
    PCB["Blok Kontrol Proses (PCB)"]
    PCB --> PID["PID (Process ID)"]
    PCB --> State["Process State (Running, Waiting, Ready)"]
    PCB --> PC["Program Counter (Alamat Instruksi Berikutnya)"]
    PCB --> Regs["Registers CPU"]
    PCB --> Mem["Alokasi Memori (Text, Data, Heap, Stack)"]
    PCB --> IO["Daftar File Open & Perangkat I/O"]
```

---

## 2.2. Siklus Hidup dan Status Proses (Process States)

```mermaid
stateDiagram-v2
    [*] --> New : Dibuat (fork)
    New --> Ready : Dipindahkan ke Antrean Memori
    Ready --> Running : Scheduler Dispatch
    Running --> Ready : Interrupt / Time Slice Habis
    Running --> Waiting : Meminta Input I/O / Event
    Waiting --> Ready : Event / I/O Selesai
    Running --> Terminated : Selesai (exit)
    Terminated --> [*]
```

---

## 2.3. Pembuatan Proses pada Sistem Unix/Linux (`fork()` dan `exec()`)

Pada Linux, proses baru dibuat melalui panggilan sistem (*System Call*):
1. **`fork()`**: Membuplikasi proses induk (*Parent*) untuk membuat proses anak (*Child*) yang identik.
2. **`exec()`**: Mengganti ruang alamat memori proses anak dengan program eksekusi baru.
3. **`wait()`**: Meminta proses induk menunggu hingga proses anak selesai dieksekusi.

```c
// Contoh C System Call fork() di Linux
#include <stdio.h>
#include <unistd.h>
#include <sys/types.h>

int main() {
    pid_t pid = fork();

    if (pid < 0) {
        printf("Gagal membuat proses anak!\n");
    } else if (pid == 0) {
        // Eksekusi pada Proses Anak
        printf("Proses Anak berjalan. PID: %d\n", getpid());
    } else {
        // Eksekusi pada Proses Induk
        printf("Proses Induk berjalan. PID Anak: %d\n", pid);
    }
    return 0;
}
```

---

## 2.4. Manajemen Proses Praktis pada Environment Linux Server

Saat mengelola server Linux, administrator server wajib memantau dan mengontrol proses yang sedang berjalan.

```mermaid
flowchart LR
    CLI["Perintah CLI Server"]
    CLI --> PS["ps aux : Menampilkan daftar seluruh proses aktif"]
    CLI --> TOP["top / htop : Monitoring penggunaan CPU & Memori real-time"]
    CLI --> PSTREE["pstree : Menampilkan pohon hirarki induk-anak proses"]
    CLI --> KILL["kill -9 <PID> : Menghentikan paksa proses bermasalah"]
```

### 🛠️ Perintah Utama Linux Server:

```bash
# 1. Menampilkan seluruh proses aktif beserta PID dan pemiliknya
ps aux | grep nginx

# 2. Monitoring penggunaan CPU dan RAM proses secara interaktif
htop

# 3. Menampilkan pohon hirarki hubungan Parent-Child proses
pstree -p

# 4. Membunuh paksa proses berdasarkan PID (Signal SIGKILL = 9)
kill -9 12345
```

---

## 2.5. Fenomena Zombie dan Orphan Process

- **Zombie Process**: Proses anak yang telah selesai dieksekusi (`exit`), namun entri statusnya masih ada di tabel PCB karena proses induk belum memanggil `wait()`.
- **Orphan Process**: Proses anak yang proses induknya mati terlebih dahulu. Di Linux, proses yatim piatu ini otomatis diangkat oleh **PID 1 (`systemd` / `init`)**.

> [!IMPORTANT]
> **Pentingnya PID 1 pada Server & Docker**: PID 1 bertanggung jawab memanen (*reap*) Zombie Processes. Jika PID 1 di server/container tidak berjalan dengan benar, entri Zombie akan menumpuk dan menghabiskan stok PID sistem.

---

Navigasi: Modul Sebelumnya: [[01_Pengantar_dan_Struktur_Sistem_Komputer]] | [[Konsep_Sistem_Operasi]] | Modul Berikutnya: [[03_Thread_Multithreading_dan_Arsitektur_Server]]
