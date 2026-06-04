# Laporan Variability: Perbandingan Model & Distribusi Harga Brand Smartphone

**Proyek:** Analisis Sentimen Smartphone di YouTube & E-Commerce

**Tanggal:** 3 Juni 2025

---

## Summary

Laporan ini menyajikan dua dimensi variabilitas dalam analisis brand smartphone. Pertama, perbandingan ranking sentimen antara pendekatan **Raw Sentiment** dan **Weighted Sentiment**, untuk melihat seberapa besar pembobotan volume data mengubah kesimpulan. Kedua, distribusi harga antar brand berdasarkan data e-commerce, untuk memahami sebaran posisi harga tiap brand di pasar Indonesia.

Temuan utama menunjukkan bahwa pembobotan mengubah ranking beberapa brand secara signifikan, dengan Vivo sebagai kasus paling dramatis (turun dari rank 1 ke rank 3). Sementara dari sisi harga, terdapat kesenjangan yang sangat lebar antara brand budget dan premium yang mencerminkan segmentasi pasar tajam di industri smartphone Indonesia.

---

## Bagian 1: Perbandingan Raw Sentiment vs Weighted Sentiment

### 1.1 Latar Belakang

Setelah prediksi sentimen dilakukan menggunakan model RoBERTa, data diagregasi per brand. Ditemukan ketidakseimbangan volume komentar yang sangat ekstrem antar brand:

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
| iPhone (Apple) | 127 |
| iQOO | 89 |
| Asus | 49 |

Xiaomi memiliki **20× lebih banyak komentar** dibanding Asus. Jika menggunakan raw average, brand dengan komentar sangat sedikit bisa mendapatkan skor ekstrem yang tidak merepresentasikan kondisi pasar riil. Untuk itu digunakan **weighted sentiment** yang memperhitungkan proporsi volume sebagai bobot kepercayaan.

### 1.2 Rumus Weighted Sentiment

```
weighted_sentiment(brand) = sentiment_score_avg(brand) × (jumlah_komentar(brand) / total_komentar)
```

Brand dengan lebih banyak komentar mendapat bobot lebih besar karena datanya lebih representatif. Brand dengan sedikit komentar skornya otomatis lebih kecil, mencerminkan tingkat kepercayaan yang lebih rendah.

### 1.3 Tabel Perbandingan: Raw vs Weighted Sentiment

| Brand | Jumlah Komentar | Raw Score | Rank Raw | Weighted Score | Rank Weighted | Perubahan |
|---|---|---|---|---|---|---|
| Xiaomi | 976 | 0.0881 | 2 | 0.025504 | 1 | ↑ +1 |
| Infinix | 487 | 0.0698 | 3 | 0.010083 | 2 | ↑ +1 |
| **Vivo** | **182** | **0.1593** | **1** | **0.008600** | **3** | **↓ −2** |
| Samsung | 460 | 0.0435 | 5 | 0.005931 | 4 | ↑ +1 |
| iPhone | 127 | 0.0630 | 4 | 0.002372 | 5 | ↓ −1 |
| Motorola | 336 | 0.0000 | 6 | 0.000000 | 6 | → 0 |
| Asus | 49 | −0.1020 | 9 | −0.001483 | 7 | ↑ +2 |
| iQOO | 89 | −0.0899 | 8 | −0.002372 | 8 | → 0 |
| Oppo | 213 | −0.0704 | 7 | −0.004448 | 9 | ↓ −2 |
| Realme | 137 | −0.1533 | 11 | −0.006228 | 10 | ↑ +1 |
| Tecno | 316 | −0.1266 | 10 | −0.011862 | 11 | ↓ −1 |

*Hanya 11 brand yang memenuhi syarat minimum 30 komentar YouTube*

### 1.4 Visualisasi Perbandingan (Deskriptif)

Distribusi perubahan ranking setelah pembobotan:

