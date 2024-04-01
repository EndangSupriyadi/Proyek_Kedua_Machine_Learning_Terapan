# Laporan Proyek Rekomendasi Machine Learning - Endang Supriyadi

## Project Overview
Membaca adalah suatu proses yang dilakukan oleh pembaca untuk menerima pesan atau informasi yang  disampaikan  oleh  penulis  melalui  media  kata-kata  atau  bahasa  tulis. Dengan membaca dapat memahami dengan luas topik yang  dibaca. Seiring berjalannya waktu banyak orang yang  melupakan  betapa  pentignya  membaca buku   membaca   buku merupakan   sumber informasi, buku merupakan jendela dunia, Dengan makin sering seseorang membaca akan memengaruhi fungsi otak dan mengingkatkan memori sesorang [1].Untuk meningkatkan minat baca seseorang pasti orang punya kesukaan buku atau jenis buku yang dibaca disini perlu adanya rekomendasi baik dari segi penulis, jenis buka dll. agar seseorang akan lebih tertarik dan hobi lagi membaca.


## _Business Understanding_
1. Problem Statements
- Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan teknik content-based filtering?
- Dengan data rating yang Anda miliki, bagaimana perusahaan dapat merekomendasikan buku lain yang mungkin disukai dan belum pernah dibaca oleh pengguna? 
2. Goals
- Menghasilkan sejumlah rekomendasi buku yang dipersonalisasi untuk pengguna dengan teknik content-based filtering.
- Menghasilkan sejumlah rekomendasi buku yang sesuai dengan preferensi pengguna dan belum pernah dikunjungi sebelumnya dengan teknik collaborative filtering.


## _Data Understanding_

Dataset _Book Recommendation Dataset_ ini memiliki 3 file
1. Users
Berisi para pengguna. Perhatikan bahwa ID pengguna (User-ID) telah dianonimkan dan dipetakan ke bilangan integer. Data demografis disediakan (_Location, Age_) jika tersedia. Jika tidak, bidang-bidang ini berisi nilai NULL.
2. Books
Buku diidentifikasi dengan ISBN masing-masing. ISBN yang tidak valid telah dihapus dari kumpulan data. Selain itu, beberapa informasi berbasis konten diberikan (_Book-Title, Book-Author, Year-Of-Publication, Publisher_), yang diperoleh dari Amazon Web Services. Perhatikan bahwa jika ada beberapa pengarang, hanya pengarang pertama yang disediakan. URL yang menautkan ke gambar sampul juga diberikan, muncul dalam tiga rasa yang berbeda (_Image-URL-S, Image-URL-M, Image-URL-L_), yaitu kecil, sedang, besar. URL ini mengarah ke situs web Amazon.

3. Ratings
Berisi informasi peringkat buku. _Ratings_ (_Book-Rating_) dapat berupa nilai eksplisit, yang dinyatakan dalam skala 1-10 (nilai yang lebih tinggi menunjukkan apresiasi yang lebih tinggi), atau implisit, yang dinyatakan dengan angka 0.

dataset buku ada 271360 rows dan 8 columns
dataset rating ada 1149780 rows dan 3 columns
dataset user ada 278858 rows dan 3 columns




### _Eksploratory Data_
membaca dataset
pertama arahkan alamat path penyimpanan dataset , setalah itu membaca dan menampilkan data dengan "read_csv" pastikan dataset berformat csv. untuk melihat jumlah data dalam dataset gunakan "dataset.shape". 

1. Membaca Dataset rating
   membaca dataset rating yang terdiri dari User-Id, ISBN, dan Book-Rating. seperti pada Tabel 1.

   Tabel 1 <br>

   | | **User-Id** | **ISBN** | **Book-Rating** |
   |-|-------------|----------|-----------------|
   |0|276725       |034545104X|0                |
   |1|276726       |0155061224|5                |
   |2|276727       |0446520802|0                |
   |3|276729       |052165615X|3                |
   |4|276729       |0521795028|6                |
  
