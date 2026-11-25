# Exploratory Data Analysis (EDA): Karakteristik Struktural dan Perilaku File PDF untuk Klasifikasi Dokumen Berbahaya (Malicious PDF)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/drive/1r62kNx2cVDzWwoXd3luLg6ufU_0_X5g2?usp=sharing)
![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas&logoColor=white)
![Seaborn](https://img.shields.io/badge/Seaborn-Visualization-4C72B0)
![Scikit-Learn](https://img.shields.io/badge/Scikit--Learn-Preprocessing-F7931E?logo=scikit-learn&logoColor=white)

## 📌 Informasi Proyek
* **Disusun oleh:** Yahyati Aulia
* **NIM:** 26MA41D001
* **Dosen Pengampu:** Dr. Giat Karyono, M.Kom.
* **Program Studi:** Magister Ilmu Komputer, Fakultas Ilmu Komputer
* **Institusi:** Universitas Amikom Purwokerto (2026)

---

## 📖 Ringkasan Proyek
File *Portable Document Format* (PDF) merupakan format dokumen digital yang sangat umum digunakan, namun sering dimanfaatkan sebagai media penyebaran *malware* melalui penyisipan skrip berbahaya (JavaScript), tindakan otomatis (*OpenAction*, *Launch*), maupun manipulasi struktur internal.

Proyek ini melakukan **Exploratory Data Analysis (EDA)** secara menyeluruh untuk memahami perbedaan karakteristik struktural dan perilaku antara dokumen PDF normal (**Benign**) dan dokumen PDF berbahaya (**Malicious**), mengevaluasi kualitas data, melakukan transformasi serta rekayasa fitur, hingga merumuskan rekomendasi model *machine learning* untuk tahap klasifikasi.

---

## 📊 Profil Dataset
Dataset yang digunakan adalah `PDF_All_features.csv` yang dapat diakses langsung melalui repositori ini:
* **Sumber Dataset (Raw CSV):** [`PDF_All_features.csv`](https://raw.githubusercontent.com/auliayahyati/EDA-Malicious-PDF/main/dataset/PDF_All_features.csv)
* **Dimensi Awal:** 19.296 baris (sampel PDF) × 42 kolom (39 `int64`, 2 `float64`, 1 `object`).
* **Distribusi Kelas Target (`label`):**
  * **Benign (0):** 9.297 dokumen (48,18%)
  * **Malicious (1):** 9.999 dokumen (51,82%)
* **Kualitas Data:** 0 *missing values* (0%), 0 data duplikat (0%), dan 0 nilai numerik negatif.

---

## 🔍 Tahapan Analisis (Alur EDA)
1. **Memahami Masalah & Data:** Perumusan masalah klasifikasi biner, inspeksi struktur, tipe data, dan statistik deskriptif awal.
2. **Penanganan Missing Values:** Verifikasi kelengkapan data (100% terisi tanpa perlu eliminasi atau imputasi).
3. **Eksplorasi Karakteristik Data:** Analisis ukuran pemusatan, penyebaran, *skewness*, *kurtosis*, dan identifikasi 8 fitur konstan (*zero-variance*).
4. **Transformasi Data & Feature Engineering:**
   * Penghapusan kolom identitas (`file_path`) dan 8 fitur konstan sehingga tersisa 32 fitur prediktor.
   * Penerapan *Log Transformation* (`np.log1p`) untuk menekan kemencengan ekstrem pada 27 fitur numerik.
   * Perbandingan skala menggunakan `StandardScaler` dan `MinMaxScaler`.
   * Pembuatan fitur baru: `bytes_per_page`, `objects_per_page`, `streams_per_object`, `total_suspicious_actions`, `has_javascript`, dan `file_size_category` (*binning* + *Ordinal Encoding*).
5. **Visualisasi Hubungan Data:** Analisis *univariate*, *bivariate* (*scatterplot*, *grouped boxplot*, *violin plot*), serta korelasi *Spearman* dan deteksi multikolinearitas.
6. **Penanganan Pencilan (Outliers):** Deteksi menggunakan IQR dan Z-Score, analisis proporsi label pada pencilan, serta penerapan strategi **Retention + Transformasi `log1p`**.

---

## 💡 Temuan Utama (Key Insights)
* **Profil Struktural yang Kontras:** Dokumen *Malicious* cenderung jauh lebih ringan dan minim struktur dibandingkan *Benign*. Perbandingan median menunjukkan `file_size` 9.686 byte vs 76.532 byte, `page_count` 0 vs 2 halaman, `metadata_size` 0 vs 250 byte, `text_length` 0 vs 6.217 karakter, dan `entropy_of_streams` 0,0 vs 6,02.
* **Validitas Header PDF:** Hampir seluruh dokumen tanpa *header* PDF valid (`valid_pdf_header = 0`, sebanyak 7.495 dokumen) berasal dari kategori *Malicious*.
* **Segmentasi Ukuran File:** Sebanyak **61,34%** dokumen *Malicious* berada pada kategori ukuran `Kecil` (hanya 3,22% pada *Benign*), sedangkan **63,41%** dokumen *Benign* berada pada kategori `Besar`.
* **Indikator Perilaku & Nilai Ekstrem:** Elemen JavaScript jauh lebih sering ditemukan pada *Malicious* (`javascript_count` 15,65% vs 3,61%; `js_count` 15,62% vs 4,51%). Bahkan, **82,33%** data pencilan pada `javascript_count` dan **78,85%** pada `js_count` merupakan dokumen *Malicious*.
* **Anomali Menarik:**
  * Fitur `uri_count` justru jauh lebih banyak ditemukan pada dokumen *Benign* (14,18% dengan rata-rata 2,84 URI) dibandingkan *Malicious* (0,26% dengan rata-rata 0,04 URI).
  * Meskipun berukuran kecil, rata-rata kepadatan objek per halaman (`objects_per_page`) pada dokumen *Malicious* mencapai **130,15 objek/halaman**, lebih dari dua kali lipat dokumen *Benign* (56,33 objek/halaman).
* **Korelasi Spearman Terkuat:**
  * *Korelasi Negatif (Cenderung Benign):* `object_count` (-0,8279), `metadata_size` (-0,8094), `total_filters` (-0,8074), `stream_count` & `endstream_count` (-0,7994), `page_count` (-0,7909).
  * *Korelasi Positif (Cenderung Malicious):* `xfa_count` (+0,2115), `has_multiple_behavioral_keywords_in_one_object` (+0,2073), `javascript_count` (+0,2005), `js_count` (+0,1809).
* **Multikolinearitas:** Ditemukan korelasi sangat tinggi ($|r| \ge 0{,}95$) antar beberapa pasangan fitur, seperti `stream_count` dengan `endstream_count` ($r = 1{,}0000$).

---

## 🚀 Rekomendasi Tahap Pemodelan Selanjutnya
1. **Algoritma Klasifikasi:** Direkomendasikan menggunakan model berbasis pohon (*tree-based ensemble*) seperti **Random Forest**, **XGBoost**, atau **LightGBM** karena tangguh terhadap *outliers* yang dipertahankan, tidak mensyaratkan distribusi normal, dan efektif menangkap interaksi non-linear.
2. **Preprocessing & Seleksi Fitur:** Hapus `file_path` serta 8 fitur *zero-variance*, sertakan fitur hasil *feature engineering* (`objects_per_page`, `has_javascript`, `file_size_category`), dan lakukan seleksi fitur untuk mereduksi multikolinearitas.
3. **Evaluasi Model:** Karena distribusi kelas sudah seimbang (48,18% vs 51,82%), teknik *oversampling* (seperti SMOTE) tidak wajib diterapkan. Evaluasi sebaiknya memprioritaskan **Recall** dan **F1-Score** pada kelas *Malicious* guna meminimalkan *False Negative*.

---

## 🔗 Tautan Proyek
* **Repositori GitHub:** [https://github.com/auliayahyati/EDA-Malicious-PDF](https://github.com/auliayahyati/EDA-Malicious-PDF/tree/main)
* **Google Colab Notebook:** [Buka Notebook EDA](https://colab.research.google.com/drive/1r62kNx2cVDzWwoXd3luLg6ufU_0_X5g2?usp=sharing)
* **Dataset CSV:** [PDF_All_features.csv](https://raw.githubusercontent.com/auliayahyati/EDA-Malicious-PDF/main/dataset/PDF_All_features.csv)

---

## ⚙️ Cara Menjalankan Notebook
1. Klik tombol **Open In Colab** di bagian atas halaman ini, atau
2. *Clone* repositori ini ke komputer lokal:
   ```bash
   git clone [https://github.com/auliayahyati/EDA-Malicious-PDF.git](https://github.com/auliayahyati/EDA-Malicious-PDF.git)
   cd EDA-Malicious-PDF
