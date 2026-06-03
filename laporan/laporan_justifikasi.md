# Laporan Justifikasi: Pemilihan Model & Weighted Sentiment

**Proyek:** Analisis Sentimen Smartphone di YouTube & E-Commerce  
**Tanggal:** 31 Mei 2025

---

## Bagian 1: Justifikasi Pemilihan Model Sentiment Analysis

### 1.1 Model yang Dibandingkan

| Model | Deskripsi |
|---|---|
| **Rule-Based** | Klasifikasi sentimen berdasarkan kamus kata (*lexicon-based*), tanpa proses training. Menggunakan daftar kata positif dan negatif untuk menghitung skor sentimen. |
| **RoBERTa (IndoBERT)** | Model deep learning berbasis transformer (`w11wo/indonesian-roberta-base-sentiment-classifier`), pre-trained pada data besar berbahasa Indonesia dan fine-tuned untuk sentiment analysis. |

---

### 1.2 Hasil Evaluasi Model

Evaluasi dilakukan terhadap **7.199 komentar YouTube** menggunakan label manual (`label_manual`) sebagai ground truth, yang dibandingkan dengan prediksi kedua model.

### 1.1 Model yang Dibandingkan
 
| Model | Deskripsi |
|---|---|
| **Rule-Based** | Klasifikasi sentimen berdasarkan kamus kata (*lexicon-based*), tanpa proses training. Menggunakan daftar kata positif dan negatif untuk menghitung skor sentimen. |
| **RoBERTa (IndoBERT)** | Model deep learning berbasis transformer (`w11wo/indonesian-roberta-base-sentiment-classifier`), pre-trained pada data besar berbahasa Indonesia dan fine-tuned untuk sentiment analysis. |
 
---
 
### 1.2 Hasil Evaluasi Model
 
Evaluasi dilakukan terhadap **7.199 komentar YouTube** menggunakan label manual (`label_manual`) sebagai ground truth, yang dibandingkan dengan prediksi kedua model.
 
#### 1.2.1 Tabel Perbandingan Metrik
 
| Metrik | Rule-Based | RoBERTa | Selisih |
|---|---|---|---|
| **Accuracy** | 69,93% | **87,37%** | +17,44% |
| **Precision** | 74,20% | **88,64%** | +14,44% |
| **Recall** | 69,93% | **87,37%** | +17,44% |
| **F1-Score** | 64,86% | **86,56%** | +21,70% |
 
> Evaluasi menggunakan **weighted average** karena distribusi label tidak seimbang — komentar Netral jauh lebih dominan dibanding Positif dan Negatif.
 
#### 1.2.2 Confusion Matrix
 
**Rule-Based Model:**
 
| | Pred. Negatif | Pred. Netral | Pred. Positif |
|---|---|---|---|
| **Actual Negatif** | 164 | 1.267 | 40 |
| **Actual Netral** | 30 | 3.511 | 107 |
| **Actual Positif** | 20 | 701 | 1.359 |
 
**RoBERTa Model:**
 
| | Pred. Negatif | Pred. Netral | Pred. Positif |
|---|---|---|---|
| **Actual Negatif** | 1.423 | 30 | 18 |
| **Actual Netral** | 47 | 3.582 | 19 |
| **Actual Positif** | 193 | 602 | 1.285 |
 
#### 1.2.3 Classification Report per Kelas
 
**Rule-Based:**
 
| Kelas | Precision | Recall | F1-Score | Support |
|---|---|---|---|---|
| Negatif | 0.77 | 0.11 | 0.19 | 1.471 |
| Netral | 0.64 | 0.96 | 0.77 | 3.648 |
| Positif | 0.90 | 0.65 | 0.76 | 2.080 |
| **Weighted Avg** | **0.74** | **0.70** | **0.65** | **7.199** |
 
**RoBERTa:**
 
| Kelas | Precision | Recall | F1-Score | Support |
|---|---|---|---|---|
| Negatif | 0.86 | 0.97 | 0.91 | 1.471 |
| Netral | 0.85 | 0.98 | 0.91 | 3.648 |
| Positif | 0.97 | 0.62 | 0.76 | 2.080 |
| **Weighted Avg** | **0.89** | **0.87** | **0.87** | **7.199** |
 
---

### 1.3 Model Terpilih []

Berdasarkan hasil evaluasi di atas, **RoBERTa** dipilih sebagai model utama karena:
 
