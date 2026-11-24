# 🔍 Exploratory Data Analysis (EDA) Karakteristik Struktural dan Perilaku File PDF untuk Klasifikasi Dokumen Berbahaya (Malicious PDF)

![Python](https://img.shields.io/badge/Python-3.8%2B-blue?logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Data%20Analysis-150458?logo=pandas&logoColor=white)
![Machine Learning](https://img.shields.io/badge/Task-Classification-green)
![Status](https://img.shields.io/badge/Status-Completed-success)

## 📌 Deskripsi Proyek

Dokumen *Portable Document Format* (PDF) merupakan salah satu format pertukaran data digital yang paling populer. Namun, kepopuleran PDF juga dimanfaatkan oleh pihak tidak bertanggung jawab untuk menyebarkan *malware* melalui penyisipan skrip JavaScript, *embedded files*, perintah *OpenAction*, serta berbagai aksi otomatis lainnya saat dokumen dibuka.

Proyek ini berfokus pada **Exploratory Data Analysis (EDA)** untuk menganalisis karakteristik struktural dan perilaku dari file PDF, guna membedakan dokumen PDF normal (*benign*) dan dokumen PDF berbahaya (*malicious*). Hasil eksplorasi ini menjadi landasan utama untuk persiapan data, seleksi fitur, serta pemodelan mesin pembelajar (*machine learning classification*).

---

## 👨‍💻 Identitas Peneliti & Institusi

- **Penulis:** Yahyati Aulia
- **Dosen Pengampu:** Dr. Giat Karyono, M.Kom.
- **Program Studi:** Magister Ilmu Komputer, Fakultas Ilmu Komputer
- **Institusi:** Universitas Amikom Purwokerto
- **Tahun:** 2026

---

## 🎯 Tujuan Utama

1. Mengidentifikasi karakteristik struktural dan perilaku file PDF berdasarkan fitur ekstraksi dataset.
2. Menganalisis perbedaan pola antara dokumen PDF *Benign* (normal) dan *Malicious* (berbahaya).
3. Menguji dan menganalisis kualitas data (*missing values*, duplikasi data, konsistensi nilai, dan *outliers*).
4. Mengetahui pola distribusi serta korelasi antarfitur.
5. Mengidentifikasi fitur potensial (*key features*) untuk membedakan PDF normal dan berbahaya.
6. Memberikan rekomendasi metode klasifikasi serta strategi pra-pemrosesan data untuk penelitian selanjutnya.

---

## 📊 Ringkasan Dataset

Dataset yang digunakan adalah `PDF_All_features.csv` yang dipublikasikan pada repositori ini.

| Parameter | Keterangan |
| :--- | :--- |
| **Jumlah Baris (Data)** | 19.296 Sampel |
| **Jumlah Kolom (Fitur)** | 42 Atribut |
| **Tipe Data** | Integer (39), Float (2), Object (1) |
| **Distribusi Kelas Target** | **Benign (0):** 9.297 sampel (48,18%)<br>**Malicious (1):** 9.999 sampel (51,82%) |
| **Missing Values** | 0 (0%) |
| **Data Duplikat** | 0 (0%) |

### Pengelompokan Fitur Utama:
1. **Atribut Identifikasi:** `file_path`
2. **Atribut Struktural:** `file_size`, `page_count`, `object_count`, `stream_count`, `entropy_of_streams`, `font_object_count`, `image_count`, `text_length`, `metadata_size`, dll.
3. **Atribut Perilaku:** `js_count`, `javascript_count`, `uri_count`, `openaction_count`, `launch_count`, `embedded_file_count`, dll.

---

## 💡 Temuan Utama Hasil Eksplorasi (EDA Insights)

### 1. Kualitas Data (Data Quality)
- **Kelengkapan Data:** Tidak ditemukan *missing values* (0 dari 810.432 sel terisi lengkap).
- **Keunikan Data:** 0% data duplikat identik.
- **Konsistensi Nilai:** 0 nilai negatif pada fitur numerik. Fitur biner (`encrypted`, `valid_pdf_header`, `uses_nonstandard_port`, `used_ocr`, `label`) memiliki kategori nilai yang valid (0 dan 1).

### 2. Fitur Tanpa Variasi (Constant / Zero-Variance Features)
Ditemukan 8 fitur yang bernilai 0 di seluruh data (tidak memiliki variasi nilai):
- `embedded_file_count`, `average_embedded_file_size`, `xref_count`, `xref_entries`, `submitform_count`, `jbig2decode_count`, `trailer_count`, `startxref_count`.
> **Rekomendasi:** Fitur-fitur ini direkomendasikan untuk dihapus pada tahap *feature selection* sebelum pelatihan model klasifikasi.

### 3. Perbedaan Karakteristik Benign vs Malicious

| Fitur / Karakteristik | PDF Benign (0) | PDF Malicious (1) | Analisis & Implikasi |
| :--- | :--- | :--- | :--- |
| **Ukuran File (`file_size`)** | Rata-rata **123,79 KB** (Median: 76,53 KB) | Rata-rata **22,51 KB** (Median: 9,68 KB) | PDF Malicious cenderung berukuran jauh lebih kecil dibanding PDF Benign. |
| **Jumlah Halaman (`page_count`)** | Rata-rata **6,19 halaman** (Median: 2) | Rata-rata **0,48 halaman** (Median: 0) | PDF Malicious umumnya tidak fokus pada isi dokumen (halaman sedikit/0). |
| **Jumlah Objek (`object_count`)** | Rata-rata **280,19** (Median: 71) | Rata-rata **261,02** (Median: 0, ada pencilan 200.000) | Distribusi sangat *skewed* ke kanan dengan *outlier* ekstrem pada kategori Malicious. |
| **JavaScript (`javascript_count`)** | Ditemukan pada **3,61%** dokumen | Ditemukan pada **15,65%** dokumen | Keberadaan skrip JavaScript jauh lebih dominan pada dokumen Malicious. |
| **Elemen JS (`js_count`)** | Ditemukan pada **4,51%** dokumen | Ditemukan pada **15,62%** dokumen | Berpola sejalan dengan `javascript_count`. |
| **Aksi Otomatis (`openaction_count`)** | Persentase **7,31%** (Mean: 0,0786) | Persentase **9,91%** (Mean: 0,0992) | Lebih banyak ditemukan pada dokumen Malicious untuk eksekusi skrip otomatis. |
| **Tautan Web (`uri_count`)** | Rata-rata **2,8381** (Kemunculan: **14,18%**) | Rata-rata **0,0399** (Kemunculan: **0,26%**) | Dokumen Benign justru lebih sering menyertakan alamat tautan/URI resmi. |

---

## 🛠️ Stack & Library yang Digunakan

- **Bahasa Pemrograman:** Python 3.x
- **Platform:** Google Colab / Jupyter Notebook
- **Library Pengolahan Data:** `pandas`, `numpy`
- **Library Visualisasi Data:** `matplotlib`, `seaborn`

---

## 🚀 Cara Menjalankan Program

### 1. Clone Repositori
```bash
git clone https://github.com/auliayahyati/EDA-Malicious-PDF.git
cd EDA-Malicious-PDF
```

### 2. Instalasi Dependensi
```bash
pip install pandas numpy matplotlib seaborn
```

### 3. Eksekusi Script Python / Colab
Anda dapat langsung membaca dataset secara online dari raw GitHub menggunakan kode berikut:

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# URL Dataset
url = "https://raw.githubusercontent.com/auliayahyati/EDA-Malicious-PDF/main/dataset/PDF_All_features.csv"

# Load Dataset
df = pd.read_csv(url)

# Inspeksi Dimensi
print(f"Dimensi Dataset: {df.shape}")
print(df.info())
```

---

## 📈 Rekomendasi Tahap Selanjutnya (Machine Learning Roadmap)

1. **Preprocessing & Feature Selection:**
   - Menghapus kolom identifikasi (`file_path`) dan 8 kolom ber-variansi nol.
   - Melakukan transformasi *logarithmic* atau *scaling* pada fitur yang sangat menceng (*skewed*) seperti `file_size` dan `object_count`.
2. **Model Klasifikasi:**
   - Menggunakan algoritma berbasis pohon seperti **Random Forest**, **XGBoost**, atau **LightGBM** yang tahan terhadap nilai *outlier* dan *skewed distribution*.
3. **Metrik Evaluasi:**
   - Fokus utama pada **Recall** dan **F1-Score** untuk meminimalkan *False Negative* (dokumen berbahaya yang lolos terdeteksi sebagai dokumen normal).

---

## 📄 Lisensi & Hak Cipta

Proyek ini disusun untuk memenuhi tugas akademik pada Program Studi Magister Ilmu Komputer Universitas Amikom Purwokerto. Bebas digunakan dan dikembangkan kembali untuk kepentingan edukasi dan penelitian riset sejenis.
