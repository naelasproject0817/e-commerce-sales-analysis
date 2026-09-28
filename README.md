# Laporan Analisis Data E-Commerce

Laporan ini menyajikan hasil analisis data transaksi *e-commerce* berdasarkan eksplorasi data awal (EDA), pembersihan & pengayaan data, serta analisis mendalam untuk menjawab lima pertanyaan bisnis utama.

---

## 1. Gambaran Umum Data (*Exploratory Data Analysis*)

* **Ukuran Dataset:** Terdiri dari **4.870 baris** dan **8 kolom**.
* **Rentang Waktu:** Data transaksi mencakup periode dari **1 Desember 2010 hingga 9 Desember 2011**.
* **Kualitas Data:**
  * **Missing Values:** Tidak ditemukan nilai kosong (*null/missing*) pada seluruh kolom.
  * **Duplikasi Data:** Ditemukan **0 data duplikat**.
* **Fitur Utama:**
  * Kuantitas pembelian (`Quantity`) bervariasi dari **1 hingga 992 unit** per transaksi (rata-rata ~13 unit).
  * Harga satuan (`UnitPrice`) bervariasi dari **0,04 hingga 145,00** (rata-rata ~2,94).
  * Ditemukan **443 baris** pada `StockCode` yang mengandung karakter huruf.

---

## 2. Pembersihan & Pengayaan Data (*Feature Engineering*)

Beberapa kolom turunan telah ditambahkan untuk memperdalam analisis waktu dan pendapatan:

1. **`Total_Amount`**: Hasil kali antara `Quantity` dan `UnitPrice` (Total Pendapatan per item).
2. **`Day_name` & `Day_Type`**: Nama hari dan kategori hari (`Weekday` vs `Weekend`).
3. **`Hour` & `Time_Category`**: Jam transaksi dan pengelompokan waktu:
   * **Morning:** 07.00 – 11.59
   * **Afternoon:** 13.00 – 16.59
   * **Night:** Di luar jam tersebut
4. **`Month_Year`**: Pengelompokan berdasarkan bulan dan tahun (`YYYY-MM`).

---

## 3. Hasil Analisis 5 Pertanyaan Bisnis (*Business Questions*)

### Question 1: Bagaimana total penjualan (pendapatan) per bulan?

#### **Ringkasan Pendapatan Bulanan**

| Bulan (`Month_Year`) | Total Pendapatan (`Total_Amount`) | Keterangan Status |
| :--- | :--- | :--- |
| **2010-12** | 9.076,82 | Transaksi Awal Periode |
| **2011-01** | 7.371,44 | Penurunan Pasca-Liburan |
| **2011-02** | 6.152,46 | **Titik Terendah (*Trough*)** |
| **2011-03** | 8.878,08 | Pemulihan Penjualan |
| **2011-04** | 7.070,30 | Penyesuaian Wajar |
| **2011-05** | 10.370,16 | Awal Tren Kenaikan |
| **2011-06** | 8.683,93 | Stabil Pertengahan Tahun |
| **2011-07** | 9.071,06 | Performa Konsisten |
| **2011-08** | 8.956,87 | Performa Konsisten |
| **2011-09** | 12.875,81 | Lonjakan Awal Musim Gugur |
| **2011-10** | 15.455,27 | Pertumbuhan Signifikan |
| **2011-11** | 17.948,84 | **Puncak Penjualan (*Peak*)** |
| **2011-12** | 5.496,43 | Data Terpotong (s/d 9 Des 2011) |

#### **Temuan Kunci**
* **Puncak Penjualan (Q4 Peak):** Penjualan melonjak signifikan dari **September (12.875,81)** hingga puncaknya di **November 2011 (17.948,84)**. Hal ini didorong oleh tren belanja musiman (*Black Friday* dan persiapan liburan akhir tahun).
* **Titik Terendah (Post-Holiday Slump):** Pendapatan terendah terjadi pada **Februari 2011 (6.152,46)**, melambangkan penurunan aktivitas belanja pasca-liburan.
* **Catatan Desember 2011:** Penurunan pada Desember 2011 disebabkan oleh batas pencatatan data yang hanya sampai tanggal **9 Desember 2011** (*data truncation*).

