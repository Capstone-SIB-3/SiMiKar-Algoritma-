# Laporan Proyek Captone  C22- 188
****
## Project Overview 
*****
## __SIMIKAR__
Lestari & Rahardjo (2013) menemukan fenomena para sarjana yang baru lulus belum sepenuhnya mempertimbangkan kemampuan, minat, dan kepribadiannya dalam memilih suatu pekerjaan. Kecenderungan mereka dalam menentukan pekerjaan yang dipilihnya berdasarkan rasa khawatir dan cemas bila terlalu lama menganggur, rasa malu pada lingkungan disekitar terutama jika belum memperoleh pekerjaan, dan adanya tuntutan moral dari orangtua. Oleh karena hal tersebut dapat diangkat sebuah permasalahan yaitu bagaimana para lulusan fresh graduate dapat mengetahui minat karir mereka berdasarkan keahlian yang mereka miliki ? 

Dari permasalahan tersebut, __SIMIKAR__ hadir untuk membantu  fresh graduate dalam merencanakan minat dan karir, __Simikar__ adalah sebuah website yang dibuat menggunakan _machine learning_  dengan tipe sistem rekomendasi untuk membantu para user yang menggunakan website __Simikar__  untuk mengetahui profesi di bidang IT ataupun profesi lainnya  yang akan cocok dengan dengan user berdasarkan kemampuan / skill yang mereka miliki 

# Business Understanding
***
### Problem Statement 
Berdasarkan penjelasan pada project overview, berikut ini merupakan batasan masalah yang perlu diselesaikan di proyek ini:
* Berdasarkan data mengenai pengguna, bagaimana membuat sistem rekomendasi yang dipersonalisasi dengan _teknik content-based filtering_ untuk merekomendasikan  berdasarkan kemampuan  ?

### Goals 
***
Tujuan dibuatnya proyek ini adalah sebagai berikut :
* Menghasilkan sejumlah rekomendasi movie yang dipersonalisasi untuk pengguna dengan teknik content-based filtering yang dapat merekomendasikan film berdasarkan kemampuan 
### Solution Approach
***
Solusi yang dapat dilakukan untuk memenuhi tujuan dari proyek ini diantaranya :

 __Content Based filtering__
Adalah rekomendasi berbasis konten yang merekomendasikan item yang memiliki kemiripan dengan item yang disukai/diinput pengguna sebelumnya.
 
Konsep Content-based filtering mempelajari profil minat pengguna baru berdasarkan data dari objek yang telah dinilai pengguna. Algoritma ini bekerja dengan menyarankan item serupa yang pernah disukai di masa lalu atau sedang dilihat di masa kini kepada pengguna. Semakin banyak informasi yang diberikan pengguna, semakin baik akurasi sistem rekomendasi.

Untuk membuat profil pengguna, dua informasi ini penting bagi sistem dengan pendekatan content-based filtering: 
* Model preferensi pengguna.
* Riwayat interaksi pengguna dengan sistem rekomendasi. 

Kelebihan dan Kekurangan dari Content-based Filtering
* Kelebihan 
** __User Independence__, tidak bergantung kepada user lain dalam memberikan rekomendasi yang ada.
** __New Item__, mampu merekomendasikan item yang belum dinilai oleh setiap pengguna.
* Kekurangan 
__New User__ , Sistem tidak dapat memberikan rekomendasi yang dapat diandalkan pada pengguna baru, karena membutuhkan penelusuran terlebih dahulu pada preferensi pengguna.

### Data Understanding
***
Dataset ini dapat diakses menggunakan [Github Repository](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/main/techloker.csv)
informasi dari dataset dapat dirangkum sebagai berikut :

tabel 1 : Ringkasan informasi dataset 
| Jenis           | Keterangan                                                                                                  |
|-----------------|-------------------------------------------------------------------------------------------------------------|
| Sumber          | [Simikar Recommendation Dataset](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/main/techloker.csv) |
| Lisensi         | C22-188                                                                                                     |
| Kategori        | Jobs                                                    |
| Jenis & Ukuran  | CSV                                                                                               |

Pada berkas yang diunduh  terdapat  berkas csv yakni Techloker.csv, untuk penjelasannya dapat dilihat di keterangan dibawah ini : 

