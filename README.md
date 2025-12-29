Halo,

Ini penjelasan lengkap untuk file kedua, **`Visualization.ipynb`**. Kalau file sebelumnya buat laporan rutin, file yang ini gue bikin khusus buat **Audit Kualitas Data (Quality Control)**.

Tujuannya simpel: Kita mau tau apakah data yang dikirim Puskesmas itu beneran valid, lengkap, atau cuma asal input. Script ini bakal generate visualisasi yang bisa langsung kita jadiin bahan evaluasi ke mereka.

Berikut bedah logic-nya dari awal sampai akhir:

### 1. Setup & Tarik Data (Cell Awal)

Gue pakai library standar: `pandas`, `seaborn`, `matplotlib`, plus `google-cloud-bigquery` buat konek ke database.

* **Sumber Data:** `stellar-orb-451904-d9.dashboard.epus_pelayanan_assessment`.
* **Settingan Penting:** Perhatikan variabel `START_DATE` dan `END_DATE`. Nanti lo tinggal ganti tanggal ini aja setiap mau run evaluasi mingguan. Targetnya ada 12 Puskesmas (list `PKM_TARGETS`).

### 2. Cek Kelengkapan Data / Missing Values (Cell: "Missing Values")

Di sini gue mau nangkep petugas yang males ngisi data vital (Tensi, Nadi, Suhu, BB, TB).

* **Logic:** Gue itung berapa banyak kolom yang `NaN` (kosong) vs terisi per petugas (`perawat_bidan_nutrisionist_sanitarian`).
* **Visualisasi:** Outputnya **Bar Chart Horizontal**.
* *Merah*: Data kosong.
* *Hijau*: Data terisi.
* Makin banyak merahnya, makin perlu ditegur petugasnya.


* **Output File:** `Visualization of missing values.zip` (Isinya folder per pkm).

### 3. Deteksi Manipulasi Data / "Heaping" (Cell: "Heaping, Heatmap")

Ini fitur andalan gue buat nebak data itu asli atau "tembakan".

* **Teori:** Kalau orang ngukur beneran, angka digit terakhirnya (misal 12**3**, 4**5**, 9**8**) harusnya nyebar merata 0-9. Kalau digit **0** atau **5** muncul berlebihan (misal >20%), biasanya itu hasil kira-kira/pembulatan, bukan pengukuran alat.
* **Logic:** Script ini ngambil digit terakhir dari setiap angka pengukuran.
* **Visualisasi (Heatmap):** Warna makin gelap di angka tertentu (misal kolom 0) menandakan indikasi manipulasi data yang kuat di Puskesmas tersebut.
* **Output File:** `Visualization of last-digit distribution.zip`.

### 4. Laporan Mingguan & Deteksi Outlier (Cell Terakhir)

Ini blok code paling panjang, fungsinya buat generate laporan komprehensif mingguan.

* **Preprocessing:** Gue bersihin data outlier yang gak masuk akal biar grafiknya gak rusak.
* *Suhu*: Gue batasi 33°C - 43°C. Kalau ada input 300°C, otomatis ke-filter sebagai outlier.
* *Pernapasan (RR)*: Gue batasi 10 - 30 napas/menit.
* Variabel lain pakai metode IQR (Interquartile Range).


* **Visualisasi:**
* *Kiri*: Histogram distribusi data.
* *Kanan*: Panel teks yang ngelist **"More than 15%"** (indikasi heaping) dan **"Outliers"** (data aneh buat dicek validasinya).


* **Output File:** `Visualization_week_2.zip` (Isi PDF laporan per Puskesmas).

### Summary & Next Action

Buat lo nanti, rutinitasnya simpel:

1. Buka Notebook ini.
2. Update `START_DATE` dan `END_DATE` di cell 3.
3. Run All Cells.
4. Download 3 file ZIP yang ke-generate di akhir, terus kirim ke tim manajemen Puskesmas sebagai bahan evaluasi.

Semoga membantu! Script ini udah gue bikin se-otomatis mungkin biar lo gak pusing coding ulang.

palingan ganti tanggal wkwkwk
