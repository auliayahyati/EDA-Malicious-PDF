::: {align="center"}
# PDF MALWARE ANALYTICS

### Exploratory Data Analysis of Structural & Behavioral PDF Features

**Membaca pola. Memahami anomali. Mendukung deteksi dokumen berbahaya.**

![Python](https://img.shields.io/badge/Python-Data%20Analysis-3776AB?style=flat-square&logo=python&logoColor=white)
![Pandas](https://img.shields.io/badge/Pandas-Analysis-150458?style=flat-square&logo=pandas&logoColor=white)
![Status](https://img.shields.io/badge/Project-EDA%20Research-00B8D9?style=flat-square)
![Year](https://img.shields.io/badge/Year-2026-7657D5?style=flat-square)
:::01 --- Project Overview
File Portable Document Format (PDF) banyak digunakan untuk bertukar
informasi digital. Namun, struktur PDF juga dapat dimanfaatkan untuk
menyisipkan elemen atau mekanisme yang berkaitan dengan aktivitas
berbahaya, seperti JavaScript, OpenAction, dan Launch.

Proyek ini menerapkan Exploratory Data Analysis (EDA) untuk
mengeksplorasi karakteristik struktural dan perilaku dokumen PDF, serta
mengamati perbedaan pola antara dokumen Benign dan Malicious.

Analisis dilakukan terhadap fitur hasil ekstraksi PDF, mulai dari ukuran
file, jumlah halaman dan objek, stream, metadata, hingga indikator
perilaku dokumen.
Fokus proyek: memahami data dan menemukan pola yang berpotensi
membantu klasifikasi PDF --- bukan membangun atau mengklaim model
deteksi malware yang sudah tervalidasi.

02 --- Research Objectives
- Mengidentifikasi karakteristik struktural dan perilaku file PDF.
- Membandingkan karakteristik dokumen Benign dan Malicious.
- Mengevaluasi kualitas data, termasuk missing values, duplikasi, dan
  outliers.
- Menganalisis distribusi, skewness, kurtosis, serta hubungan
  antarfitur.
- Mengidentifikasi fitur yang berpotensi menjadi pembeda kedua kelas.
- Menyusun rekomendasi untuk tahap klasifikasi machine learning
  berikutnya.
03 --- Dataset Snapshot
  Informasi                                  Keterangan
  Dataset                        PDF_All_features.csv
Total data awal                    19.296 dokumen
Total kolom awal                         42 kolom
Benign (Label 0)                       9.297 (48,18%)
Malicious (Label 1)                    9.999 (51,82%)
Format                                            CSV
Tipe analisis           Binary classification --- EDA

Label target
- 0 --- Benign: dokumen PDF yang dikategorikan normal.
- 1 --- Malicious: dokumen PDF yang dikategorikan berbahaya.
Dataset access
- Dataset
  CSV
- Raw
  CSV
04 --- Analytical Workflow
Analisis disusun secara bertahap untuk menjaga alur eksplorasi data
tetap terstruktur.
  Tahap                               Fokus
  Data Understanding                  Memahami sumber, struktur, dan
karakteristik dataset

  Data Inspection                     Memeriksa dimensi, kolom, tipe
data, dan statistik awal

  Data Quality                        Memeriksa missing values,
duplikasi, konsistensi, dan
outliers

  Univariate Analysis                 Mengamati distribusi setiap fitur

  Bivariate Analysis                  Membandingkan fitur berdasarkan
label Benign dan Malicious

  Distribution Analysis               Mengevaluasi skewness dan kurtosis

  Correlation Analysis                Mengidentifikasi hubungan dan
redundansi antarfitur

  Data Transformation                 Membersihkan fitur tidak informatif
dan menerapkan transformasi
Recommendations                     Menyusun arahan untuk pengembangan
model klasifikasi
05 --- Key Findings
Berikut ringkasan temuan utama dari eksplorasi dataset dalam laporan.
Data quality
- Tidak ditemukan missing values pada 42 kolom.
- Tidak ditemukan baris duplikat.
- Tidak ditemukan nilai numerik negatif pada pemeriksaan yang
  dilakukan.
- Ditemukan nilai ekstrem pada beberapa fitur, sehingga outliers perlu
  dipahami sebagai kemungkinan karakteristik dokumen, bukan otomatis
  dianggap sebagai kesalahan data.
Structural & behavioral patterns
- Dokumen Malicious memiliki rata-rata javascript_count lebih tinggi
  daripada Benign pada analisis awal: 0,3992 dibandingkan 0,2035.
- Persentase kemunculan javascript_count juga lebih tinggi pada
  Malicious: 15,65% dibandingkan 3,61%.
- openaction_count dan launch_count menunjukkan persentase
  kemunculan yang lebih tinggi pada Malicious.
- uri_count justru lebih sering muncul pada Benign dalam dataset
  ini. Karena itu, keberadaan URI tidak dapat dipakai sebagai
  satu-satunya indikator dokumen berbahaya.
- embedded_file_count bernilai nol pada seluruh data awal.
Distribution & correlation
- Sebanyak 27 dari 28 fitur numerik non-biner memiliki kemencengan
  tinggi (|skewness| > 0,5).
- Sejumlah fitur memperlihatkan nilai skewness dan kurtosis yang
  sangat tinggi, mengindikasikan distribusi tidak merata dan nilai
  ekstrem.
- Ditemukan korelasi sangat tinggi pada beberapa pasangan fitur,
  antara lain stream_count dengan endstream_count (korelasi
  Spearman 1,0000) serta javascript_count dengan
  has_multiple_behavioral_keywords_in_one_object (0,9819).
Data preparation
Dalam tahap transformasi, kolom file_path dan delapan fitur
zero-variance dihapus karena tidak memberikan informasi pembeda. Dataset
kerja kemudian memiliki 19.296 baris dan 33 kolom, terdiri dari 32
fitur prediktor dan satu target label.

Transformasi log1p diterapkan pada fitur numerik yang memiliki
kemencengan tinggi untuk membantu mengurangi skewness sekaligus
mengakomodasi nilai nol.
06 --- Technology Stack
  Tools / Library   Penggunaan
  Python            Bahasa pemrograman analisis
Google Colab      Lingkungan eksekusi notebook
Pandas            Membaca dan mengolah dataset
NumPy             Operasi numerik dan transformasi
Matplotlib        Visualisasi data
Seaborn           Visualisasi statistik
07 --- Project Resources
  Resource                            Link
  GitHub Repository                   EDA-Malicious-PDF

  Dataset CSV                         PDF_All_features.csv
Google Colab                        Open Notebook
08 --- Next Research Direction
Hasil EDA menjadi dasar untuk pengembangan model klasifikasi pada tahap
berikutnya.

Rekomendasi dari laporan:
1. Model utama: algoritma ensemble berbasis pohon seperti Random
   Forest, XGBoost, atau LightGBM karena lebih sesuai untuk data dengan
   outliers, distribusi skewed, dan hubungan fitur yang kompleks.
2. Model pembanding: Support Vector Machine (SVM) kernel RBF dengan
   data yang telah ditransformasi dan diskalakan.
3. Feature engineering: mempertimbangkan fitur objects_per_page,
   has_javascript, dan file_size_category.
4. Feature selection: mengevaluasi fitur yang sangat berkorelasi
   agar informasi redundan dapat dikurangi.
5. Evaluation: memprioritaskan Recall dan F1-Score untuk kelas
   Malicious, selain Accuracy, guna mengurangi risiko dokumen berbahaya
   terprediksi sebagai aman.
Proporsi kelas pada dataset relatif seimbang (48,18% Benign dan 51,82%
Malicious), sehingga oversampling seperti SMOTE belum menjadi kebutuhan
awal berdasarkan distribusi kelas tersebut.
09 --- Research Limitations
Analisis ini menggunakan fitur hasil ekstraksi statis. Dataset tidak
menyediakan timestamp pembuatan file maupun konteks semantik dari
payload skrip. Dengan demikian, analisis belum dapat menjelaskan
perubahan teknik serangan dari waktu ke waktu atau memastikan apakah
suatu JavaScript digunakan untuk tujuan sah maupun berbahaya.

Hasil EDA menunjukkan pola dan asosiasi dalam dataset, tetapi tidak
dengan sendirinya membuktikan sebab-akibat atau menjamin kemampuan
deteksi pada file PDF baru. Validasi melalui pemodelan dan pengujian
terpisah tetap diperlukan.
10 --- Research Identity
Author: Yahyati Aulia
Program: Informatika --- Fakultas Ilmu Komputer
Institution: Universitas Amikom Purwokerto
Year: 2026
Lecturer: Dr. Giat Karyono, M.Kom.
::: {align="center"}
**EXPLORE THE STRUCTURE. UNDERSTAND THE SIGNAL.**

*An Exploratory Data Analysis project on malicious PDF characteristics.*
:::
