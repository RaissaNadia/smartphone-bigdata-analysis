# Laporan Justifikasi: Pemilihan Model & Weighted Sentiment

**Proyek:** Analisis Sentimen Smartphone di YouTube & E-Commerce  
**Tanggal:** 31 Mei 2025

---

## Bagian 1: Justifikasi Pemilihan Model Sentiment Analysis

### 1.1 Model yang Dibandingkan

| Model | Deskripsi |
|---|---|
| **Rule-Based** | Klasifikasi sentimen berdasarkan kamus kata (`lexicon-based`), tanpa proses training. Menggunakan daftar kata positif dan negatif untuk menghitung skor. |
| **RoBERTa (IndoBERT)** | Model deep learning berbasis transformer (`w11wo/indonesian-roberta-base-sentiment-classifier`), pre-trained pada data besar berbahasa Indonesia dan fine-tuned untuk sentiment analysis. |

---

### 1.2 Hasil Evaluasi Model

#### 1.2.1 Confusion Matrix

**Rule-Based Model:**

|  | Predicted Positif | Predicted Netral | Predicted Negatif |
|---|---|---|---|
| **Actual Positif** | `[TP]` | `[FN]` | `[FN]` |
| **Actual Netral** | `[FP]` | `[TP]` | `[FN]` |
| **Actual Negatif** | `[FP]` | `[FP]` | `[TP]` |

**RoBERTa Model:**

|  | Predicted Positif | Predicted Netral | Predicted Negatif |
|---|---|---|---|
| **Actual Positif** | `[TP]` | `[FN]` | `[FN]` |
| **Actual Netral** | `[FP]` | `[TP]` | `[FN]` |
| **Actual Negatif** | `[FP]` | `[FP]` | `[TP]` |

#### Tabel Perbandingan Metrik

| Metrik | Rule-Based | RoBERTa |
|---|---|---|
| **Accuracy** | `XX%` | `XX%` |
| **Precision** | `XX%` | `XX%` |
| **Recall** | `XX%` | `XX%` |
| **F1-Score** | `XX%` | `XX%` |

---

### 1.3 Model Terpilih []

Berdasarkan hasil evaluasi di atas, model **[RoBERTa / Rule-Based]** dipilih sebagai model utama karena:

1. 

2. 

3. 

---

### 1.4 Contoh Komentar yang Salah Diprediksi

#### Contoh Kesalahan Model Rule-Based

| Komentar | Label Aktual | Prediksi Model | Analisis |
|---|---|---|---|
| `[contoh komentar 1 — isi dari hasil notebook]` | `[label]` | `[prediksi]` | `[analisis singkat]` |
| `[contoh komentar 2 — isi dari hasil notebook]` | `[label]` | `[prediksi]` | `[analisis singkat]` |

#### Contoh Kesalahan Model RoBERTa

| Komentar | Label Aktual | Prediksi Model | Analisis |
|---|---|---|---|
| `[contoh komentar 1 — isi dari hasil notebook]` | `[label]` | `[prediksi]` | `[analisis singkat]` |
| `[contoh komentar 2 — isi dari hasil notebook]` | `[label]` | `[prediksi]` | `[analisis singkat]` |

---

## Bagian 2: Justifikasi Penggunaan Weighted Sentiment

### 2.1 Data Tidak Seimbang Antar Brand

Dalam dataset komentar YouTube (total **7.199 komentar** dari `youtube_eda_with_brand_sentiment.csv`), jumlah komentar per brand sangat bervariasi:

| Brand | Jumlah Komentar | Proporsi |
|---|---|---|
| Lainnya | 4.474 | 62,1% |
| Xiaomi | 976 | 13,6% |
| Infinix | 474 | 6,6% |
| Samsung | 426 | 5,9% |
| Tecno | 198 | 2,7% |
| Vivo | 184 | 2,6% |
| Oppo | 157 | 2,2% |
| Realme | 139 | 1,9% |
| iPhone | 126 | 1,7% |
| Asus | 45 | 0,6% |

