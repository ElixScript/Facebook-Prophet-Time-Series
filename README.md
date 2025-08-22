# Facebook-Prophet-Time-Series

## Deskripsi Proyek

Proyek ini berfokus pada prediksi penjualan di masa depan untuk berbagai toko menggunakan data penjualan historis dan informasi spesifik toko. Dengan memanfaatkan teknik analisis deret waktu, khususnya Facebook Prophet, kami bertujuan untuk membangun model yang dapat memprediksi penjualan secara akurat, dengan mempertimbangkan faktor-faktor seperti promosi, hari libur, dan karakteristik toko.

## Deskripsi Dataset

Proyek ini menggunakan dua dataset:

1.  **`train.csv`**: Berisi data penjualan historis untuk berbagai toko selama periode waktu tertentu.
    *   `Store`: ID toko unik.
    *   `DayOfWeek`: Hari dalam seminggu.
    *   `Date`: Tanggal catatan.
    *   `Sales`: Penjualan harian (variabel target).
    *   `Customers`: Jumlah pelanggan.
    *   `Open`: Menunjukkan apakah toko buka (0 = tutup, 1 = buka).
    *   `Promo`: Menunjukkan apakah ada promosi yang sedang berjalan.
    *   `StateHoliday`: Menunjukkan hari libur negara bagian (a, b, c, atau 0).
    *   `SchoolHoliday`: Menunjukkan hari libur sekolah.

2.  **`store.csv`**: Memberikan informasi tambahan tentang setiap toko.
    *   `Store`: ID toko unik.
    *   `StoreType`: Tipe toko (a, b, c, d).
    *   `Assortment`: Tingkat variasi produk (a=dasar, b=tambahan, c=diperluas).
    *   `CompetitionDistance`: Jarak ke toko pesaing terdekat.
    *   `CompetitionOpenSinceMonth/Year`: Tanggal pesaing terdekat mulai buka.
    *   `Promo2`: Menunjukkan apakah ada promosi berkelanjutan.
    *   `Promo2SinceWeek/Year`: Tanggal Promo2 dimulai.
    *   `PromoInterval`: Menjelaskan interval untuk Promo2.

## Eksplorasi dan Pra-pemrosesan Data

Eksplorasi data awal menunjukkan tidak ada nilai yang hilang pada file `train.csv`, tetapi ada beberapa pada `store.csv`. Toko yang tutup (`Open` == 0) dihapus karena tidak ada penjualan. Nilai yang hilang pada informasi toko ditangani dengan mengisi `CompetitionDistance` menggunakan median (karena distribusi menceng ke kanan) dan kolom terkait promosi (`Promo2SinceWeek`, `Promo2SinceYear`, `PromoInterval`, `CompetitionOpenSinceYear`, `CompetitionOpenSinceMonth`) dengan 0 jika Promo2 bernilai 0. Kedua dataset kemudian digabungkan berdasarkan ID `Store`. Rekayasa fitur dilakukan untuk mengekstrak `Year`, `Month`, dan `Day` dari kolom `Date`.

## Metodologi: Facebook Prophet

Inti dari peramalan dibangun menggunakan Facebook Prophet, library yang tangguh untuk analisis deret waktu. Prophet memodelkan data deret waktu dengan menguraikannya menjadi tren, musiman (mingguan dan tahunan), dan dampak hari libur.

Model diterapkan pada masing-masing toko, dengan fokus pada kolom 'Date' dan 'Sales'. Model kedua menggabungkan informasi hari libur eksplisit (`StateHoliday` dan `SchoolHoliday`) untuk menangkap dampak spesifiknya terhadap penjualan.

## key findings

*   Jumlah pelanggan menunjukkan korelasi positif yang kuat dengan penjualan.
*   Promosi secara signifikan meningkatkan penjualan dan lalu lintas pelanggan.
*   Penjualan menunjukkan musiman mingguan yang jelas, memuncak pada hari Minggu dan Senin dan terendah pada hari Sabtu.
*   Musiman tahunan menunjukkan penjualan memuncak sekitar periode Natal/Tahun Baru.
*   Hari libur negara bagian dan sekolah memiliki dampak positif yang nyata pada penjualan.
*   Analisis toko individual menunjukkan tren yang bervariasi (beberapa meningkat, beberapa menurun) dan manfaat jelas dari menyertakan data hari libur untuk peramalan yang lebih akurat selama periode tersebut.
4.  **Mount Google Drive (jika menggunakan Colab):** Notebook mengasumsikan file data (`train.csv` dan `store.csv`) terletak di `/content/drive/MyDrive/Data Portofolio/`. Sesuaikan path file jika data Anda disimpan di lokasi lain.
5.  **Jalankan sel secara berurutan:** Eksekusi sel kode satu per satu untuk memuat data, melakukan pra-pemrosesan, mengeksplorasi data, dan menjalankan model Prophet.
