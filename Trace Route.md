# LAPORAN KONSEP JARINGAN

## ANALISIS CARA KERJA TRACEROUTE DAN MEKANISME TTL

**Nama:** Muhammad Fikri Nuruddin Hanif\
**NRP:** 3125600042\
**Dosen Pengampu:** Dr. Ferry Astika Saputra, ST, M.Sc

**Program Studi D4 Teknik Informatika**\
**Politeknik Elektronika Negeri Surabaya**\
**Tahun 2026**

------------------------------------------------------------------------

# A. Tujuan Praktikum

-   Memahami mekanisme kerja perintah traceroute (`tracert`) dalam
    memetakan rute jaringan.
-   Membuktikan peran bidang Time-to-Live (TTL) pada header IPv4 dalam
    mencegah *infinite routing loop*.
-   Mengidentifikasi jenis-jenis pesan Internet Control Message Protocol
    (ICMP) yang dihasilkan selama proses penelusuran paket.
-   Mengamati adaptasi pelacakan rute pada skenario jalur langsung
    (*direct path*) dan jalur memutar (*indirect/redirected path*).

# B. Topologi dan Alokasi Pengalamatan IP

Topologi menggunakan konfigurasi segitiga dengan 3 unit Router Cisco dan
2 unit PC.

  ------------------------------------------------------------------------------
  Perangkat      Port           Alamat IP       Subnet Mask       Keterangan
                                                                  Jalur
  -------------- -------------- --------------- ----------------- --------------
  PC0            Fa0            192.168.10.10   255.255.255.0     Gateway:
                                                                  192.168.10.1

  PC1            Fa0            192.168.20.10   255.255.255.0     Gateway:
                                                                  192.168.20.1

  Router0        Fa0/0          192.168.10.1    255.255.255.0     LAN PC0

  Router0        Gig2/0         10.10.20.2      255.255.255.252   Link Router0 ↔
                                                                  Router Atas

  Router0        Gig3/0         10.10.10.2      255.255.255.252   Link Bawah
                                                                  Router0 ↔
                                                                  Router2

  Router Atas    Gig2/0         10.10.20.1      255.255.255.252   Link Router
                                                                  Atas ↔ Router0

  Router Atas    Gig3/0         10.10.30.1      255.255.255.252   Link Router
                                                                  Atas ↔ Router2

  Router2        Fa0/0          192.168.20.1    255.255.255.0     LAN PC1

  Router2        Gig2/0         10.10.30.2      255.255.255.252   Link Router2 ↔
                                                                  Router Atas

  Router2        Gig3/0         10.10.10.1      255.255.255.252   Link Bawah
                                                                  Router2 ↔
                                                                  Router0
  ------------------------------------------------------------------------------

# C. Hasil Pengujian Traceroute

## 1. Skenario Jalur Langsung Bawah (3 Hops)

Pengujian pertama dilakukan dengan melewatkan paket langsung melalui
link bawah (Router0 ↔ Router2).

### Hop 1 --- 192.168.10.1

PC0 mengirim paket dengan TTL = 1. Sampai di Router0, nilainya dipotong
menjadi 0 sehingga paket di-drop dan Router0 membalas dengan ICMP Time
Exceeded (Type 11).

### Hop 2 --- 10.10.10.1

PC0 mengirim lagi dengan TTL = 2. Paket lolos dari Router0 (TTL = 1),
lalu habis menjadi 0 di Router2. Router2 meng-drop paket dan membalas
dengan ICMP Time Exceeded (Type 11).

### Hop 3 --- 192.168.20.10

PC0 mengirim paket dengan TTL = 3. Paket sukses tembus sampai tujuan
(PC1) sebelum TTL habis, lalu PC1 membalas dengan ICMP Echo Reply (Type
0) yang menandakan pelacakan rute selesai (*Trace complete*).

## 2. Skenario Jalur Memutar Atas (4 Hops)

Pengujian kedua dilakukan dengan mengubah rute statis agar lalu lintas
data melintasi Router Atas terlebih dahulu.

Pada pengujian awal skenario memutar, hop 4 mengalami *Request timed
out* karena rute balik pada Router2 masih terdapat entri ganda (*dual
route*), sehingga paket balasan dari PC1 belum dapat kembali ke PC0
secara sempurna.

Setelah rute balik di Router2 diperbaiki, pelacakan rute memutar
berhasil memetakan seluruh simpul secara utuh.

### Hop 1 --- 192.168.10.1

Gateway Router0 membalas dengan ICMP Time Exceeded saat TTL = 1.

### Hop 2 --- 10.10.20.1

Router Atas membalas dengan ICMP Time Exceeded saat paket TTL = 2 habis
pada antarmukanya.

### Hop 3 --- 10.10.30.2

Router2 membalas dengan ICMP Time Exceeded saat paket TTL = 3 melintasi
Router0, Router Atas, dan habis di Router2.

### Hop 4 --- 192.168.20.10

Paket TTL = 4 berhasil menembus ketiga router perantara dan diterima
oleh host PC1, ditutup dengan ICMP Echo Reply (*Trace complete*).

# D. Analisis dan Pembahasan

### Manipulasi TTL Hop-by-Hop

Traceroute memanfaatkan kenaikan nilai TTL secara bertahap (1, 2, 3,
...) untuk memaksa setiap router perantara membuang paket dan melaporkan
keberadaannya ke pengirim melalui pesan ICMP Time Exceeded (Type 11).

### Pembedaan Peran Simpul

Terdapat perbedaan respons antara router transit dan perangkat tujuan
akhir. Simpul transit menghasilkan ICMP Type 11, sementara perangkat
akhir menghasilkan ICMP Echo Reply (Type 0).

### Representasi Jalur Fisik yang Akurat

Perubahan konfigurasi rute statis langsung tercermin pada hasil
`tracert`, dari 3 hop (jalur bawah) menjadi 4 hop (jalur memutar atas).
Hal ini membuktikan bahwa utilitas tersebut mampu membaca perubahan
topologi dan jalur lalu lintas data secara dinamis dan presisi.
