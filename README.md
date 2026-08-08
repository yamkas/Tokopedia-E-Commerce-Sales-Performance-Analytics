## 1. Latar Belakang & Business Problem
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


