# 📋 Review Project UTS Data Science

**Tanggal Review:** 11 Juni 2026  
**Reviewer:** Automated Code & Data Review  
**Scope:** Analisis data, kalkulasi statistik, visualisasi, narasi/temuan. *Scraping code dikecualikan dari review.*

---

## ✅ Hal-Hal yang Sudah Benar

### 1. Kalkulasi Statistik Deskriptif
- Semua nilai **Mean, Median, Mode, Std, Variance, Q1, Q3, IQR** telah diverifikasi dan **hasilnya benar** — konsisten antara `analyze_data.py`, notebook (`project_data_science.ipynb`), dan file output `data/descriptive_statistics.txt`.
- Metode deteksi **outlier menggunakan IQR** (batas bawah = Q1 − 1.5×IQR, batas atas = Q3 + 1.5×IQR) sudah **benar secara formula** dan konsisten menghasilkan 37 outlier.

### 2. Kualitas Data Bersih (`periplus_books_clean.csv`)
- **Tidak ada missing values** pada semua kolom — proses cleaning berhasil.
- **Tidak ada harga negatif atau nol** — data harga valid.
- **Tidak ada kasus harga jual > harga asli** — relasi price vs original_price konsisten.
- **Konsistensi diskon**: Seluruh 178 buku yang berdiskon memiliki `discount_percent` yang cocok dengan kalkulasi manual `(original - price) / original × 100`. **Tidak ada mismatch.**
- **Tipe data** sudah benar: `price_idr`, `original_price_idr`, `discount_percent` bertipe `float64`.

### 3. Visualisasi
- Keempat grafik (histogram, boxplot, bar chart, boxplot per kategori) sudah **ter-generate dengan baik** dan sesuai dengan data.
- Label axis, judul, dan formatter Rupiah sudah terpasang.
- Histogram menunjukkan distribusi *right-skewed* yang sesuai dengan temuan narasi.

### 4. Proses Cleaning (`scrapers/clean_data.py`)
- Logic `clean_price()` dan `clean_discount()` sudah benar — regex menghapus karakter non-digit dan mengkonversi ke float.
- Pengisian `original_price_idr` dengan `price_idr` jika tidak ada diskon sudah tepat.
- Drop rows tanpa harga (`dropna(subset=['price_idr'])`) sudah sesuai.

### 5. Struktur Notebook
- Alur notebook terstruktur rapi: Load → Eksplorasi → Statistik → Statistik per Kategori → Visualisasi → Temuan & Rekomendasi.
- Narasi Markdown di notebook menjelaskan setiap langkah dengan baik.

---

## ⚠️ Temuan Masalah & Kekeliruan

### 🔴 Masalah 1: Klaim Outlier di Notebook **Tidak Akurat**
**Lokasi:** Notebook Cell 11 (Temuan Utama poin 4)

**Teks di notebook:**
> "Terdeteksi sebanyak 37 produk buku impor dengan harga di atas Rp 672,500.00, **sebagian besar berasal dari kategori Computer & IT dan Biographies & Memoirs**."

**Fakta dari data:**

| Kategori | Jumlah Outlier |
|---|---|
| Computer & IT | 27 |
| **Business & Self-Help** | **5** |
| Biographies & Memoirs | 3 |
| Fiction & Literature | 1 |
| Children's Books | 1 |

**Kekeliruan:** Kategori kedua terbanyak outlier adalah **Business & Self-Help (5 buku)**, bukan Biographies & Memoirs (3 buku). Klaim di notebook salah menyebut Biographies & Memoirs sebagai kontributor utama outlier kedua.

**Perbaikan:** Ubah kalimat menjadi:
> "...sebagian besar berasal dari kategori Computer & IT (27 buku) dan Business & Self-Help (5 buku)."

---

### 🟡 Masalah 2: Satuan Varians Ditulis "Rp" — Seharusnya "Rp²"
**Lokasi:** `analyze_data.py` (baris 49), notebook Cell 6, dan `data/descriptive_statistics.txt` (baris 6)

**Teks saat ini:**
```
Varians              : Rp 52,852,689,133.29
```

**Kekeliruan:** Varians adalah kuadrat dari standar deviasi, sehingga satuannya adalah **Rupiah kuadrat (Rp²)**, bukan Rupiah (Rp). Menuliskan "Rp" membuat pembaca berpikir ini adalah nilai uang, padahal bukan — ini adalah ukuran dispersi berdimensi kuadrat.

**Perbaikan:** Ganti prefix "Rp" menjadi tanpa unit atau tulis eksplisit "Rp²":
```
Varians              : 52,852,689,133.29 (Rp²)
```

---

### 🟡 Masalah 3: Data Duplikat Tidak Ditangani (9 Baris)
**Lokasi:** `data/periplus_books_clean.csv`

Terdapat **9 baris duplikat** berdasarkan kombinasi `title + author + category`. Ini adalah buku-buku dengan judul dan pengarang sama, tetapi edisi/ISBN/URL berbeda (contoh: edisi paperback vs hardcover, atau volume berbeda dari seri yang sama).

