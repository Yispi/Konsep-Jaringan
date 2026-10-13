# LAPORAN RESMI KONSEP JARINGAN

# VISUALISASI PYTHON

**Dosen:** Dr. Ferry Astika Saputra, ST, M.Sc

**Penyusun:** Muhammad Fikri Nuruddin Hanif

**Program Studi D-IV Teknik Informatika**\
**Politeknik Elektronika Negeri Surabaya (PENS)**\
**Tahun 2026**

------------------------------------------------------------------------

# A. Kerjakan Soal di Bawah Ini

Tentukan: - IP gateway - Host pertama - Host terakhir - Broadcast - IP
network

## Data IP

  ------------------------------------------------------------------------------------------------------------------
          No IP Address         Kelas     Network      Gateway      Host Pertama Host Terakhir     Broadcast Address
                                          Address                                                  
  ---------- -------------- ------------- ------------ ------------ ------------ ----------------- -----------------
           1 21.26.8.5            A       21.0.0.0     21.0.0.1     21.0.0.2     21.255.255.254    21.255.255.255

           2 212.6.8.3            C       212.6.8.0    212.6.8.1    212.6.8.2    212.6.8.254       212.6.8.255

           3 103.24.56.32         A       103.0.0.0    103.0.0.1    103.0.0.2    103.255.255.254   103.255.255.255

           4 1.1.1.1              A       1.0.0.0      1.0.0.1      1.0.0.2      1.255.255.254     1.255.255.255

           5 172.31.16.8          B       172.31.0.0   172.31.0.1   172.31.0.2   172.31.255.254    172.31.255.255
  ------------------------------------------------------------------------------------------------------------------

# B. Harmonisasi 1, 3, 5, 7, 9, 10

``` python
import numpy as np
import matplotlib.pyplot as plt

f = 1
harmonics = [1, 3, 5, 7, 9, 10]
t = np.linspace(0, 2, 2000)
cumulative_signal = np.zeros_like(t)

plt.figure(figsize=(10, 6))

for h in harmonics:
    component = (1 / h) * np.sin(2 * np.pi * h * f * t)
    cumulative_signal += component
    label = f"Harmonik 1 s/d {h}" if h != 10 else "Harmonik 1 s/d 10 (komponen genap)"
    plt.plot(t, cumulative_signal, label=label, alpha=0.8)

plt.title("Simulasi Pembentukan Sinyal Digital di Physical Layer (Fourier Series)")
plt.xlabel("Waktu (detik)")
plt.ylabel("Amplitudo")
plt.axhline(0, color="black", linestyle="--", linewidth=0.7)
plt.grid(True, linestyle=":", alpha=0.6)
plt.legend(loc="upper right")
plt.tight_layout()
plt.show()
```

# Analisis

## Harmonik 1 (Frekuensi Fundamental)

Menghasilkan gelombang sinus murni. Sinyal belum memiliki transisi biner
diskret yang jelas.

## Harmonik 3, 5, 7, dan 9

Bertahap meratakan puncak gelombang dan membuat lereng transisi
naik-turun menjadi curam, sehingga sinyal mendekati bentuk pulsa kotak
digital.

Hal ini membuktikan bahwa semakin lebar bandwidth kanal yang mampu
melewatkan spektrum harmonik tinggi, semakin presisi bentuk pulsa
digital yang diterima.

## Harmonik 10

Bentuk semakin kompleks karena terdapat komponen genap yang menghasilkan
riak asimetris pada sinyal.

## Relevansi terhadap Physical Layer

Visualisasi ini relevan dengan Physical Layer karena data digital pada
akhirnya harus direpresentasikan menjadi bentuk sinyal yang dapat
ditransmisikan melalui media fisik.

# Kesimpulan

Visualisasi menunjukkan bahwa sinyal digital berbentuk kotak dapat
didekati menggunakan penjumlahan beberapa sinyal sinusoidal melalui
deret Fourier.

Deret Fourier menunjukkan bahwa sebuah sinyal kompleks dapat
direpresentasikan sebagai gabungan beberapa gelombang sinus dengan
frekuensi berbeda.

Harmonik pertama menghasilkan gelombang sinus yang halus, sedangkan
penambahan harmonik ganjil 3, 5, 7, dan 9 membuat bagian HIGH dan LOW
semakin datar serta transisi semakin tajam.

Ripple dan overshoot yang terlihat di sekitar titik perubahan merupakan
karakteristik pendekatan Fourier yang terbatas dan berkaitan dengan
*Gibbs phenomenon*.

Penambahan harmonik ke-10 sebagai komponen genap membuat bentuk akhir
tidak lagi sama dengan pendekatan *square wave* simetris standar.