__Techloker.csv__
Pada berkas Techloker.csv terdapat 500 baris dengan 5 kolom, kolom-kolom tersebut terdiri 5 kolom bertipe objek, dataset ini dihasilkan dari scrapping web _www.techinasia.com_ dengan menggunakan extensions chromme _web scrapper_  , untuk  penjelasan mengenai variabel-variable pada dataset insurance ini dapat dilihat sebagai berikut:
* link : Website acuan yang kami gunakaan untuk web scrapping
* jenis_pekerjaan : jenis jenis profesi yang didapatkan dari scrapping 
* nama_perusahaan : jenis jenis perusahaan yang didapatkan dari scrapping 
* lokasi_perusahaan:  jenis jenis lokasi_perusahaan yang didapatkan dari scrapping 
* kemampuan :  jenis jenis kemampuan yang didapatkan dari scrapping 

***
## __Pra-pemrosesan Data__. 
Pada pra-pemrosesan data dapat dilakukan beberapa tahapan, antara lain :
* Memasukkan dataset kedalam ke dalam dataframe menggunakan pandas
* Menghapus kolom "link", "nama_perusahaan", "lokasi_perusahan" pada Techloker.csv
* Menampilkan jumlah entry unik berdasarkan kemampuan dan jenis_pekerjaan
* Menemukan dan menangani missing values di dataset
* menampilkan informasi dataset 

__Memasukkan dataset ke dalam ke dalam dataframe menggunakan pandas__
Pada proyek digunakan fungsi read di pandas untuk memasukkan dataset Techloker.csv ke dalam bentuk dataframe menggunakan pandas dan dataframe itu akan tersimpan dalam variabel jobs lalu tampilan dataset  akan seperti pada tabel 2

tabel 2 : Tampilan 10 dataset Awal Techloker dalam bentuk dataframe dengan pandas
| link                                                                 | jenis_pekerjaan                      | nama_perusahaan                   | lokasi_perusahaan           | kemampuan                                                                                                           |
|----------------------------------------------------------------------|--------------------------------------|-----------------------------------|-----------------------------|---------------------------------------------------------------------------------------------------------------------|
| https://www.techinasia.com/jobs/838dfc41-32e7-40b2-8b51-fe19e0d5ab59 | UI/UX Designer                       | Humata Indonesia                  | Jakarta, Indonesia (Remote) | Adobe Illustrator, Adobe Photoshop, Adobe Indesign                                                                  |
| https://www.techinasia.com/jobs/b24c85fc-f19d-4d4f-af59-4f99ba940d7a | Golang Developer - Finance Company   | SIGMATECH                         | Jakarta, Indonesia          | Golang, Backend Development, SQL Server, MySQL                                                                      |
| https://www.techinasia.com/jobs/7ea36153-a086-47fc-99de-b8bdc6a3a351 | IT QA Automation - Finance Company   | SIGMATECH                         | Jakarta, Indonesia          | Node.js, Test Automation, Selenium, Javascript, Linux                                                               |
| https://www.techinasia.com/jobs/c8f8b8d0-5c4b-467d-b1a8-8600420fbab2 | Java Developer - Insurance Company   | SIGMATECH                         | Jakarta, Indonesia          | Java Apache, Tomcat, Java J2EE, Angular, Javascript                                                                 |
| https://www.techinasia.com/jobs/94a20b93-f95c-463f-b431-49215b6e9920 | Node.JS Developer - Logistic Company | SIGMATECH                         | Jakarta, Indonesia          | Node.js, REST APIs, AJAX, CodeIgniter                                                                               |
| https://www.techinasia.com/jobs/d16ae293-3e76-4a90-b2de-3eef0e1e3469 | Legal Assistant Manager              | Halodoc ID                        | Jakarta, Indonesia          | Legal Management, Leadership                                                                                        |
| https://www.techinasia.com/jobs/dcf19c9d-be07-47bc-93f5-3e8b308b4be8 | Corporate Analyst                    | Finku                             | Jakarta, Indonesia          | Product Management, Product Marketing, Business Analysis, Business   Intelligence                                   |
| https://www.techinasia.com/jobs/3ff61e60-8844-40c7-999d-daaed40c72c3 | Business Manager                     | Influence ID                      | Jakarta, Indonesia          | Sales & Marketing, Digital Marketing, Business Development, Marketing   Communications, Sales Strategy & Management |
| https://www.techinasia.com/jobs/9efae31d-735f-4977-8f95-4121d558a59e | Team Lead .Net Developer             | PT. Informasi Teknologi Indonesia | Jakarta, Indonesia          | .NET, Java, Team, Leadership                                                                                        |
| https://www.techinasia.com/jobs/b6cdb4f0-17e2-4f6d-87f4-3c0d43bb79d1 | Social media Intern (Warung Pintar)  | SIRCLO                            | Tangerang, Indonesia        | Social Media, Social Media Marketing, Content Strategy                                                              |


