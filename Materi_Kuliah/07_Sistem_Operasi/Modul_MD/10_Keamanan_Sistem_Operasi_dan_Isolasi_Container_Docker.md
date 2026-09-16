# 10. Keamanan Sistem Operasi dan Teknologi Isolasi Container Docker

Navigasi: Modul Sebelumnya: [[09_Sistem_Berkas_dan_Manajemen_I_O]] | [[Konsep_Sistem_Operasi]]

---

## 10.1. Keamanan Sistem Operasi: Otentikasi dan Hak Akses POSIX

Keamanan sistem operasi bertugas memastikan sumber daya sistem hanya dapat diakses oleh pengguna (*Users*) dan proses yang memiliki wewenang sah.

### 🛡️ Hak Akses File POSIX Linux (`chmod` & `chown`):
Setiap berkas di Linux memiliki 3 tingkatan hak akses: **Owner (u)**, **Group (g)**, dan **Others (o)** dengan izin **Read (4)**, **Write (2)**, dan **Execute (1)**.

```bash
# Menyetel hak akses file script.sh: Owner (rwx=7), Group (r-x=5), Others (r-x=5)
chmod 755 script.sh

# Mengubah pemilik file menjadi user 'www-data' dan group 'www-data'
chown www-data:www-data /var/www/html/index.php
```

---

## 10.2. Fondasi Sistem Operasi di Balik Teknologi Container (Docker)

Banyak pengembang mengira **Docker Container** adalah *Virtual Machine* (VM) yang ringan. Secara arsitektural, Container **bukanlah VM**, melainkan **proses biasa di Linux yang diisolasi** menggunakan dua fitur inti Kernel Linux: **Linux Namespaces** dan **Control Groups (cgroups)**.

```mermaid
graph TD
    subgraph Architecture ["Perbandingan VM vs Container Docker"]
        VM["Virtual Machine: Berjalan di atas Hypervisor + Membawa Guest OS Lengkap (Berat)"]
        Container["Docker Container: Berjalan langsung di atas Host Kernel OS via Namespaces & cgroups (Sangat Ringan)"]
    end
```

---

## 10.3. Isolasi Lingkungan dengan Linux Namespaces

**Linux Namespaces** membatasi apa yang **dapat dilihat (*visibility*)** oleh sebuah proses di dalam container:

```mermaid
flowchart LR
    Kernel["Host Linux Kernel"]
    Kernel --> PIDNS["1. PID Namespace (Container merasa dirinya PID 1 mandiri)"]
    Kernel --> NETNS["2. NET Namespace (Veth Network Card Virtual Sendiri)"]
    Kernel --> MNTNS["3. MNT Namespace (Mount Point File System Terisolasi)"]
    Kernel --> IPCNS["4. IPC Namespace (Isolasi Shared Memory)"]
    Kernel --> USERNS["5. USER Namespace (Mapping User Root Container -> Non-Root Host)"]
```

---

## 10.4. Pembatasan Alokasi Sumber Daya dengan Linux Control Groups (cgroups)

Jika Namespaces membatasi apa yang dapat *dilihat* oleh container, **cgroups** membatasi berapa banyak **sumber daya (*resource quota*)** yang **dapat dikonsumsi** oleh container.

```mermaid
flowchart TD
    HostHardware["Host Hardware (16 Core CPU, 32 GB RAM)"]
    cgroups["Linux Control Groups (cgroups)"]
    ContainerA["Container Web App (Batas: 1.5 Core CPU, 512 MB RAM)"]
    ContainerB["Container DB (Batas: 4 Core CPU, 4 GB RAM)"]

    HostHardware --> cgroups
    cgroups -->|docker run --cpus 1.5 --memory 512m| ContainerA
    cgroups -->|docker run --cpus 4 --memory 4g| ContainerB
```

### 🛠️ Perintah Praktis Docker Deployment:

```bash
# Menjalankan container Nginx dengan batasan kuota RAM 256MB dan 1 Core CPU
docker run -d \
  --name web-server \
  --memory="256m" \
  --cpus="1.0" \
  -p 80:80 \
  nginx:alpine
```

---

## 10.5. Praktik Terbaik Deployment Container di Server

1. **Penanganan PID 1 Init Process**: Aplikasi utama di dalam container yang berjalan sebagai PID 1 harus dapat menangani signal `SIGTERM` dan memanen *Zombie Processes*. Disarankan menggunakan init minimalis seperti `tini` (`docker run --init`).
2. **Pengelolaan Out-Of-Memory (OOM) Container**: Jika container melebihi batas `--memory`, OOM Killer kernel akan membunuh container tersebut tanpa mengganggu container lain di server host yang sama.
3. **Multi-Stage Builds**: Memisahkan stage kompilasi dari stage eksekusi akhir untuk menghasilkan image Docker berukuran sangat kecil dan aman.

---

Navigasi: Modul Sebelumnya: [[09_Sistem_Berkas_dan_Manajemen_I_O]] | [[Konsep_Sistem_Operasi]]
