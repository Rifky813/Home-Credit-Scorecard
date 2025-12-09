<img width="500" height="450" alt="image" src="https://github.com/user-attachments/assets/5af64004-ea08-48ac-aa33-4778639e3eda" />

# Prediksi Gagal Bayar (Credit Default Prediction) - Home Credit Indonesia

**Final Task - Rakamin x Home Credit Virtual Internship**

## Latar Belakang

Banyak orang kesulitan mendapatkan pinjaman karena riwayat kredit yang tidak mencukupi atau tidak ada sama sekali. Populasi ini seringkali rentan dan dapat dimanfaatkan oleh pemberi pinjaman yang tidak dapat dipercaya.

Home Credit berusaha untuk memperluas inklusi keuangan bagi populasi *unbanked* dengan menyediakan pengalaman pinjaman yang positif dan aman. Untuk memastikan populasi yang kurang terlayani ini memiliki pengalaman pinjaman yang positif, Home Credit memanfaatkan berbagai data alternatif termasuk data telco dan transaksional untuk memprediksi kemampuan klien dalam membayar kembali pinjaman.

## Tantangan Bisnis

Home Credit Group beroperasi di segmen pasar yang berisiko tinggi, yaitu melayani individu yang memiliki riwayat kredit terbatas atau tidak ada sama sekali. Meskipun pendekatan ini mendorong inklusi keuangan, tantangan utamanya adalah bagaimana memprediksi probabilitas gagal bayar secara akurat. Sistem penilaian kredit konvensional tidak efektif dalam kasus ini karena kurangnya data historis.

## Tujuan Proyek

Tujuan dari proyek ini adalah untuk mengembangkan model *machine learning* prediktif yang mampu menganalisis data alternatif dari berbagai sumber (demografi, riwayat pinjaman sebelumnya, dan pola pembayaran) untuk mengidentifikasi karakteristik nasabah yang berpotensi mengalami kesulitan dalam membayar kembali pinjamannya.

Hasil dari model ini akan digunakan untuk menciptakan *scorecard* yang dapat membantu Home Credit membuat keputusan pinjaman yang lebih informatif, mengurangi kerugian finansial, dan melayani populasi yang kurang terlayani dengan lebih baik.

## Metodologi

Proyek ini dibagi menjadi tiga tahap utama, yang tercermin dalam notebook yang disediakan:

### 1\. Analisis Data Eksploratif (EDA) & Rekayasa Fitur (`EDA_featureEngineer.ipynb`)

Tahap ini berfokus pada pemahaman data dan penciptaan fitur-fitur baru yang relevan.

  * **Analisis Data:**

      * Menganalisis distribusi variabel target (`TARGET`) dan mengidentifikasi ketidakseimbangan kelas (imbalance) yang signifikan.

      * Memeriksa distribusi fitur-fitur numerik dan kategorikal.

      * Menganalisis nilai-nilai yang hilang (*missing values*) di seluruh dataset.

      * Mempelajari korelasi antar fitur.

  * **Rekayasa Fitur (Feature Engineering):**

      * Menciptakan fitur-fitur baru berdasarkan domain knowledge, seperti:

          * Rasio pendapatan terhadap kredit (`CREDIT_INCOME_PERCENT`)

          * Rasio anuitas terhadap pendapatan (`ANNUITY_INCOME_PERCENT`)

          * Jangka waktu kredit (`CREDIT_TERM`)

          * Agregasi data dari riwayat pinjaman sebelumnya.

### 2\. Pra-pemrosesan Data & Seleksi Fitur (`preprocessing.ipynb`)