```
Brand yang NAIK setelah weighting:
  Xiaomi    ▲ +1  976 komentar (data paling representatif)
  Infinix   ▲ +1  487 komentar (basis data kuat)
  Samsung   ▲ +1  460 komentar (konsisten)
  Asus      ▲ +2  49 komentar (skor negatifnya "teredam" karena volume kecil)
  Realme    ▲ +1  137 komentar (idem)

Brand yang TURUN setelah weighting:
  Vivo      ▼ −2  182 komentar (raw score tinggi tapi sampel kecil)
  iPhone    ▼ −1  127 komentar (idem)
  Oppo      ▼ −2  213 komentar (volume tidak mengimbangi skor negatif)
  Tecno     ▼ −1  316 komentar (skor negatif dalam, volume besar justru memperbesar dampak negatif)

Brand yang TIDAK BERUBAH:
  Motorola  → 0   (skor nol, tidak terpengaruh arah pembobotan)
  iQOO      → 0   (posisi stabil di rank 8)
```

### 1.5 Interpretasi Kasus per Kasus

**Vivo — Kasus Paling Dramatis (Raw Rank 1 → Weighted Rank 3)**

Vivo memiliki raw sentiment tertinggi (0.1593), namun hanya berdasarkan 182 komentar. Setelah pembobotan, skor ini "ditekan" karena volumenya kecil dibandingkan Xiaomi (976) dan Infinix (487). Hal ini bukan berarti Vivo buruk namun pengguna Vivo yang berkomentar memang lebih positif secara rata-rata. Namun dengan sampel yang lebih kecil, klaim "Vivo paling positif" kurang kuat secara statistik.

**Xiaomi — Naik ke Posisi Puncak (Raw Rank 2 → Weighted Rank 1)**

Dengan 976 komentar (hampir dua kali lipat Infinix), data Xiaomi adalah yang paling representatif dari semua brand. Weighted sentiment Xiaomi (0.025504) hampir 2,5× lebih tinggi dari Infinix (0.010083), mencerminkan betapa dominannya volume komentar dalam pembobotan ini.

**Asus — Naik meski Skor Negatif (Raw Rank 9 → Weighted Rank 7)**

Ini adalah kasus yang sering disalahpahami. Asus naik bukan karena sentimennya membaik, melainkan karena volume komentarnya yang sangat kecil (49) membuat dampak negatifnya kecil dalam sistem pembobotan. Weighted sentiment Asus (−0.001483) jauh lebih kecil absolutnya dibanding Tecno (−0.011862) yang memiliki 316 komentar dengan skor negatif.

**Tecno — Konsisten di Posisi Terbawah**

Tecno adalah satu-satunya brand yang turun setelah weighting meskipun volumenya cukup besar (316 komentar). Hal ini justru memperburuk posisinya: volume yang besar dikombinasikan dengan skor negatif dalam (−0.1266) menghasilkan weighted sentiment paling rendah (−0.011862). Data Tecno dianggap paling "terpercaya" di antara brand-brand negatif dan kepercayaan itu mengonfirmasi bahwa masalah sentimennya memang nyata.

### 1.6 Kesimpulan: Mengapa Weighted Lebih Baik?

| Aspek | Raw Sentiment | Weighted Sentiment |
|---|---|---|
| Perlakuan sampel kecil | Sama dengan sampel besar | Diberi bobot lebih kecil |
| Risiko distorsi outlier | Tinggi (Vivo bisa over-klaim) | Rendah |
| Representativitas pasar | Rendah | Lebih tinggi |
| Konsistensi dengan evaluasi model | Tidak (evaluasi model pakai weighted avg) | Ya (konsisten) |

Pendekatan weighted dipilih karena konsisten dengan filosofi analisis secara keseluruhan: semakin banyak data, semakin terpercaya kesimpulannya. Penggunaan weighted average juga selaras dengan cara evaluasi model dilakukan (weighted average karena distribusi kelas tidak seimbang).

