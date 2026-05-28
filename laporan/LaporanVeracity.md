# 🔍 Laporan Veracity — Big Data 7V
## Multi-Platform Big Data Analysis: Smartphone Sales & Sentiment Correlation

---

## Definisi Veracity

**Veracity** dalam Big Data mengacu pada **kebenaran, kelengkapan, dan kepercayaan data** — seberapa jauh data bebas dari noise, inkonsistensi, missing value, duplikat, dan anomali. Veracity juga mencakup bagaimana masalah-masalah tersebut **diselesaikan (resolusi)**.

---

## 1. Veracity Dataset E-Commerce

### 1a. Shopee

#### Sebelum Cleaning
| Masalah | Detail |
|---------|--------|
| Total baris | 1.194 baris |
| Produk non-HP | 179 produk (aksesori, casing, charger, dll) |
| Tipe data `Harga_Produk` | `object` (string "Rp 1.200.000") |
| Tipe data `Produk_Terjual` | `object` (string "3rb+", "500", dll) |
| Missing value `Produk_Terjual` | Ada (jumlah terdeteksi saat cleaning) |
| Missing value `Lokasi_Toko` | Ada |
| Inkonsistensi lokasi | Format tidak seragam (e.g. "Surabaya" vs "Kota Surabaya" vs "Kab. Surabaya") |

#### Resolusi yang Dilakukan
| Masalah | Cara Penanganan |
|---------|----------------|
| Produk non-HP | Drop baris menggunakan keyword blacklist (headset, charger, casing, dll) |
| Format harga string | Hapus "Rp", titik, koma → konversi ke `int64` |
| Format terjual string | Parsing regex: "rb" × 1000, "k" × 1000, angka biasa → `float64` |
| Missing `Produk_Terjual` | Imputasi dengan **0** (tidak diketahui = belum terjual) |
| Missing `Lokasi_Toko` | Imputasi dengan **modus** (kota terbanyak = Surabaya) |
| Inkonsistensi lokasi | Normalisasi ke format standar: `"nama_kota (kota/kabupaten)"` |

#### Setelah Cleaning
| Metrik | Nilai |
|--------|-------|
| Total baris | **1.015 baris** (-179 produk non-HP) |
| Missing value | **0** |
| Tipe data Harga | `int64` ✅ |
| Tipe data Terjual | `float64` ✅ |

---

### 1b. Lazada

#### Sebelum Cleaning
| Masalah | Detail |
|---------|--------|
| Total baris | 4.000 baris |
| Produk non-HP | 71 produk (filter dari 3.137 setelah seleksi kolom) |
| Tipe data `Harga_Produk` | `object` (string) |
| Tipe data `Produk_Terjual` | `object` (string) |
| Inkonsistensi lokasi | Format beragam: "Kota Jakarta Selatan", "Solo", "Kab. Bandung Barat" |

#### Resolusi yang Dilakukan
| Masalah | Cara Penanganan |
|---------|----------------|
| Produk non-HP | Drop baris menggunakan keyword blacklist (diperluas: mesin, rusak, icloud) |
| Format harga/terjual | Parsing regex konsisten dengan Shopee |
| Inkonsistensi lokasi | `direct_map` untuk kasus khusus (Solo→Surakarta) + normalisasi prefix |

#### Setelah Cleaning
| Metrik | Nilai |
|--------|-------|
| Total baris | **3.066 baris** (-71 produk non-HP) |
| Missing value | **0** |
| Tipe data Harga | `int64` ✅ |
| Tipe data Terjual | `float64` ✅ |

---

### 1c. Tokopedia

#### Sebelum Cleaning
| Masalah | Detail |
|---------|--------|
| Total baris | 921 baris |
| Produk non-HP | 282 produk (keyword blacklist lebih luas) |
| Harga outlier | Ada harga > Rp 25.000.000 (bukan smartphone) |
| Inkonsistensi lokasi | Banyak variasi prefix kota/kabupaten |

#### Resolusi yang Dilakukan
| Masalah | Cara Penanganan |
|---------|----------------|
| Produk non-HP | Drop baris (keyword blacklist paling lengkap: +tas hp, speaker, gimbal, dll) |
| Harga outlier | Filter langsung: Rp 0 – Rp 25.000.000 |
| Inkonsistensi lokasi | Normalisasi regex + whitelist kota besar Indonesia |

#### Setelah Cleaning
| Metrik | Nilai |
|--------|-------|
| Total baris | **639 baris** (-282 produk non-HP) |
| Missing value | **0** |
| Tipe data Harga | `float64` ✅ |
| Tipe data Terjual | `float64` ✅ |

---

### 1d. E-Commerce Merged (Gabungan 3 Platform)

#### Sebelum Deduplication
| Metrik | Nilai |
|--------|-------|
| Total baris (Shopee + Lazada + Tokopedia) | **4.663 baris** |
| Data duplikat lintas platform | **226 baris** |
| Missing value | 0 |