**Contoh kasus duplikat:**

| Buku | Harga 1 | Harga 2 | Keterangan |
|---|---|---|---|
| Strange Houses (Uketsu) | Rp 360,000 | Rp 395,000 | ISBN berbeda, bisa beda edisi |
| The Diary of a CEO (Bartlett) | Rp 428,000 | Rp 275,000 | ISBN berbeda, paperback vs hardcover |
| Fearless (Roberts) | Rp 218,000 | Rp 215,000 | ISBN berbeda |
| Warriors Graphic Novel Part... | Rp 318,000 × 3 | — | Volume 1, 2, 3 dihitung duplikat karena title truncated |

**Dampak:** Duplikat ini **sedikit menginflasi** jumlah sampel (593 vs ~584 unik) dan dapat mempengaruhi statistik deskriptif, meski dampaknya kecil.

**Catatan:** Ini bukan kesalahan fatal — beberapa di antaranya memang produk berbeda (beda ISBN/edisi). Tapi sebaiknya disebutkan dalam narasi analisis bahwa ada kasus ini dan keputusan untuk tetap menyertakannya harus dijustifikasi.

---

### 🟡 Masalah 4: Buku Out-of-Stock Ikut Dianalisis Tanpa Penjelasan
**Lokasi:** Dataset dan seluruh analisis

Terdapat **27 buku** dengan `in_stock = 0` (out of stock) yang tetap disertakan dalam analisis harga. Sebaran per kategori:

| Kategori | Buku OOS |
|---|---|
| Computer & IT | 17 |
| Business & Self-Help | 4 |
| Biographies & Memoirs | 4 |
| Fiction & Literature | 2 |

**Dampak:** Harga buku yang sudah tidak tersedia mungkin tidak lagi relevan (bisa outdated). Tidak ada narasi di notebook yang menjelaskan keputusan untuk tetap menyertakan buku OOS.

**Saran:** Tambahkan 1–2 kalimat di bagian Eksplorasi Data (Cell 3/4) yang menjelaskan keputusan analitis ini, misalnya: *"Terdapat 27 buku berstatus out-of-stock yang tetap disertakan dalam analisis karena harga tercantum masih valid sebagai data historis."*

---

### 🟢 Masalah 5 (Minor): `descriptive_statistics.txt` Tidak Lengkap
**Lokasi:** `data/descriptive_statistics.txt`

File output tidak menyertakan:
- **Batas bawah outlier** (Rp −59,500.00)
- **Batas atas outlier** (Rp 672,500.00)
- **Range (Rentang)**

Padahal informasi ini dicetak ke terminal di `analyze_data.py`. Sebaiknya disimpan juga ke file agar dokumentasi lengkap.

---

### 🟢 Masalah 6 (Minor): Label Sumbu X Histogram Tumpang Tindih
**Lokasi:** `plots/price_distribution.png`

Label angka Rupiah di sumbu X histogram saling menumpuk (*overlapping*) karena format "Rp X,XXX,XXX" terlalu panjang untuk jumlah tick marks yang ada. Ini membuat grafik agak sulit dibaca.

**Saran:** Tambahkan rotasi label: `plt.xticks(rotation=45)` atau kurangi jumlah tick, atau gunakan format singkat seperti "250rb", "500rb", dst.

---

## 📊 Ringkasan Review

| Aspek | Status | Catatan |
|---|---|---|
| Kalkulasi statistik (mean, median, dll.) | ✅ Benar | Semua angka terverifikasi cocok |
| Formula IQR & deteksi outlier | ✅ Benar | Formula standar, hasil konsisten |
| Konsistensi diskon | ✅ Benar | 0 mismatch dari 178 buku diskon |
| Kualitas data (missing, negatif, dll.) | ✅ Bersih | Tidak ada anomali kritis |
| Klaim distribusi outlier per kategori | ❌ Keliru | Business & Self-Help, bukan Biographies & Memoirs |
| Satuan varians | ⚠️ Kurang tepat | Ditulis "Rp", seharusnya "Rp²" atau tanpa unit |
| Data duplikat | ⚠️ Perlu justifikasi | 9 baris duplikat, dampak minor |
| Buku OOS dalam analisis | ⚠️ Perlu penjelasan | 27 buku OOS tanpa narasi justifikasi |
| File statistik output | ℹ️ Minor | Kurang info batas outlier & range |
| Label axis histogram | ℹ️ Minor | Tumpang tindih, sulit dibaca |

---

## 🎯 Prioritas Perbaikan

1. **[WAJIB]** Perbaiki klaim outlier di notebook — ganti "Biographies & Memoirs" → "Business & Self-Help"
2. **[DISARANKAN]** Perbaiki satuan Varians dari "Rp" → "Rp²" atau hilangkan prefix
3. **[OPSIONAL]** Tambahkan justifikasi untuk duplikat dan buku OOS di narasi
4. **[OPSIONAL]** Lengkapi `descriptive_statistics.txt` dan perbaiki label histogram