2. Melihat Dataset book
   membaca dataset book yang terdiri dari ISBN, Book-Title, Book-Author, Year-Of-Publication dan Publisher seperti di Tabel 2. <br>
   Tabel 2 <br>

   
   | | **ISBN** | **Book-Title**              | **Book-Author**     | **Year-Of-Publication** | **Publisher**            |	
   |-|----------|-----------------------------|---------------------|-------------------------|--------------------------|
   |0|0195153448|Classical Mythology          |Mark P. O. Morford   |2002                     |Oxford University Press   |
   |1|0002005018|Clara Callan                 |Richard Bruce Wright |2001                     |HarperFlamingo Canada     |
   |2|0060973129|Decision in Normandy         |Carlo D'Este         |1991                     |HarperPerennial           |
   |3|0374157065|Flu: The Story of the Great..|Gina Bari Kolata     |1999                     |Farrar Straus Giroux	    |
   |4|0393045218|The Mummies of Urumchi       |E. J. W. Barber	     |1999                     |W. W. Norton &amp; Company|
<br>



#### _Univariate Analysis_

Menvisualisasikan dan meneliti distribusi rating dataframe, pada gambar 1 rating terbanyak adalah 0 karena 0 bukanlah nilai NaN jadi tetap dimasukan dalam proyek ini<br>
gambar 1 <br>

<a href="https://imgbb.com/"><img src="https://i.ibb.co/6mLd8JG/download-16.png" alt="download-16" border="0"></a>

<br>

Menvisualisasikan dan meneliti distribusi tahun terbitnya buku dari book dataframe disini data tahun terbit cenderung meningkat setiap tahunnya terbanyak tahun 2002 lihat data di gambar 2<br>
gambar 2 <br>

<a href="https://imgbb.com/"><img src="https://i.ibb.co/0Fwj476/download-17.png" alt="download-17" border="0"></a>

<br>



## _Content Filtered Recommendation System_
### _Data Preparation_
<br>
melakukan transformasi pada data sehingga menjadi bentuk yang cocok untuk proses pemodelan

##### Cek Nilai _Missing Value_
dengan" .sum() akan menampilkan data _missing value_. _missing value_ merupakan nilai yang tidak ada atau NaNN yang ada di dataset. _missing value_ bisa mempengaruhi kualiatas prediksi model sehingga harus dihapus atau ganti dengan nilai mean, count, dll. lalu mencek nilai _missing value_ dari kolom open, high dan low.
##### Membuang Data _Missing Value_ dan Duplikasi
dengan menggunakan perintah "dataset.dropna()" maka data NaaN akan terhapus. lalu untuk Menghapus Duplikasi yaitu dengan menggunakan perintah "dataset.drop_duplicates()" yang nantinya jika ada row yang duplikat akan terhapus.

mengubah dataframe dari buku menjadi list dengan "dataset[].tolist()"

### _Modeling_
TF-IDF yang merupakan kepanjangan dari Term Frequency-Inverse Document Frequency memiliki fungsi untuk mengukur seberapa pentingnya suatu kata terhadap kata - kata lain dalam dokumen. Kita umumnya menghitung skor untuk setiap kata untuk menandakan pentingnya dalam dokumen dan corpus. Metode sering digunakan dalam Information Retrieval dan Text Mining.
dengan menggunakan "from sklearn.feature_extraction.text import TfidfVectorizer" yang nantinya akan memunculkan nama nama penulisnya.
lalu setelah itu akan melakukan fit dan transformasi ke dalam matriks, matriks tersebut adalah tfidf_matrix
<br>
Gambar 3 <br>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/PWsz70x/Screenshot-2024-04-01-173228.png" alt="Screenshot-2024-04-01-173228" border="0"></a> <br>
Pada tfidf_matrix terdapat 10000 ukuran data dan 5575 nama penulis buku

melihat data frame dari matriks dari judul buku dengan penulis - penulis buku seperti pada tabel 3
Tabel 3 <br>

|book_title	|masters|paolo|lagasse|fargues|jewell|
|-----------|-------|-----|-------|-------|------|
|Unforgettable (Harlequin Intrigue, No 348)|0.0|0.0|0.0|0.0|0.0|
|All Our Yesterdays|0.0|0.0|0.0|0.0|0.0|
|Anna and the King of Siam|0.0|0.0|0.0|0.0|0.0|
|Now You See It . . .|0.0|0.0|0.0|0.0|0.0|

