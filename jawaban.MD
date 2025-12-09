## 1. Penjelasan Sistem Bilangan

Bagian ini menjelaskan empat jenis sistem bilangan dan cara konversinya. Anda bisa membagi tugas ini kepada satu atau dua anggota kelompok untuk dijelaskan di papan tulis atau slide.

### A. Empat Jenis Sistem Bilangan
Berdasarkan materi Sesi 1, berikut adalah penjelasannya:
1.  **Desimal (Basis 10):** Sistem yang kita gunakan sehari-hari. Terdiri dari 10 simbol angka (0-9).
2.  **Biner (Basis 2):** Sistem bilangan Biner hanya memiliki 2 simbol, yaitu 0 dan 1. Yang merupakan komponen dasar atau bahasa dasar komputer.
3.  **Oktal (Basis 8):** Sistem bilangan memiliki 8 simbol (0-7). Sering digunakan untuk menyingkat penulisan biner (3 bit biner = 1 digit oktal).
4.  **Hexadesimal (Basis 16):** Sistem bilangan dengan 16 simbol (0-9 dan A-F). Digunakan untuk representasi data yang lebih ringkas (4 bit biner = 1 digit hexa).

### B. Konversi Bilangan Desimal: 75
Berikut adalah cara menghitung konversi angka **75 (desimal)** ke sistem lain:

**1. Desimal ke Biner (Membagi dengan 2)**
Lakukan pembagian berulang dengan 2 dan catat sisa baginya (Remainder) dari bawah ke atas.
* $75 \div 2 = 37$, sisa **1** (LSB)
* $37 \div 2 = 18$, sisa **1**
* $18 \div 2 = 9$, sisa **0**
* $9 \div 2 = 4$, sisa **1**
* $4 \div 2 = 2$, sisa **0**
* $2 \div 2 = 1$, sisa **0**
* $1 \div 2 = 0$, sisa **1** (MSB)

$$Hasil: 75_{10} = 1001011_{2}$$

**2. Desimal ke Oktal (Konversi dari Biner)**
Cara termudah adalah mengelompokkan hasil biner di atas menjadi 3 bit dari kanan.
* Biner: `001` | `001` | `011`
* Desimal: `1` | `1` | `3`

$$Hasil: 75_{10} = 113_{8}$$

**3. Desimal ke Hexadesimal (Konversi dari Biner)**
Kelompokkan hasil biner menjadi 4 bit dari kanan.
* Biner: `0100` | `1011`
* Nilai: `4` | `11` (Dalam Hexa, 11 = B)

$$Hasil: 75_{10} = 4B_{16}$$


---

### C. Konversi Bilangan Biner: 110101
Berikut adalah cara menghitung konversi **110101 (biner)**:

**1. Biner ke Desimal**
Jumlahkan nilai posisi bit yang bernilai 1 (kalkulasi bobot $2^n$).
$$
\begin{aligned}
(1 \times 32) + (1 \times 16) + (0 \times 8) + (1 \times 4) + (0 \times 2) + (1 \times 1) \\
= 32 + 16 + 0 + 4 + 0 + 1 \\
= 53_{10}
\end{aligned}
$$

**2. Biner ke Hexadesimal**
Kelompokkan 4 bit dari kanan.
* Kelompok 1 (Kanan): `0101` $\rightarrow$ Nilai desimalnya 5.
* Kelompok 2 (Kiri): `0011` (tambahkan nol di depan) $\rightarrow$ Nilai desimalnya 3.

$$Hasil: 110101_{2} = 35_{16}$$

### D. Mengapa Menggunakan Sistem Biner?
Dalam rangkaian digital, sistem biner digunakan karena perangkat elektronik (hardware) lebih mudah dirancang untuk mengenali **2 level tegangan** saja daripada 10 level tegangan.
* **Logic 0 (Low):** Tegangan 0 - 0.8 V.
* **Logic 1 (High):** Tegangan 2 - 5 V.
Sistem ini meminimalkan kesalahan deteksi sinyal dibandingkan jika harus membedakan 10 level tegangan yang berbeda.

---

## 2. Perancangan State Machine (3 State)

Untuk bagian ini, kita akan merancang sebuah *counter* sederhana yang berpindah status dari S0 $\rightarrow$ S1 $\rightarrow$ S2 $\rightarrow$ kembali ke S0. Kita akan menggunakan **D Flip-Flop** karena karakteristiknya yang mudah dipahami ($Output Next = Input D$).

### A. Tabel Transisi (State Transition Table)
Kita membutuhkan 2 bit ($Q_1$ dan $Q_0$) untuk menampung 3 state (karena $2^2 = 4$ kombinasi, cukup untuk 3 state).
* **S0:** 00
* **S1:** 01
* **S2:** 10


**Tabel Transisi dan Input Flip-Flop:**

| State Sekarang | Encoding ($Q_1 Q_0$) | State Berikutnya | Encoding ($Q_1^+ Q_0^+$) | Input D Flip-Flop 1 ($D_1$) | Input D Flip-Flop 0 ($D_0$) |
| :--- | :---: | :--- | :---: | :---: | :---: |
| **S0** | 0 0 | **S1** | 0 1 | 0 | 1 |
| **S1** | 0 1 | **S2** | 1 0 | 1 | 0 |
| **S2** | 1 0 | **S0** | 0 0 | 0 | 0 |

*(Catatan: State 11 adalah kondisi "don't care" atau tidak digunakan).*

### B. Menentukan Input FF (Karakteristik D-FF)
Pada D Flip-Flop, input $D$ harus sama dengan nilai *Next State* ($Q^+$) yang diinginkan.

1.  **Persamaan untuk $D_1$ (Input FF 1):**
    * Lihat kolom $D_1$. Bernilai **1** hanya ketika state sekarang adalah **S1 (0 1)**.
    * Maka logikanya: **$D_1 = \bar{Q_1} \cdot Q_0$**
    *(Artinya: $D_1$ aktif jika $Q_1$ Low DAN $Q_0$ High)*.

2.  **Persamaan untuk $D_0$ (Input FF 0):**
    * Lihat kolom $D_0$. Bernilai **1** hanya ketika state sekarang adalah **S0 (0 0)**.
    * Maka logikanya: **$D_0 = \bar{Q_1} \cdot \bar{Q_0}$**
    *(Artinya: $D_0$ aktif jika $Q_1$ Low DAN $Q_0$ Low)*.

### C. Gambar Rangkaian Akhir (Deskripsi Visual)
Untuk vlog, Anda bisa menggambar skema berikut di papan tulis atau aplikasi simulasi:

1.  Siapkan **2 buah D Flip-Flop** (FF1 dan FF0).
2.  Sambungkan sinyal **Clock** ke kedua FF agar sinkron.
3.  **Rangkaian Kombinasional Input:**
    * Untuk input **$D_1$**: Gunakan gerbang **AND**. Masukkan input dari output $\bar{Q}_1$ (Q bar FF1) dan $Q_0$ (Q normal FF0).
    * Untuk input **$D_0$**: Gunakan gerbang **AND**. Masukkan input dari output $\bar{Q}_1$ (Q bar FF1) dan $\bar{Q}_0$ (Q bar FF0). (Atau gunakan gerbang **NOR** dari input $Q_1$ dan $Q_0$).
4.  Output rangkaian diambil dari $Q_1$ dan $Q_0$ untuk menunjukkan state S0, S1, atau S2.