__Menghapus kolom "link", "nama_perusahaan", "lokasi_perusahan" pada Techloker.csv__

Pada proyek ini dilakukan penghapusan kolom _link_, _nama_perusahaan_ , dan  _lokasi_perusahaan_ pada Techloker.csv dengan fungsi _drop_, penghapusan ini dilakukan karena pada proyek ini tidak menggunakan kolom-kolom tersebut  lalu tampilan dataset  akan seperti pada tabel 3.

tabel 3: Tammpilan 10 dataset awal setelah di drop kolom _link_, _nama_perusahaan_ , dan _lokasi_perusahaan_

| jenis_pekerjaan                      | kemampuan                                                                                                             |
|--------------------------------------|-----------------------------------------------------------------------------------------------------------------------|
| UI/UX Designer                       | Adobe Illustrator, Adobe Photoshop, Adobe   Indesign                                                                  |
| Golang Developer - Finance Company   | Golang, Backend Development, SQL Server,   MySQL                                                                      |
| IT QA Automation - Finance Company   | Node.js, Test Automation, Selenium,   Javascript, Linux                                                               |
| Java Developer - Insurance Company   | Java Apache, Tomcat, Java J2EE, Angular,   Javascript                                                                 |
| Node.JS Developer - Logistic Company | Node.js, REST APIs, AJAX, CodeIgniter                                                                                 |
| Legal Assistant Manager              | Legal Management, Leadership                                                                                          |
| Corporate Analyst                    | Product Management, Product Marketing,   Business Analysis, Business Intelligence                                     |
| Business Manager                     | Sales & Marketing, Digital Marketing,   Business Development, Marketing Communications, Sales Strategy &   Management |
| Team Lead .Net Developer             | .NET, Java, Team, Leadership                                                                                          |

__Menampilkan jumlah entry unik berdasarkan kemampuan dan jenis pekerjaan__

Pada proyek ini menggunakan fungsi  _unique_ untuk menghitung nilai unik yang terdapat di masing masing kolom dataset kemudian terdapat fungsi _len_ untung menghitung total dari nilai _unique_tersebut lalu setelah dihitung menggunakan fungsi _len_ akan dicetak hasilnya menggunakan fungsi _print_, hasil dari menampilkan jumlah entry, ditampilkan pada gambar 1

gambar 1 : Nilai unik pada kolom jenis pekerjaan
![This is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/1.PNG?raw=true)

gambar 2 : Nilai unik pada kolom kemampuan 

![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/2.PNG?raw=true)


__Menemukan dan menangani Missing Value__

Pada proyek ini digunakan fungsi isnull().sum() yang berfungsi untuk menemukan nilai missing value di masing masing kolom dataset. missing value sendiri dapat diartikan sebagai nilai atribut yang kosong pada objek data. kemudian hasil dari fungsi isnull().sum() diatas dapat dilihat pada gambar 4 , sedangkan gambar 3 adalah dataset sebelum dicek _missing value_  lalu setelah itu kami menggunakan fungsi _dropna_ untuk menghapus nilai _missing value_ dengan hasil tabel setelah di drop dapat dilihat di tabel 4

gambar 3:

![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/3.JPG?raw=true)