Dalam sistem rekomendasi, kita perlu mencari cara supaya item yang kita rekomendasikan tidak terlalu jauh dari data pusat, oleh karena itu kita butuh derajat kesamaan pada item, dalam proyek ini, buku dengan derajat kesamaan antar buku dengan cosine similarity hasilnya seperti ini <br>
array([[1., 0., 0., ..., 0., 0., 0.], <br>
       [0., 1., 0., ..., 0., 0., 0.],<br>
       [0., 0., 1., ..., 0., 0., 0.],<br>
       ...,<br>
       [0., 0., 0., ..., 1., 0., 0.],<br>
       [0., 0., 0., ..., 0., 1., 0.],<br>
       [0., 0., 0., ..., 0., 0., 1.]])<br>

Nilai K pada fungsi menandakan jumlah rekomendasi yang akan ditampilkan
Atribut argpartition berguna untuk mengambil sejumlah nilai k, dalam fungsi ini 5 tertinggi dari tingkat kesamaan yang berasal dari dataframe cosine_sim_df

##### Mencari Rekomendasi dari buku yang sudah dibaca 
Buku yang dibaca yaitu The Diaries of Adam and Eve berikut detailnya sesuai dengan tabel 4

Tabel 4 <br>

|        |**book_ISBN**|**book_title**                |**book_author**|**book_year_of_publication**|
|--------|-------------|------------------------------|---------------|------------|
|**4700**|0965881199   |The Diaries of Adam and Eve	|Mark Twain	    |1998        |

<br>
selanjutnya yaitu menampilkan buku rekomendasi. mungkin beberapa kasus ada yang menampilkan rekomendasi yang sama jadi perlu dihapus jika ada rekomendasi yang sama dengan perintah "rekomendasi.drop_duplicates()". berikut ini 5 rekomendasi buku yang ada di tabel 5
<br>
Tabel 5<br>

