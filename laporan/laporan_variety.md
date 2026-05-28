# 📊 Laporan Variety — Big Data 7V
## Multi-Platform Big Data Analysis: Smartphone Sales & Sentiment Correlation

---

## Definisi Variety

**Variety** dalam konsep Big Data mengacu pada **keberagaman jenis, format, dan struktur data** yang digunakan dalam analisis. Data tidak hanya berasal dari satu sumber dengan satu format, melainkan dari berbagai platform dengan jenis kolom yang berbeda-beda (numerik, teks bebas, kategorikal).

---

## 1. Sumber Data & Jenis Format

| Sumber | Format Asal | Struktur |
|--------|-------------|----------|
| Shopee | `.csv` (scraping) | Tabular / Semi-terstruktur |
| Lazada | `.csv` (scraping) | Tabular / Semi-terstruktur |
| Tokopedia | `.csv` (scraping) | Tabular / Semi-terstruktur |
| YouTube Komentar | `.csv` + `.zip` | Teks tidak terstruktur |

> Project ini menggabungkan **data terstruktur** (e-commerce) dan **data tidak terstruktur** (teks komentar YouTube) — inilah inti dari Variety Big Data.

---

## 2. Dokumentasi Kolom — Dataset E-Commerce

### 2a. Shopee (sebelum cleaning)

| No | Nama Kolom Asli | Tipe Data | Jenis | Keterangan |
|----|-----------------|-----------|-------|------------|
| 1 | `judul_produk` | object | Teks bebas | Nama listing produk |
| 2 | `harga` | object | Numerik (kotor) | Format string "Rp 1.200.000" |
| 3 | `terjual` | object | Numerik (kotor) | Format string "3rb+" atau angka |
| 4 | `kota` | object | Kategorikal | Nama kota penjual |

### 2b. Lazada (sebelum cleaning)

| No | Nama Kolom Asli | Tipe Data | Jenis | Keterangan |
|----|-----------------|-----------|-------|------------|
| 1 | `brand` | object | Teks bebas | Nama/judul produk dari Lazada |
| 2 | `harga` | object | Numerik (kotor) | Format string |
| 3 | `penjualan` | object | Numerik (kotor) | Format string "1rb+" |
| 4 | `lokasi_toko` | object | Kategorikal | Nama kota dengan variasi prefix |

### 2c. Tokopedia (sebelum cleaning)

| No | Nama Kolom Asli | Tipe Data | Jenis | Keterangan |
|----|-----------------|-----------|-------|------------|
| 1 | `type_hp` | object | Teks bebas | Nama produk |
| 2 | `harga` | object | Numerik (kotor) | Format string |
| 3 | `terjual` | object | Numerik (kotor) | Format string |
| 4 | `lokasi_toko` | object | Kategorikal | Nama kota |

### 2d. E-Commerce Merged (setelah cleaning & merge)

| No | Nama Kolom Baru | Tipe Data | Jenis | Keterangan |
|----|-----------------|-----------|-------|------------|
| 1 | `Produk` | object | Teks bebas | Nama produk (dinormalisasi) |
| 2 | `Harga_Produk` | int64/float64 | **Numerik kontinu** | Harga dalam rupiah (Rp 100.000 – Rp 25.000.000) |
| 3 | `Produk_Terjual` | float64 | **Numerik diskrit** | Jumlah unit terjual (di-clip outlier) |
| 4 | `Lokasi_Toko` | object | **Kategorikal** | Format: "nama kota (kota/kabupaten)" |
| 5 | `E-Commerce` | object | **Kategorikal** | Nilai: Shopee / Lazada / Tokopedia |
| 6 | `Produk_Clean` | object | Teks (processed) | Judul produk lowercase, tanpa tanda baca |
| 7 | `Brand_Ekstrak` | object | **Kategorikal** | Nama brand hasil ekstraksi (20+ brand) |
| 8 | `Segmen_Harga` | category | **Kategorikal ordinal** | Budget / Mid-Range / Upper-Mid / Premium |
| 9 | `Valid_Terjual` | bool | **Boolean** | True jika Produk_Terjual > 0 |

**📌 Kolom baru yang ditambahkan setelah preprocessing (kolom 6–9): ini adalah bentuk Variety yang berkembang!**

---

## 3. Dokumentasi Kolom — Dataset YouTube

### 3a. Sebelum Cleaning (gabungan 2 sumber)

