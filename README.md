# Laporan Proyek Machine Learning - Muhammad Harsye Ibra

## Project Overview

Anime, animasi Jepang yang terkenal, telah menjadi fenomena global dengan basis penggemar yang luas di seluruh dunia. Namun, dengan bertambahnya jumlah anime yang tersedia, penonton seringkali menghadapi kesulitan dalam menemukan rekomendasi anime yang sesuai dengan minat dan preferensi mereka. Permasalahan ini dapat diatasi dengan mengembangkan sistem rekomendasi yang efektif, seperti metode content-based filtering dan collaborative filtering.

Kesulitan dalam menemukan rekomendasi anime yang sesuai dapat disebabkan oleh pertumbuhan yang pesat dalam industri anime. Setiap tahun, puluhan atau bahkan ratusan anime baru dirilis, dengan berbagai genre, plot, dan tema. Penonton yang tidak terbiasa dengan dunia anime mungkin merasa kewalahan dan kesulitan menavigasi melalui berbagai pilihan yang ada. Bahkan bagi penonton yang sudah berpengalaman, memilih anime yang sesuai dengan preferensi mereka masih bisa menjadi tantangan. Perbedaan preferensi dan minat individu juga berperan dalam kesulitan ini. Setiap penonton memiliki preferensi yang unik, seperti genre yang disukai, karakter yang diminati, alur cerita yang menarik, dan gaya animasi tertentu. Sistem rekomendasi yang umumnya digunakan, seperti rekomendasi berdasarkan popularitas atau peringkat tertinggi, tidak selalu efektif dalam mengakomodasi kebutuhan individu. Penonton seringkali menginginkan rekomendasi yang lebih personal yang sesuai dengan minat dan preferensi mereka secara spesifik. Selain itu, Kurangnya sumber rekomendasi yang andal juga menjadi hambatan. Sementara ada beberapa situs web dan platform yang menyediakan rekomendasi anime, tidak semuanya akurat atau memadai. Beberapa situs web mungkin mengandalkan sistem rekomendasi yang kurang canggih atau hanya berfokus pada popularitas, yang dapat menghasilkan rekomendasi yang tidak memenuhi kebutuhan individu.

Untuk mengatasi kesulitan ini, pengembangan sistem rekomendasi anime menjadi solusi yang menarik. Metode content-based filtering dan collaborative filtering merupakan dua pendekatan yang umum digunakan dalam pengembangan sistem rekomendasi. Metode content-based filtering mencoba untuk memahami preferensi penonton dengan menganalisis konten atau fitur anime itu sendiri, seperti genre, sinopsis, karakter, dan gaya animasi. Dengan membandingkan preferensi penonton dengan atribut-atribut tersebut, sistem dapat merekomendasikan anime yang memiliki kesamaan dalam konten yang diminati. Metode collaborative filtering, di sisi lain, mencoba memanfaatkan data dari penonton lain untuk merekomendasikan anime. Dengan mengumpulkan informasi dari penonton dengan minat serupa, sistem dapat memberikan rekomendasi berdasarkan kesamaan preferensi dengan pengguna lain.

Proyek ini sangat penting dilakukan karena dengan sistem rekomendasi yang sesuai dan akurat dengan preferensi pengguna maka akan meningkatkan kepuasan pengguna dan membantu menghasilkan informasi yang berguna bagi industri anime.

Terdapat beberapa penelitian tentang sistem rekomendasi anime , antara lain dengan menggunakan metode *collaborative filtering* dengan menggunakan dataset yang sama tetapi hanya menggunakan 100.000 data saja dikarenakan keterbatasan kemampuan komputasi. Model tersebut memperoleh hasil evaluasi *Root-mean-square error* (RMSE) sebesar 2.5375[1]. Pada penelitian lainnya merupakan penelitian pembuatana sistem rekomendasi yang dihasilkan berdasarkan poster anime yang diekstrak menggunakan metode *Blended Alternate Least Squares with Explanation* (BALSE) dengan hasil evaluasi menggunakan RMSE sebesar 1.14954 pada seluruh data tes [2] .

## Business Understanding

### Problem Statements

Dari latar belakang masalah yang sudah dijelaskan, masalah yang dapat diselesaikan pada proyek ini adalah :
- Bagaimana cara memberikan rekomendasi anime kepada pengguna berdasarkan preferensi dan *rating* yang diberikan ?
- Bagaimana cara membuat sistem rekomendasi yang dapat memberikan rekomendasi *anime* yang relevan terhadap pengguna ?
- Bagaimana cara membuat model sistem rekomendasi dengan performa yang baik ?

### Goals