Tabel 4: 
| link                                                                 | jenis_pekerjaan                      | nama_perusahaan                   | lokasi_perusahaan           | kemampuan                                                                                                           |
|----------------------------------------------------------------------|--------------------------------------|-----------------------------------|-----------------------------|---------------------------------------------------------------------------------------------------------------------|
| https://www.techinasia.com/jobs/838dfc41-32e7-40b2-8b51-fe19e0d5ab59 | UI/UX Designer                       | Humata Indonesia                  | Jakarta, Indonesia (Remote) | Adobe Illustrator, Adobe Photoshop, Adobe Indesign                                                                  |
| https://www.techinasia.com/jobs/b24c85fc-f19d-4d4f-af59-4f99ba940d7a | Golang Developer - Finance Company   | SIGMATECH                         | Jakarta, Indonesia          | Golang, Backend Development, SQL Server, MySQL                                                                      |
| https://www.techinasia.com/jobs/7ea36153-a086-47fc-99de-b8bdc6a3a351 | IT QA Automation - Finance Company   | SIGMATECH                         | Jakarta, Indonesia          | Node.js, Test Automation, Selenium, Javascript, Linux                                                               |
| https://www.techinasia.com/jobs/c8f8b8d0-5c4b-467d-b1a8-8600420fbab2 | Java Developer - Insurance Company   | SIGMATECH                         | Jakarta, Indonesia          | Java Apache, Tomcat, Java J2EE, Angular, Javascript                                                                 |
| https://www.techinasia.com/jobs/94a20b93-f95c-463f-b431-49215b6e9920 | Node.JS Developer - Logistic Company | SIGMATECH                         | Jakarta, Indonesia          | Node.js, REST APIs, AJAX, CodeIgniter                                                                               |
| https://www.techinasia.com/jobs/d16ae293-3e76-4a90-b2de-3eef0e1e3469 | Legal Assistant Manager              | Halodoc ID                        | Jakarta, Indonesia          | Legal Management, Leadership                                                                                        |
| https://www.techinasia.com/jobs/dcf19c9d-be07-47bc-93f5-3e8b308b4be8 | Corporate Analyst                    | Finku                             | Jakarta, Indonesia          | Product Management, Product Marketing, Business Analysis, Business   Intelligence                                   |
| https://www.techinasia.com/jobs/3ff61e60-8844-40c7-999d-daaed40c72c3 | Business Manager                     | Influence ID                      | Jakarta, Indonesia          | Sales & Marketing, Digital Marketing, Business Development, Marketing   Communications, Sales Strategy & Management |
| https://www.techinasia.com/jobs/9efae31d-735f-4977-8f95-4121d558a59e | Team Lead .Net Developer             | PT. Informasi Teknologi Indonesia | Jakarta, Indonesia          | .NET, Java, Team, Leadership                                                                                        |
| https://www.techinasia.com/jobs/b6cdb4f0-17e2-4f6d-87f4-3c0d43bb79d1 | Social media Intern (Warung Pintar)  | SIRCLO                            | Tangerang, Indonesia        | Social Media, Social Media Marketing, Content Strategy                                                              |

gambar 4 : 
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/4.JPG?raw=true)

__menampilkan informasi dataset__

Pada proyek ini digunakan fungsi info() di pandas yang digunakan untuk menampilkan informasi dari dataset, informasi seperti tipe data yang terdapat di masing masing kolom dataset, hasil dari fungsi info() dapat dilihat pada tabel 5

tabel 5:
| Data |      Column     |  Non-Null Count |  Dtype |
|:----:|:---------------:|:---------------:|:------:|
|  --- |      ------     | --------------- |  ----- |
|   0  | jenis_pekerjaan |   498 non-null  | object |
|   1  |    kemampuan    |   498 non-null  | object |

***
## __Data Preparation__
 ##### __Content Based filtering__
 
Pada persiapan data  data dapat dilakukan beberapa tahapan, antara lain :
* Mengurutkan jobs berdasarkan kemampuan kemudian memasukkannya ke dalam variabel fix_jobs 
* Mengecek berapa jumlah fix_jobs
* Mengecek kategori kemampuan yang unik 
* Membuang data duplikat pada variabel preparation dengan paramater kemampuan
* Mengonversi data series jenis pekerjaan dan kemampuan menjadi dalam bentuk list
* Membuat _dictionary_ untuk data jenis pekerjaan dan kemampuan 
* Memasukkan dataframe jobs_new kedalam variable jobs_recomend dan berikan 5 sample teratas 

__Mengurutkan jobs berdasarkan kemampuan kemudian memasukkannya ke dalam variabel fix_jobs__

Pada bagian ini terdapat langkah mengurutkan variabel _movie_clean_ berdasarkan movieId dengan urutan bertipe _ascending_ setelah itu dimasukkan kedalam variabel fix_jobs, untuk hasil urutan tabel _jobs_clean_ dapat dilihat di tabel 6

tabel 6 : 
|                  jenis_pekerjaan                 |                     kemampuan                     |
|:------------------------------------------------:|:-------------------------------------------------:|
|             Team Lead .Net Developer             |            .NET, Java, Team, Leadership           |
|       Downstream Engineer - Banking Company      | .NET, SQL Server, Cloud Computing, Amazon Web ... |
| Application Support Engineer Developer - Bank... |       .NET, SQL Server, Microsoft SQL Server      |
|                  .Net Developer                  | ASP.NET, .NET, Software Development, Software ... |
|                     Net. Core                    | ASP.NET, Apache, Git, TFS, Mechanical Engineering |
|                        ...                       |                        ...                        |
|             Content Writer Internship            |  Writer, SEO, Copywriting, Copywriting & Editing  |
|                   iOS Developer                  |       iOS Development, Android, iOS, Design       |
|                   iOS Developer                  |  iOS Development, Mobile Development, Agile, So.. |
|             iOS Engineer (Yogyakarta)            |   iOS Development, Software, Software Architectu  |
|                   IOS Developer                  |    iOS Development, Swift, SDLC ,JSON, XML, SQL   |

