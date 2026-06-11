# UTS Data Science - Semester Genap 2025/2026

Repositori ini digunakan untuk pengerjaan proyek Ujian Tengah Semester (UTS) mata kuliah **Data Science** (Kelas IF404) Program Studi PJJ Informatika S1.

## Anggota Kelompok
* Gading Nasution (Founder & Team Lead)

## Studi Kasus
**Analisis Harga dan Kategori Buku Impor pada E-Commerce Periplus.com**

## Struktur Repositori
* `README.md` - Informasi umum proyek.
* `BREAKDOWN_UTS.md` - Rencana kerja dan breakdown tugas UTS.
* `scrapers/` - Folder berisi script untuk pengumpulan dan pembersihan data.
  * `scrape_periplus.py` - Script scraper untuk mengambil data buku dari Periplus.com.
  * `clean_data.py` - Script untuk pembersihan data mentah hasil scraping.

---

## 🛠️ Panduan Menjalankan Scraper

### 1. Prasyarat (Requirements)
Pastikan Anda sudah menginstal Python 3 dan library yang dibutuhkan. Jalankan perintah berikut untuk menginstal dependencies:

```bash
pip install requests beautifulsoup4 pandas
```

### 2. Cara Menjalankan Scraper
Semua perintah harus dijalankan dari **root directory** repositori ini agar file dataset tersimpan di lokasi yang benar.

#### Langkah A: Scraping Data Mentah
Jalankan script scraper untuk mengambil data dari Periplus.com. Script ini akan mengambil sekitar 500-600 baris data dari 5 kategori berbeda dan menyimpannya sebagai `periplus_books_raw.csv`.

```bash
python scrapers/scrape_periplus.py
```

*Catatan: Script ini menggunakan delay acak (1.0 s.d 2.5 detik) di setiap halaman agar proses scraping berjalan aman dan tidak membebani server target.*

#### Langkah B: Pembersihan Data
Setelah proses scraping selesai dan file `periplus_books_raw.csv` terbentuk, jalankan script pembersihan untuk merapikan format harga, menghitung diskon, menangani missing values, dan menghasilkan dataset bersih `periplus_books_clean.csv`.

```bash
python scrapers/clean_data.py
```