1. **Unggul di seluruh metrik evaluasi** — RoBERTa melampaui Rule-Based di semua metrik, dengan selisih F1-Score terbesar mencapai **+21,70%**. F1-Score diprioritaskan karena distribusi kelas tidak seimbang, sehingga accuracy saja tidak cukup representatif.
2. **Jauh lebih baik mendeteksi sentimen negatif** — Recall kelas Negatif Rule-Based hanya **11%**, artinya 89% komentar negatif salah diklasifikasi menjadi Netral. Sebaliknya RoBERTa mencapai recall Negatif **97%**, jauh lebih handal mendeteksi keluhan pengguna.
3. **Mampu memahami konteks kalimat** — RoBERTa dilatih pada data besar berbahasa Indonesia sehingga mampu menangkap nuansa bahasa informal, slang, dan variasi penulisan yang umum di komentar YouTube. Rule-Based hanya mengandalkan daftar kata yang terbatas, sehingga gagal pada kalimat dengan konteks kompleks.

---

### 1.4 Contoh Komentar yang Salah Diprediksi
 
#### Kesalahan Model Rule-Based
 
| Komentar | Label Manual | Prediksi | Analisis |
|---|---|---|---|
| `"user samsung one ui ui enak..."` | Positif | Netral | Kalimat panjang dengan banyak kata netral, kata positif seperti "enak" tenggelam dalam konteks |
| `"tecno pova ultra 5g"` | Negatif | Netral | Komentar sangat pendek tanpa kata sentimen eksplisit, Rule-Based tidak dapat mengklasifikasikan |
| `"infinix gt pro non gaming rekomended"` | Netral | Positif | Kata "rekomended" terdeteksi sebagai positif padahal komentar bersifat netral/informatif |
| `"bingung z9 z10"` | Negatif | Netral | Kata "bingung" tidak terdeteksi sebagai negatif dalam kamus |
| `"gaming juta"` | Netral | Positif | Tidak ada kata positif eksplisit, namun Rule-Based salah mengklasifikasikan |
 
> Rule-Based cenderung mengklasifikasikan komentar ke kelas Netral karena keterbatasan kamus kata, sehingga performa pada kelas Negatif sangat rendah (recall 11%).
 
#### Kesalahan Model RoBERTa
 
| Komentar | Label Manual | Prediksi | Analisis |
|---|---|---|---|
| `"tolong buatin rekomendasi lebaran"` | Positif | Netral | Komentar permintaan/request, RoBERTa mengklasifikasikan sebagai Netral karena tidak ada ekspresi emosi eksplisit |
| `"bingung seri juta bagus bagus beli handphone..."` | Positif | Negatif | Kata "bingung" memengaruhi prediksi menjadi Negatif meski konteksnya positif (membanding-bandingkan pilihan) |
| `"harga ram worth it g ya"` | Positif | Negatif | Kalimat mengandung tanda tanya yang membuat model menginterpretasikan sebagai keraguan/negatif |
| `"binggung tecno camo pro beli review"` | Netral | Negatif | Kata "bingung" dan nama brand mendorong prediksi ke Negatif |
 
> Kelemahan RoBERTa terutama pada kelas Positif (recall 62%) — beberapa komentar positif yang bersifat permintaan atau membandingkan produk cenderung diprediksi sebagai Netral atau Negatif.
 
---

## Bagian 2: Justifikasi Penggunaan Weighted Sentiment

### 2.1 Masalah: Data Tidak Seimbang Antar Brand
 
Setelah prediksi sentimen dilakukan menggunakan **RoBERTa**, data diagregasi per brand dari file `Youtube Sentiment_Roberta.csv`. Brand "Lainnya" (komentar yang tidak menyebut brand HP spesifik) dikeluarkan dari analisis, dan brand dengan komentar kurang dari 30 difilter karena tidak representatif.
 
Jumlah komentar per brand HP yang tersisa (≥ 30 komentar):
 
| Brand | Jumlah Komentar |
|---|---|
| Xiaomi | 976 |
| Infinix | 487 |
| Samsung | 460 |
| Motorola | 336 |
| Tecno | 316 |
| Oppo | 213 |
| Vivo | 182 |
| Realme | 137 |
| iPhone | 127 |
| iQOO | 89 |
| Asus | 49 |
 
Ketidakseimbangan volume antar brand sangat ekstrem — Xiaomi memiliki **976 komentar** sedangkan Asus hanya **49 komentar**. Jika menggunakan *raw average* saja, brand dengan komentar sedikit bisa mendapatkan skor yang terlalu ekstrem dan tidak merepresentasikan persepsi pasar secara akurat.
 
---

### 2.2 Solusi: Weighted Sentiment Score
 
Untuk mengatasi masalah di atas, digunakan **weighted sentiment** yang memperhitungkan proporsi jumlah komentar sebagai bobot kepercayaan.
 
#### Rumus Weighted Sentiment
 
```
weighted_sentiment(brand) = sentiment_score_avg(brand) × (jumlah_komentar(brand) / total_komentar)
```
 
