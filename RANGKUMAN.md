
data as a service

1. Pada dasarnya ada 2 tipe DBMS: SQL dan NoSQL.

History
SQL awalnya dikembangkan di IBM oleh Donald D. Chamberlin dan Raymond F. Boyce setelah mempelajari 
tentang model relasional dari Ted Codd [14] pada awal 1970-an. [15] Versi ini, awalnya disebut SEQUEL
(Structured English Query Language), dirancang untuk memanipulasi dan mengambil data yang disimpan 
dalam sistem manajemen basis data kuasi-relasional asli IBM, System R, yang dikembangkan oleh sebuah
kelompok di Laboratorium Penelitian IBM San Jose selama tahun 1970-an. [ 15]


Upaya pertama Chamberlin dan Boyce tentang bahasa basis data relasional adalah Square, tetapi sulit 
digunakan karena notasi subskrip. Setelah pindah ke Laboratorium Penelitian San Jose pada tahun 1973,
mereka mulai mengerjakan SEQUEL. [14] Akronim SEQUEL kemudian diubah menjadi SQL karena "SEQUEL" 
adalah merek dagang dari perusahaan Hawker Siddeley Dynamics Engineering Limited yang berbasis di 
Inggris. [16]

Setelah menguji SQL di lokasi pengujian pelanggan untuk menentukan kegunaan dan kepraktisan sistem, 
IBM mulai mengembangkan produk komersial berdasarkan prototipe System R mereka termasuk System / 38,
SQL / DS, dan DB2, yang tersedia secara komersial pada tahun 1979, 1981, dan 1983, masing-masing. [17]

Pada akhir 1970-an, Relational Software, Inc. (sekarang Oracle Corporation) melihat potensi konsep
yang dijelaskan oleh Codd, Chamberlin, dan Boyce, dan mengembangkan RDMS berbasis SQL mereka sendiri
dengan aspirasi menjualnya kepada Angkatan Laut AS, Central Intelligence Agensi, dan agensi pemerintah
AS lainnya. Pada Juni 1979, Relational Software, Inc. memperkenalkan implementasi SQL, Oracle V2 
(Version2) yang tersedia secara komersial pertama untuk komputer VAX.

Pada tahun 1986, kelompok standar ANSI dan ISO secara resmi mengadopsi definisi bahasa "Database 
Language SQL". Versi baru dari standar ini diterbitkan pada tahun 1989, 1992, 1996, 1999, 2003, 2006,
2008, 2011 [14] dan, paling baru, 2016.

Design
SQL menyimpang dalam beberapa cara dari fondasi teoretisnya, model relasional dan kalkulus tuple-nya.
Dalam model itu, tabel adalah sekumpulan tupel, sedangkan dalam SQL, tabel dan hasil kueri adalah 
daftar baris: baris yang sama dapat muncul beberapa kali, dan urutan baris dapat digunakan dalam 
kueri (misalnya dalam klausa LIMIT) .

Para kritikus berpendapat bahwa SQL harus diganti dengan bahasa yang mengembalikan secara ketat ke 
fondasi asli: misalnya, lihat The Third Manifesto. Namun, tidak ada bukti yang diketahui bahwa 
keunikan tersebut tidak dapat ditambahkan ke SQL itu sendiri, atau setidaknya variasi dari SQL.
Dengan kata lain, sangat mungkin bahwa SQL dapat "diperbaiki" atau setidaknya ditingkatkan dalam hal
ini sehingga industri mungkin tidak harus beralih ke bahasa permintaan yang sama sekali berbeda untuk
mendapatkan keunikan. Debat tentang hal ini tetap terbuka.

Syntax
Bahasa SQL dibagi menjadi beberapa elemen bahasa, termasuk:

Klausa, yang merupakan komponen penyusun pernyataan dan pertanyaan. (Dalam beberapa kasus, ini opsional.) [18]
Ekspresi, yang dapat menghasilkan nilai skalar, atau tabel yang terdiri dari kolom dan baris data
Predikat, yang menentukan kondisi yang dapat dievaluasi menjadi logika tiga nilai SQL (3VL) (benar / salah / tidak diketahui) atau nilai kebenaran Boolean dan digunakan untuk membatasi efek pernyataan dan kueri, atau untuk mengubah aliran program.
Kueri, yang mengambil data berdasarkan kriteria tertentu. Ini adalah elemen penting dari SQL.
Pernyataan, yang mungkin memiliki efek gigih pada skema dan data, atau dapat mengontrol transaksi, aliran program, koneksi, sesi, atau diagnostik.
Pernyataan SQL juga termasuk terminator pernyataan titik koma (";"). Meskipun tidak diperlukan pada setiap platform, ia didefinisikan sebagai bagian standar dari tata bahasa SQL.
Ruang kosong yang tidak penting biasanya diabaikan dalam pernyataan dan pertanyaan SQL, membuatnya lebih mudah untuk memformat kode SQL agar mudah dibentuk.

