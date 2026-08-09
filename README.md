## 1. Background & Business Problems
Perusahaan e-commerce sering kali menghadapi tantangan di mana program promosi banting harga tidak memberikan dampak yang diharapkan pada performa penjualan. Berdasarkan observasi awal pada dataset, ditemukan indikasi inefisiensi promosi, di mana banyak produk dengan persentase diskon tinggi (`discount_percent`) memiliki angka volume transaksi (`sold_count`) yang rendah dan stagnan.

Untuk mendiagnosis akar masalah bisnis ini, dilakukan pendekatan analisis terstruktur menggunakan **Framework 5 Whys**:

1. **Why 1:** Mengapa produk dengan persentase diskon tinggi penjualannya tetap rendah?
   * *Jawaban:* Karena jumlah klik halaman atau tingkat kunjungan konsumen (`view_count`) pada produk tersebut sangat rendah.
2. **Why 2:** Mengapa jumlah klik pengunjung (`view_count`) produk diskon tersebut rendah?
   * *Jawaban:* Karena posisi produk kalah saing di halaman pencarian utama, tergeser oleh kompetitor dengan reputasi merchant dan penilaian bintang (`rating`) yang jauh lebih tinggi.
3. **Why 3:** Mengapa penilaian bintang (`rating`) produk tersebut tidak maksimal?
   * *Jawaban:* Karena jumlah ulasan/testimoni negatif (`review_count`) dari pembeli terdahulu cukup banyak terkait kualitas fisik barang yang meleset dari deskripsi asli toko.
4. **Why 4:** Mengapa kualitas produk dinilai buruk dan meleset dari ekspektasi deskripsi?
   * *Jawaban:* Karena pihak penjual memotong biaya produksi secara ekstrem agar bisa membanting harga murni demi mengejar persentase diskon (`discount_percent`) yang mencolok di aplikasi.
5. **Why 5 (Akar Masalah Utama):**
   * *Kesimpulan:* Strategi promosi salah fokus. Manajemen terlalu mengandalkan trik psikologi diskon coret (`discount_percent`) dan mengorbankan kualitas produk, padahal perilaku pembeli modern saat ini jauh lebih mengutamakan aspek **Reputasi Merchant** dan **Kepuasan Ulasan (`rating`)**.

## 2. Business Questions 
Berdasarkan akar masalah di atas, analisis data ini akan difokuskan untuk menjawab 5 pertanyaan bisnis strategis berikut:

1. **BQ 1 (Efektivitas Promosi):** Apakah pemberian persentase diskon (`discount_percent`) yang tinggi berbanding lurus dengan peningkatan jumlah produk yang terjual (`sold_count`)?
   * *Metrik:* `discount_percent`, `sold_count`, `price`
2. **BQ 2 (Kepercayaan Konsumen):** Manakah faktor yang lebih sensitif mendorong volume penjualan (`sold_count`) produk: harga yang murah (`price`) atau penilaian kepuasan konsumen (`rating`)?
   * *Metrik:* `price`, `rating`, `sold_count`
3. **BQ 3 (Operasional Geografis):** Bagaimana sebaran performa penjualan (`sold_count`) berdasarkan lokasi asal toko (`shop_city`) dan jalur logistiknya (`status` Active vs Warehouse)?
   * *Metrik:* `shop_city`, `status`, `sold_count`, `rating`
4. **BQ 4 (Logistik & Berat Barang):** Bagaimana korelasi antara berat produk (`weight`) dengan volume penjualan (`sold_count`) di pasar e-commerce?
   * *Metrik:* `weight`, `weight_unit`, `sold_count`
5. **BQ 5 (Optimasi Konten Halaman):** Bagaimana pengaruh panjang teks deskripsi produk (`description`) terhadap tingkat kepuasan konsumen (`rating`) dan volume penjualan (`sold_count`) di Tokopedia?

## 3. Stakeholder Identification 
Hasil dari analisis data dan rancangan dasbor ini ditujukan kepada pihak-pihak internal berikut untuk pengambilan keputusan berbasis data (*data-driven actions*):

* **VP of Marketing / CMO:** Menggunakan analisis BQ 1 & BQ 2 untuk mengoptimalkan anggaran biaya subsidi promosi agar tidak dialokasikan pada merchant dengan rating buruk.
* **Head of Logistics & Fulfillment:** Menggunakan analisis BQ 3 & BQ 4 untuk memantau efektivitas gudang otomatis (`Warehouse`) dan efisiensi ongkos kirim berdasarkan zonasi kota.
* **Category / Product Manager:** Menggunakan analisis BQ 5 untuk menentukan standardisasi pengisian halaman etalase produk (seperti minimal panjang karakter deskripsi).
* **Merchant Relations Manager:** Menggunakan ID unik merchant hasil ekstraksi untuk menyaring, mengelompokkan, dan memberikan pelatihan berkala bagi penjual dengan performa rendah.

## 4. Scope of Work (SOW) & Data Selection
Untuk menjaga efisiensi proses komputasi, ketepatan waktu proyek, serta menghindari perluasan masalah yang tidak terkontrol (*Scope Creep*), proyek portofolio ini menerapkan batasan masalah yang ketat.