**Keterangan:**
- `sentiment_score_avg` = rata-rata skor sentimen per brand (Positif=1, Netral=0, Negatif=-1)
- `jumlah_komentar` = jumlah komentar brand tersebut
- `total_komentar` = total seluruh komentar brand yang dianalisis
- Pembagian dengan total komentar menghasilkan **proporsi** sebagai bobot
**Mengapa proporsi?** Brand dengan lebih banyak komentar mendapat bobot lebih besar karena datanya lebih representatif. Brand dengan sedikit komentar skornya otomatis lebih kecil, mencerminkan tingkat kepercayaan yang lebih rendah terhadap data tersebut.
 
---

### 2.3 Tabel Hasil: Raw Sentiment vs Weighted Sentiment
 
| Brand | Jumlah Komentar | Raw Score Avg | Rank Raw | Weighted Sentiment | Rank Weighted | Perubahan |
|---|---|---|---|---|---|---|
| Xiaomi | 976 | 0.0881 | 2 | 0.025504 | 1 | ↑ +1 |
| Infinix | 487 | 0.0698 | 3 | 0.010083 | 2 | ↑ +1 |
| Vivo | 182 | **0.1593** | 1 | 0.008600 | 3 | ↓ -2 |
| Samsung | 460 | 0.0435 | 5 | 0.005931 | 4 | ↑ +1 |
| iPhone | 127 | 0.0630 | 4 | 0.002372 | 5 | ↓ -1 |
| Motorola | 336 | 0.0000 | 6 | 0.000000 | 6 | → 0 |
| Asus | 49 | -0.1020 | 9 | -0.001483 | 7 | ↑ +2 |
| iQOO | 89 | -0.0899 | 8 | -0.002372 | 8 | → 0 |
| Oppo | 213 | -0.0704 | 7 | -0.004448 | 9 | ↓ -2 |
| Realme | 137 | -0.1533 | 11 | -0.006228 | 10 | ↑ +1 |
| Tecno | 316 | -0.1266 | 10 | -0.011862 | 11 | ↓ -1 |
 
---

### 2.4 Interpretasi Perubahan Ranking
 
**Brand yang turun setelah weighting:**
 
- **Vivo** turun dari rank **1 → 3**: Raw sentiment-nya tertinggi (0.1593), tetapi jumlah komentarnya hanya 182 — relatif kecil dibanding Xiaomi (976). Skor tinggi ini kurang terpercaya karena sampelnya lebih kecil.
- **Oppo** turun dari rank **7 → 9**: Volume komentar (213) tidak cukup mengimbangi skor negatifnya, sehingga weighted sentiment-nya lebih buruk.
- **Tecno** turun dari rank **10 → 11**: Meski volumenya cukup besar (316), skor negatif yang dalam (-0.1266) menghasilkan weighted sentiment paling rendah dari semua brand.
**Brand yang naik setelah weighting:**
 
- **Xiaomi** naik dari rank **2 → 1**: Dengan 976 komentar (terbanyak), data Xiaomi paling representatif sehingga mendapat bobot tertinggi.
- **Samsung** naik dari rank **5 → 4**: Volume 460 komentar cukup besar untuk mendapat bobot yang signifikan.
- **Asus** naik dari rank **9 → 7**: Meski skornya negatif, jumlah komentarnya yang kecil (49) membuat dampak negatifnya lebih kecil setelah weighting.
---

### 2.5 Kesimpulan Justifikasi Weighted Sentiment
 
Pendekatan weighted sentiment dipilih karena:
 
1. **Lebih representatif terhadap kondisi data nyata** — Tidak memberikan bobot yang sama pada brand dengan 49 komentar (Asus) versus brand dengan 976 komentar (Xiaomi). Semakin banyak komentar, semakin terpercaya skor sentimen-nya.
2. **Mengurangi bias outlier** — Brand dengan sedikit komentar namun skor ekstrem tidak mendistorsi keseluruhan ranking. Contohnya Vivo yang memiliki raw sentiment tertinggi tetapi volumenya kecil, setelah weighting turun ke rank 3.
3. **Sesuai untuk analisis Big Data dengan distribusi tidak seimbang** — Weighted average digunakan karena proporsi jumlah data pada setiap brand berbeda-beda. Pendekatan ini konsisten dengan penggunaan weighted average pada evaluasi model, di mana metrik dihitung dengan mempertimbangkan proporsi setiap kelas.
---
 
*Laporan ini merupakan bagian dari proyek akhir analisis sentimen smartphone. Untuk detail teknis, dapat dilihat pada notebook `Analisis_Sentimen_Rule_Based_&_RoBERTa.ipynb` dan `Sentiment_Analysis_weighted_sentiment_&_perbandingan_hasil_FIX.ipynb`.*
