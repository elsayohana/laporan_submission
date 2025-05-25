## **Domain Proyek**
Kanker payudara merupakan salah satu jenis kanker paling umum dan menjadi penyebab utama kematian akibat kanker pada wanita di seluruh dunia. Menurut data dari World Health Organization (WHO), pada tahun 2020 terdapat lebih dari 2,3 juta kasus baru kanker payudara dengan sekitar 685.000 kematian secara global. Deteksi dini dan diagnosis yang akurat sangat penting untuk meningkatkan peluang kesembuhan serta mengoptimalkan efektivitas pengobatan.

Metode diagnosis tradisional seperti mammografi, biopsi, dan pemeriksaan fisik memiliki keterbatasan, seperti biaya tinggi, sifat invasif, serta ketergantungan pada keahlian tenaga medis. Dalam konteks ini, teknologi **machine learning** menjadi solusi potensial yang dapat memberikan dukungan keputusan klinis secara lebih cepat, akurat, dan efisien berbasis data.

Salah satu dataset yang banyak digunakan untuk pengembangan sistem pendeteksi kanker payudara adalah _Breast Cancer Wisconsin (Diagnostic) Data_, yang berisi fitur-fitur statistik dari citra mikroskopik inti sel tumor, seperti radius, tekstur, perimeter, smoothness, dan lainnya. Dataset ini digunakan untuk membedakan antara tumor jinak (benign) dan ganas (malignant).

Berbagai penelitian telah membuktikan efektivitas algoritma machine learning seperti **Support Vector Machine (SVM)**, **Random Forest**, **Logistic Regression**, dan **K-Nearest Neighbors (KNN)** dalam melakukan klasifikasi kanker payudara. Misalnya, studi oleh Paul et al. (2023), Kumar et al. (2024), dan Tariq et al. (2023) menunjukkan bahwa model berbasis machine learning mampu mencapai akurasi deteksi melebihi 95% dalam diagnosis dini kanker payudara.

Berdasarkan latar belakang tersebut, proyek ini bertujuan untuk membangun model klasifikasi kanker payudara berbasis machine learning yang andal dan akurat. Model ini diharapkan dapat menjadi alat bantu bagi tenaga medis dalam proses skrining awal dan pengambilan keputusan klinis yang lebih baik.