Ketidakseimbangan ini sangat ekstrem, **Asus hanya memiliki 45 komentar** dibanding Xiaomi yang memiliki 976. Jika kita menggunakan rata-rata sentiment biasa (*raw average*), brand dengan komentar sangat sedikit bisa mendapatkan skor yang terlalu ekstrem hanya karena beberapa komentar, sehingga tidak merepresentasikan persepsi pasar secara akurat.

---

### 2.2 Solusi Penanganan Menggunakan Weighted Sentiment Score

Untuk mengatasi masalah di atas, digunakan **weighted sentiment** yang memperhitungkan proporsi jumlah komentar sebagai bobot kepercayaan.

#### Rumus Weighted Sentiment

```
weighted_sentiment(brand) = sentiment_score_avg(brand) × (jumlah_komentar(brand) / total_komentar)
```

**Keterangan:**
- `sentiment_score_avg` = rata-rata skor sentimen per brand (Positif=1, Netral=0, Negatif=-1)
- `jumlah_komentar` = jumlah komentar brand tersebut
- `total_komentar` = total seluruh komentar (7.199)
- Pembagian dengan total komentar menghasilkan **proporsi** yang menjadi bobot

**Mengapa proporsi?** Brand dengan lebih banyak komentar mendapat bobot lebih besar karena datanya lebih representatif. Brand dengan sedikit komentar skornya otomatis lebih kecil, mencerminkan kepercayaan yang lebih rendah terhadap data tersebut.

---

### 2.3 Tabel Hasil (Raw Sentiment vs Weighted Sentiment)

| Brand | Jumlah Komentar | Raw sentiment_score_avg | Weighted Sentiment | Rank Raw | Rank Weighted | Perubahan |
|---|---|---|---|---|---|---|
| Lainnya | 4.474 | 0.1238 | 0.076955 | 6 | 1 | ↑ +5 |
| Xiaomi | 976 | 0.0830 | 0.011252 | 8 | 2 | ↑ +6 |
| Infinix | 474 | 0.1245 | 0.008196 | 5 | 3 | ↑ +2 |
| Samsung | 426 | 0.0939 | 0.005556 | 7 | 4 | ↑ +3 |
| Vivo | 184 | 0.1793 | 0.004584 | 1 | 5 | ↓ -4 |
| Tecno | 198 | 0.1414 | 0.003889 | 3 | 6 | ↓ -3 |
| iPhone | 126 | 0.1270 | 0.002223 | 4 | 7 | ↓ -3 |
| Oppo | 157 | 0.0701 | 0.001528 | 9 | 8 | ↑ +1 |
| Asus | 45 | 0.1556 | 0.000972 | 2 | 9 | ↓ -7 |
| Realme | 139 | 0.0504 | 0.000972 | 10 | 10 | → 0 |

---

### 2.4 Interpretasi Perubahan Ranking

**Brand yang turun drastis setelah weighting:**

- **Asus** turun dari rank **2 menjadi 9**: Raw sentiment-nya tinggi (0.1556), tetapi hanya memiliki **45 komentar**, paling sedikit dari semua brand. Skor tinggi ini tidak dapat dipercaya karena sampelnya terlalu kecil.
- **Vivo** turun dari rank **1 menjadi 5**: Meskipun memiliki raw sentiment tertinggi (0.1793), jumlah komentarnya hanya 184. Sehingga tidak cukup besar untuk mendapat bobot tinggi.

**Brand yang naik setelah weighting:**

- **Xiaomi** naik dari rank **8 menjadi 2**: Raw sentiment-nya memang lebih rendah (0.0830), tapi memiliki **976 komentar**. Dimana menjadi brand dengan data terbesar kedua setelah Lainnya. Volume yang besar membuat skor Xiaomi lebih terpercaya.
- **Samsung** naik dari rank **7 menjadi 4**: Dengan 426 komentar, Samsung memiliki basis data yang cukup representatif.

---

### 2.5 Kesimpulan Justifikasi Weighted Sentiment

Pendekatan weighted sentiment dipilih karena:

1. 
2. 
3. 

---