__Mengecek berapa jumlah fix_movie__

Pada bagian adalah pengecekkan entry unik pada variabel fix_movie yang berdasarkan movieId setelah itu dilakukan perhitungan dengan fungsi _len_, untuk melihat output dapat dilihat pada gambar 5 :

gambar 5 :
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/6.JPG?raw=true)

__Mengecek kategori kemampuan yang unik__

Pada bagian ini adalah Pada bagian adalah pengecekkan entry unik pada variabel fix_jobs yang berdasarkan kemampuan setelah itu dilakukan perhitungan dengan fungsi len, untuk melihat output dapat dilihat pada gambar 6

gambar 6 : Output hasil entry unik variabel unik fix_movie berdasarkan kemampuan
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/7.JPG?raw=true)

__Membuang data duplikat pada variabel preparation dengan paramater kemampuan__

Pada bagian ini dilakukan  penghapusan  data duplikat di variabel fix_jobs berdasarkan kemampuan menggunakan fungsi drop_duplicates setelah itu dimasukkan pada variabel preparation_2, setelah dihapus maka baris yang sebelumnya di tabel 6 sebanyak 498 rows x 2columns menjadi 491 rows × 2 columns

tabel 7 : 
|                  jenis_pekerjaan                 |                     kemampuan                     |
|:------------------------------------------------:|:-------------------------------------------------:|
|             Team Lead .Net Developer             |            .NET, Java, Team, Leadership           |
|       Downstream Engineer - Banking Company      | .NET, SQL Server, Cloud Computing, Amazon Web ... |
| Application Support Engineer Developer - Bank... |       .NET, SQL Server, Microsoft SQL Server      |
|                  .Net Developer                  | ASP.NET, .NET, Software Development, Software ... |
|                     Net. Core                    | ASP.NET, Apache, Git, TFS, Mechanical Engineering |
|                        ...                       |                        ...                        |
|             Content Writer Internship            |  Writer, SEO, Copywriting, Copywriting & Editing  |
|                   iOS Developer                  |       iOS Development, Android, iOS, Design       |
|                   iOS Developer                  |  iOS Development, Mobile Development, Agile, So.. |
|             iOS Engineer (Yogyakarta)            |   iOS Development, Software, Software Architectu  |
|                   IOS Developer                  |    iOS Development, Swift, SDLC ,JSON, XML, SQL   |

491 rows × 2 columns         

__Mengonversi data series jenis pekerjaan dan kemampuan menjadi dalam bentuk list__

Pada bagian ini adalah bagian untuk mengkonversikan data series jenis_pekerjaan dan kemampuan yang ada di variabel preparation_jobs menjadi dalam bentuk list setelah itu totalkan jumlah hasil konversi bentuk list dari jenis_pekerjaan dan kemampuan menggunakan fungsi len kemudian cetak hasilnya menggunakan fungsi print dengan hasilnya dapat dilihat pada gambar 7

gambar 7 : Hasil list data series jenis_pekerjaan dan kemampuan 
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/8.JPG?raw=true)

__Membuat dictionary untuk data jenis pekerjaan dan kemampuan__

Pada bagian ini terdapat langkah membuat dictionary untuk jenis_pekerjaan dan kemampuan dalam satu data frame dan dimasukkan ke dalam variabel jobs_new setelah itu untuk hasil dapat dilihat pada tabel 8 :

tabel 8: hasil dictionary jenis_pekerjaan dan kemampuan
|                  jenis_pekerjaan                 |                     kemampuan                     |
|:------------------------------------------------:|:-------------------------------------------------:|
|             Team Lead .Net Developer             |            .NET, Java, Team, Leadership           |
|       Downstream Engineer - Banking Company      | .NET, SQL Server, Cloud Computing, Amazon Web ... |
| Application Support Engineer Developer - Bank... |       .NET, SQL Server, Microsoft SQL Server      |
|                  .Net Developer                  | ASP.NET, .NET, Software Development, Software ... |
|                     Net. Core                    | ASP.NET, Apache, Git, TFS, Mechanical Engineering |
|                        ...                       |                        ...                        |
|             Content Writer Internship            |  Writer, SEO, Copywriting, Copywriting & Editing  |
|                   iOS Developer                  |       iOS Development, Android, iOS, Design       |
|                   iOS Developer                  |  iOS Development, Mobile Development, Agile, So.. |
|             iOS Engineer (Yogyakarta)            |   iOS Development, Software, Software Architectu  |
|                   IOS Developer                  |    iOS Development, Swift, SDLC ,JSON, XML, SQL   |