Dari masalah yang telah dijabarkan, maka tujuan dari proyek ini adalah:
- Menghasilkan rekomendasi anime kepada pengguna dengan membuat sistem rekomendasi *anime*.
- Membuat sistem rekomendasi anime yang dapat memberikan rekomendasi *anime* yang relevan terhadap pengguna.
- Menghasilkan model sistem rekomendasi dengan performa yang baik.

### Solution Approach

Untuk mencapai tujuan proyek diatas, maka akan digunakan dua pendekatan dalam membuat sistem rekomendasi, yaitu :
- *Content-based filtering*, yaitu pendekatan yang memberikan rekomendasi *anime* yang yang memiliki kesamaan dengan *anime* yang sedang ditonton atau dicari oleh pengguna.
- *Collaborative filtering*, yaitu pendekatan yang memberikan rekomendasi yang berasal dari data *anime* yang disukai atau pernah ditonton oleh pengguna dan pengguna lain.


## Data Understanding

Data yang digunakan pada proyek ini merupakan *dataset* yang berasal dari Kaggle dengan judul [*Anime Recommendations Database*](https://www.kaggle.com/datasets/CooperUnion/anime-recommendations-database). *Dataset* ini merupakan data rekomendasi dari 73516 pengguna dari website [myanimelist.net](myanimelist.net). Dataset ini berisi data preferensi dari 73516 pengguna pada 12294 *anime*. Setiap pengguna dapat menambahkan *anime* ke dalam daftar "sudah ditonton" mereka dan dapat memberikan rating pada *anime* tersebut. Dataset ini terdiri dari 2 files yaitu file anime.csv dengan total 12294 baris dan 7 kolom berisi daftar dari judul hingga rating keseluruhan anime dan file rating.csv dengan total 7813737 baris dan 3 kolom yang berisi user_id dan info dari anime yang diberi rating serta rating yang diberikan.

**Anime.csv**

anime_id - id unik yang mewakili judul *anime*.
name - judul lengkap *anime*.
genre - genre *anime*.
type - movie, TV, OVA, dll.
episodes - banyaknya episode anime, 1 jika sebuah film.
rating - rata-rata rating anime, antara 1 sampai 10.
members - jumlah anggota yang berada di group komunitas anime ini.

**Rating.csv**

user_id - id pengguna yang di-*generate* secara random.
anime_id - id anime yang pernah diberi rating oleh pengguna.
rating - *rating* yang diberikan pengguna antara 1 sampai 10 (-1 jika pengguna sudah menonton anime tetapi tidak memberi rating).


Dari kedua file tersebut masih terdapat data yang bernilai NaN atau null dan beberapa data duplikat. Akan dilakukan pembersihan data dengan metode yang berbeda untuk masing masing pendekatan.

### Exploratory Data Analysis

 Pada dataset anime.csv (yang selanjutnya akan ditulis dataset anime) memiliki kolom type yang berisi data media yang digunakan untuk penayangan anime tersebut

 ![Image](https://github.com/IbraHarsye/anime-recommendation-system/assets/56579824/27383eab-d5bd-4879-907b-f3dfe8c986fe)
 Gambar 1. Kolom type pada dataset anime

 Selanjutnya pada dataset rating.csv (yang selanjutnya akan disebut dataset rating) terdapat persebaran rating dari user. Persebaran data terbanyak berada diantara nilai 7, 8, dan 9.
 
![Image](https://github.com/IbraHarsye/anime-recommendation-system/assets/56579824/83c4a31c-8e90-4e89-a913-67243232872a)
 Gambar 2. Kolom rating pada dataset rating

Tabel. 1 Data statistik kolom rating pada dataset rating

|         | rating        |
|-------- |-------------- |
| mean    | 6.144030e+00  |
| std     | 3.727800e+00  |
| min     | -1.000000e+00	|
| 25%     | 6.000000e+00	|
| 50%     | 7.000000e+00  |
| 75%     | 9.000000e+00  |
| max     | 1.000000e+01	|

Pada dataset rating dapat dilihat rating minimal adalah -1 dan maksimal adalah 10. data yang bernilai -1 ini merupakan nilai yang diberikan jika seorang pengguna menonton sebuah *anime* dan belum memberikan rating. Baris data ini tidak akan digunakan dan akan dihapus nanti karena, tidak memberikan informasi tentang preferensi pengguna tersebut.

## Data Preparation

*Data preparation* yang dilakukan untuk pendekatan *content-based filtering* dan *collaboarative filtering* memiliki beberapa perbedaan dikarenakan pada *content-based filtering* hanya dibutuhkan kolom nama dan genre. Sedangkan pada *collaboarative filtering* menggunakan dataset gabungan dari dataset anime dan dataset rating. Tetapi proses *data cleaning* pada kedua dataset tetap sama.

### *Content-based Filtering*

Dataset yang digunakan untuk *Content-based Filtering* adalah hanya dataset anime. Pertama dataset akan dicek apakah terdapat kolom yang bernilai null maupun baris yang duplikat. Ternyata terdapat data null pada kolom genre, type , dan rating. Kolom null tersebut tidak akan dihapus melainkan diisi dengan value whitespace karena kita membutuhkan kolom *name* dan *genre*.

Setelah itu kolom genre akan kita ekstrak datanya menjadi matriks 2 dimensi untuk setiap anime dikarenakan kolom genre berisi beberapa genre dari anime tersebut. Hal ini dilakukan untuk memudahkan pemetaan genre dan anime kedalam *Term Frequency-Inverse Document Frequency (TF-IDF) Vectorizer*


### *Collaborative Filtering*

Pada *Collaborative Filtering* digunakan 2 dataset tersebut. Dataset akan digabungkan dengan mengacu pada kolom anime_id sehingga dikarenakan pada kedua dataset memiliki kolom anime_id tersebut. Dataset gabungan ini memiliki 7813727 baris dan 9 kolom.

Setelah digabungkan kemudian akan dicek apakah terdapat kolom yang bernilai null maupun baris yang duplikat. Ternyata terdapat kolom yang bernilai null yang kemudian akan diisi dengan whitespace agar tidak terhapus saat menghapus kolom null. Kemudian kolom rating_user yang bernilai -1 akan diubah menjadi null dan akan dihapus karena, baris kolom tersebut tidak memiliki informasi yang mewakili preferensi dari pengguna.

Ketika data sudah bersih dari Null value dan data duplikat kemudian akan diambil baris data yang miliki *members* pada kolom members dengan nilai lebih dari 250000. Hal ini dilakukan agar pengguna menerima rekomendasi anime yang berkualitas dan juga ditonton banyak orang. Jumlah dataset ini setelah difilter menjadi 1615445 baris dan 9 kolom. Kemudian data akan dibagi menjadi data latih dan data tes dengan rasio 90% : 10%. Data tes memiliki rasio yang lebih sedikit dikarenakan data yang tersedia cukup banyak jadi 10% data tes sudah termasuk banyak.

## Modeling
Proyek ini menyajikan dua solusi pendekatan untuk pembuatan sistem rekomendasi yaitu *content-based filtering* dan *collaboratibe filtering*
Untuk sistem rekomendasi *content-based filtering* akan digunakan matriks TF-IDF dan cosine similarity untuk menentukan kemiripan antar item yang nantinya bisa direkomendasikan kepada user yang juga melihat item tersebut.

Untuk sistem rekomendasi *collaborative filtering* akan digunakan model machine learning dengan menggunakan dot product dari data rating yang akan menampilkan rekomendasi untuk satu user sesuai dengan preferesi mereka sebelumnya.

### *Content-based Filtering*
Pada pendekatan ini, digunakan *TF-IDF vectorizer* dan *Cosine similarity* untuk memberikan rekomendasi berdasarkan kemiripan antar item. dalam konteks proyek ini adalah kemiripan anime dengan melihat kemiripan genrenya.

Pertama, akan dilakukan pemetaan genre kedalam matriks TF-IDF. Matriks ini merepresentasikan setiap genre anime sebagai vektor berdasarkan frekuensi kata dalam genre anime tersebut. TF-IDF mengukur seberapa penting kata dalam sebuah dokumen. Semakin tinggi nilai TF-IDF maka menunjukan bahwa kata tersebut relevan dalam sebuah dokumen. 

Langkah pertama dalam pembangunan matriks TF-IDF adalah menghitung Term Frequency (TF), yaitu frekuensi kata dalam genre anime. Selanjutnya, perlu dihitung Inverse Document Frequency (IDF), yaitu kebalikan dari frekuensi kata di seluruh dokumen. Ini mengurangi bobot kata-kata yang umum dan meningkatkan bobot kata-kata yang jarang muncul. Akhirnya, matriks TF-IDF terbentuk dengan mengalikan nilai TF dengan nilai IDF. Matriks ini akan menjadi representasi numerik dari genre anime dalam dataset.

Selanjutnya adalah menghitung *cosine similarity* antara setiap judul anime. *Cosine similarity* mengukur kesamaan antara dua vektor dengan mengukur jarak sudut antara vektor tersebut. Jika nilai *cosine similarity* semakin kecil maka semakin mirip anime-anime tersebut. Untuk menghitung *cosine similarity* , dapat digunakan formula berikut :

$$ cosine similarity = {{A . B} \over {||A|| ||B||}} $$

Dengan menghitung *cosine similarity* antar judul anime dengan menggunakan genre, kita akan memperoleh matriks similiarity antar anime dalam dataset. Nilai ini dapat digunakan untuk menemukan anime-anime dengan genre yang mirip. Semakin tinggi nilai *cosine similarity*-nya maka semakin mirip anime tersebut dengan anime yang lainnya.

Dengan menggunakan matriks TF-IDF dan *cosine similarity* pada pendekatan *content-based filtering* , dapat diperoleh rekomendasi anime yang memiliki genre yang mirip dengan anime yang sedang dicari oleh pengguna. Hal ini memungkinkan sistem rekomendasi untuk mengidentifikasi dan merekomendasikan anime yang memiliki kesamaan genre dengan anime yang sedang dicari oleh pengguna.

Kelebihan pendekatan *content-based filtering* ini adalah dapat memberikan rekomendasi yang relevan berdasarkan genre anime dan bisa memberikan rekomendasi anime baru yang sedang tayang. Namun kekurangan dari pendekatan ini adalah tidak bisa memberikan rekomendasi berdasarkan preferensi pengguna dan pengguna lainnya, dan hanya bisa memberikan rekomendasi pada saat dilakukan pencarian menggunakan *keyword* tertentu.

Output dari pendekatan ini adalah Top-N anime dengan genre yang sama (beberapa judul anime memiliki nilai *cosine similarity* tertinggi)

Tabel 2. Top 10 Anime yang direkomendasikan dari *keyword* "The Last: Naruto the Movie"

| name                            | genre                                          | type    | rating |
|---------------------------------|------------------------------------------------|---------|--------|
| Ranma ½ OVA                     | Comedy, Martial Arts, Romance, Shounen         | OVA     | 7.87   |
| Ogami Matsugorou                | Action, Martial Arts, Romance, School, Shounen | OVA     | 6.08   |
| Kyutai Panic Adventure!         | Action, Martial Arts, Shounen, Super Power     | Special | 5.21   |
| Princess Army: Wedding★Combat   | Action, Martial Arts, Romance, Shoujo          | OVA     | 5.48   |
| Shin Karate Jigoku-hen          | Action, Martial Arts                           | OVA     | 5.87   |
| Shinken Densetsu: Tight Road    | Action, Martial Arts                           | TV      | 5.67   |
| Hokuto no Ken: Legend of Heroes | Action, Martial Arts                           | Special | 6.18   |
| Toushinden                      | Action, Martial Arts                           | OVA     | 5.43   |
| Geori-eui Mubeopja              | Action, Martial Arts                           | Movie   | 5.53   |
| Feng Ji Yun Nu                  | Action, Martial Arts                           | OVA     | 4.76   |


### *Collaborative Filtering*

Untuk pendekatan *Collaborative Filtering* digunakan model *RecommenderNet* dengan menggunakan library TensorFlow untuk mempelajari pola preferensi pengguna terhadap anime yang ditonton. Model ini menggunakan *layer embedding* untuk merepresentasikan pengguna, anime, dan rating yang diberikan. Dengan melakukan operasi *dot product* pada *layer embedding* maka memungkinkan untuk menghasilkan rekomendasi berdasarkan preferensi dari pengguna.

Perbedaan pendekatan ini dengan *content-based filtering* adalah adanya pola preferensi pengguna yang dapat diprediksi, sehingga pengguna akan mendapatkan rekomendasi berdasarkan anime yang disukainya.

Kelebihan dari *Collaborative Filtering* ini adalah kemampuannya dalam menemukan pola preferensi pelanggan yang kompleks dan merekomendasikan produk berdasarkan preferensi serupa dari pelanggan lain. Namun kekurangan dari pendekatan ini adalah adanya masalah jika pelanggan baru atau produk baru tidak memiliki cukup interaksi untuk memberikan rekomendasi yang akurat.

Output dari pendekatan ini adalah Top-N anime yang mirip dengan anime yang sebelumnya sudah pengguna tonton dan diberi rating.

Tabel 3. Top 10 Anime yang direkomendasikan untuk user 36391
| name                          | genre                                                             |
|-------------------------------|-------------------------------------------------------------------|
| Steins;Gate                   | Sci-Fi, Thriller                                                  |
| Gintama                       | Action, Comedy, Historical, Parody, Samurai, Sci-Fi, Shounen      |
| Sen to Chihiro no Kamikakushi | Adventure, Drama, Supernatural                                    |
| Cowboy Bebop                  | Action, Adventure, Comedy, Drama, Sci-Fi, Space                   |
| One Punch Man                 | Action, Comedy, Parody, Sci-Fi, Seinen, Super Power, Supernatural |
| Mononoke Hime                 | Action, Adventure, Fantasy                                        |
| Great Teacher Onizuka         | Comedy, Drama, School, Shounen, Slice of Life                     |
| Boku dake ga Inai Machi       | Mystery, Psychological, Seinen, Supernatural                      |
| Hellsing Ultimate             | Action, Horror, Military, Seinen, Supernatural, Vampire           |
| Baccano!                      | Action, Comedy, Historical, Mystery, Seinen, Supernatural         |

## Evaluation
Pada tahap evaluasi, Digunakan Precision pada pendekatan *content-based filtering* dan Root Mean Squared Error (RMSE) pada pendekatan *collaborative filtering* sebagai *metrics evaluation*. Berikut penjelasannya :

`Presisi` Merupakan rasio prediksi benar positif dibandingkan dengan keseluruhan hasil yang diprediksi positif. Dalam konteks proyek ini, presisi mengukur seberapa baik model dalam memberikan rekomendasi anime dengan genre yang sama.

$$Presisi = {TP \\over TP + FP}.$$

dimana:
* TP (*True Positive*) : jumlah item rekomendasi yang relevan yang benar-benar dipilih oleh pengguna.
* FP (*False Positive*) : jumlah item rekomendasi yang tidak relevan yang dipilih oleh pengguna.

Dilakuan pencarian anime yang sama dengan keyword anime "The Last: Naruto the Movie" yang memiliki genre Action, Comedy, Martial Arts, Shounen, Super Power.

Dari 10 rekomendasi anime, 10 anime memiliki genre yang sama dengan "The Last: Naruto the Movie" yang berarti model memiliki tingkat presisi 100%


`root-mean-square error` adalah ukuran yang sering digunakan untuk mengukur perbedaan antara nilai yang diprediksi oleh model atau estimator dan nilai yang diamati. Semakin rendah nilainya makan semakin baik performa model tersebut dalam melakukan prediksi. Formula dari RMSE adalah sebagai berikut :

$$RMSE = \sqrt {\frac{1}{N} \sum_{i=1}^{N} (\hat{y_{i}} - y_{i})^2}$$

dimana:

- $$\hat{y}$$ = rating prediksi
- y = rating sebenarnya
- n = jumlah data

![RMSE](https://github.com/IbraHarsye/anime-recommendation-system/assets/56579824/a56ee835-75e1-4432-a45b-52ca8e10e403)
Gambar 3. Plot Metrik Evaluasi RMSE pada Model RecommenderNet

Training dilakukan sebanyak 6 epoch dikarenakan pada epoch ke 6 RMSE model meningkat dan model akan berhenti melakukan training. Hasil akhir training menunjukan RMSE sebesar 0.1279 dan RMSE pada validation sebesar 0.1314. RMSE training dan validasi memiliki selisih yang tidak terlalu jauh yang menandakan model *good fit* dan tidak mengalami *overfitting*

## Conclusion
Sistem rekomendasi memberikan performa yang baik dengan menggunakan pendekatan *content-based filtering* maupun *collaborative filtering*

Pada pendekatan *content-based filtering*, sistem berhasil memberikan nilai evaluasi presisi hingga 100% pada contoh pencarian keyword anime.

dan pada pendekatan *collaborative filtering*, diperoleh RMSE sebesar 0.1279 dan RMSE pada data validasi sebesar 0.1314 dan ketika diuji kepada pengguna, anime yang direkomendasikan rata-rata memiliki beberapa genre yang sama.


## Daftar Pustaka
[1] [Girsang, A.S., Al Faruq, B., Herlianto, H.R. and Simbolon, S., 2020, June. Collaborative recommendation system in users of anime films. In Journal of Physics: Conference Series (Vol. 1566, No. 1, p. 012057). IOP Publishing.](https://iopscience.iop.org/article/10.1088/1742-6596/1566/1/012057/meta)

[2] [Vie, J.J., Yger, F., Lahfa, R., Clement, B., Cocchi, K., Chalumeau, T. and Kashima, H., 2017, November. Using posters to recommend anime and mangas in a cold-start scenario. In 2017 14th IAPR International Conference on Document Analysis and Recognition (ICDAR) (Vol. 3, pp. 21-26). IEEE.](https://ieeexplore.ieee.org/abstract/document/8270232/)


**---Ini adalah bagian akhir laporan---**
