# Laporan Value: Analisis Komparatif Brand Smartphone

**Proyek:** Analisis Sentimen Smartphone di YouTube & E-Commerce

**Tanggal:** 3 Juni 2025

---

## Summary

Laporan ini menyajikan analisis komparatif performa brand smartphone berdasarkan tiga dimensi utama yaitu terdiri dari sentimen publik di YouTube, volume penjualan di e-commerce, dan rata-rata harga produk. Dari total 16 brand yang berhasil diekstrak dari data e-commerce, sebanyak 14 brand memenuhi syarat untuk dihitung composite score-nya setelah di-merge dengan data sentimen YouTube. Dari 14 brand tersebut, 11 brand digunakan pada gap analysis karena memiliki minimal 30 komentar YouTube sehingga datanya cukup representatif.

Temuan utama menunjukkan bahwa **Infinix** menempati posisi teratas composite score berkat kombinasi sentimen positif dan penjualan tertinggi. Sentimen publik terbukti berkorelasi positif sedang dengan penjualan (r = 0.5354, signifikan), sementara hubungan antara harga dan penjualan tidak terbukti signifikan secara statistik. Gap analysis mengungkap bahwa sebagian besar brand berada di kuadran "Star Brand", namun beberapa brand dengan sentimen negatif tetap mencatatkan penjualan yang cukup tinggi dengan adanya indikasi bahwa faktor di luar sentimen turut berperan besar.

---

## Metodologi

### Alur Filtrasi Data

Proses analisis dimulai dari data mentah yang mengalami tiga tahap penyaringan:

```
E-Commerce (16 brand)
        ↓ merge inner dengan sentiment YouTube
Composite Score (14 brand)
        ↓ filter minimum 30 komentar YouTube
Gap Analysis (11 brand)
```

Setiap tahap penyusutan memiliki alasan yang terdokumentasi. Brand yang gugur di tahap merge (Poco, Itel) tidak memiliki padanan di data sentimen YouTube. Brand yang gugur di tahap gap analysis (Sony, Nokia, Huawei) memiliki komentar YouTube terlalu sedikit (< 30) sehingga skor sentimennya tidak cukup representatif untuk perbandingan.

### Definisi Composite Score

Composite score dihitung dari tiga faktor yang dinormalisasi ke skala 0–1 menggunakan Min-Max Normalization:

```
Composite Score = (norm_sentiment × 0.40) + (norm_terjual × 0.35) + (norm_harga × 0.25)
```

| Faktor | Bobot | Alasan |
|---|---|---|
| Weighted Sentiment | 40% | Mencerminkan persepsi publik yang paling langsung relevan dengan reputasi brand |
| Total Terjual | 35% | Indikator konkret penerimaan pasar |
| Avg Harga | 25% | Konteks segmen pasar, bukan indikator kualitas (bobot kecil disengaja) |

> **Catatan metodologi:** Harga tidak diinverse karena harga tinggi tidak selalu berarti buruk melainkan hanya mencerminkan segmen yang berbeda. Bobot 25% diberikan sebagai konteks, bukan penalti.

### Definisi Weighted Sentiment

Weighted sentiment memperhitungkan proporsi jumlah komentar sebagai bobot kepercayaan:

```
weighted_sentiment(brand) = sentiment_score_avg(brand) × (jumlah_komentar(brand) / total_komentar)
```

Pendekatan ini dipilih karena distribusi komentar antar brand sangat tidak seimbang dimana Xiaomi memiliki 976 komentar sementara Asus hanya 49 komentar. Raw average tanpa pembobotan akan memberikan kepercayaan yang setara pada data yang sangat berbeda volumenya.

---

## Insight 1: Brand Terbaik Berdasarkan Composite Score

### Tabel Ranking Composite Score (14 Brand)