| No | Nama Kolom | Tipe Data | Jenis | Keterangan |
|----|-----------|-----------|-------|------------|
| 1 | `Channel_Sumber` | object | Kategorikal | Gadgetin / Jagat Review / Pixa Focus ID / DKID Media |
| 2 | `Author` | object | Teks | Username komentator |
| 3 | `Comment` | object | **Teks tidak terstruktur** | Komentar asli |
| 4 | `Likes` | object | Numerik (kotor) | Jumlah like komentar |
| 5 | `Time` | object | Datetime (kotor) | Timestamp komentar |
| 6 | `Tipe_Komentar` | object | Kategorikal | Utama / Balasan |

### 3b. Setelah Cleaning (kolom yang ditambahkan)

| No | Nama Kolom Baru | Tipe Data | Jenis | Keterangan |
|----|-----------------|-----------|-------|------------|
| 7 | `Comment_Raw` | object | Teks | Backup komentar asli sebelum cleaning |
| 8 | `Comment_Clean` | object | Teks (processed) | Komentar setelah decode HTML, hapus URL, mention, hashtag |
| 9 | `Comment_Norm` | object | Teks (processed) | Komentar setelah normalisasi kata alay (kamus 80+ kata) |
| 10 | `Comment_Final` | object | Teks (processed) | Komentar setelah stemming (siap analisis sentimen) |
| 11 | `Tokens` | list | Teks tokenized | Daftar token sebelum stopword removal |
| 12 | `Tokens_Stop` | list | Teks tokenized | Daftar token setelah stopword dihapus |
| 13 | `Tokens_Stem` | list | Teks tokenized | Daftar token setelah stemming (Sastrawi) |
| 14 | `Tahun` | int64 | **Numerik diskrit** | Tahun komentar |
| 15 | `Bulan` | int64 | **Numerik diskrit** | Bulan komentar (1–12) |
| 16 | `Hari` | object | **Kategorikal** | Nama hari (Senin–Minggu, dalam Bahasa Indonesia) |
| 17 | `Jam` | int64 | **Numerik diskrit** | Jam posting (0–23) |
| 18 | `Panjang_Karakter` | int64 | **Numerik diskrit** | Panjang komentar dalam karakter |
| 19 | `Jumlah_Kata` | int64 | **Numerik diskrit** | Jumlah kata dalam komentar |

**📌 Total kolom YouTube bertambah dari 6 → 19 kolom setelah preprocessing!**

---

## 4. Ringkasan Variety

### Distribusi Jenis Kolom (Final)

| Jenis Data | E-Commerce | YouTube | Total |
|------------|-----------|---------|-------|
| Numerik kontinu | 1 (Harga) | 0 | 1 |
| Numerik diskrit | 1 (Terjual) | 5 (Likes, Tahun, Bulan, Jam, Pjg_Karakter, Jml_Kata) | 7 |
| Kategorikal nominal | 3 (Lokasi, Platform, Brand) | 3 (Channel, Tipe, Hari) | 6 |
| Kategorikal ordinal | 1 (Segmen Harga) | 0 | 1 |
| Teks bebas | 1 (Produk) | 4 (Comment versi raw/clean/norm/final) | 5 |
| Teks tokenized | 0 | 3 (Tokens, Tokens_Stop, Tokens_Stem) | 3 |
| Boolean | 1 (Valid_Terjual) | 0 | 1 |
| Datetime | 0 | 1 (Time) | 1 |

### Pertumbuhan Kolom (Sebelum vs Sesudah)

| Dataset | Kolom Awal | Kolom Akhir | Kolom Ditambahkan |
|---------|-----------|------------|-------------------|
| Shopee | 4 | 5 | +1 (E-Commerce) |
| Lazada | 4 | 5 | +1 (E-Commerce) |
| Tokopedia | 4 | 5 | +1 (E-Commerce) |
| E-Commerce Merged | 5 | 9 | +4 (Produk_Clean, Brand_Ekstrak, Segmen_Harga, Valid_Terjual) |
| YouTube | 6 | 19 | +13 (Comment versi, tokens, fitur waktu, fitur teks) |

---

## 5. Kesimpulan Variety

Project ini memiliki **variety yang tinggi** karena:

1. **Multi-format**: Data berasal dari file CSV tabular (e-commerce) dan teks bebas tidak terstruktur (komentar YouTube)
2. **Multi-platform**: 4 platform berbeda (Shopee, Lazada, Tokopedia, YouTube) dengan skema kolom yang berbeda
3. **Multi-tipe**: Menggabungkan data numerik, kategorikal, teks mentah, teks terproses, datetime, dan boolean
4. **Berkembang**: Jumlah kolom bertambah signifikan setelah preprocessing, terutama dataset YouTube (6 → 19 kolom) sebagai hasil feature engineering
5. **Bahasa khusus**: Dataset YouTube menggunakan bahasa Indonesia informal/gaul yang memerlukan penanganan NLP khusus (kamus alay, Sastrawi stemmer, nlp_id lemmatizer)