---

### Question 2: Kapan periode jam sibuk (peak hours) transaksi berlangsung?

#### **Distribusi Transaksi Berdasarkan Waktu**

| Kategori Waktu (`Time_Category`) | Jam Operational | Jumlah Transaksi | Persentase (%) |
| :--- | :--- | :--- | :--- |
| **Afternoon** | 12:00 – 16:59 | **2.684** | **55,1%** |
| **Morning** | 07:00 – 11:59 | 2.112 | 43,4% |
| **Night** | 17:00 – 06:59 | 74 | 1,5% |

#### **Temuan Kunci**
* **Puncak Aktivitas Transaksi:** Pelanggan paling sering melakukan transaksi pada **siang hingga sore hari (*Afternoon*)** dengan total **2.684 transaksi (55,1%)**.
* **Pagi Hari sebagai Kontributor Kedua:** Waktu **pagi (*Morning*)** mencatat **2.112 transaksi (43,4%)**. Secara akumulatif, **98,5% transaksi terjadi antara pukul 07:00 hingga 16:59**.
* **Aktivitas Malam Rendah:** Transaksi pada malam hari (*Night*) sangat minimal (hanya 1,5%), menunjukkan bahwa mayoritas pelanggan berbelanja pada jam kerja atau jam aktif harian.

---

### Question 3: Hari dan segmen hari apa yang mencatatkan Average Revenue tertinggi?

#### **1. Rata-Rata Pendapatan Berdasarkan Hari (*Day Name*)**

| Hari (`Day_name`) | Total Pendapatan | Jumlah Transaksi | Rata-Rata Pendapatan / Transaksi |
| :--- | :--- | :--- | :--- |
| **Thursday** | 25.048,51 | 1.018 | **24,61** |
| **Wednesday** | 22.840,43 | 935 | **24,43** |
| **Monday** | 18.528,75 | 802 | **23,10** |
| **Tuesday** | 23.366,13 | 1.019 | **22,93** |
| **Friday** | 14.887,14 | 651 | **22,87** |
| **Sunday** | 13.354,08 | 445 | **30,01** |

> *Catatan:* Hari **Sabtu (*Saturday*)** tidak memiliki pencatatan transaksi pada dataset ini.

#### **2. Rata-Rata Pendapatan Berdasarkan Kategori Hari (*Day Type*)**

| Kategori Hari (`Day_Type`) | Total Pendapatan | Jumlah Transaksi | Rata-Rata Pendapatan / Transaksi |
| :--- | :--- | :--- | :--- |
| **Weekend** (Sunday) | 13.354,08 | 445 | **30,01** |
| **Weekday** (Mon–Fri) | 104.670,96 | 4.425 | **23,65** |

#### **Temuan Kunci**
* **Hari Tertinggi:** **Kamis (*Thursday*)** mencatatkan rata-rata pendapatan harian tertinggi di kelompok kerja dengan **24,61** per transaksi, diikuti oleh **Rabu (*Wednesday*)** sebesar **24,43**.
* **Weekend vs. Weekday:** Meskipun volume transaksi terbanyak terjadi pada hari kerja (*Weekday*), kategori **Weekend (Sunday)** menghasilkan **rata-rata nilai transaksi tertinggi (30,01 per transaksi)**. Hal ini mengindikasikan bahwa pelanggan yang berbelanja di akhir pekan cenderung membeli dalam jumlah unit lebih banyak atau memilih produk bernilai lebih tinggi.

---

### Question 4: Negara mana saja 5 teratas yang menyumbang total kuantitas pembelian tertinggi?

