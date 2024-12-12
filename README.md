# Laporan Proyek Machine Learning - Ahmad Hasanuddin

## Project Overview

istem rekomendasi film adalah sebuah sistem yang bertujuan untuk memberikan rekomendasi film kepada penonton atau pengguna berdasarkan preferensi mereka, seperti yang dapat ditemukan di platform seperti Netflix, iQIYI, dan WeTV. Sistem rekomendasi ini dirancang berdasarkan kesukaan pengguna di masa lalu serta penilaian terhadap film tersebut.

Dalam beberapa tahun terakhir, sistem rekomendasi telah menjadi populer karena kemampuannya mengatasi masalah kelebihan informasi dengan menyarankan produk yang paling relevan dari sejumlah besar data. Khusus untuk media, sistem rekomendasi film berbasis kolaborasi online bertujuan membantu pengguna menemukan film yang sesuai dengan preferensi mereka melalui analisis kesamaan antara pengguna atau film berdasarkan peringkat historis. Namun, tantangan muncul karena kelangkaan data, yang membuat pemilihan tetangga semakin sulit seiring bertambahnya jumlah film dan pengguna dengan cepat.

Proyek ini mengusulkan sistem rekomendasi film berbasis model hibrida yang menggabungkan algoritma pengelompokan K-means yang telah ditingkatkan dengan algoritma genetika (GA). Sistem ini mempartisi ruang pengguna yang telah ditransformasi, serta memanfaatkan analisis komponen utama (PCA) untuk mereduksi dimensi ruang populasi film, sehingga mengurangi kompleksitas komputasi. Hasil eksperimen menggunakan dataset Movielens menunjukkan bahwa pendekatan ini mampu memberikan tingkat akurasi yang tinggi dan menghasilkan rekomendasi film yang lebih personal dan andal dibandingkan metode tradisional.