2. Pelajari Data as a Service di Wikipedia serta Cloud database.
Untuk Desktop sebagai layanan (DaaS), lihat Virtualisasi desktop.
Dalam komputasi, data sebagai layanan, atau DaaS, diaktifkan oleh perangkat lunak sebagai layanan 
(SaaS). [1] Seperti semua teknologi "sebagai layanan" (aaS), DaaS membangun konsep bahwa produk 
datanya dapat diberikan kepada pengguna berdasarkan permintaan, [2] terlepas dari pemisahan geografis
atau organisasi antara penyedia dan konsumen. Arsitektur berorientasi layanan (SOA), dan meluasnya 
penggunaan API, telah menjadikan platform tempat data berada sebagai tidak relevan. [3]

DaaS dimulai terutama di mashups Web dan, sejak 2015, telah semakin dipekerjakan baik secara 
komersial, maupun dalam organisasi seperti PBB. [4]

Secara tradisional, sebagian besar organisasi telah menggunakan data yang disimpan dalam repositori
mandiri, yang untuknya perangkat lunak dikembangkan secara khusus untuk mengakses dan menyajikan data dalam bentuk yang dapat dibaca manusia. Salah satu hasil dari paradigma ini adalah penggabungan data dan perangkat lunak yang diperlukan untuk menafsirkannya menjadi satu paket, dijual sebagai produk konsumen. Karena jumlah perangkat lunak yang dibundel dengan paket data berkembang biak, dan diperlukan interaksi antara satu sama lain, lapisan antarmuka lain diperlukan. Antarmuka ini, secara kolektif dikenal sebagai integrasi aplikasi perusahaan (EAI), sering cenderung mendorong vendor lock-in, karena umumnya mudah untuk mengintegrasikan aplikasi yang dibangun di atas teknologi pondasi yang sama. [5]

Hasil dari paket perangkat lunak / data konsumen gabungan dan middleware EAI yang dibutuhkan telah
meningkatkan jumlah perangkat lunak untuk dikelola dan dikelola organisasi, hanya untuk penggunaan 
data tertentu. Selain biaya perawatan rutin, sejumlah pembaruan perangkat lunak yang mengalir 
diperlukan karena format data berubah. Keberadaan situasi ini berkontribusi pada daya tarik DaaS 
kepada konsumen data, karena memungkinkan pemisahan biaya data dan penggunaan data dari biaya 
lingkungan atau platform perangkat lunak tertentu. Sensing as a Service [6] [7] (S2aaS) adalah model 
bisnis yang mengintegrasikan data Internet of Things untuk membuat pasar perdagangan data.

Vendor, seperti MuleSoft, Oracle Cloud dan Microsoft Azure, melakukan pengembangan DaaS yang lebih 
cepat menghitung volume data yang besar; mengintegrasikan dan menganalisis data itu; dan 
menerbitkannya secara waktu nyata, menggunakan API layanan Web yang mematuhi batasan arsitektur REST
(API RESTful).

3. Arsitektur Data as a Service biasanya diwujudukan dengan menggunakan arsitektur microservices.
Compiler dan development tools bersama dengan REST atau gRPC atau GraphQL serta berbagai protokol
untuk implementasi function as a service digunakan untuk membangun service. Akses ke data biasanya 
melibatkan DBMS (SQL maupun NoSQL). Untuk Go, bisa digunakan Gin untuk menghasilkan data JSON dalam 
respon.

Apa itu layanan microser?
Layanan Mikro - juga dikenal sebagai arsitektur layanan mikro - adalah gaya arsitektur yang menyusun 
aplikasi sebagai kumpulan layanan yang

Sangat dapat dirawat dan diuji
Hubungan renggang
Dapat digunakan secara independen
Diatur di sekitar kemampuan bisnis
Dimiliki oleh tim kecil

Arsitektur microservice memungkinkan pengiriman aplikasi yang besar dan kompleks dengan cepat, 
sering, dan andal. Ini juga memungkinkan suatu organisasi untuk mengembangkan tumpukan teknologinya.

4. Akses ke DBMS di Go dilakukan dengan menggunakan pustaka standar serta driver. Untuk MySQL serta 
berbagai DBMS SQL (RDBMS), digunakan paket database/sql. Untuk NoSQL (contoh: MongoDB), digunakan
driver MongoDB untuk Go.










