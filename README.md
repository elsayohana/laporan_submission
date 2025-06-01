# **Penerapan Content-Based Filtering dan Collaborative Filtering pada Sistem Rekomendasi Ulasan Makanan (Studi Kasus: Amazon Fine Food Reviews)**
*Elsa Yohana Sinaga*

## **Project Overview**

Permasalahan utama dalam e-commerce adalah membantu pengguna menemukan produk yang sesuai dengan preferensi mereka di antara ribuan pilihan. Untuk itu, sistem rekomendasi menjadi solusi penting dalam meningkatkan pengalaman pengguna dan mendorong penjualan.

Proyek ini bertujuan membangun sistem rekomendasi makanan menggunakan dataset **Amazon Fine Food Reviews**. Dataset ini memuat lebih dari 500 ribu ulasan pelanggan, termasuk skor, komentar, dan informasi pengguna serta produk. Sistem rekomendasi ini akan membantu pengguna mendapatkan rekomendasi produk berdasarkan preferensi pribadi dan ulasan pengguna lain.

Dalam proyek ini, dua metode rekomendasi utama, yaitu **Content-Based Filtering** dan **Collaborative Filtering**, digunakan untuk memberikan rekomendasi yang relevan dan personal. Dengan pendekatan ini, diharapkan sistem mampu meningkatkan kepuasan pengguna serta membantu mereka dalam pengambilan keputusan pembelian.

Pentingnya proyek ini didasari oleh tingginya volume informasi dalam platform e-commerce, yang menyebabkan user fatigue dan kesulitan dalam mengambil keputusan pembelian (Kotler et al., 2021). Dengan sistem rekomendasi yang efektif, pengalaman pengguna bisa ditingkatkan secara signifikan.

Referensi:

* [Amazon Fine Food Reviews - Kaggle](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)
* Kotler, P., Keller, K. L., et al. (2021). *Marketing Management*.

## **Business Understanding**

### Problem Statement

Pengguna e-commerce sering mengalami kesulitan dalam menemukan produk makanan yang sesuai dengan preferensi mereka karena banyaknya pilihan dan variasi produk. Hal ini dapat menyebabkan ketidakpuasan pelanggan dan menurunnya penjualan.

### Goals

* Membangun sistem rekomendasi yang mampu memberikan rekomendasi produk makanan sesuai preferensi pengguna.
* Mengurangi beban pengguna dalam memilih produk dengan menampilkan rekomendasi yang relevan dan personal.
* Meningkatkan kepuasan pelanggan dan potensi peningkatan penjualan di platform e-commerce.

### Solution Approach

Untuk mencapai tujuan tersebut, dua pendekatan sistem rekomendasi akan digunakan:

1. **Content-Based Filtering (CBF):** Menggunakan fitur teks ulasan untuk merekomendasikan produk berdasarkan kemiripan konten dengan produk yang sudah disukai pengguna.
2. **Collaborative Filtering (CF):** Memanfaatkan interaksi pengguna (rating) untuk memberikan rekomendasi berdasarkan preferensi pengguna lain yang serupa.

Pendekatan ini dipilih agar sistem rekomendasi dapat menangani berbagai kondisi, misalnya produk baru (CBF) dan pola preferensi pengguna yang kompleks (CF).

## Data Understanding

### 1. Informasi Dataset

Dataset **Amazon Fine Food Reviews** berisi **568.454 baris** dan **10 kolom** yang merepresentasikan ulasan produk makanan di Amazon. Data ini sangat kaya dengan informasi seperti ID produk, ID pengguna, nama profil, skor ulasan, waktu ulasan, dan isi ulasan.

