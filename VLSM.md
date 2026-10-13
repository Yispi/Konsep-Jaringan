# LAPORAN RESMI KONSEP JARINGAN

# VLSM

**Dosen:**\
Dr Ferry Astika Saputra ST, M.Sc

**Penyusun:**\
Muhammad Fikri Nuruddin Hanif

**Program Studi D-IV Teknik Informatika**\
**Politeknik Elektronika Negeri Surabaya (PENS)**\
**Tahun 2026**

------------------------------------------------------------------------

# Pembagian Jaringan Menggunakan VLSM

Sebuah kampus memiliki alokasi jaringan `10.252.108.0/24` yang akan
dibagi untuk empat segmen dengan kebutuhan host sebagai berikut:

  Segmen                    Kebutuhan Host
  ----------------------- ----------------
  Laboratorium A                   90 host
  Laboratorium B                   60 host
  Administrasi                     14 host
  Tautan Point-to-Point         4 endpoint

## Yang harus ditentukan untuk setiap segmen

-   IP Network
-   Subnet Mask (prefix baru)
-   Host Pertama
-   Host Terakhir
-   Broadcast
-   Jumlah host yang tersedia
-   Sisa alokasi IP yang belum terpakai (jika ada)

## Hasil Pembagian VLSM

  ----------------------------------------------------------------------------------------
  Kebutuhan   Network          Host Range         Prefiks     Broadcast        Host
                                                                               Terpakai
  ----------- ---------------- ------------------ ----------- ---------------- -----------
  Lab A (90)  10.252.108.0     10.252.108.1 -     /25         10.252.108.127   1 - 90
                               10.252.108.126                                  

  Lab B (60)  10.252.108.128   10.252.108.129 -   /26         10.252.108.191   129 - 189
                               10.252.108.190                                  

  Admin (14)  10.252.108.192   10.252.108.193 -   /28         10.252.108.207   193 - 206
                               10.252.108.206                                  

  P2P (4)     10.252.108.208   10.252.108.209 -   /29         10.252.108.215   209 - 212
                               10.252.108.214                                  
  ----------------------------------------------------------------------------------------