Tahap ini menyiapkan data agar siap digunakan untuk pelatihan model.

  * **Pembersihan Data:**

      * **Imputasi Missing Values:** Mengisi nilai yang hilang menggunakan strategi yang sesuai (misalnya, `SimpleImputer` dengan median untuk numerik dan modus untuk kategorikal).

      * **Encoding:** Mengubah fitur kategorikal menjadi representasi numerik menggunakan One-Hot Encoding.

  * **Scaling:** Menyamakan skala semua fitur numerik menggunakan `MinMaxScaler` untuk memastikan tidak ada fitur yang mendominasi model.

  * **Pembagian Data:** Membagi data menjadi set pelatihan (*train*) dan validasi (*validation*).

  * **Penanganan Ketidakseimbangan Data:** Mengatasi masalah kelas yang tidak seimbang menggunakan teknik *oversampling*, yaitu `RandomOverSampler`, untuk menyeimbangkan distribusi kelas `TARGET` pada data pelatihan.

  * **Seleksi Fitur:**

      * Mengurangi jumlah fitur untuk meningkatkan kinerja model dan mengurangi komputasi.

      * Dua metode digunakan untuk perbandingan: `SelectKBest` dengan **Chi-Square (`chi2`)** dan **Mutual Information (`mutual_info_classif`)**.

### 3\. Pemodelan & Evaluasi (`models.ipynb`)

Tahap ini adalah inti dari proyek, di mana model dilatih, di-tuning, dan dievaluasi.

  * **Model yang Diuji:**

    1.  `LogisticRegression` (sebagai baseline)

    2.  `RandomForestClassifier`

    3.  `LGBMClassifier` (Light Gradient Boosting Machine)

  * **Metrik Evaluasi:** Mengingat dataset yang tidak seimbang, **ROC-AUC Score** dipilih sebagai metrik evaluasi utama. `Classification Report` (Precision, Recall, F1-Score) dan `Confusion Matrix` juga digunakan untuk analisis lebih mendalam.

  * **Hyperparameter Tuning:** `GridSearchCV` digunakan untuk mencari kombinasi parameter terbaik untuk model dengan kinerja tertinggi (LGBM).

## Hasil

Berdasarkan proses evaluasi:

  * **Model Terbaik:** `LGBMClassifier` (LightGBM) secara konsisten memberikan skor ROC-AUC tertinggi dibandingkan dengan Logistic Regression dan Random Forest.

  * **Seleksi Fitur Terbaik:** Set fitur yang dihasilkan oleh metode **Chi-Square (`chi2`)** memberikan hasil yang sedikit lebih baik saat digunakan dengan model LGBM.

  * **Hasil Akhir:** Model `LGBMClassifier` yang telah di-tuning menggunakan `GridSearchCV` dan dilatih pada data yang telah di-oversampling dengan fitur-fitur pilihan `chi2` digunakan untuk membuat prediksi akhir pada data tes (`application_test.csv`).

## Kesimpulan & Rekomendasi

Model *machine learning* yang dikembangkan, khususnya `LGBMClassifier`, terbukti mampu memprediksi probabilitas gagal bayar nasabah dengan cukup baik menggunakan data alternatif.

**Rekomendasi Bisnis:**

1.  **Implementasi Model:** Model ini dapat diimplementasikan ke dalam sistem *credit scoring* Home Credit untuk menilai aplikasi pinjaman baru secara otomatis.

2.  **Pembuatan Scorecard:** Probabilitas yang dihasilkan model (0 hingga 1) dapat dikonversi menjadi *credit score* yang mudah dipahami.

3.  **Segmentasi Nasabah:** Berdasarkan skor ini, nasabah dapat disegmentasi menjadi beberapa kategori (misalnya, Risiko Rendah, Risiko Sedang, Risiko Tinggi), yang dapat memandu keputusan bisnis:

      * **Risiko Rendah:** Persetujuan otomatis.

      * **Risiko Sedang:** Membutuhkan tinjauan manual atau penyesuaian jumlah pinjaman/jangka waktu.

      * **Risiko Tinggi:** Penolakan otomatis untuk mengurangi kerugian.

Langkah ini akan membantu Home Credit mencapai tujuannya untuk mengurangi risiko kredit sekaligus tetap memajukan misi inklusi keuangan.

## Struktur Repositori

```
.
├── EDA_featureEngineer.ipynb   # Notebook untuk Analisis Data dan Rekayasa Fitur
├── preprocessing.ipynb         # Notebook untuk Pra-pemrosesan dan Seleksi Fitur
├── models.ipynb                # Notebook untuk Pelatihan, Tuning, dan Evaluasi Model
└── README.md                   # Dokumentasi proyek (file ini)

```