|   |**book_title**|**book_author**|
|---|--------------|---------------|
|0 | ADVENTURES OF HUCKLEBERRY FINN (ENRICHED CLASS...	|Mark Twain|
|1|Adventures of Huckleberry Finn	|Mark Twain|
|2|The Complete Short Stories of Mark Twain (Bant...	|Mark Twain|
|3|Treasury of Illustrated Classics: Adventures o..| Mark Twain|
|4|A Connecticut Yankee in King Arthur's Court (D...	|Mark Twain|


### _Evaluation_
memakai metrik evaluasi akurasi di mana akurasi adalah:
Jumlah buku yang direkomendasikan sesuai penulis / Jumlah buku yang ditulis oleh penulis yang sama
Variabel books_that_have_been_read_row di bawah ini akan mengambil satu row dari buku yang pernah dibaca sebelumnya, dan variabel books_that_have_been_read_author adalah penulis buku dari buku yang pernah dibaca sebelumnya
Variabel books_with_the_same_author menunjukkan jumlah buku yang sudah ditulis oleh penulis buku yang berasal dari buku yang pernah dibaca sebelumnya
Ternyata buku yang telah ditulis oleh Mark Twain berjumlah 16 buku
dengan nilai akurasi 31.25%

## _Collaborative Filtered Recommendation System_
Collaborative Based Filtering adalah sistem rekomendasi berdasarkan pendapat suatu komunitas.
Kelebihan pada Collaborative Based Filtering bila dibandingkan dengan Content Based Filtering adalah pengguna dapat mengeksplorasi item atau konten di luar preferensi pengguna. Pengguna pun juga dapat mendapat rekomendasi sesuai dengan kecenderungan publik yang dianalisa lewat penilaian pengguna - pengguna lainnya.
Kekurangan pada Collaborative Based Filtering adalah pengguna kurang mendapatkan rekomendasi sesuai preferensi pribadi. Konten - konten yang diberikan oleh sistem rekomendasi lebih banyak berasal dari preferensi publik dan bukan preferensi pribadi.

Pada Collaborative Based Filtering, menggunakan penilaian dari pengguna - pengguna untuk mendapatkan rekomendasi buku - buku.
### _Data Preparation_
mengubah user_id, book_id ke type integer dan rating menjadi float

#### cek jumlah pengguna dan buku
Gambar 4<br>
<a href="https://ibb.co/1dkKHj1"><img src="https://i.ibb.co/F0FJGC2/Screenshot-2024-04-01-181310.png" alt="Screenshot-2024-04-01-181310" border="0"></a>
<br>
Pada Gambar 4 terdapat jumlah pengguna: 679, jumlah buku :4688 , Min Rating : 0.0 dan Max Rating : 10.0

#### Train Test Split

yaitu membagi data latih dan data uji sebesar 80:20 karena perbandingan itu sangat sering digunakan dan cenderung efesien.
yang ouputnya seperti gambar 5 ini
Gambar 5 <br>
<a href="https://imgbb.com/"><img src="https://i.ibb.co/Nj39pdN/Screenshot-2024-04-01-182102.png" alt="Screenshot-2024-04-01-182102" border="0"></a>
<br>

### _Model Depeloment_

Pada tahap ini, model menghitung skor kecocokan antara pengguna dan buku dengan teknik embedding. Pertama, kita melakukan proses embedding terhadap data user dan buku. Selanjutnya, lakukan operasi perkalian dot product antara embedding user dan buku. Selain itu, kita juga dapat menambahkan bias untuk setiap user dan buku. Skor kecocokan ditetapkan dalam skala [0,1] dengan fungsi aktivasi sigmoid.

Model ini menggunakan Binary Crossentropy untuk menghitung loss function, Adam (Adaptive Moment Estimation) sebagai optimizer, dan root mean squared error (RMSE) sebagai metrics evaluation. 

Gambar 6 <br>

<a href="https://imgbb.com/"><img src="https://i.ibb.co/Qp5xg46/download-19.png" alt="download-19" border="0"></a>
<br>
Perhatikanlah pada gambar 6, proses training model cukup smooth dan model konvergen pada epochs sekitar 30. Dari proses ini, kita memperoleh nilai error akhir sebesar sekitar 0.1983 dan error pada data validasi sebesar 0.3443. 



### Mendapatkan Rekomendasi
mendefinisikan ulang book_datase dan rating_dataset
akan mengambil user_id secara acak dari rating_dataset. Dari user_id ini kita perlu mengetahui buku - buku apa saja yang pernah dibaca dan yang belum pernah dibaca, sehingga kita hanya dapat merekomendasikan buku - buku yang belum dibaca.
hasilnya seperti gambar  7 ini <br>

Gambar 7 <br>

<a href="https://ibb.co/cvZdt5g"><img src="https://i.ibb.co/SxMp0Ts/Screenshot-2024-04-01-185641.png" alt="Screenshot-2024-04-01-185641" border="0">

<br>
Gambar 7 menampilkan rekomendasi buku buku dengan _user_ : 278418
diantaranya menampilkan buku : <br>
Prince Caspian : C. S. Lewis <br>
The House With a Clock in Its Walls : John Bellairs <br>
Songs in Ordinary Time (Oprah's Book Club (Paperback)) : Mary McGarry Morris <br>
At Home in Mitford (The Mitford Years) : Jan Karon <br>
Young Wives : Olivia Goldsmith <br>
Julie of the Wolves (Julie of the Wolves) : Jean Craighead George <br>
More Scary Stories To Tell In The Dark : Alvin Schwartz <br>
Where the Lilies Bloom : Bill Cleaver <br>
CADDIE WOODLAWN : Carol Ryrie Brink <br>
The Thief of Always : Clive Barker <br>



Referensi Jurnal : <br>
[1]	A. Suryana, I. B. Zaki, J. Sua, G. Phua, J. Jekson, and C. Celvin, “Pentingnya Membaca Buku bagi Generasi Baru di Era Teknologi Bersama Komunitas Ayobacabatam,” Natl. Conf. Community Serv. Proj., vol. 3, pp. 715–720, 2021, [Online]. Available: https://journal.uib.ac.id/index.php/nacospro/article/view/6010

sumber dataset https://www.kaggle.com/datasets/arashnic/book-recommendation-dataset