| Rank | Brand | Composite Score | Norm Sentiment | Norm Terjual | Norm Harga | Kategori |
|---|---|---|---|---|---|---|
| 1 | **Infinix** | **0.7366** | 0.8746 | 1.0000 | 0.1473 | ✅ Baik |
| 2 | **Vivo** | **0.6815** | 1.0000 | 0.6233 | 0.2532 | ✅ Baik |
| 3 | **Samsung** | **0.6804** | 0.8480 | 0.6925 | 0.3953 | ✅ Baik |
| 4 | Sony | 0.6214 | 0.9285 | 0.0000 | 1.0000 | ⚠️ Perlu catatan |
| 5 | **Apple** | **0.5949** | 0.8672 | 0.1570 | 0.7723 | ✅ Baik |
| 6 | **Xiaomi** | **0.5436** | 0.9866 | 0.2339 | 0.2684 | ✅ Baik |
| 7 | Nokia | 0.3627 | 0.8266 | 0.0915 | 0.0000 | ⚠️ Perlu catatan |
| 8 | Huawei | 0.3526 | 0.4794 | 0.0009 | 0.6421 | — |
| 9 | Motorola | 0.2931 | 0.5331 | 0.0025 | 0.3161 | — |
| 10 | Oppo | 0.2746 | 0.3960 | 0.1153 | 0.3033 | — |
| 11 | Asus | 0.2513 | 0.2488 | 0.0000 | 0.6072 | — |
| 12 | iQOO | 0.2287 | 0.3367 | 0.0033 | 0.3715 | — |
| 13 | Realme | 0.1220 | 0.1153 | 0.0685 | 0.2076 | — |
| 14 | **Tecno** | **0.0637** | 0.0000 | 0.0367 | 0.2034 | ❌ Terendah |

*Threshold composite score ≥ 0.5 ditetapkan sebagai batas "performa baik"*

### Interpretasi

**Infinix (0.7366) : Juara Keseluruhan.** Kunci keunggulan Infinix bukan pada sentimen tertinggi (Vivo lebih tinggi), melainkan pada norm_terjual sempurna (1.0000) yang mencerminkan penjualan 618.540 unit, tertinggi dari semua brand. Ini menunjukkan Infinix berhasil mengkonversi persepsi positif menjadi pembelian nyata di pasar budget Indonesia.

**Vivo (0.6815)    : Sentimen Terbaik, Penjualan Nomor 3.** Vivo memiliki weighted sentiment tertinggi (norm = 1.0000), namun penjualannya di bawah Infinix dan Samsung. Ini mengindikasikan Vivo sangat dicintai penggunanya yang sudah ada, tetapi belum mampu menjangkau pasar yang lebih luas.

**Samsung (0.6804) : Konsisten di Semua Dimensi.** Samsung tidak unggul di satu dimensi pun secara individual, tetapi skornya merata di sentiment (0.8480), penjualan (0.6925), dan harga (0.3953). Konsistensi ini mencerminkan kekuatan brand yang mapan dengan ekosistem produk luas dari entry-level hingga flagship.

**Sony (0.6214):** Sony masuk top 4 bukan karena performa pasar riil, melainkan karena norm_harga mendekati maksimum (1.0000) akibat harga rata-rata Rp 10,5 juta. Padahal Sony hanya memiliki 9 komentar YouTube dan 52 unit terjual sehingga datanya sangat tidak representatif. Posisi Sony di top 4 merupakan **artefak metodologi**, bukan cerminan performa pasar sesungguhnya. Hal ini dicatat sebagai keterbatasan analisis.

**Xiaomi (0.5436) : Sentimen Bagus, Penjualan Mengecewakan.** Xiaomi memiliki norm_sentiment tinggi (0.9866), hampir setara Vivo, namun penjualannya relatif rendah (144.710 unit) untuk brand dengan volume komentar terbesar (976 komentar). Ini menarik untuk diselidiki lebih lanjut.

---

## Insight 2: Korelasi Weighted Sentiment vs Total Terjual

### Hasil Uji Korelasi Pearson

| Pasangan Variabel | Pearson r | P-value | Interpretasi |
|---|---|---|---|
| Weighted Sentiment vs Total Terjual | **+0.5354** | 0.0485 | Korelasi positif sedang, **signifikan** |
| Avg Harga vs Total Terjual | **−0.3239** | 0.2586 | Korelasi negatif sedang, **tidak signifikan** |

### Korelasi 1 — Weighted Sentiment vs Total Terjual (r = +0.5354)

Dengan p-value = 0.0485 (< 0.05), korelasi ini **terbukti signifikan secara statistik**. Artinya, ada hubungan nyata antara sentimen publik di YouTube dengan volume penjualan di e-commerce, brand yang lebih banyak mendapat komentar positif cenderung terjual lebih banyak.

Pola yang terlihat pada scatter plot mendukung kesimpulan ini:

- **Kuadran kanan atas** (sentimen tinggi + terjual tinggi): Infinix, Vivo, Samsung (brand-brand ini konsisten unggul di kedua dimensi)
- **Kuadran kiri bawah** (sentimen rendah + terjual rendah): Tecno, Realme, Asus (konsisten bermasalah di kedua dimensi)
- **Anomali Xiaomi:** Sentimen tinggi (0.599) tetapi penjualan hanya 144.710 unit, jauh di bawah garis tren. Ini mengindikasikan ada faktor eksternal yang menekan penjualan Xiaomi meskipun persepsi publik positif (kemungkinan berkaitan dengan distribusi atau kompetisi harga di segmen yang sama dengan Infinix).

