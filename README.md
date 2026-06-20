# 📱 Multi-Platform Big Data Analysis: Smartphone Sales & Sentiment Correlation

> Analisis korelasi antara sentimen publik di YouTube dengan performa penjualan smartphone di marketplace Indonesia (Shopee, Lazada, Tokopedia).

---

## 🎯 Deskripsi Project

Project ini menganalisis **8 dimensi Big Data (8V)** menggunakan dua sumber data utama:

| Sumber | Deskripsi | Jumlah Data |
|--------|-----------|-------------|
| 🛒 E-Commerce | Listing produk smartphone dari Shopee, Lazada, Tokopedia | 4.437 produk bersih |
| 🎥 YouTube | Komentar dari 4 channel review gadget Indonesia | 7.412 komentar bersih |

**Pertanyaan riset utama:**
> *Apakah brand smartphone yang mendapat sentimen positif di YouTube juga memiliki penjualan lebih tinggi di marketplace?*

**8V Big Data yang dianalisis:**

| No | Dimensi | Fokus Analisis |
|----|---------|-----------------|
| 1 | **Volume** | Jumlah data e-commerce & komentar YouTube yang diproses |
| 2 | **Variety** | Ragam sumber (3 marketplace + 4 channel YouTube), format data |
| 3 | **Velocity** | Kecepatan akumulasi komentar & listing produk dari waktu ke waktu |
| 4 | **Value** | Nilai bisnis dari korelasi sentimen ↔ penjualan |
| 5 | **Veracity** | Tingkat kebenaran & kebersihan data setelah preprocessing |
| 6 | **Variability** | Inkonsistensi makna/sentimen pada konteks yang berbeda-beda |
| 7 | **Validity** | Kesesuaian data dengan tujuan analisis (relevansi & struktur) |
| 8 | **Visualization** | Representasi visual data melalui dashboard interaktif |

---

## 👥 Anggota Tim

| Nama | Tugas Utama |
|------|-------------|
| Retno | Cleaning e-commerce, Korelasi & Gap Analysis, Laporan Value & Variability |
| Zelga | Cleaning YouTube, RoBERTa Model, Laporan Volume |
| Raissa | README, Laporan Variety & Veracity, EDA E-Commerce, Dashboard Interaktif (HTML), Control dan backup |
| Putri | EDA YouTube, Sentiment Analysis, Rule-Based Model, Laporan Validity |

---

## 🗂️ Struktur Folder

```
project/
│
├── 📁 data_raw/                    # Data mentah (sebelum cleaning)
│   ├── data_shopee_kotor.csv
│   ├── data_lazada_kotor.csv
│   ├── data_tokopedia_kotor.csv
│   ├── gabungan_e-commerce_gadget.csv
│   └── DATA_KOMEN_FULL_3_VIDEO.csv
│
├── 📁 data_clean/                  # Data bersih hasil preprocessing
│   ├── data_shopee_clean.csv
│   ├── data_lazada_clean.csv
│   ├── data_tokopedia_clean.csv
│   ├── ecommerce_clean_merged.csv  # Gabungan 3 platform
│   ├── Youtubeclean_final.csv      # Data YouTube lengkap
│   └── Youtube_sentiment.csv       # Data siap analisis sentimen
│
├── 📁 notebooks/                   # Jupyter Notebook per tahap
│   ├── 01_preprocessing_shopee.ipynb
│   ├── 02_preprocessing_lazada.ipynb
│   ├── 03_preprocessing_tokopedia.ipynb
│   ├── 04_merge_ecommerce.ipynb
│   ├── Cleaning_Youtube.ipynb
│   ├── 03_eda_youtube.ipynb
│   ├── 04_sentiment_analysis.ipynb
│   ├── 05_rule_base_model.ipynb
│   ├── 05B_Roberta_model.ipynb
│   ├── 05_model_comparison.ipynb
│   ├── 06_correlation_analysis.ipynb
│   ├── 07_gap_analysis.ipynb
│   └── 08_dashboard_preparation.ipynb
│
├── 📁 models/                      # Model hasil training & artefak model
│   ├── rule_based_model/
│   └── roberta_model/
│
├── 📁 laporan/                     # Laporan 8V Big Data
│   ├── laporan_volume.md
│   ├── laporan_variety.md
│   ├── laporan_veracity.md
│   ├── laporan_velocity.md
│   ├── laporan_value.md
│   ├── laporan_variability.md
│   ├── laporan_validity.md
│   └── laporan_visualization.md
│
├── 📁 essai/                       # Catatan eksperimen, draft analisis & eksplorasi tambahan
│
├── index.html                      # Dashboard interaktif (HTML, CSS, JS)
├── README.md                       # File ini
└── requirements.txt                # Dependensi Python
```

---

## ⚙️ Cara Install & Menjalankan

### 1. Clone Repository

