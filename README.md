# Proyek-Akhir-Membuat-Model-Sistem-Rekomendasi-Movie
Nama : Sievin Nathanael

## Project Overview
  Pada era digital ini sudah banyak sekali website-website untuk menonton/streaming movie bertebaran di internet mulai dari Netflix, idlix, iqiyi, disney+, dan lain sebagainya. Dan salah satu permasalahan yang sering dihadapi para pengguna *website* tersebut adalah mereka bingung akan movie apa yang harus di tontonnya dikarenakanq mereka sudah kehabisan *stock movie* yang ingin di tonton dan bingung mencari movie yang memiliki genre yang sama dan rating yang bagus sesuai dengan selera orang tersebut. Oleh karena itu sistem rekomendasi ini dapat membantu para user yang sedang kebingungan dalam mencari movie yang cocok dengannya dan sistem rekomendasi ini dapat diterapkan di *website-website streaming movie* tersebut.
  

  
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

## Data Understanding
Dataset yang digunakan dalam proyek ini berasal dari kaggle yaitu : [Movie recommendation Dataset]([https://www.kaggle.com/datasets/fedesoriano/stroke-prediction-dataset](https://www.kaggle.com/datasets/rohan4050/movie-recommendation-data)).

informasi terkait dataset :
- 
Jumlah  movie :  9742
Jumlah  user yang memberikan rating :  610
Jumlah  ratings dari user :  9724
Jumlah tag/genre dari movie  :  1572
- Dataset memiliki format CSV (Comma-Seperated Values).
- Menggunakan 3 dataset yaitu : 
  1. 'movies.csv' = merupakan daftar judul movie yang tersedia.Berikut adalah isi dari dataset berikut :
      - MovieId : id dari movie
      - Title : Judul movie
      - genres : genre dari movie
      berisi 9742 baris data dan 3 kolom 
      
      ![image](https://user-images.githubusercontent.com/73600512/201588372-b31de08d-5696-4787-ab37-3efd89f55a4d.png)


      
  2. 'ratings.csv' = merupakan daftar rating dari user terhadap movie
      - UserId : id dari user yang memberikan rating
      - MovieId : id dari movie
      - rating : rating yang diberikan user
      - timestamp : penunjuk waktu
      
      ![image](https://user-images.githubusercontent.com/73600512/201588416-d7bcb859-8142-4ffd-9a2a-ba84b741cdf6.png)


      berisi 100836 baris data dan 4 kolom
      
      ![image](https://user-images.githubusercontent.com/73600512/201588207-572c7d66-5096-4c82-9028-ff52dc2a2ad9.png)
      
      Dari hasil visualisasi dataset di atas, kita dapat memahami lebih lanjut terkait data yang akan kita olah.
      pada dataset rating menunjukan bahwa sebagian besar user memberikan rating "4" terhadap movie yang di tonton
      
  3. 'tags.csv' = merupakan daftar kata kunci / genre dari movie
      - UserId = id user
      - movieId : id movie
      - tag : genres/kata kunci dari movie
      - timestamp : penunjuk waktu

      ![image](https://user-images.githubusercontent.com/73600512/201588480-00754324-dee9-4a21-8823-3cdb261d6413.png) 


       berisi 3683  baris data dan 4 kolom
- Terdapat missing value dalam beberapa dataset 

## Data Preparation

### Data Cleaning
pada tahapan ini digunakan library pandas untuk membantu proses pembersihan data
- merging data / menggabungkan data menjadi satu tabel agar lebih gampang di telaah dan dianalisis
- melakukan pengecekan dataset untuk menemukan missing value, melakukan drop terhadap missing value 
- melakuan *drop* terhadap kolom 'Timestamp' dikarenakan tidak memiliki hubungan dengan sistem rekomendasi yang akan dibuat
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

## Modelling & Result


pada tahap modelling ada beberapa hal yang dilakukan yaitu :
### Membuat kelas RecommenderNet 
RecommenderNet berguna untuk menghitung tingkat / skor kecocokan antar user dengan movie, 

dalam pembuatan recommenderNet digunakan beberapa parameter untuk menghasilkan model recommender, berikut adalah variabel/parameter yang digunakan :
| 1 | Num_user       | jumlah total user                                                 |
|---|----------------|-------------------------------------------------------------------|
| 2 | num_movie      | jumlah total movie                                                |
| 3 | embedding_size | ukuran dimensi yang digunakan untuk embedding data user dan movie |

dengan menggunakan "num_user" dan "num_movie" sebagai inputan untuk menghitung skor kecocokan antar user dan movie, lalu "embedding_size" berguna untuk memberikan batasan dimensi embedding pada proses perhitungan kecocokan. Semakin besar ukuran dimensi embeddingnya atau *embedding size* maka akan semakin tinggi akurasi dari perhitungan tetapi dapat menimbulkan *overfit* oleh karena itu untuk mencari parameter "embedding_size" yang tepat, maka dilakukanlah *hyperparameter tuning* agar bisa mendapatkan "embedding_size" yang cocok dan tepat untuk model ini. RecommenderNet ini termasuk kedalam proses pendekatan menggunakan *Collaborative based learning*


### Hyperparameter tuning
pada tahap ini dilakukan hyperparameter tuning untuk menentukan paramater mana yang menghasilkan hasil yang paling optimal nantinya, dan hasil dari dilakukannya hyperparameter tuning ini menunjukan ukuran dimensi embedding yang paling optimal adalah : 1


### Result

berikut adalah hasil modelling sistem rekomendasi berdasarkan 2 buah model solusi yaitu :

1. Content based learning 

berikut adalah hasil rekomendasi dari model content based learning 

![image](https://user-images.githubusercontent.com/73600512/201622441-db511350-53d2-46a4-b857-c7126dd8b365.png)

disini user memilih movie dengan judul "toy story (1995) untuk dijadikan sebagai patokan movie yang disukai user

| no | Title                                          | jumlah total userGenres                         |
|----|------------------------------------------------|-------------------------------------------------|
| 1  | The Good Dinosaur (2015)                       | Adventure\|ANimation\|children\|Comedy\|Fantasy |
| 2  | Adventures of Rocky and Bullwinkle, The (2000) | Adventure\|Animation\|Children\|Comedy\|fantasy |
| 3  | Moana (2016)                                   | Adventure\|Animation\|Children\|COmedy\|Fantasy |
| 4  | Wild, The (2006)                               | Adventure\|Animation\|Children\|Comedy\|Fantasy |
| 5  | Emperor's New GRoove, The (2000)               | Adventure\|Animation\|Children\|Comedy\|fantasy |

 lalu sistem akan memunculkan rekomendasi movie yang memiliki kata kunci / tag yang mirip dengan movie "toy story (1995), hasil dari rekomendasi itu berasal dari movie yang memiliki derajat kesamaan (similiarity degree) yang sama.

content based learning rekomendasi yang berasal dari perilaku  user, dengan model content based learning maka sistem akan mengambil genre & tag dari movie yang pernah di tonton user dahulu, lalu dari hasil history user tersebut maka sistem akan memunculkan rekomendasi movie yang memiliki genre dan tag yang sejenis

2. collaborative based learning

| no | Title                                          | jumlah total userGenres                         |
|----|------------------------------------------------|-------------------------------------------------|
| 1  | Kolya (Kolja) (1996)                           | Comedy, Drama |
| 2  | Jules and Jim (Jules et Jim) (1961)            | Drama, Romance |
| 3  | Neon Genesis Evangelion: The End of Evangelion (1997)           | Action,Animation,Drama,Fantasy, |
| 4  | Memories of Murder (Salinui chueok) (2003)     | Crime,Drama,Mystery,Thriller                  |
| 5  | Tekkonkinkreet (Tekkon kinkurîto) (2006)       | AAction,Adventure,Animation,Crime,Fantasy |
| 6  | Day of the Doctor, The (2013)                  | Adventure,Drama,Sci-Fi                          |
| 7  | Captain Fantastic (2016)                       | Drama                                           |
| 8  | Band of Brothers (2001)                        | Action,Drama,War                                |
| 9  | Three Billboards Outside Ebbing, Missouri (2017) | Crime,Drama                                   |

pada model ini akan menghasilkan rekomendasi berdasarkan rating tertinggi terhadap movie dari user, pertama sistem akan menunjukan movie yang memiliki rating paling tinggi, lalu sistem akan menunjukan 10 rekomendasi berdasarkan movie dengan rating tertinggi

## Evaluasi

1. Content based learning
 untuk model ini digunakan metrik *precision* untuk mendapatkan hasil rekomendasi, berikut adalah rumus dari *precision matrix* model
 
![image](https://user-images.githubusercontent.com/73600512/201631000-42c639cf-d5ed-4684-8741-c6dc7df78b1b.png)


2. Collaborative based learning
 
Metrik evaluasi yang digunakan pada model collaborative based lerarning root mean squared error (RMSE). Akurasi menentukan tingkat kemiripan antara hasil rekomendasi dengan nilai yang sebenarnya. Berikut formula RMSE :

![image](https://user-images.githubusercontent.com/73600512/201583080-dba9112b-58c0-4aa4-a260-d6b83236b74d.png)

At : Nilai Aktual.

ft = Nilai hasil peramalan.

N = banyaknya dataset



semakin rendah nilai RMSE menunjukkan bahwa variasi nilai yang dihasilkan oleh suatu model prakiraan mendekati variasi nilai obeservasinya. RMSE menghitung seberapa berbedanya seperangkat nilai. Semakin kecil nilai RMSE, semakin dekat nilai yang diprediksi dan diamati.

Berikut ini adalah plot metrik RMSE setelah proses pelatihan model.


![image](https://user-images.githubusercontent.com/73600512/201581657-0ccd95b2-b917-43ae-ae4f-24acb8ea9027.png)

dari hasil visualisasi dengan matplotlib diatas dapat disimpulkan bahwa dari proses diatas dengan menggunakan epoch 25, menghasilkan error pada validasi sebesar 0.20 dan error pada data train sebesar 0.18

## Conclusion

Dari penelitian diatas maka dapat dihasilkan sebuah sistem rekomendasi movie yang dapat memberikan rekomendasi kepada usernya melalui 2 buah pendekatan yaitu berdasarkan content based learning dan colaborative based learning. Yang dimana sistem rekomendasi tersebut memberikan rekomendasi movie sesuai dengan rating yang diberikan user lain serta memberikan rekomendasi movie sesuai dengan history / kegiatan masa lalu user.

## Refrensi
[1] M. Chenna Keshava, P. Narendra Reddy, S. Srinivasulu, & B. Dinesh Naik. (2020). Machine Learning Model for Movie Recommendation System. International Journal of Engineering Research And, V9(04), 800–805. https://doi.org/10.17577/ijertv9is040741

[2] Moody, J. (2019). What does RMSE really mean?. Towards Data Science. https://towardsdatascience.com/what-does-rmse-really-mean-806b65f2e48e