#### **Peta Matriks Volume Transaksi (`Day_Type` vs `Time_Category`)**

| Kategori Waktu | Weekday (Transaksi) | Weekend (Transaksi) | Total Transaksi |
| :--- | :--- | :--- | :--- |
| **Afternoon** (12:00 – 16:59) | 2.398 | 286 | **2.684** |
| **Morning** (07:00 – 11:59) | 1.953 | 159 | **2.112** |
| **Night** (17:00 – 06:59) | 74 | 0 | **74** |
| **Total** | **4.425** | **445** | **4.870** |

#### **Temuan Kunci**
* **Kombinasi Dominan:** Volume transaksi terbesar terkonsentrasi pada **Weekday - Afternoon (2.398 transaksi)** dan **Weekday - Morning (1.953 transaksi)**.
* **Pola Akhir Pekan:** Pada hari akhir pekan (*Weekend/Sunday*), seluruh transaksi terkonsentrasi di waktu **Siang (286 transaksi)** dan **Pagi (159 transaksi)**, tanpa ada transaksi sama sekali di malam hari (*Night* = 0).
* **Implikasi Operasional:** Promosi instan (*flash sales*) atau penayangan iklan berbayar paling efektif dijalankan pada rentang waktu **Weekday Siang (12:00–16:59)**.

---

### Question 5: Faktor mana yang paling kuat mendorong total pendapatan, jumlah barang atau harga barang?

#### **1. Alasan Pemisahan & Eksklusi United Kingdom (UK)**
* **Dominasi Data & Skala *Outlier*:** United Kingdom (UK) merupakan pasar domestik utama di mana volume transaksi dan kuantitas barangnya mendominasi secara absolut. UK menyumbang porsi terbesar dari keseluruhan dataset secara signifikan.
* **Mencegah Distorsi Visualisasi:** Jika UK tetap dimasukkan ke dalam grafik perbandingan antar-negara, angka UK yang sangat besar akan membuat skala grafik tertekan (*extreme outlier*). Hal ini mengakibatkan perbedaan volume antar-negara internasional lainnya tidak terlihat dengan jelas (*compressed*).
* **Strategi Analisis Terpisah:** Performa pasar UK dianalisis secara terpisah (*dedicated analysis*) untuk mengevaluasi pasar domestik, sedangkan analisis ini difokuskan khusus pada evaluasi **ekspansi pasar internasional (*international market penetration*)**.

---

#### **2. Top 5 Negara Luar UK Berdasarkan Kuantitas Pembelian**

| Peringkat | Negara (`Country`) | Total Kuantitas Barang (`Quantity`) | Persentase dari Total Non-UK (%) |
| :---: | :--- | :--- | :--- |
| **1** | **EIRE (Irlandia)** | **1.810 unit** | **33,2%** |
| **2** | **Germany** | **1.353 unit** | **24,8%** |
| **3** | **France** | **1.164 unit** | **21,3%** |
| **4** | **Netherlands** | **676 unit** | **12,4%** |
| **5** | **Australia** | **449 unit** | **8,2%** |

---

#### **3. Temuan Kunci Pasar Internasional**
* **Dominasi Pasar Regional Eropa:** Tiga negara teratas (**EIRE, Jerman, dan Prancis**) secara akumulatif menyumbang lebih dari **79%** dari total kuantitas barang yang diekspor ke luar UK.
* **Potensi Pasar Jarak Jauh:** **Australia** berhasil masuk ke jajaran Top 5 dengan **449 unit**, membuktikan adanya permintaan organik yang kuat dari luar benua Eropa meskipun terdapat tantangan jarak dan biaya pengiriman.
* **Rekomendasi Bisnis:** Perusahaan dapat mempertimbangkan pembukaan *fulfillment center* atau jaringan logistik khusus di Eropa Daratan (seperti Jerman atau Belanda) untuk mempercepat durasi pengiriman dan menekan biaya logistik bagi pasar internasional utama ini.