Meski korelasi terbukti, kekuatannya hanya **sedang** (r = 0.53). Artinya sentimen bukan satu-satunya penentu penjualan melainkan terdapat faktor lain seperti strategi promosi, ketersediaan produk, dan distribusi turut berperan signifikan.

### Korelasi 2 — Avg Harga vs Total Terjual (r = −0.3239)

Dengan p-value = 0.2586 (> 0.05), korelasi ini **tidak terbukti signifikan**. Meskipun arah negatif (brand mahal cenderung terjual lebih sedikit), hubungan ini tidak bisa digeneralisasikan dari data yang ada.

Contoh yang menjelaskan mengapa hubungan ini tidak sederhana: Samsung memiliki harga rata-rata Rp 4,4 juta (mid-high) tetapi terjual 428.385 unit, sedangkan Nokia dengan harga sangat murah (Rp 437 ribu) hanya terjual 56.666 unit. **Brand equity, ragam lini produk, dan promosi marketplace lebih menentukan penjualan daripada harga semata.**

---

## Insight 3: Gap Analysis (Overrated vs Underrated)

### Metodologi Gap Analysis

Gap analysis dilakukan pada **11 brand** yang memiliki minimal 30 komentar YouTube. Empat brand (Sony, Nokia, Huawei, dan brand lainnya) dikeluarkan karena data sentimennya tidak cukup representatif untuk perbandingan. Kuadran ditentukan berdasarkan median weighted sentiment dan median total terjual sebagai titik batas.

### Hasil Gap Analysis

| Kuadran | Brand | Weighted Sentiment | Total Terjual | Interpretasi |
|---|---|---|---|---|
| ⭐ **Star Brand** | Infinix | +0.4305 | 618,540 | Sentimen positif + penjualan tinggi |
| ⭐ **Star Brand** | Vivo | +0.6191 | 385,559 | |
| ⭐ **Star Brand** | Samsung | +0.3905 | 428,385 | |
| ⭐ **Star Brand** | Apple | +0.4194 | 97,177 | |
| ⭐ **Star Brand** | Xiaomi | +0.5990 | 144,710 | |
| 💎 **Hidden Gem** | Motorola | −0.0830 | 1,610 | Sentimen netral-negatif tapi penjualan rendah |
| ⚠️ **Overrated** | Oppo | −0.2890 | 71,376 | Sentimen negatif tapi penjualan masih tinggi |
| 📉 **Underperformer** | Asus | −0.5104 | 73 | Sentimen negatif + penjualan rendah |
| 📉 **Underperformer** | iQOO | −0.3783 | 2,100 | |
| 📉 **Underperformer** | Realme | −0.7111 | 42,424 | |
| 📉 **Underperformer** | Tecno | −0.8845 | 22,778 | |

### Interpretasi per Kuadran

**Star Brand (5 brand):** Infinix, Vivo, Samsung, Apple, dan Xiaomi adalah brand yang berhasil membangun persepsi positif sekaligus mencetak penjualan yang kuat. Kelima brand ini layak dijadikan benchmark. Namun perlu dicatat, "Star Brand" tidak berarti identik, Vivo unggul di sentimen, Infinix unggul di volume, Samsung unggul di konsistensi.

**Overrated — Oppo:** Oppo memiliki weighted sentiment negatif (−0.2890) tetapi masih berhasil menjual 71.376 unit. Ini menunjukkan Oppo masih hidup dari inersia brand lama dan mungkin loyalitas pelanggan yang sudah ada, meskipun persepsi baru di YouTube tidak menguntungkan. Tanpa perbaikan sentimen, posisi Oppo berisiko tergerus dalam jangka menengah.

**Hidden Gem — Motorola:** Kategorisasi Motorola sebagai "Hidden Gem" perlu diinterpretasikan dengan hati-hati. Penjualan Motorola yang sangat rendah (1.610 unit) kemungkinan besar disebabkan oleh distribusi yang terbatas di e-commerce Indonesia, bukan semata karena persepsi negatif. Sentimen −0.0830 hampir netral, dan ini bisa berarti peluang jika distribusinya diperbaiki.

**Underperformer (4 brand):** Asus, iQOO, Realme, dan Tecno menghadapi tantangan ganda — sentimen negatif sekaligus penjualan rendah. Tecno adalah yang paling kritis dengan sentiment score terendah (−0.8845) dan composite score terendah (0.0637).

---

