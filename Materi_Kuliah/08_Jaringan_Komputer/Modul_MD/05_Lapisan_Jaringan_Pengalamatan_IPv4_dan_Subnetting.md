# 5. Lapisan Jaringan, Pengalamatan IPv4, dan Subnetting VLSM

Navigasi: Modul Sebelumnya: [[04_Konversi_Sistem_Bilangan_Biner_Desimal_dan_Heksadesimal]] | [[Konsep_Jaringan_Komputer]] | Modul Berikutnya: [[06_Pengalamatan_IPv6_dan_Protokol_ICMP]]

---

## 5.1. Struktur Alamat IPv4

Alamat **IPv4** adalah alamat logis 32-bit yang dibagi menjadi 4 oktet (masing-masing 8-bit) yang dipisahkan oleh titik (*dotted decimal*). Setiap alamat IPv4 terdiri dari dua bagian: **Network ID** dan **Host ID**.

```text
Contoh Alamat IPv4 dengan Subnet Mask:
IP Address  : 192.168.1.10
Subnet Mask : 255.255.255.0 (/24 Notasi CIDR)

|--- Network Portion (24-bit) ---|--- Host Portion (8-bit) ---|
|         192.168.1.             |             10             |
```

---

## 5.2. Jenis Alamat Komunikasi IPv4

1. **Network Address**: Alamat pertama dalam subnet yang mengidentifikasi jaringan itu sendiri (misal: `192.168.1.0`). Tidak dapat dipakai oleh host.
2. **Host Address**: Alamat yang dialokasikan untuk perangkat ujung (misal: `192.168.1.1` s.d. `192.168.1.254`).
3. **Broadcast Address**: Alamat terakhir dalam subnet untuk mengirim data ke **seluruh host** di jaringan (misal: `192.168.1.255`).

### 🔒 Alamat IP Privat vs Publik (RFC 1918):
- **Rentang IP Privat (Tidak dirouting di Internet Publik)**:
  - Kelas A: `10.0.0.0` s.d. `10.255.255.255`
  - Kelas B: `172.16.0.0` s.d. `172.31.255.255`
  - Kelas C: `192.168.0.0` s.d. `192.168.255.255`

---

## 5.3. Teknik Subnetting IPv4 dan CIDR Notation

**Subnetting** adalah proses membagi satu blok jaringan IP besar menjadi beberapa jaringan kecil (*sub-networks*) untuk efisiensi alamat dan keamanan.

### 📐 Rumus Utama Subnetting:
- **Jumlah Subnet Dihasilkan**: $2^n$ (di mana $n$ = jumlah bit yang dipinjam dari Host portion).
- **Jumlah Host per Subnet**: $2^h - 2$ (di mana $h$ = jumlah bit host sisa; dikurangi 2 untuk Network ID & Broadcast ID).

### Contoh Penghitungan Subnetting `/26` (`255.255.255.192`):
- Notasi CIDR: `/26` (26 bit 1 dan 6 bit 0).
- Bit Host ($h$) = $32 - 26 = 6$ bit.
- Jumlah Host usable = $2^6 - 2 = 64 - 2 = \mathbf{62\text{ Host/Subnet}}$.
- Ukuran Blok Subnet = $256 - 192 = \mathbf{64}$.

```text
Tabel Subnet /26 dari 192.168.1.0:
Subnet 1: Network ID 192.168.1.0   | Host: 192.168.1.1 - 192.168.1.62   | Broadcast: 192.168.1.63
Subnet 2: Network ID 192.168.1.64  | Host: 192.168.1.65 - 192.168.1.126 | Broadcast: 192.168.1.127
Subnet 3: Network ID 192.168.1.128 | Host: 192.168.1.129 - 192.168.1.190| Broadcast: 192.168.1.191
Subnet 4: Network ID 192.168.1.192 | Host: 192.168.1.193 - 192.168.1.254| Broadcast: 192.168.1.255
```

---

Navigasi: Modul Sebelumnya: [[04_Konversi_Sistem_Bilangan_Biner_Desimal_dan_Heksadesimal]] | [[Konsep_Jaringan_Komputer]] | Modul Berikutnya: [[06_Pengalamatan_IPv6_dan_Protokol_ICMP]]
