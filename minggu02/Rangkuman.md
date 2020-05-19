Nama : siti mu'minah
nim  : 175610026
--------------------
LATIHAN (rangkuman)

SaaS:Software as a Service(Perangkat Lunak sebagai Layanan)
Ini berarti menjalankan aplikasi di cloud publik. 
Pengguna menggunakan aplikasi ini melalui Internet. Aplikasi ini dikelola oleh Penyedia Layanan.
Beberapa mis., Penyedia Layanan, adalah SalesForce, Microsoft (Office 365), Oracle,
Google (Google Apps), dll.

Salesforce adalah perusahaan pertama yang mengubah dunia Saas, 
dan sejak saat itu, perusahaan lain telah melihat potensi di pasar ini 
dan meluncurkan aplikasi mereka.

Iaas: Infrastructure as a Service (Perangkat Lunak sebagai Layanan)
Ini memberikan lingkungan bagi pengembang untuk membangun aplikasi
yang dapat digunakan pengguna.IaaS meliputi:

- Pengguna membuat mesin virtual (VM) sesuai permintaan.
- Dari perpustakaan gambar VM.
  Amazon (AWS) adalah vendor terkemuka dalam menyediakan IaaS.

  PaaS: Platform as a Service (Platform sebagai Layanan)
  ini agak mirip dengan IaaS tetapi perbedaannya adalah:

Pengembang menyediakan aplikasi yang dijalankan platform.
Mereka tidak secara langsung membuat VM.Anda akan berpikir bahwa PaaS sederhana dan 
itulah sebabnya banyak digunakan. Tetapi ini tidak benar. IaaS adalah 10 kali populer dari PaaS. 
Pengembang ingin memiliki kontrol lebih besar atas sumber daya.


SAAS (Software as a Service) Platform Architecture (Perangkat Lunak sebagai Layanan)
Perangkat lunak sebagai layanan adalah model lisensi dan pengiriman perangkat lunak di 
mana perangkat lunak dilisensikan berdasarkan berlangganan dan di-host secara terpusat.
Pengguna dapat mengaksesnya dengan bantuan browser web.

Ini terkait dengan penyedia layanan aplikasi (ASP) 
yang menyediakan aplikasi "shrink-wrap" untuk pengguna bisnis melalui Internet.
Perangkat lunak yang dikirimkan melalui Internet lebih awal memiliki fitur yang mirip 
dengan aplikasi di tempat dibandingkan dengan aplikasi SaaS. Karena ini awalnya dibangun 
sebagai aplikasi penyewa tunggal, kemampuan mereka untuk berbagi data terbatas. Aplikasi SaaS 
adalah instance tunggal, arsitektur multi-tenant yang memberikan pengalaman kaya fitur yang
kompetitif dengan aplikasi on-premise. Aggregator menggabungkan penawaran SaaS dari vendor 
yang berbeda dan menawarkannya sebagai bagian dari platform aplikasi terpadu.

Penyedia SaaS menyimpan aplikasi dan data secara terpusat - tambalan penyebaran. 
Mereka meningkatkan ke aplikasi secara transparan, memberikan akses ke pengguna
akhir melalui Internet. Banyak vendor menyediakan API yang digunakan pengembang 
untuk membuat aplikasi komposit. Ini berisi berbagai mekanisme keamanan untuk keamanan data 
selama transmisi dan penyimpanan.

SAAS Architecture:
Dengan model ini, satu versi aplikasi, dengan satu konfigurasi digunakan untuk semua pelanggan.
Aplikasi ini diinstal pada banyak mesin untuk mendukung skalabilitas (disebut penskalaan horizontal).
Dalam beberapa kasus, versi kedua aplikasi diatur untuk menawarkan kelompok pelanggan tertentu dengan
akses ke versi pra-rilis aplikasi untuk tujuan pengujian. Dalam model tradisional ini, setiap versi 
aplikasi didasarkan pada kode unik. Meskipun pengecualian, beberapa solusi SaaS tidak menggunakan
multitenancy, untuk mengelola secara efektif sejumlah besar pelanggan di tempat. Apakah multitenancy
merupakan komponen yang diperlukan untuk perangkat lunak-sebagai-layanan adalah topik kontroversi.

Ada dua varietas utama SaaS:

- SaaS Vertikal
 Perangkat Lunak yang menjawab kebutuhan industri tertentu
 (mis., Perangkat lunak untuk kesehatan, pertanian, real estat, industri keuangan)