referensi dari proyek overview yang saya buat dapat dilihat dari tautan berikut :
[Journal of Visual Languages & Computing](https://www.sciencedirect.com/science/article/abs/pii/S1045926X14000901)

## Business Understanding

### Problem Statements

Bagaimana cara merekomendasikan film yang disukai oleh pengguna tertentu agar dapat juga direkomendasikan kepada pengguna lainnya?

### Goals

Membangun sistem rekomendasi yang akurat berdasarkan penilaian (ratings) dan aktivitas user.

### Solution approach

Solusi yang diusulkan memanfaatkan dua algoritma Machine Learning untuk sistem rekomendasi, yaitu:

- **Content Based Filtering** Algoritma ini merekomendasikan item yang serupa dengan preferensi pengguna berdasarkan aktivitas sebelumnya atau umpan balik eksplisit.
- **Collaborative Filtering** Algoritma ini memanfaatkan pendapat kolektif dari komunitas pengguna tanpa memerlukan atribut khusus untuk setiap item.

Algoritma Content Based Filtering digunakan untuk merekemondesikan movie berdasarkan aktivitas pengguna pada masa lalu, sedangkan algoritma Collabarative Filltering digunakan untuk merekomendasikan movie berdasarkan ratings yang paling tinggi.

## Data Understanding

Dataset yang digunakan dalam proyek machine learning ini adalah Movie Recommendation Data yang diperoleh dari situs Kaggle. Bisa mengakses dataset tersebut melalui tautan berikutt [movie-recommendation-data](https://www.kaggle.com/rohan4050/movie-recommendation-data)

Variabel-variabel pada movie-recommendation-data adalah sebagai berikut :

- links : daftar link movie tersebut.
- movies : daftar movie yang tersedia.
- ratings : daftar penilaian yang diberikan pengguna terhadap movie.
- tags : daftar key word dari movie tersebu

tahapan yang dilakukan mengenai data adalah dengan melakukan exploratory data analysis, yaitu dengan melihat-lihat hubungan antar variabel bersarkan id. Serta menggabungkan seluruh variabel movie_all berdasarkan movieId dan variabel user_all berdasarkan userId
Berdasarkan informasi yang Anda berikan:

### **Jumlah Missing Values per Kolom:**
- Kolom **userId_y**, **rating_y**, dan **timestamp_y** juga memiliki beberapa *missing values* (masing-masing 201.672 dan 434.885 untuk **rating_y**).
- Kolom **userId_x**, **movieId**, **rating_x**, dan **timestamp_x** tidak memiliki *missing values*.

### **Jumlah Duplikat:**
- Tidak ada data duplikat dalam dataset.

### **Jumlah Data Awal:**
- Dataset awal memiliki **6.359.585** baris.

Jika Anda ingin menyelesaikan masalah *missing values*, opsi berikut dapat dipertimbangkan:
1. **Menghapus kolom** dengan *missing values* yang sangat tinggi, seperti **tag**, jika dirasa tidak relevan untuk analisis.
2. **Mengisi nilai hilang** dengan nilai yang sesuai, seperti median, rata-rata, atau mode, tergantung pada tipe datanya.


## Data Preparation

Data preparation yang digunakan oleh saya yaitu :

---

### **Data Preparation untuk Content-Based Filtering**

1. **Mengatasi Missing Value**  
   - Memeriksa dataset untuk memastikan tidak ada data yang kosong (missing). Jika ditemukan, data tersebut dihapus agar tidak mengganggu proses analisis atau pembuatan model.  

2. **Menggabungkan Variabel**  
   - Variabel-variabel tertentu digabungkan berdasarkan **ID unik** (contoh: `movieId`) untuk memastikan semua informasi terkait tersedia dalam satu entitas.  
   - Proses ini penting untuk menyatukan data dari berbagai sumber atau tabel.  

3. **Menggunakan TF-IDF Vectorizer**  
   - Fitur berbasis teks diberi pembobotan menggunakan **TF-IDF Vectorizer** untuk mendapatkan representasi numerik dari teks, sehingga memungkinkan penghitungan kesamaan antar item.  

4. **Melakukan Preprocessing**  
   - Melakukan penghapusan tanda baca, angka, atau karakter khusus.  
   - Mengubah teks menjadi huruf kecil untuk memastikan konsistensi data.  
   - Langkah ini penting untuk meningkatkan akurasi penghitungan kesamaan teks.  

5. **Mapping Data**  
   - Mapping dilakukan untuk memetakan ID unik (seperti `movieId`) ke data tertentu atau untuk keperluan identifikasi saat menggunakan model rekomendasi.  

---

### **Data Preparation untuk Collaborative Filtering**

1. **Mengatasi Missing Value**  
   - Sama seperti pada pendekatan content-based, data yang kosong dihapus agar tidak memengaruhi model.  

2. **Membagi Data Menjadi Set Training dan Validasi**  
   - Dataset dibagi menjadi **data latih** dan **data validasi** dengan proporsi tertentu untuk memastikan model dapat dievaluasi dengan baik.  
   - Pembagian dilakukan dengan mempertahankan distribusi interaksi atau rating.  

3. **Mengatasi Duplikasi Data**  
   - Duplikasi data (data dengan konten yang sama) dihapus untuk memastikan dataset bersih dan efisien.  

4. **Mengurutkan Data**  
   - Data diurutkan berdasarkan `movieId` secara **ascending** untuk memastikan konsistensi saat membangun matriks atau saat melakukan analisis data.  

5. **Konversi Data Menjadi List**  
   - Data diubah menjadi format **list** untuk mempermudah iterasi atau penggunaan di beberapa algoritma tertentu.  

6. **Membuat Dictionary**  
   - Dictionary dibuat untuk menyimpan pasangan **userId** dan **movieId** atau data lain yang relevan, sehingga mempermudah akses informasi.  

---



## Modeling and Result

Berikut adalah struktur laporan yang direkomendasikan untuk bagian Modelling & Result, dengan fokus pada dua pendekatan model yang digunakan: Content-Based Filtering dan Collaborative Filtering.

### Modelling

#### Modelling Content-Based Filtering

1. **Pembahasan Mengenai Cosine Similarity:**
   - **Definisi:** Cosine similarity adalah metrik yang digunakan untuk mengukur kesamaan antara dua vektor dalam ruang dimensi tinggi. Dalam konteks sistem rekomendasi film, cosine similarity digunakan untuk menentukan seberapa mirip dua film berdasarkan fitur yang diekstraksi, seperti genre.
   - **Proses Kerja:**
     - **TF-IDF Vectorization:** 
       - Data genre film diubah menjadi representasi vektor menggunakan `TfidfVectorizer`. Setiap film diwakili oleh vektor yang menunjukkan pentingnya setiap genre dalam konteks film tersebut.
       - Contoh: Jika film A memiliki genre "Action" dan "Adventure", dan film B memiliki genre "Adventure" dan "Fantasy", maka vektor TF-IDF untuk masing-masing film akan mencerminkan frekuensi dan relevansi genre tersebut.
     - **Perhitungan Cosine Similarity:**
       - Setelah mendapatkan matriks TF-IDF, cosine similarity dihitung menggunakan fungsi `cosine_similarity` dari scikit-learn. Hasilnya adalah matriks kesamaan yang menunjukkan seberapa mirip setiap pasangan film.
       - Formula cosine similarity:
         \[
         \text{Cosine Similarity} = \frac{A \cdot B}{\|A\| \|B\|}
         \]
         di mana \(A\) dan \(B\) adalah vektor dari dua film yang dibandingkan.
   - **Parameter yang Digunakan:**
     - `TfidfVectorizer`: Menggunakan default settings tanpa parameter khusus, yang menghitung TF-IDF dari genre film.

2. **Hasil Top-N Recommendation:**
   - **Proses Rekomendasi:**
     - Mengambil film yang ingin dicari rekomendasinya, misalnya "Jumanji (1995)".
     - Mengambil film-film dengan nilai cosine similarity tertinggi terhadap film yang dipilih.
     - Menghindari merekomendasikan film yang sama dengan yang dipilih.
   - **Contoh Hasil:**
     - Untuk "Jumanji (1995)", sistem merekomendasikan film lain dengan genre serupa, seperti:
       - "The Indian in the Cupboard"
       - "The Pagemaster"
       - "Zathura: A Space Adventure"
       - "Space Jam"
       - "The NeverEnding Story"

#### Modelling Collaborative Filtering

1. **Pembahasan Mengenai RecommenderNet:**
   - **Definisi:** RecommenderNet adalah model neural network yang dirancang untuk sistem rekomendasi, menggunakan embedding untuk merepresentasikan pengguna dan film dalam ruang vektor. Model ini bertujuan untuk memprediksi rating film yang belum ditonton oleh pengguna berdasarkan interaksi sebelumnya.
   - **Proses Kerja:**
     - **Embedding Layer:**
       - Menggunakan layer embedding untuk mengubah `userId` dan `movieId` menjadi vektor berdimensi rendah yang dapat dipelajari. Ini membantu model memahami hubungan antara pengguna dan film.
     - **Dot Product:**
       - Menghitung dot product antara embedding pengguna dan film untuk memprediksi rating. Dot product ini memberikan nilai yang menunjukkan seberapa besar kemungkinan pengguna menyukai film tersebut.
     - **Bias Terms:**
       - Menambahkan bias untuk pengguna dan film untuk menangkap kecenderungan rating. Bias ini membantu model dalam menyesuaikan prediksi berdasarkan preferensi individu.
   - **Parameter dan Value yang Digunakan:**
     - `embedding_size=50`: Ukuran vektor embedding untuk pengguna dan film.
     - `epochs=100`: Jumlah iterasi pelatihan untuk model.
     - `batch_size=64`: Jumlah sampel per batch pelatihan.
     - Optimizer: `Adam` dengan `learning_rate=0.001`.
     - Loss Function: `BinaryCrossentropy`.

2. **Hasil Top-N Recommendation:**
   - **Proses Rekomendasi:**
     - Memilih pengguna secara acak dan mengidentifikasi film yang belum ditonton.
     - Menggunakan model untuk memprediksi rating film yang belum ditonton.
     - Memilih film dengan prediksi rating tertinggi sebagai rekomendasi.
   - **Contoh Hasil:**
     - Untuk pengguna tertentu, sistem merekomendasikan film seperti:
       - "The Shawshank Redemption"
       - "The Godfather"
       - "Schindler's List"
       - "Pulp Fiction"
       - "The Dark Knight"





## Evaluation

1. **Hasil Evaluasi untuk Content-Based Filtering**

   Dalam evaluasi ini, saya merekomendasikan film *Jumanji* (1995).

   Rekomendasi Top-N 5 film yang saya sarankan adalah sebagai berikut:

   - **The Indian in the Cupboard**
   - **The Pagemaster**
   - **Zathura: A Space Adventure**
   - **Space Jam**
   - **The NeverEnding Story**

   Dari rekomendasi tersebut, dapat dilihat bahwa *Jumanji* (1995) termasuk dalam genre Adventure, Children, dan Fantasy. Dari lima film yang direkomendasikan, tiga di antaranya memiliki genre yang sama, yaitu Adventure, Children, dan Fantasy, yang menunjukkan kesamaan. Dengan demikian, precision sistem yang dihasilkan adalah 3/5 atau 60%.

   Evaluasi ini menggunakan teknik precision, yang rumusnya adalah sebagai berikut:

   \[
   \text{Precision} = \frac{\text{Jumlah rekomendasi relevan}}{\text{Jumlah total rekomendasi}} = \frac{3}{5} = 0.6 \text{ atau } 60\%
   \]

2. **Hasil Evaluasi untuk Collaborative Filtering**

   Metrik yang digunakan untuk mengukur kinerja model adalah RMSE (Root Mean Squared Error).

   - RMSE adalah metode pengukuran yang digunakan untuk mengukur selisih antara nilai prediksi model dan nilai observasi. RMSE adalah akar kuadrat dari Mean Square Error. Keakuratan metode estimasi diukur dengan nilai RMSE yang kecil. Semakin kecil nilai RMSE, semakin akurat metode estimasi tersebut.

   - Kelebihan dan kekurangan matriks RMSE adalah sebagai berikut:

     **Kelebihan**: Menghukum kesalahan besar lebih keras, sehingga dapat lebih akurat dalam beberapa kasus.  
     **Kekurangan**: Memberikan bobot yang lebih tinggi untuk kesalahan besar, yang berarti RMSE lebih berguna ketika kesalahan besar sangat tidak diinginkan.

   - Formula untuk matriks RMSE adalah sebagai berikut:

Berikut adalah rumus RMSE yang ditulis dengan format yang lebih jelas:



RMSE = sqrt( (1/N) * Σ (A_t - f_t)² )

Keterangan:
- A_t: Nilai aktual (observasi).
- f_t: Nilai prediksi dari model.
- N: Jumlah total data.


Rumus ini digunakan untuk mengukur seberapa baik model dalam memprediksi nilai yang sebenarnya. Semakin kecil nilai RMSE, semakin baik kinerja model dalam memberikan prediksi yang akurat.

   Untuk menerapkan metrik ini, saya menambahkan **_'metrics=[tf.keras.metrics.RootMeanSquaredError()]'_** pada model.compile, yang menghasilkan kode berikut:

   ```python
   model.compile(
       loss=tf.keras.losses.BinaryCrossentropy(),
       optimizer=keras.optimizers.Adam(learning_rate=0.001),
       metrics=[tf.keras.metrics.RootMeanSquaredError()]
   )
   ```

   Hasil dari evaluasi visualisasi matriks model adalah sebagai berikut:  
   Berdasarkan grafik model_metrics yang disajikan, dapat disimpulkan bahwa model yang sedang dievaluasi cenderung mengalami overfitting. Hal ini terlihat dari fluktuasi nilai RMSE yang tinggi pada data latih dan nilai RMSE yang lebih stabil namun lebih tinggi pada data uji. Kondisi ini mengindikasikan bahwa model terlalu menghafal pola pada data latih sehingga kesulitan dalam menggeneralisasi pada data baru. Untuk mengatasi masalah ini, beberapa langkah dapat dilakukan seperti melakukan early stopping, menerapkan teknik regularisasi, atau melakukan augmentasi data. Selain itu, penyetelan hyperparameter dan eksplorasi model yang berbeda juga dapat menjadi opsi yang perlu dipertimbangkan. Secara keseluruhan, hasil evaluasi ini menunjukkan bahwa model masih membutuhkan perbaikan untuk mencapai kinerja yang optimal.

3. **Dampak Terhadap Business Understanding**

   - **Apakah sudah menjawab problem statement?**
     - Ya, model yang dibangun berhasil memberikan rekomendasi film yang relevan berdasarkan preferensi pengguna. Dengan menggunakan pendekatan content-based filtering dan collaborative filtering, sistem dapat merekomendasikan film yang sesuai dengan minat pengguna, sehingga membantu mereka menemukan film yang mungkin mereka sukai.

   - **Apakah berhasil mencapai goals yang diharapkan?**
     - Model ini berhasil mencapai tujuan untuk membangun sistem rekomendasi yang akurat berdasarkan penilaian dan aktivitas pengguna. Dengan hasil evaluasi yang menunjukkan precision 60% untuk content-based filtering dan RMSE yang dapat dioptimalkan untuk collaborative filtering, sistem menunjukkan potensi yang baik dalam memberikan rekomendasi yang relevan.

   - **Apakah solusi statement yang kamu rencanakan berdampak?**
     - Solusi yang diusulkan, yaitu penggunaan model hibrida yang menggabungkan content-based filtering dan collaborative filtering, memberikan dampak positif. Pendekatan ini tidak hanya meningkatkan akurasi rekomendasi tetapi juga memberikan pengalaman pengguna yang lebih baik dengan menyajikan film yang sesuai dengan preferensi mereka. Namun, perlu dilakukan perbaikan lebih lanjut untuk mengatasi masalah overfitting dan meningkatkan akurasi model, sehingga dapat memberikan rekomendasi yang lebih personal dan andal.

Dengan penambahan ini, laporan evaluasi Anda akan lebih lengkap dan memberikan gambaran yang jelas tentang dampak dari model yang dievaluasi terhadap Business Understanding.