### **Referensi**
- Paul, R., Bhandari, M., & Singh, P. (2023). _Breast Cancer Diagnosis Using Machine Learning Models_. 2023 International Conference on Computing, Communication and Networking Technologies (ICCCNT). [https://doi.org/10.1109/ICCCNT56457.2023.10283017](https://doi.org/10.1109/ICCCNT56457.2023.10283017)
- Kumar, A., Raj, P., & Mehta, R. (2024). _Comparative Analysis of ML Algorithms for Breast Cancer Classification_. 2024 International Conference on Machine Learning and Computing (ICMLC). [https://doi.org/10.1109/ICMLC60489.2024.10593942](https://doi.org/10.1109/ICMLC60489.2024.10593942)
- Tariq, A., Shah, M. A., & Khan, M. (2023). _Predictive Modelling for Early Diagnosis of Breast Cancer Using Machine Learning_. _Cancers_, 15(15), 3824. [https://doi.org/10.3390/cancers15153824](https://doi.org/10.3390/cancers15153824)

## **Business Understanding**

### **Problem Statement**

Deteksi dini kanker payudara sangat krusial untuk meningkatkan peluang kesembuhan pasien serta mengurangi risiko komplikasi serius. Namun, metode diagnosis tradisional seperti mammografi dan biopsi cenderung mahal, memerlukan waktu yang lama, serta membutuhkan keahlian medis yang tinggi. Selain itu, interpretasi hasil pemeriksaan secara manual bersifat subjektif dan rentan terhadap kesalahan diagnosis.
Dalam konteks ini, dibutuhkan sistem pendukung keputusan berbasis **machine learning** yang dapat membantu mengklasifikasikan jenis tumor secara otomatis, cepat, dan akurat, sebagai pelengkap bagi proses diagnosis medis konvensional.

### **Goals**

Proyek ini bertujuan untuk membangun sistem klasifikasi prediktif yang mampu menentukan apakah suatu tumor payudara bersifat **jinak (benign)** atau **ganas (malignant)** berdasarkan fitur-fitur yang tersedia dalam _Breast Cancer Wisconsin (Diagnostic) Data_. Secara spesifik, sistem ini diharapkan dapat:

- Meningkatkan **akurasi dan efisiensi** proses diagnosis kanker payudara.
- Memberikan **rekomendasi awal yang objektif** sebagai alat bantu bagi tenaga medis.
- Menghasilkan model prediktif dengan performa tinggi, dengan target **akurasi minimal 95%** pada data pengujian.
- Menggunakan metrik evaluasi seperti **accuracy**, **precision**, **recall**, dan **F1-score** untuk menilai performa model.

### **Solution Statement**
Untuk mencapai tujuan yang telah ditetapkan, proyek ini mengusulkan dua pendekatan solusi utama yang akan dibandingkan dan dievaluasi:

#### **Solusi 1: Penerapan Dua Algoritma Klasifikasi**
Dua algoritma machine learning akan diimplementasikan untuk melakukan klasifikasi jenis tumor:
1. **Random Forest Classifier** – algoritma berbasis ensemble yang mampu menangani data berdimensi tinggi dan meminimalkan overfitting melalui mekanisme voting antar pohon keputusan.
2. **Logistic Regression** – algoritma klasifikasi linier yang sederhana, efisien, dan memiliki interpretabilitas tinggi, cocok sebagai baseline model.
Kedua model akan dievaluasi berdasarkan metrik **accuracy**, **precision**, **recall**, dan **F1-score** untuk menilai kinerja klasifikasi masing-masing.

#### **Solusi 2: Hyperparameter Tuning**
Untuk meningkatkan performa dari kedua algoritma, akan dilakukan optimasi hyperparameter menggunakan teknik:
- **Grid Search Cross Validation** atau
- **Randomized Search Cross Validation**,
  yang bertujuan menemukan kombinasi parameter optimal untuk mencapai generalisasi terbaik pada data pengujian.
Model dengan performa terbaik berdasarkan evaluasi akan dipilih sebagai solusi akhir untuk digunakan dalam sistem prediksi kanker payudara.

## Data Understanding

Dataset yang digunakan dalam proyek ini adalah **Breast Cancer Wisconsin (Diagnostic) Dataset**, yang berisi hasil pemeriksaan sel kanker payudara dari pasien wanita. Dataset ini tersedia secara publik melalui platform Kaggle dan UCI Machine Learning Repository, dan sering digunakan untuk membangun model klasifikasi dalam mendeteksi kanker payudara secara dini.
📎 [Breast Cancer Wisconsin Dataset - Kaggle](https://www.kaggle.com/datasets/uciml/breast-cancer-wisconsin-data)

Dataset ini terdiri dari **569 baris** (observasi) dan **33 kolom** (fitur), sesuai dengan hasil dari `DataFrame.info()`. Namun, tidak semua kolom relevan digunakan dalam pemodelan. Kolom `Unnamed: 32` tidak mengandung data (semuanya `null`) dan akan dihapus dalam proses pembersihan data.

### Struktur Dataset

* **Total baris**: 569
* **Total kolom**: 33
* **Kolom target**: `diagnosis` (tipe `object`), berisi dua kategori:

  * `M` (Malignant / Ganas)
  * `B` (Benign / Jinak)
* **Kolom non-relevan**:

  * `id`: hanya sebagai identifikasi, tidak dibutuhkan dalam pemodelan.
  * `Unnamed: 32`: tidak memiliki data (`0 non-null`), akan dihapus.
* **Total fitur numerik valid**: 30 (bertipe `float64`)

### Variabel-variabel pada dataset

Fitur yang tersedia berasal dari pengukuran statistik terhadap gambar digital jaringan tumor payudara. Setiap karakteristik sel diukur dalam tiga bentuk statistik: **mean**, **standard error (se)**, dan **worst** (nilai maksimum), dari atribut berikut:

* **radius**: Jarak dari pusat ke batas luar sel.
* **texture**: Variasi intensitas keabuan.
* **perimeter**: Keliling sel.
* **area**: Luas area sel.
* **smoothness**: Kelembutan kontur sel (perubahan lokal pada panjang kontur).
* **compactness**: Rumus `(perimeter² / area) - 1.0`, menggambarkan kepadatan.
* **concavity**: Tingkat kecekungan bentuk sel.
* **concave points**: Jumlah titik cekung pada kontur.
* **symmetry**: Tingkat simetri sel.
* **fractal dimension**: Kompleksitas kontur (pengukuran dimensi fraktal).

Total ada **30 fitur numerik utama**, masing-masing diukur dalam tiga versi:

* `*_mean` — nilai rata-rata
* `*_se` — nilai simpangan baku
* `*_worst` — nilai terburuk

Contoh nama kolom:

* `radius_mean`, `radius_se`, `radius_worst`
* `texture_mean`, `texture_se`, `texture_worst`, dst.

### Target variabel

* **diagnosis**:

  * `M` (Malignant) akan dikodekan sebagai `1`
  * `B` (Benign) akan dikodekan sebagai `0`

### Informasi Dataset
```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 569 entries, 0 to 568
Data columns (total 33 columns):
 #   Column                   Non-Null Count  Dtype
---  ------                   --------------  -----
 0   id                       569 non-null    int64
 1   diagnosis                569 non-null    object
 2   radius_mean              569 non-null    float64
 3   texture_mean             569 non-null    float64
 4   perimeter_mean           569 non-null    float64
 5   area_mean                569 non-null    float64
 6   smoothness_mean          569 non-null    float64
 7   compactness_mean         569 non-null    float64
 8   concavity_mean           569 non-null    float64
 9   concave points_mean      569 non-null    float64
 10  symmetry_mean            569 non-null    float64
 11  fractal_dimension_mean   569 non-null    float64
 12  radius_se                569 non-null    float64
 13  texture_se               569 non-null    float64
 14  perimeter_se             569 non-null    float64
 15  area_se                  569 non-null    float64
 16  smoothness_se            569 non-null    float64
 17  compactness_se           569 non-null    float64
 18  concavity_se             569 non-null    float64
 19  concave points_se        569 non-null    float64
 20  symmetry_se              569 non-null    float64
 21  fractal_dimension_se     569 non-null    float64
 22  radius_worst             569 non-null    float64
 23  texture_worst            569 non-null    float64
 24  perimeter_worst          569 non-null    float64
 25  area_worst               569 non-null    float64
 26  smoothness_worst         569 non-null    float64
 27  compactness_worst        569 non-null    float64
 28  concavity_worst          569 non-null    float64
 29  concave points_worst     569 non-null    float64
 30  symmetry_worst           569 non-null    float64
 31  fractal_dimension_worst  569 non-null    float64
 32  Unnamed: 32              0 non-null      float64
dtypes: float64(31), int64(1), object(1)
memory usage: 146.8 KB
```
Dataset **Breast Cancer Wisconsin (Diagnostic)** terdiri dari **569 baris** dan **33 kolom**, dengan rincian:
* **1 kolom ID**: `id` (integer), hanya sebagai identifikasi unik dan tidak digunakan dalam pemodelan.
* **1 kolom target**: `diagnosis` (kategori), berisi:
  * `M` = Malignant (ganas)
  * `B` = Benign (jinak)
* **30 kolom fitur numerik** (`float64`): hasil pengukuran statistik dari citra digital jaringan tumor, dalam tiga versi: `mean`, `se` (standard error), dan `worst`.
* **1 kolom tidak berguna**: `Unnamed: 32` (kosong seluruhnya), akan dihapus.
Tidak ada nilai hilang di kolom yang digunakan, sehingga dataset siap untuk tahap preprocessing dan eksplorasi.

### Statistik Deskriptif
* Dataset memiliki **569 entri** untuk semua fitur kecuali kolom `Unnamed: 32` yang seluruhnya kosong.
* Kolom `id` berfungsi sebagai penanda unik dan tidak relevan untuk analisis → **dihapus**.
* Kolom `Unnamed: 32` berisi nilai kosong semua → **dihapus**.
* Beberapa fitur seperti `area_mean` dan `concavity_worst` menunjukkan distribusi yang **skewed** dan mengandung **outlier**.
* Fitur seperti `smoothness_mean` relatif stabil dengan distribusi yang lebih normal.
* Karena skala dan distribusi fitur sangat beragam, perlu dilakukan **normalisasi atau standardisasi** untuk menghindari bias model.
* Disarankan untuk melakukan pengecekan outlier lebih lanjut menggunakan boxplot.

> ![Boxplot](https://github.com/user-attachments/assets/bdd4fac2-1637-4368-9e46-9aa16c1f8dfd)

### Ringkasan Diagnosis
* Target klasifikasi terdiri dari dua kategori:
  * `B` (Benign / Jinak) sebanyak 62.7%
  * `M` (Malignant / Ganas) sebanyak 37.3%
* Distribusi kelas ini **tidak terlalu imbalance**, tapi tetap perlu diperhatikan agar model tidak bias ke kelas mayoritas.
* Oleh karena itu, evaluasi dengan metrik seperti **F1-score** sangat dianjurkan selain akurasi.
  
> ![Pie_Chart](https://github.com/user-attachments/assets/f05a2369-358f-4994-b05d-59b79cddc046)

### Missing Values dan Duplikat

* Dataset **tidak memiliki missing value** pada fitur yang penting.
* Kolom `Unnamed: 32` kosong semua → dihapus untuk kebersihan data.
* Tidak ditemukan data duplikat → data sudah unik dan bersih.

### Distribusi dan Preprocessing Fitur
* Kolom `id` dan `Unnamed: 32` sudah dihapus karena tidak relevan.
* Sebagian besar fitur memiliki distribusi **right-skewed** seperti `area_mean`, `concavity_mean`.
* Beberapa fitur memiliki distribusi yang lebih normal seperti `smoothness_mean` dan `symmetry_mean`.
* Fitur dengan akhiran `_se` dan `_worst` banyak mengandung nilai ekstrem (outlier).
* Karena itu, preprocessing seperti transformasi (log, Box-Cox) dan normalisasi perlu diterapkan agar model lebih stabil dan tidak bias.

## Data Preparation
Tahap ini bertujuan untuk menyiapkan data dengan membersihkan kolom tidak relevan, mengubah format target, mengurangi skewness, normalisasi, dan membagi data untuk pelatihan dan pengujian model machine learning.

### **1. Pembersihan Data**
- **Menghapus Kolom Tidak Diperlukan**:
  Kolom `id` dan `Unnamed: 32` tidak memberikan kontribusi terhadap proses klasifikasi dan dihapus.
- **Transformasi Target**:
  - Kolom `diagnosis` berisi label **M** (malignant) dan **B** (benign).
  - Dilakukan pemetaan: `'M' → 1`, `'B' → 0`.

```python
df.drop(['id', 'Unnamed: 32'], axis=1, inplace=True)
df['diagnosis'] = df['diagnosis'].map({'M': 1, 'B': 0})
```

### **2. Pemisahan Fitur dan Target**
- **Fitur (`X`)**: Semua kolom kecuali `diagnosis`.
- **Target (`y`)**: Kolom `diagnosis`.

```python
X = df.drop('diagnosis', axis=1)
y = df['diagnosis']
```

### **3. Feature Selection (Pengurangan Dimensi)**
- **Menghapus fitur yang sangat berkorelasi tinggi** dan berpotensi menimbulkan **multikolinearitas**:

```python
drop_cols = ['perimeter_mean', 'area_mean', 'concave points_mean',
             'perimeter_se', 'area_se', 'radius_worst', 'texture_worst',
             'perimeter_worst', 'area_worst', 'concavity_worst', 'concave points_worst']
X.drop(columns=drop_cols, inplace=True)
```

### **4. Transformasi Skewness**
- **Masalah**: Beberapa fitur memiliki distribusi yang sangat miring (**skewness > 1**).
- **Solusi**: Diterapkan transformasi `log1p` untuk mengurangi skewness.

```python
skewed_features = X.skew().abs()
skewed_features = skewed_features[skewed_features > 1].index
X[skewed_features] = X[skewed_features].apply(lambda x: np.log1p(x))
```

### **5. Splitting Data**
- **Rasio**: 80% data latih, 20% data uji.
- **Stratifikasi**: Berdasarkan `y` agar proporsi label seimbang.

```python
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42, stratify=y)
```

### **6. Normalisasi**
- Skala fitur berbeda-beda, sehingga dilakukan **Standardisasi** menggunakan `StandardScaler`.
- **Mean = 0**, **Standard Deviation = 1**.

```python
scaler = StandardScaler()
X_train = pd.DataFrame(scaler.fit_transform(X_train), columns=X.columns)
X_test = pd.DataFrame(scaler.transform(X_test), columns=X.columns)
```

## **Modeling**

### Tujuan
Membangun model klasifikasi untuk memprediksi jenis tumor payudara — **malignant (ganas)** atau **benign (jinak)** — berdasarkan fitur medis hasil pemeriksaan.

### Pemilihan Algoritma
Dua algoritma dipilih untuk dibandingkan berdasarkan akurasi dan relevansi dalam konteks medis:

#### 1. Logistic Regression
* **Cara Kerja:**
  Logistic Regression adalah model linear yang memprediksi probabilitas sebuah kelas menggunakan fungsi sigmoid. Model ini menghitung bobot (koefisien) untuk tiap fitur, sehingga output berupa nilai probabilitas antara 0 dan 1, yang kemudian diklasifikasikan ke kelas target.
* **Parameter Utama yang Disesuaikan:**

  * `C`: Invers dari regularisasi; nilai kecil berarti regularisasi kuat untuk mencegah overfitting.
  * `solver`: Algoritma optimasi (misal `liblinear` cocok untuk dataset kecil, `lbfgs` untuk lebih besar).
  * `max_iter`: Maksimum iterasi untuk konvergensi optimasi.
* **Parameter Terbaik dari Hyperparameter Tuning:**
  ```python
  {
    'C': 0.1,
    'solver': 'liblinear',
    'max_iter': 100
  }
  ```
  (Parameter ini diperoleh melalui Grid Search dengan 5-fold CV dan scoring F1-score.)

#### 2. Random Forest
* **Cara Kerja:**
  Random Forest adalah ensemble dari banyak pohon keputusan (decision trees). Setiap pohon dilatih pada subset data dan fitur secara acak. Prediksi akhir diperoleh dengan voting mayoritas dari semua pohon, sehingga model ini lebih stabil dan kuat terhadap overfitting, serta mampu menangkap hubungan non-linear antar fitur.
* **Parameter Utama yang Disesuaikan:**
  * `n_estimators`: Jumlah pohon dalam hutan.
  * `max_depth`: Maksimum kedalaman tiap pohon untuk mengontrol kompleksitas.
  * `max_features`: Jumlah fitur yang dipilih secara acak pada setiap split.
  * `min_samples_split`: Minimum jumlah sampel untuk melakukan pembagian node.
* **Parameter Terbaik dari Hyperparameter Tuning:**
  ```python
  {
    'n_estimators': 200,
    'max_depth': 20,
    'max_features': 'sqrt',
    'min_samples_split': 2
  }
  ```
  (Didapat dari proses tuning yang sama dengan Logistic Regression.)

Model dilatih dengan parameter terbaik hasil hyperparameter tuning. Logistic Regression menggunakan regularisasi yang cukup kuat dengan solver `liblinear` dan batas iterasi 100. Random Forest menggunakan 200 pohon dengan kedalaman maksimal 20 dan fitur yang dipilih secara acak berdasarkan akar kuadrat jumlah fitur (`sqrt`).

Parameter ini dipilih untuk memaksimalkan performa model khususnya F1-score, yang sangat penting dalam konteks klasifikasi medis untuk menyeimbangkan sensitivitas dan presisi.

## Evaluation

### Metrik Evaluasi Model
Untuk menilai performa kedua model yang dibangun, digunakan metrik **Accuracy**, **Precision**, **Recall**, dan **F1-score**. Berikut hasil evaluasi pada data uji:

| Model               | Accuracy | Precision (Class 1) | Recall (Class 1) | F1-Score |
| ------------------- | -------- | ------------------- | ---------------- | -------- |
| Logistic Regression | 96.49%   | 0.95                | 0.95             | 0.95     |
| Random Forest       | 92.98%   | 0.95                | 0.86             | 0.90     |

**Detail Classification Report (Logistic Regression):**
* Precision 0.97 (kelas 0), 0.95 (kelas 1)
* Recall 0.97 (kelas 0), 0.95 (kelas 1)
* F1-score rata-rata 0.96

**Detail Classification Report (Random Forest):**
* Precision 0.92 (kelas 0), 0.95 (kelas 1)
* Recall 0.97 (kelas 0), 0.86 (kelas 1)
* F1-score rata-rata 0.92

Dari hasil ini, **Logistic Regression menunjukkan performa lebih baik dan lebih seimbang pada kedua kelas** dibandingkan Random Forest, terutama pada recall kelas malignant yang krusial untuk deteksi dini kanker payudara.

### Hubungan dengan Business Understanding
Tujuan utama proyek ini adalah membangun model klasifikasi yang dapat membantu tenaga medis memprediksi jenis tumor payudara, apakah **malignant (ganas)** atau **benign (jinak)** berdasarkan fitur medis.
* **Apakah model ini menjawab problem statement?**
  Ya, model berhasil memberikan prediksi dengan akurasi tinggi (> 96% untuk Logistic Regression) dan metrik F1-score yang menunjukkan keseimbangan antara precision dan recall, sangat penting untuk mengurangi risiko kesalahan diagnosa.
* **Apakah goals tercapai?**
  Model mencapai goal utama yaitu memaksimalkan kemampuan klasifikasi tumor dengan tingkat kesalahan rendah. Dengan F1-score 0.95, model memberikan hasil prediksi yang dapat diandalkan sebagai alat bantu diagnosis.
* **Dampak terhadap solusi dan bisnis**
  Implementasi model ini di fasilitas medis dapat meningkatkan kecepatan dan akurasi deteksi tumor, membantu dokter membuat keputusan yang lebih baik dan cepat. Ini dapat mengurangi kesalahan diagnosa, meningkatkan outcome pasien, dan efisiensi proses screening.
* **Penanganan Imbalance Class**
  Penyesuaian class weight pada Logistic Regression dan penggunaan F1-score sebagai metrik utama menunjukkan perhatian terhadap ketidakseimbangan data, sehingga model tidak bias terhadap kelas mayoritas (benign).

### Kesimpulan
Model Logistic Regression yang dituning dengan hyperparameter terbaik berhasil mencapai performa unggul pada data uji. Model ini efektif menjawab masalah klasifikasi tumor payudara dan berpotensi memberikan dampak positif nyata dalam dunia medis untuk deteksi dini kanker payudara.




