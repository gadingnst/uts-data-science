# UTS Data Science - Semester Genap 2025/2026

Repositori ini digunakan untuk pengerjaan proyek Ujian Tengah Semester (UTS) mata kuliah **Data Science** (Kelas IF404) Program Studi PJJ Informatika S1.

---

## 📝 Soal Ujian Tengah Semester (UTS)

**Mata Kuliah:** Data Science  
**Kelas:** IF404  
**Prodi:** PJJ Informatika S1  
**Waktu:** 01 s.d. 14 Juni 2026  
**Dosen:** Ir. Ahmad Chusyairi, M.Com., CDS., IPM., ASEAN Eng  
**Sifat Ujian:** Kelompok – Project Base Learning  

### Petunjuk Pengerjaan Soal:
1. Perhatikan waktu yang telah disetting untuk mengerjakan Ujian, jika lewat batas waktu maka jawaban Anda tidak akan diterima.
2. Tulis lengkap identitas dan uraian pekerjaan Anda pada Makalah.
3. Jawaban Ujian tetap diupload oleh masing-masing anggota kelompok (bukan hanya perwakilan saja). $\rightarrow$ Jika dikerjakan secara kelompok (1 kelompok minimal 3 mahasiswa dan maksimal 5 mahasiswa).
4. **Teknis Pengumpulan:**
   * Upload file makalah dengan format: `DS_IF404_NamaLengkap.pdf`
   * Upload file project via Google Drive jika ukuran filenya terlalu besar dan lampirkan link Google Drive.

### Soal:
1. Pembuatan makalah dan analisis untuk *data science* dimana terdapat: pengantar data science, konsep dasar statistika, penyajian data, peringkasan data, eksplorasi data (kualitas dan pola sebaran data), teknik pengumpulan data, visualisasi data dengan studi kasus yang bertema: *e-commerce*, *e-education* (pilih salah satu) dan bermanfaat bagi pengguna. **(Bobot 50)**
2. Pembuatan *project data science* dilengkapi dataset sesuai dengan studi kasus pada soal nomor (1). **(Bobot 50)**
3. Semua file diupload pada GDrive, linknya diletakan pada makalah tersebut dan diupload di LMS Edlink yang sudah disediakan.

---

## 👥 Anggota Kelompok
* Gading Nasution (Founder & Team Lead)

## 📖 Studi Kasus Proyek
**Analisis Harga dan Kategori Buku Impor pada E-Commerce Periplus.com**

## 📂 Struktur Repositori
* `README.md` - Informasi umum proyek dan soal UTS.
* `BREAKDOWN_UTS.md` - Rencana kerja dan breakdown tugas UTS.
* `scrapers/` - Folder berisi script untuk pengumpulan dan pembersihan data.
  * `scrape_periplus.py` - Script scraper untuk mengambil data buku dari Periplus.com.
  * `clean_data.py` - Script untuk pembersihan data mentah hasil scraping.
* `data/` - Folder berisi dataset hasil scraping (raw dan clean).

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
Jalankan script scraper untuk mengambil data dari Periplus.com. Script ini akan mengambil sekitar 500-600 baris data dari 5 kategori berbeda dan menyimpannya sebagai `data/periplus_books_raw.csv`.

```bash
python scrapers/scrape_periplus.py
```

*Catatan: Script ini menggunakan delay acak (1.0 s.d 2.5 detik) di setiap halaman agar proses scraping berjalan aman dan tidak membebani server target.*

#### Langkah B: Pembersihan Data
Setelah proses scraping selesai dan file `data/periplus_books_raw.csv` terbentuk, jalankan script pembersihan untuk merapikan format harga, menghitung diskon, menangani missing values, dan menghasilkan dataset bersih `data/periplus_books_clean.csv`.

```bash
python scrapers/clean_data.py
```
