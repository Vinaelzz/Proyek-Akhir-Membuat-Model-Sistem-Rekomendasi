# Proyek-Akhir-Membuat-Model-Sistem-Rekomendasi-Movie
Nama : Sievin Nathanael

##Project Overview
  pada era digital ini sudah banyak sekali website-website untuk menonton/streaming movie bertebaran di internet mulai dari Netflix, idlix, iqiyi, disney+, dan lain sebagainya. Dan salah satu permasalahan yang sering dihadapi para pengguna website tersebut adalah mereka bingung akan movie apa yang harus di tontonnya dikarenakanq mereka sudah kehabisan stock movie yang ingin di tonton dan bingung mencari movie yang memiliki genre yang sama dan rating yang bagus sesuai dengan selera orang tersebut. Oleh karena itu sistem rekomendasi ini dapat membantu para user yang sedang kebingungan dalam mencari movie yang cocok dengannya dan sistem rekomendasi ini dapat diterapkan di website-website streaming movie tersebut.
  
  REFRENSI TARUH DI BAWAH.
  
## Business Understanding

### Problem Statements & Goals
Berdasarkan kondisi yang telah diuraikan sebelumnya, peneliti akan mengembangkan model yang dapat memberikan rekomendasi terhadap user terkait movie yang cocok dengannya. Berikut adalah jabaran dari problem statementnya :

1. Bagaimana cara memberikan rekomendasi movie yang sesuai dengan kepribadian user.
2. bagaimana cara memberikan rekomendasi yang disukai oleh kebanyakan user kepada user tertentu.

Untuk  menjawab pertanyaan tersebut, peneliti akan membuat model rekomendasi dengan tujuan atau goals sebagai berikut:

1. Membuat sistem rekomendasi berdasarkan history movie yang di tonton user
2. Membuat sistem rekomendasi berdasarkan rating yang diberikan user lain terhadap sebuah movie

### Solution Approach
Dalam pembuatan model ini terdapat 2 model solusi yang digunakan yaitu :

1. Content Based Filtering adalah algoritma yang merekomendasikan movie berdasarkan kegiatan/tindakan masalalu user atau history user.
2. Collaborative Filtering. adalah algoritma yang berdasarkan pada rating user lainnya, dan dia tidak memerlukan history dari setiap user

##Data Understanding
Dataset yang digunakan dalam proyek ini berasal dari kaggle yaitu : [Movie recommendation Dataset]([https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset](https://www.kaggle.com/datasets/rohan4050/movie-recommendation-data)).


informasi terkait dataset :
- Dataset memiliki format CSV (Comma-Seperated Values).
- Menggunakan 3 dataset yaitu : 
  1. 'movies.csv' = merupakan daftar judul movie yang tersedia.Berikut adalah isi dari dataset berikut :
      - MovieId : id dari movie
      - Title : Judul movie
      - genres : genre dari movie
      berisi 9742 baris data dan 3 kolom 
  2. 'ratings.csv' = merupakan daftar rating dari user terhadap movie
      - UserId : id dari user yang memberikan rating
      - MovieId : id dari movie
      - rating : rating yang diberikan user
      - timestamp : penunjuk waktu
      berisi 100836 baris data dan 4 kolom
  3. 'tags.csv' = merupakan daftar kata kunci / genre dari movie
      - UserId = id user
      - movieId : id movie
      - tag : genres/kata kunci dari movie
      - timestamp : penunjuk waktu
       berisi 3683  baris data dan 4 kolom
- Terdapat missing value dalam beberapa dataset 

## Data Preparation

### Data Cleaning
- merging data / menggabungkan data menjadi satu tabel agar lebih gampang di telaah dan dianalisis
- melakukan pengecekan dataset untuk menemukan missing value, melakukan drop terhadap missing value 
- melakuan drop terhadap kolom 'Timestamp' dikarenakan tidak memiliki hubungan dengan sistem rekomendasi yang akan dibuat
### Analisis
- melakukan Univariate exploratory data analysis untuk menganalisis tiap faktor/fitur yang ada secara terpisah sehingga kita bisa mengetahui elemen unik/outliers dari faktor tersebut dan outliers tersebut akan di drop untuk menghindari ketidakakuratan hasil predictive analysis.
- dan dari hasil univariate exloratory data anlysis tidak ditemukan outliers yang berarti
### Encoding
tahap encoding adalah tahap menyandikan variabel menjadi dalam bentuk integer, pada proyek ini variabel yang di encode adalah variabel "movieId", dan "UserID"<
### Randomize dataset
pengacakan data agar distribusi datanya menjadi random seblum dilakukan modelling.
### Splitting data
pada tahap ini data dibagi menjadi 2 bagian yaitu, 80% data train ( data yang akan dilatih) dan 20% data test ( data yang akan di uji), splitting ini dilakukan untuk mempermudah pengukuran kinerja model machine learningnya
### Standarization
Pada data rating yang digunakan pada proyek ini berada pada rentang 0.5  hingga 5.0. Penerapan standarisasi ini akan mempermudah proses training nantinya, dikarenakan akan teradapat kemungkinan munculnya bias jika data tidak di standarisasi terlebih dahulu.

## Modelling

pada tahap modelling ada beberapa hal yang dilakukan yaitu :

### Membuat kelas RecommenderNet 
dalam pembuatan recommenderNet digunakan beberapa parameter untuk menghasilkan model recommender, berikut adalah variabel/parameter yang digunakan :
| 1 | Num_user       | jumlah total user                                                 |
|---|----------------|-------------------------------------------------------------------|
| 2 | num_movie      | jumlah total movie                                                |
| 3 | embedding_size | ukuran dimensi yang digunakan untuk embedding data user dan movie |

### Hyperparameter tuning
pada tahap ini dilakukan hyperparameter tuning untuk menentukan paramater mana yang menghasilkan hasil yang paling optimal nantinya, dan hasil dari dilakukannya hyperparameter tuning ini adalah sebagai berikut :

![image](https://user-images.githubusercontent.com/73600512/201580295-82bf619a-999f-49cd-93fa-771f6aca5b50.png)

ukuran dimensi embedding yang paling optimal adalah : 2

## Evaluasi
Metrik evaluasi yang digunakan pada proyek ini root mean squared error (RMSE). Akurasi menentukan tingkat kemiripan antara hasil rekomendasi dengan nilai yang sebenarnya (y_test). Mean squared error (MSE) mengukur error dalam model statistik dengan cara menghitung rata-rata error dari kuadrat hasil aktual dikurang hasil prediksi. Berikut formulan MSE :

![image](https://user-images.githubusercontent.com/73600512/201581347-4f8e50d2-9252-480e-bf35-6661a22e5124.png)


i	=	variable i
{N}	=	number of non-missing data points
x_{i}	=	actual observations time series
\hat{x}_{i}	=	estimated time series