Dari total **36 kolom data mentah (*raw columns*)** hasil scraping Tokopedia, dilakukan penyaringan secara selektif sehingga hanya menyisakan **9 kolom utama** yang masuk dalam cakupan analisis (*In-Scope*). Kolom sisa lainnya (seperti properti aset media gambar, tautan URL eksternal, dan ID internal sistem) resmi diabaikan karena tidak memiliki kontribusi langsung terhadap pemetaan metrik bisnis.

### 🟩 A. Scope of Variables Used

1. **`sold_count`** *(Numerik)*: Target variabel utama sebagai indikator volume transaksi sukses untuk mengukur tingkat larisnya suatu produk.
2. **`price`** *(Numerik)*: Variabel finansial harga bersih konsumen, digunakan untuk menguji sensitivitas elastisitas harga pasar.
3. **`discount_percent`** *(Teks/Numerik)*: Besaran angka diskon coret, digunakan untuk mengevaluasi efektivitas biaya promosi toko.
4. **`rating`** *(Numerik)*: Skor kepuasan pelanggan skala 1.0 s/d 5.0, digunakan sebagai representasi utama reputasi kualitas produk.
5. **`review_count`** *(Numerik)*: Akumulasi jumlah ulasan tertulis pembeli, mencerminkan tingkat interaksi riil produk di pasar.
6. **`shop_city`** *(Teks/Lokasi)*: Lokasi kota asal operasional merchant berada, digunakan untuk kebutuhan pemetaan spasial/geografis.
7. **`status`** *(Kategori)*: Jalur manajemen logistik produk, membandingkan efisiensi pengiriman mandiri (`Active`) vs gudang otomatis (`Warehouse` / Dilayani Tokopedia).
8. **`weight`** *(Numerik)*: Berat fisik produk dalam satuan gram, digunakan untuk mengukur dampak sensitivitas beban ongkos kirim terhadap minat beli.
9. **`description`** *(Teks)*: Konten penjelasan detail spesifikasi produk, digunakan untuk analisis kuantitatif panjang karakter halaman etalase.


### 🟥 B. Out of Scope
* **No Real-Time Monitoring:** Analisis ini bersifat statis (*snapshot data*). Dasbor tidak dihubungkan ke API live Tokopedia, sehingga fluktuasi stok atau harga hari ini tidak tercakup.
* **No Internal Accounting Financials:** Proyek tidak membedah margin keuntungan bersih (*Net Profit*), biaya modal modal (*COGS*), atau beban pajak internal perusahaan karena keterbatasan ketiadaan kolom laporan keuangan internal pada raw data.
* **No Machine Learning Advanced Modeling:** Fokus proyek dibatasi pada lingkup *Descriptive & Diagnostic Analytics* (apa yang terjadi dan mengapa terjadi). Pemodelan prediktif tingkat lanjut (seperti *Predictive Sales Forecasting* atau *Sentiment Analysis AI*) dikeluarkan dari target *deliverables* proyek saat ini.

## 5. KPI Utama Bisnis & Target Sukses 

| Pertanyaan Bisnis (BQs) | KPI Utama Bisnis | Target Target / Sukses |
| :--- | :--- | :--- |
| **BQ 1: Efektivitas Promosi** | **Promo Revenue ROI**<br>(Mengukur pengembalian profit dari modal diskon) | Korelasi positif yang signifikan; peningkatan diskon wajib diikuti kenaikan volume transaksi **minimal 25%**. |
| **BQ 2: Kepercayaan Konsumen & Strategi Harga** |**Customer Satisfaction Score (CSAT)**<br>(Rata-rata kepuasan terhadap volume transaksi)<br><br> **Premium Pricing Penetration Rate**<br>(Efektivitas penjualan segmen harga menengah-atas) | Mendominasi segmen produk ber-rating > 4.5 dengan kontribusi penjualan **minimum 60%** dari total pasar.<br><br>Menjaga stabilitas volume transaksi pada segmen produk premium (harga menengah-atas) agar berkontribusi **minimal 15%** terhadap total penjualan guna menjaga profit margin perusahaan. |
| **BQ 3: Operasional Geografis** | **Fulfillment SLA Rate**<br>(Persentase adopsi pengiriman gudang otomatis) | Produk berstatus 'Warehouse' menghasilkan volume transaksi **40% lebih cepat** dibanding toko mandiri. |
| **BQ 4: Logistik Berat Barang** | **Shipping Cost Sensitivity Index**<br>(Sensitivitas beban logistik produk) | Mengurangi hambatan *checkout* pembeli pada produk berat (>2 kg) dengan memicu volume konversi stabil. |
| **BQ 5: Optimasi Konten Halaman** | **Product Page Conversion Rate**<br>(Efektivitas kualitas pengisian konten etalase) | Halaman produk dengan panjang deskripsi > 500 karakter memicu rata-rata volume penjualan 15% lebih tinggi dan mempertahankan nilai kepuasan pelanggan di atas 4.90.* |