```bash
git clone https://github.com/RaissaNadia/smartphone-bigdata-analysis.git
cd smartphone-bigdata-analysis
```

### 2. Buat Virtual Environment (Direkomendasikan)

```bash
python -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
```

### 3. Install Dependensi

```bash
pip install -r requirements.txt
```

### 4. Jalankan Notebook Sesuai Urutan

```bash
jupyter notebook
```

Jalankan notebook **sesuai urutan angka** di folder `notebooks/`:

| Urutan | Notebook | Tujuan |
|--------|----------|--------|
| 1-3 | `01_preprocessing_*.ipynb` | Cleaning per platform |
| 4 | `04_merge_ecommerce.ipynb` | Gabungkan 3 platform |
| 5 | `Cleaning_Youtube.ipynb` | Cleaning komentar YouTube |
| 6-7 | `03_eda_*.ipynb` | Exploratory Data Analysis |
| 8-9 | `04_sentiment_analysis.ipynb` | Analisis sentimen |
| 10-11 | `05_*.ipynb` | Model & perbandingan |
| 12-13 | `06_correlation_analysis.ipynb`, `07_gap_analysis.ipynb` | Korelasi & gap |
| 14 | `08_dashboard_preparation.ipynb` | Persiapan data dashboard |

## 5. Akses Dashboard Interaktif

Dashboard telah berhasil dideploy menggunakan GitHub Pages sehingga dapat diakses secara online tanpa perlu instalasi atau konfigurasi tambahan.

**Live Dashboard:**  
https://raissanadia.github.io/smartphone-bigdata-analysis/

Melalui dashboard ini, pengguna dapat mengeksplorasi hasil analisis data smartphone, visualisasi korelasi antar variabel, perhitungan composite score, serta berbagai insight yang diperoleh dari proses Big Data Analytics.

### Menjalankan Secara Lokal (Opsional)

Jika ingin melakukan pengembangan atau modifikasi dashboard secara mandiri, clone repository terlebih dahulu:

```bash
git clone https://github.com/RaissaNadia/smartphone-bigdata-analysis.git
cd smartphone-bigdata-analysis
```

Karena dashboard menggunakan file CSV dan JSON yang diakses melalui JavaScript, disarankan menjalankannya menggunakan local server:

```bash
python -m http.server 8000
```

Kemudian buka browser dan akses:

```text
http://localhost:8000
```

> Catatan: Penggunaan local server membantu menghindari pembatasan keamanan browser (CORS) saat membaca file data lokal.

---

## 📊 Metodologi

```
Data Raw ──► Preprocessing ──► EDA ──► Sentiment Analysis ──► Korelasi & Gap ──► Dashboard Interaktif (HTML)
              (8V Big Data)          (Rule-Based + RoBERTa)   (Weighted Score)
```

### Pendekatan Analisis Sentimen
Menggunakan **weighted sentiment** untuk menangani ketidakseimbangan data antar brand:

```
weighted_sentiment = sentiment_score_avg × log(1 + jumlah_komentar)
```

### Composite Score
Menggabungkan 4 faktor untuk ranking brand:

```
composite_score = (0.4 × weighted_sentiment) + (0.3 × normalized_terjual) 
                + (0.2 × normalized_rating) + (0.1 × normalized_harga_value)
```

---

## 🔍 Highlight Temuan

- **Brand terbaik**: Berdasarkan composite score, ditampilkan pada dashboard interaktif (`index.html`)
- **Korelasi sentimen-penjualan**: Dianalisis di notebook `06_correlation_analysis.ipynb`
- **Gap analysis**: Brand *overrated* vs *underrated* di `07_gap_analysis.ipynb`
- **Visualisasi**: Seluruh temuan utama dirangkum secara interaktif di dashboard berbasis web (HTML/CSS/JS)

---

## 📦 Dependensi Utama

Lihat `requirements.txt` untuk daftar lengkap. Library utama:

- `pandas`, `numpy` — manipulasi data
- `scikit-learn` — preprocessing & evaluasi model
- `transformers` — model RoBERTa
- `PySastrawi`, `nlp_id` — NLP Bahasa Indonesia
- `matplotlib`, `seaborn`, `plotly` — visualisasi & EDA
- `wordcloud` — visualisasi kata

> Dashboard interaktif **tidak menggunakan Streamlit**, melainkan dibangun langsung dengan HTML, CSS, dan JavaScript (`index.html`) agar bisa dibuka tanpa server backend.

---

## 📝 Catatan

- Data raw tidak di-push ke GitHub (ukuran besar). Hubungi tim untuk akses data.
- Proses stemming YouTube membutuhkan waktu ±5 menit (7.000+ baris).
- Model RoBERTa membutuhkan GPU untuk inferensi yang cepat.
- Folder `essai/` berisi catatan eksperimen dan eksplorasi tambahan di luar pipeline utama.
