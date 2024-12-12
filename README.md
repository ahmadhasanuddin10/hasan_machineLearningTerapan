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

- Mengatasi missing value : Memeriksa data untuk memastikan tidak ada yang kosong, dan jika ada, saya akan menghapusnya.
- Membagi data menjadi set training dan validasi: Untuk memisahkan data yang akan digunakan untuk pelatihan dan validasi model.
- Menggabungkan variabel: Menggabungkan beberapa variabel berdasarkan ID yang unik.
- Mengurutan data : untuk mengurutkan data berdasarkan movieId secara asceding.
- Mengatasi duplikasi data : Mengidentifikasi dan mengatasi data yang memiliki nilai atau konten yang sama.
- Konversi data menjadi iist :  Mengubah data menjadi format list.
- Membuat dictionary : Membuat dictionary berdasarkan data yang tersedia.
- Menggunakan TfidfVectorizer :Melakukan pembobotan pada data.
- melakukan preprocessing : Menghilangkan masalah yang dapat mempengaruhi hasil proses data.
- mapping data : Melakukan mapping untuk data yang ada.

## Modeling and Result

Proses pemodelan yang saya lakukan pada data ini melibatkan pembuatan dua algoritma machine learning, yaitu content-based filtering dan collaborative filtering. Untuk algoritma content-based filtering, saya menggunakan preferensi pengguna di masa lalu, sementara untuk collaborative filtering, saya memanfaatkan tingkat rating dari film yang bersangkutan.

Berikut adalah hasil dari kedua algoritma tersebut:

Proses pemodelan yang saya lakukan pada data ini melibatkan pembuatan dua algoritma machine learning, yaitu content-based filtering dan collaborative filtering. Untuk algoritma content-based filtering, saya menggunakan preferensi pengguna di masa lalu, sementara untuk collaborative filtering, saya memanfaatkan tingkat rating dari film yang bersangkutan.

Berikut adalah hasil dari kedua algoritma tersebut:

1. **Hasil dari content-based filtering**  
   Berdasarkan preferensi pengguna di masa lalu, film yang disukai oleh pengguna adalah *Jumanji* (1995) dengan genre Adventure, Children, dan Fantasy.  
   Dari hasil tersebut, lima rekomendasi teratas berdasarkan algoritma content-based filtering adalah film-film dengan genre yang sama, yaitu Adventure, Children, dan Fantasy, yang disarankan oleh sistem berdasarkan kesukaan pengguna sebelumnya.

2. **Hasil dari collaborative filtering**  
   Berdasarkan rating yang ada, film dengan genre comedy mendapatkan rating tertinggi. Selanjutnya, sepuluh rekomendasi teratas dari sistem adalah film dengan genre comedy dan drama, yang dipilih berdasarkan rating yang diberikan oleh pengguna lain.

## Evaluation

1. **Hasil Evaluasi untuk Content-Based Filtering**

Dalam evaluasi ini, saya merekomendasikan film *Jumanji* (1995).

Rekomendasi Top-N 5 film yang saya sarankan adalah sebagai berikut:

Dari rekomendasi tersebut, dapat dilihat bahwa *Jumanji* (1995) termasuk dalam genre Adventure, Children, dan Fantasy. Dari lima film yang direkomendasikan, tiga di antaranya memiliki genre yang sama, yaitu Adventure, Children, dan Fantasy, yang menunjukkan kesamaan. Dengan demikian, precision sistem yang dihasilkan adalah 3/5 atau 60%.

Evaluasi ini menggunakan teknik precision, yang rumusnya adalah sebagai berikut:

2. **Hasil Evaluasi untuk Collaborative Filtering**

Metrik yang digunakan untuk mengukur kinerja model adalah RMSE (Root Mean Squared Error).

- RMSE adalah metode pengukuran yang digunakan untuk mengukur selisih antara nilai prediksi model dan nilai observasi. RMSE adalah akar kuadrat dari Mean Square Error. Keakuratan metode estimasi diukur dengan nilai RMSE yang kecil. Semakin kecil nilai RMSE, semakin akurat metode estimasi tersebut.

- Kelebihan dan kekurangan matriks RMSE adalah sebagai berikut:

  **Kelebihan**: Menghukum kesalahan besar lebih keras, sehingga dapat lebih akurat dalam beberapa kasus.  
  **Kekurangan**: Memberikan bobot yang lebih tinggi untuk kesalahan besar, yang berarti RMSE lebih berguna ketika kesalahan besar sangat tidak diinginkan.

- Formula untuk matriks RMSE adalah sebagai berikut:

Keterangan:  
At: Nilai Aktual.  
ft: Nilai hasil prediksi.  
N: Jumlah dataset.

Untuk menerapkan metrik ini, saya menambahkan **_'metrics=[tf.keras.metrics.RootMeanSquaredError()]'_** pada model.compile, yang menghasilkan kode berikut:

Hasil dari evaluasi visualisasi matriks model adalah sebagai berikut:  
DBerdasarkan grafik model_metrics yang disajikan, dapat disimpulkan bahwa model yang sedang dievaluasi cenderung mengalami overfitting. Hal ini terlihat dari fluktuasi nilai RMSE yang tinggi pada data latih dan nilai RMSE yang lebih stabil namun lebih tinggi pada data uji. Kondisi ini mengindikasikan bahwa model terlalu menghafal pola pada data latih sehingga kesulitan dalam menggeneralisasi pada data baru. Untuk mengatasi masalah ini, beberapa langkah dapat dilakukan seperti melakukan early stopping, menerapkan teknik regularisasi, atau melakukan augmentasi data. Selain itu, penyetelan hyperparameter dan eksplorasi model yang berbeda juga dapat menjadi opsi yang perlu dipertimbangkan. Secara keseluruhan, hasil evaluasi ini menunjukkan bahwa model masih membutuhkan perbaikan untuk mencapai kinerja yang optimal.
