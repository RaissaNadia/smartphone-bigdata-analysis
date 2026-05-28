# Laporan Volume — Big Data E-Commerce Gadget
 
**Tanggal:** 28 Mei 2025  
**Sumber Data:** Scraping dari 3 platform e-commerce (Shopee, Lazada, Tokopedia)

---

## 1. Definisi Volume dalam Konteks Dataset Ini

Volume mengacu pada **jumlah total data** yang dikumpulkan, diproses, dan dihasilkan 
dari proses scraping dan cleaning data produk smartphone di 3 platform e-commerce 
Indonesia. Volume diukur dalam satuan baris (record produk) dan kolom (atribut/fitur).

---

## 2. Volume Data Mentah (Sebelum Cleaning)

| Platform   | Jumlah Baris | Jumlah Kolom | Kolom yang Tersedia |
|------------|-------------|--------------|---------------------|
| Shopee     | 1.194       | 10           | link_produk, judul_produk, harga, potongan_harga, flex-none src, rating, terjual, jenis, inline src, kota |
| Lazada     | 4.000       | 10           | link_produk, brand, harga, lokasi_toko, potongan, voucher_klaim, penjualan, jumlah_rating, keunggulan, metode_transaksi |
| Tokopedia  | 921         | 10           | link_produk, potongan_harga, type_hp, harga, harga_awal, promo, rating, terjual, toko, lokasi_toko |
| **Total**  | **6.115**   | **10**       | Belum terstandarisasi antar platform |

---

## 3. Proses Cleaning & Reduksi Data

Tahapan yang menyebabkan **pengurangan volume** (data dibuang):

| Tahap | Keterangan |
|-------|------------|
| Seleksi kolom | Dari 10 kolom → 5 kolom relevan per platform |
| Filter produk non-HP | Membuang aksesoris, spare part, laptop, tablet, dll |
| Filter harga tidak wajar | Harga < Rp 100.000 atau > Rp 25.000.000 dibuang |
| Drop duplikat | Tokopedia: 53 baris duplikat dihapus |
| Standarisasi kolom | Semua platform diseragamkan ke 5 kolom standar |

Tahapan yang **tidak mengurangi volume** (data dipertahankan), sehingga data tidak hilang:

| Tahap | Keterangan |
|-------|------------|
| Clip harga | Nilai ekstrem dibatasi Rp 100.000 – Rp 25.000.000 |
| Clip terjual | Nilai ekstrem dibatasi di persentil ke-99 |
| Imputasi missing value | Diisi dengan median/0/'Tidak Diketahui' |
| Normalisasi lokasi | Standarisasi format nama kota |
| Ekstraksi brand | Penambahan kolom Brand_Ekstrak |

---

## 4. Volume Data Bersih (Sesudah Cleaning per Platform)

| Platform   | Sebelum Cleaning | Sesudah Cleaning | Baris Berkurang | % Berkurang | Kolom Akhir |
|------------|-----------------|-----------------|-----------------|-------------|-------------|
| Shopee     | 1.194           | 1.015           | 179             | 15,0%       | 5           |
| Lazada     | 4.000           | 3.066           | 934             | 23,4%       | 5           |
| Tokopedia  | 921             | 577             | 344             | 37,3%       | 5           |
| **Total**  | **6.115**       | **4.658**       | **1.457**       | **23,8%**   | **5**       |

> **Catatan:** Pengurangan baris bukan merupakan kehilangan data penting, melainkan 
> hasil seleksi produk yang relevan (smartphone) dan pembersihan data tidak valid.

---

## 5. Volume Data Setelah Merge (Dataset Final)

Setelah ketiga dataset bersih digabungkan pada Notebook 04:

| Keterangan | Nilai |
|------------|-------|
| Total baris sebelum dedup merge | 4.658 |
| Duplikat lintas platform dihapus | 225 |
| **Total baris final** | **4.433** |
| **Total kolom final** | **8** |
| Missing value tersisa | 0 |

**Komposisi per platform setelah merge:**

| Platform  | Jumlah Baris | Persentase |
|-----------|-------------|------------|
| Lazada    | 3.051       | 68,8%      |
| Shopee    | 840         | 19,0%      |
| Tokopedia | 542         | 12,2%      |
| **Total** | **4.433**   | **100%**   |

**Kolom pada dataset final:**

| Kolom | Tipe Data | Keterangan |
|-------|-----------|------------|
| Produk | object | Nama/judul produk |
| Harga_Produk | float64 | Harga dalam Rupiah |
| Produk_Terjual | float64 | Jumlah unit terjual |
| Lokasi_Toko | object | Lokasi penjual |
| E-Commerce | object | Platform asal (Shopee/Lazada/Tokopedia) |
| Brand_Ekstrak | object | Brand hasil ekstraksi otomatis |
| Segmen_Harga | object | Segmen harga (Budget/Mid-Range/Upper-Mid/Premium) |
| Valid_Terjual | bool | Flag data terjual valid (> 0) |

---

## 6. Ringkasan Volume: Sebelum vs Sesudah Cleaning

| Tahap | Baris | Kolom |
|-------|-------|-------|
| Raw (mentah) | 6.115 | 10 (belum terstandarisasi) |
| Bersih per platform | 4.658 | 5 (terstandarisasi) |
| Final setelah merge | 4.433 | 8 (+ fitur tambahan) |

- **Pengurangan total:** 1.682 baris (27,5%) dari data mentah
- **Standarisasi kolom:** dari 10 kolom berbeda per platform → 5 kolom seragam
- **Fitur tambahan hasil engineering:** Brand_Ekstrak, Segmen_Harga, Valid_Terjual

---

## 7. Kesimpulan

Dataset e-commerce gadget ini mencakup **4.433 record produk smartphone** 
dari 3 platform besar di Indonesia. Proses cleaning berhasil mengurangi noise 
sebesar **27,5%** dari total data mentah, menghasilkan dataset yang bersih, 
terstandarisasi, dan siap untuk analisis lanjutan termasuk merge dengan data 
sentimen YouTube dan pemodelan big data.