---

## Bagian 2: Distribusi Harga per Brand

### 2.1 Catatan Metodologi

Data harga yang tersedia adalah **satu titik waktu** (snapshot dari hasil scraping e-commerce). Tidak tersedia data historis untuk membuat grafik tren waktu. Oleh karena itu, analisis ini disajikan sebagai **distribusi dan perbandingan harga antar brand** yang justru lebih informatif untuk memahami posisi segmen masing-masing brand di pasar Indonesia.

### 2.2 Data Distribusi Harga (16 Brand E-Commerce)

| Brand | Avg Harga (Rp) | Total Listing | Total Terjual | Segmen |
|---|---|---|---|---|
| Nokia | 436,720 | 65 | 56,666 | Budget |
| Infinix | 1,918,194 | 582 | 618,540 | Budget |
| Tecno | 2,483,104 | 374 | 22,778 | Budget-Mid |
| Realme | 2,525,422 | 297 | 42,424 | Budget-Mid |
| Vivo | 2,983,920 | 347 | 385,559 | Mid-range |
| Xiaomi | 3,136,497 | 581 | 144,710 | Mid-range |
| Oppo | 3,487,174 | 375 | 71,376 | Mid-range |
| Poco | 3,645,838 | 121 | 38,505 | Mid-range |
| Motorola | 3,616,361 | 36 | 1,610 | Mid-range |
| iQOO | 4,173,743 | 14 | 2,100 | Mid-high |
| Samsung | 4,412,702 | 818 | 428,385 | Mid-high |
| Asus | 6,544,578 | 20 | 73 | Premium |
| Huawei | 6,896,000 | 8 | 630 | Premium |
| Apple | 8,205,675 | 154 | 97,177 | Premium |
| Sony | 10,495,600 | 5 | 52 | Ultra-premium |

*16 brand dari data e-commerce sebelum filter merge dengan data sentimen YouTube*

### 2.3 Visualisasi Distribusi (Deskriptif)

Sebaran harga rata-rata dari terendah ke tertinggi:

```
Segmen Budget (< Rp 2 juta):
  Nokia    Rp    437K ████ (56.666 terjual)
  Infinix  Rp  1,918K ████████████████████ (618.540 terjual) ← DOMINAN

Segmen Budget-Mid (Rp 2–3 juta):
  Tecno    Rp  2,483K ████ (22.778 terjual)
  Realme   Rp  2,526K ██████ (42.424 terjual)

Segmen Mid-range (Rp 3–4 juta):
  Vivo     Rp  2,984K ████████████ (385.559 terjual)
  Xiaomi   Rp  3,136K █████ (144.710 terjual)
  Oppo     Rp  3,487K ████ (71.376 terjual)
  Poco     Rp  3,646K ███ (38.505 terjual)
  Motorola Rp  3,616K █ (1.610 terjual)

Segmen Mid-high (Rp 4–6 juta):
  iQOO     Rp  4,174K █ (2.100 terjual)
  Samsung  Rp  4,413K ████████████ (428.385 terjual) ← ANOMALI POSITIF

Segmen Premium (> Rp 6 juta):
  Asus     Rp  6,545K ░ (73 terjual)
  Huawei   Rp  6,896K ░ (630 terjual)
  Apple    Rp  8,206K ████ (97.177 terjual)
  Sony     Rp 10,496K ░ (52 terjual)
```

### 2.4 Pola dan Temuan Menarik

**Temuan 1: Infinix mendominasi segmen yang paling besar**

Segmen budget Rp 1–2 juta adalah segmen dengan volume terjual terbesar di dataset ini. Infinix berhasil mengambil posisi dominan di sini dengan 618.540 unit terjual, hampir **60% dari total penjualan seluruh brand** yang dianalisis. Ini bukan kebetulan; harga Rp 1,9 juta berada tepat di sweet spot daya beli kelas menengah ke bawah Indonesia.