## Insight 4: Harga vs Total Terjual. Apakah Mahal Berarti Laku?

### Data Harga dan Penjualan per Brand

| Brand | Avg Harga (Rp) | Total Terjual | Segmen Harga |
|---|---|---|---|
| Nokia | 436,720 | 56,666 | Budget |
| Infinix | 1,918,194 | 618,540 | Budget |
| Tecno | 2,483,104 | 22,778 | Budget |
| Realme | 2,525,422 | 42,424 | Budget |
| Vivo | 2,983,920 | 385,559 | Mid-range |
| Xiaomi | 3,136,497 | 144,710 | Mid-range |
| Motorola | 3,616,361 | 1,610 | Mid-range |
| Oppo | 3,487,174 | 71,376 | Mid-range |
| iQOO | 4,173,743 | 2,100 | Mid-range |
| Samsung | 4,412,702 | 428,385 | Mid-high |
| Poco | 3,645,838 | 38,505 | Mid-range |
| Asus | 6,544,578 | 73 | Premium |
| Huawei | 6,896,000 | 630 | Premium |
| Apple | 8,205,675 | 97,177 | Premium |
| Huawei | 6,896,000 | 630 | Premium |
| Sony | 10,495,600 | 52 | Ultra-premium |

### Interpretasi

Dari hasil uji korelasi, **harga tidak terbukti menjadi penentu utama penjualan** (r = −0.3239, p = 0.2586). Beberapa pola menarik yang terlihat dari data:

Infinix membuktikan bahwa harga budget (Rp 1,9 juta) yang dikombinasikan dengan strategi pasar yang tepat bisa menghasilkan penjualan tertinggi. Sebaliknya, Nokia yang bahkan lebih murah (Rp 437 ribu) tidak berhasil mencetak penjualan yang sebanding sehingga membuktikan bahwa harga murah saja tidak cukup tanpa ekosistem distribusi dan brand awareness yang kuat.

Samsung adalah kasus paling menarik: dengan harga rata-rata Rp 4,4 juta (tertinggi di antara brand volume tinggi), Samsung tetap berhasil menjual 428.385 unit. Hal ini dimungkinkan oleh lini produk yang sangat luas karena Samsung berjualan dari entry-level hingga flagship, sehingga rata-rata harganya "tertarik ke atas" oleh model flagship tetapi volume penjualannya didorong oleh model mid-range.

Apple menunjukkan pola yang konsisten untuk segmen premium: harga tinggi (Rp 8,2 juta) dengan penjualan yang "wajar" untuk segmen tersebut (97.177 unit). Brand premium Apple cukup kuat untuk mempertahankan permintaan meski di harga yang jauh di atas rata-rata.

**Kesimpulan Insight 4:** Pertanyaan "apakah mahal = laku?" tidak bisa dijawab dengan ya atau tidak. Yang lebih tepat adalah: **segmen menentukan ekspektasi**. Brand budget seperti Infinix berkompetisi di volume; brand premium seperti Apple berkompetisi di margin. Yang gagal adalah brand yang terjebak di tengah tanpa kejelasan positioning seperti Asus dan Huawei yang berharga premium tetapi tidak memiliki brand pull yang cukup di pasar Indonesia.

---

## Kesimpulan Laporan Value

Dari keempat insight di atas, dapat ditarik beberapa poin kunci:

1. **Infinix adalah brand dengan performa paling seimbang** antara sentimen dan penjualan untuk konteks pasar Indonesia saat ini, didorong oleh dominasi di segmen budget yang merupakan segmen terbesar.

2. **Sentimen berkorelasi positif dengan penjualan**, namun hubungannya tidak deterministik karena faktor distribusi, promosi, dan brand legacy tetap berperan besar.

3. **Harga bukan penentu utama penjualan**, brand berhasil bukan karena murah atau mahal, tetapi karena konsisten melayani segmen yang dipilihnya dengan baik.

4. **Oppo perlu perhatian khusus**, satu-satunya brand yang masih menjual cukup banyak meskipun sentimennya negatif, mengindikasikan risiko penurunan yang mungkin tertunda bukan terhindar.

5. **Keterbatasan analisis utama** adalah inkonsistensi volume data antar brand (terutama Sony dengan 9 komentar dan 52 unit terjual) yang dapat mendistorsi hasil composite score jika tidak diberi catatan.

---

*Laporan ini merupakan bagian dari proyek akhir analisis sentimen smartphone. Lihat juga: `laporan_variability.md` untuk analisis tren dan perbandingan model, serta `kesimpulan_project.md` untuk ringkasan eksekutif.*
