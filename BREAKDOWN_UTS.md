# Breakdown Pengerjaan UTS Data Science (IF404)
**Studi Kasus: Analisis Harga dan Kategori Buku Impor pada E-Commerce Periplus.com**

Dokumen ini memuat rencana kerja (roadmap) dan pembagian tugas untuk menyelesaikan UTS Data Science sebelum deadline tanggal **14 Juni 2026**.

---

## 📅 Timeline & Prioritas Kerja

### Fase 1: Data Acquisition & Preprocessing (Target: 11 - 12 Juni 2026)
1. **Pembuatan Scraper Periplus.com (`scrape_periplus.py`)**
   * Menggunakan `requests` dan `BeautifulSoup` untuk mengekstrak data dari halaman kategori/pencarian Periplus.
   * Target atribut data: `title`, `price_idr`, `category`, `stock_status`, dan `rating` (jika ada).
   * Batasan: Mengambil minimal 100 sampel produk buku lintas kategori (misal: Fiction, Business, Technology) untuk analisis sebaran.
2. **Pembersihan Data (`clean_data.py`)**
   * Membersihkan string harga (menghapus format "Rp", titik ribuan, spasi) dan konversi ke tipe data numerik (`float` atau `int`).
   * Menangani data kosong (*missing values*) jika ada.
   * Menyimpan dataset bersih ke `periplus_books_clean.csv`.

### Fase 2: Analisis Statistik & Visualisasi (Target: 12 Juni 2026)
1. **Kalkulasi Statistika Deskriptif (`analyze_data.py`)**
   * Menghitung nilai pemusatan data: *Mean*, *Median*, *Mode*.
   * Menghitung nilai penyebaran data: *Standard Deviation*, *Variance*, *Range* (Min-Max), *Kuartil 1 (Q1)*, *Kuartil 3 (Q3)*, dan *Interquartile Range (IQR)*.
   * Melakukan deteksi pencilan (*outliers*) menggunakan metode IQR.
2. **Pembuatan Visualisasi Data (Folder `/plots/`)**
   * **Histogram:** Distribusi harga buku impor keseluruhan.
   * **Boxplot:** Deteksi outlier harga secara visual.
   * **Bar Chart:** Sebaran jumlah buku per kategori.
   * **Grouped Bar Chart / Boxplot per Kategori:** Membandingkan harga buku antar kategori (misal: apakah buku Bisnis lebih mahal dibanding Novel?).

### Fase 3: Dokumentasi & Penyusunan Laporan (Target: 13 Juni 2026)
1. **Pembuatan Jupyter Notebook (`project_data_science.ipynb`)**
   * Mengintegrasikan proses scraping, cleaning, kalkulasi statistik, dan visualisasi ke dalam satu notebook interaktif yang rapi.
   * Menambahkan penjelasan naratif (*markdown cell*) di setiap langkah.
2. **Penyusunan Makalah (`makalah_data_science.md`)**
   * Menyusun naskah akademis sesuai format soal UTS:
     * **BAB I: Pengantar Data Science** (Definisi & peran di e-commerce).
     * **BAB II: Teknik Pengumpulan Data** (Penjelasan scraper Periplus).
     * **BAB III: Eksplorasi & Kualitas Data** (Proses cleaning & kualitas data).
     * **BAB IV: Konsep Dasar Statistika & Peringkasan Data** (Tabel hasil kalkulasi statistik).
     * **BAB V: Penyajian & Visualisasi Data** (Penjelasan grafik hist/boxplot/bar).
     * **BAB VI: Studi Kasus & Manfaat bagi Pengguna** (Rekomendasi bisnis berbasis data untuk e-commerce/Periplus).

### Fase 4: Finalisasi & Submission (Target: 14 Juni 2026)
1. **Konversi Laporan ke PDF**
   * Mengonversi `makalah_data_science.md` ke format PDF dengan nama file sesuai instruksi: `DS_IF404_GadingNasution.pdf`.
2. **Upload Google Drive & LMS**
   * Mengunggah dataset (CSV), notebook (IPYNB), dan PDF Makalah ke Google Drive.
   * Menyematkan link folder Google Drive di dalam makalah.
   * Mengumpulkan file PDF makalah di LMS Edlink.

---

## 🛠️ Rencana Mulai (Langkah Pertama)

Rekomendasi langkah awal kita adalah **membuat Scraper Periplus terlebih dahulu (`scrape_periplus.py`)**. 

**Alasan:**
* Data adalah fondasi dari seluruh tugas ini. Tanpa data riil dari Periplus, kita tidak bisa melakukan analisis statistik deskriptif maupun membuat visualisasi data.
* Kita perlu memvalidasi struktur HTML Periplus.com secara langsung untuk memastikan selektor CSS/HTML-nya tepat dan tidak terhalang proteksi bot.