__Memasukkan dataframe jobs_new kedalam variable jobs_recomend dan berikan 5 sample teratas__

Pada bagian ini terdapat langkah untuk memasykkan dataframe jobs_new kedalam variable baru yaitu jobs_recomend setelah itu akan ditampilkan sample 5 teratas dataset menggunakan fungsi _sample_, dan untuk hasilnya dapat dilihat pada tabel 9

tabel 9: 

|             jenis_pekerjaan             |                     kemampuan                    |
|:---------------------------------------:|:------------------------------------------------:|
|         Customer Success Officer        |    Business Development, Customer Relationship   |
|          Pricing Analyst Intern         |     Google Apps, Analytics, Analytical Skills    |
|             Product Designer            |  UI/UX Design, Product Design, Brand & Identity  |
| IT Business Analyst Internship (Remote) |  Product Development, Product Strategy, Product  |
|         Senior ReactJS Developer        | React.js, Web Development, Front-end Development |

***
## __Model Development__
#### __Content Based filtering__
 
Pada model development dengan content based dapat dilakukan beberapa tahapan, antara lain :
* Menggunakan teknik _TF-IDF Vectorizer_ yang digunakan untuk  untuk menemukan representasi fitur penting dari setiap kategori kemampuan
* Melakukan fit model setelah itu akan ditransformasikan kedalam matriks kemudian akan dilakuakan pengecekan ukuran matriks 
* Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense()
* Membuat dataframe untuk melihat tf-idf matrix untuk melihat korelasi antara jenis pekerjaan dan kemampuan
* Menghitung cosine similarity pada matrix tf-idf
* Membuat dataframe dari variabel cosine_sim dengan baris dan kolom berupa jenis pekerjaan dan kemampuan
* Melihat similarity matrix pada setiap kemampuan

__Menggunakan teknik TF-IDF Vectorizer yang digunakan untuk untuk menemukan representasi fitur penting dari setiap kategori kemampuan__

Pada bagian ini dilakukan tahap menemukan representasi fitur penting dari setiap kategori kemampuan dengan langkah pertama yaitu Inisialisasi TfidfVectorizer setelah itu Melakukan perhitungan idf pada data ___kemampuan___ lalu Mapping array dari fitur index integer ke fitur nama, untuk melihat output, dapat dilihat pada gambar 8

gambar 8 :
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/9.JPG?raw=true)

__Melakukan fit model setelah itu akan ditransformasikan kedalam matriks kemudian akan dilakuakan pengecekan ukuran matriks__

Pada bagian ini dilakukan tahap fit model array TF-IDF Vectorizer dengan parameter kemampuan kedalam bentuk matriks setelah itu akan dilakukan pengecekkan ukuran matriks, untuk melihat ukuran matriks dapat dilihat pada gambar 9 :

gambar 9: 
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/10.JPG?raw=true)

Perhatikanlah, matriks yang kita miliki berukuran (481, 358). Nilai 481 merupakan ukuran data dan 358 merupakan matrik kategori kemampuan. 

__Mengubah vektor tf-idf dalam bentuk matriks dengan fungsi todense()__

Pada tahap ini dilakukan tahap untuk  menghasilkan vektor tf-idf kedalam bentuk matriks, kita menggunakan fungsi todense(), dan untuk melihat hasilnya dapat dilihat pada gambar 10:

gambar 10:
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/11.JPG?raw=true)

Selanjutnya, mari kita lihat matriks tf-idf untuk beberapa jenis_pekerjaan dan kategori kemampuan

__Membuat dataframe untuk melihat tf-idf matrix untuk melihat korelasi antara jenis pekerjaan dan kemampuan__

Pada tahap ini dilakukan tahap untuk membuat dataframe untuk melihat tf-idf matrix dengan kolom di isi menggunakan jenis get_names yang berisi kemampuan dan baris diisi dengan jenis pekerjaan, untuk melihat outputnya dapat dilihat pada tabel 10:

Tabel 10 :