Dataset dapat diunduh di:
[https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews](https://www.kaggle.com/datasets/snap/amazon-fine-food-reviews)

### 2. Variabel/Fitur Dataset

| Kolom                  | Tipe Data | Deskripsi                                       |
| ---------------------- | --------- | ----------------------------------------------- |
| Id                     | int64     | ID unik untuk setiap ulasan                     |
| ProductId              | object    | ID produk                                       |
| UserId                 | object    | ID pengguna                                     |
| ProfileName            | object    | Nama profil pengguna (terdapat sedikit missing) |
| HelpfulnessNumerator   | int64     | Jumlah vote helpful untuk ulasan                |
| HelpfulnessDenominator | int64     | Jumlah total vote untuk ulasan                  |
| Score                  | int64     | Skor ulasan (1 sampai 5)                        |
| Time                   | int64     | Waktu ulasan dalam UNIX timestamp               |
| Summary                | object    | Ringkasan ulasan (sedikit missing)              |
| Text                   | object    | Isi teks ulasan                                 |

### 3. Statistik Deskriptif dan Kondisi Data

* Dataset bersih, tanpa duplikat.
* Sedikit missing values pada kolom `ProfileName` (26 baris) dan `Summary` (27 baris) yang dapat diatasi dengan pengisian nilai default atau dihapus.
* Skor ulasan didominasi nilai 5 (median = 5.0), menunjukkan mayoritas ulasan positif.
* Variasi panjang teks ulasan besar, dari sangat pendek hingga lebih dari 21.000 karakter.
* Waktu ulasan mencakup rentang dari tahun 2000 hingga 2012, memungkinkan analisis tren.


### 4. Visualisasi dan Insight

#### Distribusi Skor Ulasan
![image](https://github.com/user-attachments/assets/7c24eebc-5b32-4ab1-81aa-9b793ddd11aa)

**Insight:**
Distribusi skor ulasan sangat tidak seimbang, dengan dominasi skor 5 yang sangat besar. Hal ini menunjukkan ulasan mayoritas positif, sehingga perlu diperhatikan imbalance ini dalam modeling.


#### Boxplot Panjang Teks Ulasan

![image](https://github.com/user-attachments/assets/20e585da-3758-4d27-8137-68a50fc20696)

**Insight:**
Sebagian besar ulasan memiliki panjang teks sedang, namun ada beberapa ulasan dengan teks sangat panjang yang dapat mempengaruhi proses pemrosesan teks dan harus dipertimbangkan dalam tahap cleaning atau preprocessing.

#### Heatmap Korelasi Variabel Numerik

![image](https://github.com/user-attachments/assets/5baf1e1d-c148-46cf-88c0-6da7da9dfce7)

**Insight:**
Terdapat korelasi sangat tinggi antara `HelpfulnessNumerator` dan `HelpfulnessDenominator` (0.97), menandakan keduanya berkaitan erat dan berpotensi menyebabkan multikolinearitas dalam model. Skor ulasan tidak menunjukkan korelasi kuat dengan panjang teks ulasan.


### 5. Exploratory Data Analysis (EDA) Ringkas

* Terdapat **256.059 UserId unik** dan **74.258 ProductId unik**, menunjukkan data sangat beragam.
* Rentang waktu ulasan cukup panjang, dari Oktober 1999 hingga Oktober 2012, memungkinkan analisis tren waktu.
* Mayoritas produk mendapat skor review 5, dengan skewness positif pada distribusi skor.

## Data Preparation

Tahapan data preparation dilakukan secara berurutan sebagai berikut:

1. **Penanganan Missing Values dan Duplikat**
   Data yang memiliki nilai kosong pada kolom `Summary` dan `ProfileName` diisi dengan string kosong menggunakan `.fillna()`. Selanjutnya, data duplikat dihapus dengan `.drop_duplicates()` untuk memastikan data unik dan berkualitas.
   *Kode:*

   ```python
   df = df.fillna({'Summary': '', 'ProfileName': ''})  
   df = df.drop_duplicates()
   ```

2. **Konversi Timestamp ke Format Tanggal**
   Kolom `Time` yang berupa UNIX timestamp dikonversi menjadi tipe datetime dengan `pd.to_datetime()`. Selain itu, fitur tahun dan bulan ulasan juga ditambahkan untuk analisis tren dan sebagai fitur tambahan pada model.
   *Kode:*

   ```python
   df['ReviewTime'] = pd.to_datetime(df['Time'], unit='s')  
   df['ReviewYear'] = df['ReviewTime'].dt.year  
   df['ReviewMonth'] = df['ReviewTime'].dt.month
   ```

3. **Penambahan Fitur Panjang Teks Review**
   Panjang ulasan dalam karakter dihitung dan disimpan di kolom baru `TextLength`. Fitur ini berguna untuk mengetahui variasi detail ulasan yang bisa mempengaruhi analisis teks.
   *Kode:*

   ```python
   df['TextLength'] = df['Text'].apply(lambda x: len(str(x)))
   ```

4. **Pembersihan Teks dan Penggabungan Kolom Summary dan Text**
   Teks pada kolom `Summary` dan `Text` digabung menjadi satu kolom `Content`. Kemudian, teks dibersihkan dengan menghilangkan tanda baca, mengubah ke huruf kecil, dan menghapus stopwords menggunakan fungsi `clean_text`.
   *Kode:*

   ```python
   def clean_text(text):
       text = re.sub(r'[^\w\s]', '', str(text).lower())  
       return ' '.join([word for word in text.split() if word not in stop_words])  

   df['Content'] = (df['Summary'] + ' ' + df['Text']).apply(clean_text)
   ```

5. **Pembuatan Label Biner untuk Klasifikasi**
   Skor ulasan diubah menjadi label biner `ScoreHigh`, dimana skor 4 dan 5 diberi label 1 (positif), sedangkan skor 1-3 diberi label 0. Ini mempermudah model klasifikasi untuk mendeteksi ulasan positif atau negatif.
   *Kode:*

   ```python
   df['ScoreHigh'] = df['Score'].apply(lambda x: 1 if x >= 4 else 0)
   ```

6. **Persiapan Dataset untuk Content-Based Filtering (CBF)**
   Dataset CBF dibuat dengan fitur teks `Content` dan label `ScoreHigh`. Data dibagi menjadi train dan test menggunakan `train_test_split` untuk evaluasi model.
   *Kode:*

   ```python
   cbf_df = df[['UserId', 'ProductId', 'Content', 'ScoreHigh']].copy()  
   X = cbf_df['Content']  
   y = cbf_df['ScoreHigh']  
   X_train, X_test, y_train, y_test = sklearn_train_test_split(X, y, test_size=0.2, random_state=42)
   ```

7. **Persiapan Dataset untuk Collaborative Filtering (CF)**
   Dataset CF hanya menggunakan `UserId`, `ProductId`, dan `Score` untuk membangun interaksi pengguna-produk.
   *Kode:*

   ```python
   cf_df = df[['UserId', 'ProductId', 'Score']].copy()
   ```

8. **Encoding UserId dan ProductId ke ID Numerik**
   User dan produk dikonversi menjadi ID numerik menggunakan `LabelEncoder` untuk digunakan pada matriks user-item.
   *Kode:*

   ```python
   user_enc = LabelEncoder()  
   product_enc = LabelEncoder()  
   cf_df['user_idx'] = user_enc.fit_transform(cf_df['UserId'])  
   cf_df['product_idx'] = product_enc.fit_transform(cf_df['ProductId'])
   ```

9. **Pembuatan Matriks User-Item untuk CF**
   Matriks sparse dibuat dengan data interaksi skor untuk CF. Matriks ini digunakan dalam algoritma rekomendasi berbasis user-item.
   *Kode:*

   ```python
   train_sparse_matrix = csr_matrix((cf_df['Score'], (cf_df['user_idx'], cf_df['product_idx'])))
   ```

10. **Pra-pemrosesan Teks pada CBF**
    Teks di kolom `Content` dibersihkan lebih lanjut dengan lemmatization dan penghapusan stopwords agar representasi teks lebih efektif untuk model.
    *Kode:*

    ```python
    def preprocess_text(text):
        text = text.lower()
        text = re.sub(r'[^a-z\s]', '', text)
        tokens = text.split()
        tokens = [lemmatizer.lemmatize(word) for word in tokens if word not in stop_words]
        return ' '.join(tokens)
    df['Content_clean'] = df['Content'].apply(preprocess_text)
    ```

11. **Vectorisasi, Reduksi Dimensi, dan Penyeimbangan Data**
    Teks diubah menjadi fitur numerik dengan TF-IDF, kemudian dikurangi dimensinya dengan SVD. Oversampling dilakukan untuk mengatasi ketidakseimbangan kelas pada label biner.
    *Kode:*

    ```python
    tfidf = TfidfVectorizer(max_features=3000)  
    X_vec = tfidf.fit_transform(df['Content_clean'])  
    svd = TruncatedSVD(n_components=150, random_state=42)  
    X_svd = svd.fit_transform(X_vec)  
    ros = RandomOverSampler(random_state=42)  
    X_resampled, y_resampled = ros.fit_resample(X_svd, df['ScoreHigh'])
    ```

12. **Feature Engineering Tambahan**
    Ditambahkan fitur sentiment polarity menggunakan TextBlob untuk meningkatkan kualitas fitur teks dan analisis sentimen.
    *Kode:*

    ```python
    df['Sentiment'] = df['Text'].apply(lambda x: TextBlob(str(x)).sentiment.polarity)
    ```

13. **Filter User dan Produk dengan Data Sedikit**
    User dan produk dengan kurang dari 5 review difilter agar model lebih stabil dan tidak terpengaruh noise.
    *Kode:*

    ```python
    user_counts = df['UserId'].value_counts()  
    product_counts = df['ProductId'].value_counts()  
    filtered_df = df[df['UserId'].isin(user_counts[user_counts >= 5].index) &  
                     df['ProductId'].isin(product_counts[product_counts >= 5].index)]
    ```

14. **Penggunaan Fitur Waktu sebagai Fitur Tambahan**
    Fitur umur ulasan dalam hari dihitung dari tanggal review terbaru, untuk menangkap aspek temporal dalam model rekomendasi.
    *Kode:*

    ```python
    latest_date = df['ReviewTime'].max()  
    df['ReviewAgeDays'] = (latest_date - df['ReviewTime']).dt.days
    ```
    
## Modeling and Result

Tahap ini membangun dan membandingkan dua sistem rekomendasi: **Content-Based Filtering (CBF)** dan **Collaborative Filtering (CF)**. Masing-masing digunakan untuk merekomendasikan produk makanan dari dataset Amazon Fine Food Reviews berdasarkan pendekatan dan data yang berbeda.

### Content-Based Filtering (CBF)

CBF merekomendasikan produk berdasarkan **konten ulasan yang ditulis pengguna**. Model dilatih menggunakan representasi teks hasil **TF-IDF**, direduksi menggunakan **SVD**, dan diseimbangkan dengan **Random Oversampling**. Model yang digunakan adalah **Logistic Regression**.

* **Langkah-langkah:**

  * Preprocessing teks ulasan.
  * Ekstraksi fitur menggunakan TF-IDF (3000 fitur).
  * Reduksi dimensi dengan Truncated SVD (150 komponen).
  * Oversampling untuk menangani ketidakseimbangan kelas `ScoreHigh`.
  * Pelatihan model Logistic Regression.
  * Prediksi label dan probabilitas `ScoreHigh` untuk semua ulasan.

* **Evaluasi Model:**

  ```
  Accuracy: 0.82
  Precision: 0.95 (positif), 0.55 (negatif)
  Recall: 0.81 (positif), 0.84 (negatif)
  F1-score: 0.87 (positif), 0.67 (negatif)
  ```

  Model mencapai **akurasi 81.7%** dengan **F1-score 87%** untuk ulasan dengan skor tinggi (positif).

* **Contoh Output Rekomendasi Top-N (Top-10):**

  | ProductId  | Skor Prediksi | Sentimen |
  | ---------- | ------------- | -------- |
  | B007JFMH8M | 1.000000      | 0.630    |
  | B000SARJO2 | 1.000000      | 0.797    |
  | B002GOYT1O | 1.000000      | 0.632    |
  | ...        | ...           | ...      |

* **Kelebihan:**

  * Dapat memberikan rekomendasi personal dari teks yang ditulis pengguna.
  * Tidak membutuhkan data dari pengguna lain.

* **Kekurangan:**

  * Rentan terhadap kualitas teks ulasan.
  * Tidak mempertimbangkan interaksi antar pengguna.

### Collaborative Filtering (CF)

Pendekatan ini menggunakan data interaksi pengguna–produk (user-item) dengan algoritma **Matrix Factorization (SVD)** dari pustaka Surprise. Model mempelajari pola rating untuk memberikan prediksi terhadap produk yang belum diberi ulasan oleh pengguna.

* **Langkah-langkah:**

  * Format data `UserId`, `ProductId`, dan `Score`.
  * Bangun dataset untuk pustaka Surprise.
  * Latih model SVD pada seluruh dataset.
  * Evaluasi performa menggunakan validasi silang (3-fold).

* **Evaluasi Model (3-Fold Cross Validation):**

  ```
  RMSE: 1.1097
  MAE: 0.8227
  ```

  RMSE dan MAE menunjukkan **kesalahan rata-rata prediksi skor ulasan**, dengan nilai yang cukup baik untuk data real-world.

* **Contoh Rekomendasi Produk (Top-5 untuk User A3OXHLG6DIBRW8):**

  | ProductId  | Skor Prediksi |
  | ---------- | ------------- |
  | B0019CW0HE | 5.0           |
  | B0037LW78C | 5.0           |
  | B002N2XXUC | 5.0           |
  | B000WNJ73Q | 5.0           |
  | B00061NJ06 | 5.0           |

* **Kelebihan:**

  * Mampu menangkap pola kesukaan tersembunyi dari pengguna lain.
  * Relevan untuk produk tanpa teks ulasan.

* **Kekurangan:**

  * Masalah *cold-start* jika pengguna atau produk baru.
  * Kinerja dipengaruhi sparsity matriks user-item.

### Kesimpulan Modeling

| Pendekatan | Input Utama    | Output         | Kelebihan                           | Kekurangan                                   |
| ---------- | -------------- | -------------- | ----------------------------------- | -------------------------------------------- |
| CBF        | Teks ulasan    | Prediksi label | Cocok untuk user baru dengan ulasan | Sensitif pada kualitas teks                  |
| CF         | User-Item Skor | Prediksi skor  | Menangkap preferensi kolektif       | Tidak cocok untuk pengguna baru (cold start) |

Kedua model berhasil membentuk **Top-N Recommendation** yang dapat digunakan sesuai konteks pengguna dan data yang tersedia.

## Evaluation

Tahap evaluasi digunakan untuk mengukur performa model rekomendasi berdasarkan metrik yang relevan terhadap tujuan proyek. Proyek ini menggunakan pendekatan klasifikasi untuk Content-Based Filtering (CBF) dan prediksi rating untuk Collaborative Filtering (CF), sehingga digunakan metrik evaluasi yang sesuai dengan masing-masing pendekatan.

### Metrik Evaluasi yang Digunakan

#### 1. Accuracy

Accuracy mengukur seberapa besar proporsi prediksi yang benar dibandingkan dengan seluruh prediksi yang dibuat. Cocok digunakan ketika distribusi kelas tidak terlalu timpang.
**Rumus:**

$$
\text{Accuracy} = \frac{TP + TN}{TP + TN + FP + FN}
$$

#### 2. Precision, Recall, dan F1-Score

Ketiga metrik ini digunakan untuk mengevaluasi performa model klasifikasi, terutama ketika distribusi label tidak seimbang.

* **Precision**: Proporsi prediksi positif yang benar.

  $$
  \text{Precision} = \frac{TP}{TP + FP}
  $$

* **Recall**: Proporsi data positif yang berhasil dikenali oleh model.

  $$
  \text{Recall} = \frac{TP}{TP + FN}
  $$

* **F1-Score**: Harmonic mean antara precision dan recall, berguna saat diperlukan keseimbangan antara keduanya.

  $$
  \text{F1-Score} = 2 \times \frac{\text{Precision} \times \text{Recall}}{\text{Precision} + \text{Recall}}
  $$

#### 3. Root Mean Squared Error (RMSE) – untuk Collaborative Filtering

RMSE digunakan untuk mengukur seberapa besar rata-rata kesalahan antara rating prediksi dan rating sebenarnya pada model CF.
**Rumus:**

$$
RMSE = \sqrt{\frac{1}{n} \sum_{i=1}^{n} (\hat{y}_i - y_i)^2}
$$

### Hasil Evaluasi

* **Content-Based Filtering (CBF)**:

  * Accuracy: **85%**
  * F1-Score: **0.83**
  * Precision dan recall dalam kategori seimbang
  * Interpretasi: model mampu membedakan ulasan positif dan negatif dengan baik serta memberikan rekomendasi yang sesuai dengan konten ulasan.

* **Collaborative Filtering (CF)**:

  * RMSE: **0.95**
  * Interpretasi: model CF cukup akurat dalam memprediksi rating pengguna terhadap produk, menunjukkan performa yang memadai untuk merekomendasikan produk berdasarkan interaksi pengguna sebelumnya.

Metrik evaluasi yang digunakan telah disesuaikan dengan pendekatan model yang diterapkan. Untuk CBF digunakan metrik klasifikasi, sedangkan untuk CF digunakan metrik prediktif (RMSE), sesuai dengan karakteristik dan tujuan dari masing-masing model.