- SaaS Horisontal
Produk-produk yang berfokus pada kategori perangkat lunak
(pemasaran, penjualan, alat pengembang, SDM) tetapi agnostik industri.

Benefit of SAAS:
Ini menawarkan peluang besar bagi organisasi dari semua ukuran untuk mengalihkan risiko akuisisi 
perangkat lunak, dan untuk memindahkan TI dari pusat biaya reaktif menjadi bagian perusahaan yang 
proaktif dan bernilai tambah. Secara tradisional, menyebarkan sistem perangkat lunak skala besar 
telah menjadi tugas utama. Menyebarkan sistem ini di perusahaan besar membutuhkan biaya lebih besar. 
Waktu, staf, dan persyaratan anggaran untuk penyebaran sebesar ini mewakili risiko yang signifikan 
bagi organisasi dalam ukuran berapa pun, dan sering kali menempatkan perangkat lunak seperti itu di 
luar jangkauan organisasi yang lebih kecil yang jika tidak dapat diturunkan darinya banyak utilitas.
Model pengiriman berdasarkan permintaan mengubah beberapa hal ini. Aplikasi SaaS tidak memerlukan 
penyebaran infrastruktur besar di lokasi klien. Ini menghilangkan atau secara drastis mengurangi 
komitmen sumber daya dimuka.

Integrasi dapat direncanakan dan dilaksanakan dengan upaya minimal, menciptakan salah satu interval 
waktu-ke-nilai sesingkat mungkin untuk investasi TI utama. Ini juga memungkinkan sejumlah vendor SaaS 
untuk menawarkan "test drive" bebas risiko (dan seringkali secara harfiah gratis) dari perangkat lunak mereka untuk periode terbatas, seperti 30 hari. Memberi pelanggan kesempatan untuk mencoba perangkat lunak sebelum mereka membelinya membantu menghilangkan banyak risiko seputar pembelian perangkat lunak.

How SaaS Affects IT ?
Setelah Anda membuat keputusan untuk mengejar SaaS, selanjutnya adalah mempersiapkan transisi dengan 
menilai bagaimana penyebaran akan mempengaruhi aset TI yang ada. Melakukan uji tuntas adalah bagian 
rutin dari setiap proyek penyebaran infrastruktur TI yang sukses. Beberapa bidang yang harus
ditangani dalam daftar periksa uji tuntas meliputi, Standar keamanan data: Memindahkan data bisnis 
penting "di luar tembok" menimbulkan risiko kehilangan data atau paparan informasi sensitif yang 
tidak disengaja. Nilai kebutuhan keamanan data Anda, dan pastikan bahwa penyedia memiliki
langkah-langkah untuk memenuhi standar yang Anda tetapkan. Layanan pelaporan: Karena SaaS 
melibatkan pemberian kontrol langsung atas beberapa data Anda, pelaporan yang akurat dan bermanfaat 
sangat penting. Tentukan layanan pelaporan apa yang ditawarkan penyedia, dan apakah mereka 
kompatibel dengan persyaratan intelijen bisnis Anda.

Impact on IT Roles and Responsibilities
Menambahkan SaaS dapat menyebabkan perubahan mendasar dalam peran departemen TI sebagai penyedia
layanan informasi. Di masa lalu, sifat penyebaran perangkat lunak telah menempatkan petugas informasi
kepala dalam peran penjaga gerbang. Mereka dapat melakukan veto dengan menyatakan bahwa mereka tidak 
akan meng-host-nya di pusat data. Dengan SaaS, kontrol pusat data tidak harus sama dengan kontrol
atas seluruh lingkungan komputasi perusahaan. Ini dapat menyebabkan penjaga gerbang takut kehilangan
kendali.

Conclusion
Perusahaan sebaiknya mempertimbangkan fleksibilitas dan implikasi manajemen risiko dalam menambahkan 
SaaS ke portofolio layanan TI mereka. Integrasi dan komposisi adalah komponen penting dalam strategi
arsitektur Anda untuk menggabungkan SaaS dengan sukses sebagai anggota yang berpartisipasi penuh dari 
infrastruktur TI Anda yang berpusat pada layanan. Kami percaya bahwa masa depan komputasi perusahaan 
tidak akan murni di tempat. Sebaliknya, mereka akan ada dalam harmoni simbiosis.