|                                                         banking | resources | strategic | advertising | partnerships |       ux | executive | portfolio | calling | teamwork |   |   |
|----------------------------------------------------------------:|----------:|----------:|------------:|-------------:|---------:|----------:|----------:|--------:|---------:|---|---|
|                                                 Jenis_Pekerjaan |           |           |             |              |          |           |           |         |          |   |   |
|                         Research Manager                        |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                    Digital Marketing Manager                    |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                 Customer Success Officer (China)                |       0.0 |       0.0 |         0.0 |          0.0 | 0.401075 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
| Social Media & Content Manager for Girls Beyond of Female Daily |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                          IOS Developer                          |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                         Product Manager                         |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|            Backend Developer (JAVA) - Middle - Hybrid           |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                        Operational Staff                        |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                    Associate Product Manager                    |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                          .Net Developer                         |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |

__Menghitung cosine similarity pada matrix tf-idf__

Pada bagian ini dilakukan tahap _cosine similarity_ yang bertujuan untuk  menghitung derajat kesamaan (similarity degree) antar kemampuan dengan teknik cosine similarity dan untuk menggunakannya, kami menggunakan fungsi cosine_similarity dari library sklearn setelah menginisialisasi fungsi _cosine similarity_panggil fungsi tfidf_matrix lalu simpan dalam variable cosine_sim kemudian akan muncull output berupa array matriks , untuk hasilnya dapat dilihat pada gambar 10 :

Gambar 10 :
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/15.JPG?raw=true)

__Membuat dataframe dari variabel cosine_sim dengan baris dan kolom berupa jenis pekerjaan dan kemampuan__

Pada bagian ini dilakukan tahap pembuatan dataframe dari variabel cosine_sim dengan  baris berupa Kemampuan dan column berupa jenis pekerjaan setelah itu hasilnya akan dimasukkan kedalam variable _cosine_sim_df_ kemudian untuk melihat ukuran dataframe dapat menggunaka fungsi _shape_ lalu untuk cetak ukuran dari dataframe _cosine_sim_df_  menggunakan fungsi print 

__Melihat similarity matrix pada setiap kemampuan__

Pada bagian ini dilakukan tahap melihat sample matriks berdasarkan kemampuan dengan sampel berupa baris sebanyak 10 dan kolom sebanyak 10, dan untuk melihat hasilnta dapat dilihat di tabel 11 :

Tabel 11: 
|                                                         banking | resources | strategic | advertising | partnerships |       ux | executive | portfolio | calling | teamwork |   |   |
|----------------------------------------------------------------:|----------:|----------:|------------:|-------------:|---------:|----------:|----------:|--------:|---------:|---|---|
|                                                 Jenis_Pekerjaan |           |           |             |              |          |           |           |         |          |   |   |
|                         Research Manager                        |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                    Digital Marketing Manager                    |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                 Customer Success Officer (China)                |       0.0 |       0.0 |         0.0 |          0.0 | 0.401075 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
| Social Media & Content Manager for Girls Beyond of Female Daily |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                          IOS Developer                          |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                         Product Manager                         |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|            Backend Developer (JAVA) - Middle - Hybrid           |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                        Operational Staff                        |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                    Associate Product Manager                    |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |
|                          .Net Developer                         |       0.0 |       0.0 |         0.0 |          0.0 | 0.000000 |       0.0 |       0.0 |     0.0 |      0.0 |   |   |

***
## __Mendapatkan Rekomendasi__

Pada Mendapatkan rekomendasi dengan content based dapat dilakukan beberapa tahapan, antara lain :
*  Mengambil data dengan menggunakan argpartition untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan    
*  Mengambil data dengan similarity terbesar dari index yang ada
*  Drop kemampuan agar kemampuan yang dicari tidak muncul dalam daftar rekomendasi
*  Menampilkan hasil fungsi Jobs_Recommendations
*  Menampilkan Rekomendasi berdasarkan kemampuan
*  Menampilkan list rekomendasi berdasarkan Kemiripan Kemampuan
*  Convert Hasilnya kedalam csv dan pickle 

Untuk melakukan 3 tahapan tersebut, kami membuat sebuat fungsi jobs_recomendation yang memiliki 4 paratemeter yaitu kemampuan yang memiliki fungsi sebagai index kemiripan dataframe, similiarty_data yang berisi dataframe dari cosine_sim_df yaitu Dataframe mengenai similarity yang telah kita definisikan sebelumnya, lalu ada items yang digunakan untuk mendefinisikan kemiripan, dalam hal ini adalah 'Jenis_Pekerjaan’ dan ‘kemampuan’, kemudian k yaitu  Banyak rekomendasi yang ingin diberikan. setelah itu kami melakukan tahap 

__Mengambil data dengan menggunakan argpartition untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan__

Pada bagian ini dilakukan tahap untuk Mengambil data dengan menggunakan argpartition dari dataframe similarity dari cosine_dim_df dengan parameter yang dikunci adalah kemampuan  untuk melakukan partisi secara tidak langsung sepanjang sumbu yang diberikan setelah itu dataframe akan diubah kebentuk numpy dan terakhir diberikan range datanya 