#### Penanganan Outlier Harga (setelah merge)
| Metrik | Sebelum Clip | Setelah Clip |
|--------|-------------|-------------|
| Harga minimum | Rp 100 | Rp 100.000 |
| Harga maksimum | Rp 39.250.000 | Rp 25.000.000 |
| Jumlah baris | 4.437 | **4.437** (tidak ada yang hilang — pakai clip) |

#### Penanganan Outlier Produk Terjual
| Metrik | Detail |
|--------|--------|
| Batas atas (Q3 + 1.5×IQR) | Dihitung per data valid (terjual > 0) |
| Metode | Clip ke persentil 99 (data tidak hilang) |

#### Setelah Deduplication & Outlier Handling
| Metrik | Nilai |
|--------|-------|
| **Total baris bersih** | **4.437 baris** |
| Duplikat | **0** ✅ |
| Missing value | **0** ✅ |
| Distribusi platform | Lazada: 3.051 \| Shopee: 840 \| Tokopedia: 546 |

---

## 2. Veracity Dataset YouTube

### Sebelum Cleaning
| Masalah | Detail |
|---------|--------|
| Total baris (2 sumber digabung) | **7.792 baris** |
| Sumber 1 (3 video channel lain) | 2.725 baris |
| Sumber 2 (Gadgetin 6000) | 5.067 baris |
| Data duplikat | **28 baris** |
| Missing value `Comment` | Ada (ditemukan saat pengecekan) |
| Komentar terlalu pendek (< 2 kata) | **283 komentar** |
| Teks kotor | HTML entity (`&amp;`), emoji, URL, mention (@), hashtag (#), angka murni |
| Kata tidak baku / alay | Kata seperti "gak", "bgt", "hp", "spek", dll |
| Inkonsistensi waktu | Kolom `Time` bertipe `object`, bukan datetime |

### Resolusi yang Dilakukan
| Masalah | Cara Penanganan |
|---------|----------------|
| Duplikat | Drop berdasarkan kombinasi Author + Comment + Time |
| Missing `Comment` | Drop baris (`dropna`) |
| HTML entity | `html.unescape()` |
| Emoji & non-ASCII | `encode('ascii', 'ignore')` |
| URL, mention, hashtag | Regex replace |
| Angka murni | Regex `\b\d+\b` remove |
| Pengulangan huruf (bagussss) | Normalisasi ke 2 huruf max |
| Kata alay | Kamus normalisasi **80+ kata** (gak→tidak, bgt→banget, dll) |
| Komentar terlalu pendek | Drop baris dengan < 2 kata setelah cleaning |
| Kolom Time | Konversi ke `datetime` dengan timezone UTC |
| Kolom Likes | Konversi dari `object` ke `int64` |

### Setelah Cleaning (Lengkap)
| Tahap | Jumlah Baris | Keterangan |
|-------|-------------|------------|
| Raw (gabungan) | 7.792 | Data mentah dari 2 sumber |
| Setelah hapus duplikat | 7.764 | -28 baris |
| Setelah hapus missing Comment | 7.695 | -69 baris |
| Setelah filter < 2 kata | **7.412** | -283 baris |
| **Final bersih** | **7.412** | Missing: 0 \| Duplikat: 0 ✅ |

### Distribusi Channel (Data Bersih)
| Channel | Jumlah | Persentase |
|---------|--------|-----------|
| Gadgetin | 4.808 | 64,9% |
| Jagat Review | 1.410 | 19,0% |
| Pixa Focus ID | 847 | 11,4% |
| DKID Media | 347 | 4,7% |

---

## 3. Ringkasan Veracity — Sebelum vs Sesudah

### E-Commerce

| Dataset | Baris Awal | Baris Akhir | Berkurang | Missing Akhir | Duplikat Akhir |
|---------|-----------|------------|-----------|--------------|----------------|
| Shopee | 1.194 | 1.015 | 179 (non-HP) | 0 ✅ | 0 ✅ |
| Lazada | 4.000 | 3.066 | 934 (non-HP + filter) | 0 ✅ | 0 ✅ |
| Tokopedia | 921 | 639 | 282 (non-HP) | 0 ✅ | 0 ✅ |
| **Merged** | **4.663** | **4.437** | 226 (duplikat) | **0 ✅** | **0 ✅** |

### YouTube

| Tahap | Baris | Masalah Diselesaikan |
|-------|-------|----------------------|
| Raw | 7.792 | — |
| Final | **7.412** | Duplikat ✅ \| Missing ✅ \| Teks kotor ✅ \| Kata alay ✅ |

---

## 4. Kesimpulan Veracity

Setelah proses preprocessing menyeluruh, kedua dataset mencapai **tingkat veracity yang tinggi**:

- **E-Commerce**: Missing value = 0, duplikat = 0, outlier ditangani dengan clip (data tidak hilang), format kolom konsisten dan siap analisis numerik.
- **YouTube**: Missing value = 0, duplikat = 0, teks melalui 4 tahap cleaning (raw → clean → normalized → stemmed), siap untuk analisis sentimen.
- **Resolusi berbasis keputusan**: Setiap penanganan memiliki justifikasi (contoh: outlier di-clip bukan di-drop agar data tidak berkurang; terjual = 0 diimputasi bukan di-drop karena produk baru juga valid).
