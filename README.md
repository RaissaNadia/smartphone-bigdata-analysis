Readme · MDCopy📱 Multi-Platform Big Data Analysis: Smartphone Sales & Sentiment Correlation

Analisis korelasi antara sentimen publik di YouTube dengan performa penjualan smartphone di marketplace Indonesia (Shopee, Lazada, Tokopedia).


🎯 Deskripsi Project
Project ini menganalisis 7 dimensi Big Data (7V) menggunakan dua sumber data utama:
SumberDeskripsiJumlah Data🛒 E-CommerceListing produk smartphone dari Shopee, Lazada, Tokopedia4.437 produk bersih🎥 YouTubeKomentar dari 4 channel review gadget Indonesia7.412 komentar bersih
Pertanyaan riset utama:

Apakah brand smartphone yang mendapat sentimen positif di YouTube juga memiliki penjualan lebih tinggi di marketplace?


👥 Anggota Tim
NamaTugas UtamaRetnoCleaning e-commerce, Korelasi & Gap Analysis, Laporan Value & VariabilityZelgaCleaning YouTube, RoBERTa Model, Laporan VolumeRaissaREADME, Laporan Variety & Veracity, EDA E-Commerce, DashboardPutriEDA YouTube, Sentiment Analysis, Rule-Based Model, Laporan Validity

🗂️ Struktur Folder
project/
│
├── 📁 data_raw/                    # Data mentah (sebelum cleaning)
│   ├── data_shopee_kotor.csv
│   ├── data_lazada_kotor.csv
│   ├── data_tokopedia_kotor.csv
│   └── DATA_KOMEN_FULL_3_VIDEO.csv
│
├── 📁 data_clean/                  # Data bersih hasil preprocessing
│   ├── data_shopee_clean.csv
│   ├── data_lazada_clean.csv
│   ├── data_tokopedia_clean.csv
│   ├── ecommerce_clean_merged.csv  # Gabungan 3 platform
│   ├── Youtubeclean_final.csv      # Data YouTube lengkap
│   └── Youtube_sentiment.csv      # Data siap analisis sentimen
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
├── 📁 laporan/                     # Laporan 7V Big Data
│   ├── laporan_volume.md
│   ├── laporan_variety.md
│   ├── laporan_veracity.md
│   ├── laporan_velocity.md
│   ├── laporan_value.md
│   ├── laporan_variability.md
│   └── laporan_validity.md
│
├── 📁 assets/                      # Output visualisasi
│   ├── dashboard_final.png
│   ├── dashboard_preview.png
│   ├── wordcloud_*.png
│   └── heatmap_korelasi.png
│
├── 📁 dashboard/                   # Streamlit app
│   └── app.py
│
├── 📁 results/                     # Output analisis final
│   ├── sentiment_brand_agg.csv
│   ├── final_brand_comparison.csv
│   └── final_ranked.csv
│
├── README.md                       # File ini
└── requirements.txt                # Dependensi Python

⚙️ Cara Install & Menjalankan
1. Clone Repository
bashgit clone https://github.com/<username>/<repo-name>.git
cd <repo-name>
2. Buat Virtual Environment (Direkomendasikan)
bashpython -m venv venv

# Windows
venv\Scripts\activate

# Mac/Linux
source venv/bin/activate
3. Install Dependensi
bashpip install -r requirements.txt
4. Jalankan Notebook Sesuai Urutan
bashjupyter notebook
Jalankan notebook sesuai urutan angka di folder notebooks/:
UrutanNotebookTujuan1-301_preprocessing_*.ipynbCleaning per platform404_merge_ecommerce.ipynbGabungkan 3 platform5Cleaning_Youtube.ipynbCleaning komentar YouTube6-703_eda_*.ipynbExploratory Data Analysis8-904_sentiment_analysis.ipynbAnalisis sentimen10-1105_*.ipynbModel & perbandingan12-1306_correlation_analysis.ipynbKorelasi & gap1408_dashboard_preparation.ipynbPersiapan dashboard
5. Jalankan Dashboard Interaktif
bashstreamlit run dashboard/app.py
Dashboard akan terbuka di browser: http://localhost:8501

📊 Metodologi
Data Raw ──► Preprocessing ──► EDA ──► Sentiment Analysis ──► Korelasi & Gap ──► Dashboard
              (7V Big Data)          (Rule-Based + RoBERTa)   (Weighted Score)   (Streamlit)
Pendekatan Analisis Sentimen
Menggunakan weighted sentiment untuk menangani ketidakseimbangan data antar brand:
weighted_sentiment = sentiment_score_avg × log(1 + jumlah_komentar)
Composite Score
Menggabungkan 4 faktor untuk ranking brand:
composite_score = (0.4 × weighted_sentiment) + (0.3 × normalized_terjual) 
                + (0.2 × normalized_rating) + (0.1 × normalized_harga_value)

🔍 Highlight Temuan

Brand terbaik: Berdasarkan composite score (lihat results/final_ranked.csv)
Korelasi sentimen-penjualan: Dianalisis di notebook 06_correlation_analysis.ipynb
Gap analysis: Brand overrated vs underrated di 07_gap_analysis.ipynb


📦 Dependensi Utama
Lihat requirements.txt untuk daftar lengkap. Library utama:

pandas, numpy — manipulasi data
scikit-learn — preprocessing & evaluasi model
transformers — model RoBERTa
PySastrawi, nlp_id — NLP Bahasa Indonesia
matplotlib, seaborn, plotly — visualisasi
streamlit — dashboard interaktif
wordcloud — visualisasi kata


📝 Catatan

Data raw tidak di-push ke GitHub (ukuran besar). Hubungi tim untuk akses data.
Proses stemming YouTube membutuhkan waktu ±5 menit (7.000+ baris).
Model RoBERTa membutuhkan GPU untuk inferensi yang cepat.