__Mengambil data dengan similarity terbesar dari index yang ada__

Pada bagian ini dilakukan tahap untuk Mengambil data dengan similarity terbesar dari index yang ada 

__Drop kemampuan agar kemampuan yang dicari tidak muncul dalam daftar rekomendasi__

Pada bagian ini dilakukan tahap untuk menghapus kolom kemampuan dengan fungsi  _drop_  agar kemampuan yang dicari tidak muncul dalam daftar rekomendasi

__Menampilkan hasil fungsi Jobs_Recommendations__

Pada bagian ini dilakukan tahap untuk menampilkan  sample hasil dari fungsi  jobs_recomendation dengan hasilnya dapat diliha dari gambar 11:

Gambar 11: 
![this is an image](https://github.com/Capstone-SIB-3/SiMiKar-Algoritma-/blob/source-gambar/16.JPG?raw=true)

__Menampilkan Rekomendasi berdasarkan kemampuan__


Pada bagian ini dilakukan tahap untuk menampilkan rekomendasi dari kemampuan ___Management, Microsoft, Communication Skills___, untuk hasilnya dapat dilihat pada tabel 12

Tabel 12:
|             Jenis_Kemampuan             |                          Kemampuan                          |
|:---------------------------------------:|:-----------------------------------------------------------:|
|     Secretary to President Director     |         Management, Microsoft, Communication Skills         |
| Compensation & Benefit Staff - Cikarang |        Data Management, Microsoft Excel, Communicatio       |
|          Data analyst reviewer          |        Python, MySQL, Communication Skills, Microsoft       |
|           Agronomy Specialist           |    Business Development, Management, Communication skills   |
|             Host Livestream             |      Communication Skills, Sales Strategy & Management      |
|           Executive Secretary           |       Microsoft Office, Communication Skills, English       |
|          HR Recruitment Intern          |      Human Resources, Recruiting, Communication Skills      |
|             Farm Technician             |        Analytical Skills, Communication Skills, Proje       |
|  Senior Environmental & Social Officer  |        Business Development & Partnerships, Microsof        |
|      Senior Product Manager - CITIS     | Business Analysis, Product Management, Communication skills |

___Menampilkan list rekomendasi berdasarkan Kemiripan Kemampuan__

Pada bagian ini, kami membuat sebuah variable baru yang bernama __rekomendasi__ yang menampung dataframe dari fungsi job_recommendations dengan parameter kemampuan yang dicari yaitu Laravel, PHP, Vue.js, setelah itu kami memanggil kembali variable rekomendasi untuk menampilkan list rekomendasinya dan untuk hasilnya dapat dilihat pada tabel 13:

Tabel 13 :
|   Jenis_Pekerjaan  |                    Kemampuan                   |
|:------------------:|:----------------------------------------------:|
| Fullstack Engineer |              Laravel, PHP, Vue.js              |
|      Tech Lead     |             React.js, Laravel, PHP             |
|  Backend Developer |   Backend Development, Laravel, Web Services,  |
|  Backend Developer |              Laravel, PHP, Node.js             |
|  Backend Developer | MySQL, Git, Amazon Web Services (AWS), Python, |
|  Backend Developer |             PHP, Cassandra, Node.js            |
|  Backend Developer | Backend Development, Laravel, Web Services, RE |
|  Backend Developer |              Laravel, PHP, Node.js             |
|  Backend Developer | MySQL, Git, Amazon Web Services (AWS), Python, |
|  Backend Developer |             PHP, Cassandra, Node.js            |


***
## __Evalution__
#### __Content based filtering__
Metric yang digunakan pada sistem rekomendasi kemampuan berdasarkan teknik __Content based filtering__ adalah accuracy precision. Precision adalah metrik yang membandingkan rasio prediksi benar atau positif dengan keseluruhan hasil yang diprediksi positif dengan rumus
```python 
Precission = (TP) / (TP+FP).
    
keterangan:
TP = True Positif (prediksi positif dan hal tersebut benar)
FP = False Negatif (prediksi positif dan hal tersebut salah)
```
Alasan memilih Akurasi adalah karena metrik ini memungkinkan Anda untuk membandingkan positif atau proporsi prediksi positif dengan keseluruhan hasil prediksi positif. Dalam hal ini, persentase artikel yang direkomendasikan bergenre sama atau mirip dibandingkan dengan genre dari kemampuan yang dimasukkan.