**Temuan 2: Samsung adalah outlier positif di segmen mid-high**

Samsung dengan harga rata-rata Rp 4,4 juta (tertinggi di antara brand volume tinggi) tetap berhasil menjual 428.385 unit. Ini dimungkinkan karena rata-rata harga Samsung "tertarik ke atas" oleh model flagship (Galaxy S-series), sementara volume penjualan sesungguhnya didorong oleh model mid-range (Galaxy A-series). Samsung adalah brand yang paling berhasil melayani banyak segmen sekaligus.

**Temuan 3: Kesenjangan harga premium sangat lebar dan tidak berbanding lurus dengan penjualan**

Jarak harga antara Infinix (Rp 1,9 juta) dan Sony (Rp 10,5 juta) mencapai **5,5× lipat**. Namun penjualan Sony (52 unit) hanya 0,008% dari penjualan Infinix. Ini mengonfirmasi bahwa pasar smartphone Indonesia sangat terkonsentrasi di segmen budget-mid, dan brand premium menghadapi pasar yang jauh lebih kecil secara volume.

**Temuan 4: Harga murah saja tidak menjamin penjualan tinggi**

Nokia dengan harga hanya Rp 437 ribu (termurah di antara semua brand) hanya berhasil menjual 56.666 unit, jauh di bawah Infinix yang 11× lebih mahal. Ini memperkuat temuan dari analisis korelasi bahwa **brand awareness, distribusi, dan ekosistem produk lebih menentukan penjualan daripada harga semata**.

**Temuan 5: Brand premium tanpa ekosistem kuat hampir tidak terlihat**

Asus (73 unit), Huawei (630 unit), dan Sony (52 unit) ketiganya berharga di atas Rp 6 juta, hampir tidak memiliki jejak penjualan yang berarti. Berbeda dengan Apple yang dengan harga Rp 8,2 juta masih berhasil menjual 97.177 unit, didukung oleh ekosistem iOS dan loyalitas pelanggan yang sangat kuat.

### 2.5 Distribusi Harga — Ringkasan Statistik

| Metrik | Nilai |
|---|---|
| Brand termurah | Nokia (Rp 437K) |
| Brand termahal | Sony (Rp 10,5 juta) |
| Rentang harga | Rp 437K – Rp 10,5 juta (24× lipat) |
| Brand dengan volume terjual terbesar | Infinix (618.540 unit) |
| Brand dengan volume terjual terkecil | Sony (52 unit) |
| Median harga (dari 16 brand) | ~Rp 3,5 juta (mid-range) |

---

## Kesimpulan Laporan Variability

**Dari sisi perbandingan model sentimen:**

Penggunaan weighted sentiment terbukti memberikan gambaran yang lebih akurat dibandingkan raw average, terutama untuk brand dengan volume komentar ekstrem di kedua ujung. Perubahan ranking yang terjadi khususnya Vivo turun dan Xiaomi naik ini merupakan koreksi yang metodologis, bukan kelemahan data. Laporan ini merekomendasikan penggunaan weighted sentiment sebagai metrik utama dalam seluruh analisis lanjutan.

**Dari sisi distribusi harga:**

Pasar smartphone Indonesia terpolarisasi: volume terbesar ada di segmen budget (didominasi Infinix), sementara brand premium bersaing di pasar yang jauh lebih kecil secara volume. Samsung adalah pengecualian yang berhasil menjangkau berbagai segmen sekaligus berkat lini produk yang paling luas. Brand yang gagal adalah mereka yang berada di segmen premium tanpa brand equity yang cukup kuat untuk membenarkan harga premiumnya di mata konsumen Indonesia.

---

*Laporan ini merupakan bagian dari proyek akhir analisis sentimen smartphone. Lihat juga: `laporan_value.md` untuk analisis komparatif brand, serta `kesimpulan_presentasi.md` untuk ringkasan eksekutif.*
