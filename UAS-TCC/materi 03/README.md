Aplikasi Containerisasi dan Orkestra Layanan Mikro
Kami akan mulai dari skrip Python dasar yang mengikis tautan dari laman web tertentu dan secara bertahap mengubahnya menjadi 
tumpukan aplikasi multi-layanan. Kode demo tersedia di repo Link Extractor . Kode ini disusun dalam langkah-langkah yang 
secara bertahap memperkenalkan perubahan dan konsep baru. Setelah selesai, tumpukan aplikasi akan berisi layanan microser 
berikut:

1. Aplikasi web yang ditulis dalam PHP dan disajikan menggunakan Apache yang mengambil URL sebagai input dan merangkum tautan yang diekstrak darinya
2. Aplikasi web berbicara ke server API yang ditulis dengan Python (dan Ruby) yang menangani ekstraksi tautan dan mengembalikan respons JSON
3. Cache Redis yang digunakan oleh server API untuk menghindari pengambilan berulang dan ekstraksi tautan untuk halaman yang sudah dikikis
4. Server API hanya akan memuat halaman tautan input dari web jika tidak ada dalam cache. Tumpukan pada akhirnya akan terlihat seperti gambar di bawah ini:

Langkah:
Pengaturan Panggung
Mari kita mulai dengan kloning repositori kode demo, mengubah direktori kerja, dan memeriksa democabang keluar.

Langkah 0: Skrip Extractor Tautan Dasar
Periksa step0cabang dan daftar file di dalamnya.
The linkextractor.pyfile yang menarik di sini, jadi mari kita lihat isinya:
Ini adalah skrip Python sederhana yang mengimpor tiga paket: sysdari pustaka standar dan dua paket pihak ketiga populer 
requestsdan bs4. Argumen baris perintah yang disediakan pengguna (yang diharapkan menjadi URL ke halaman HTML) digunakan untuk mengambil halaman menggunakan requestspaket, kemudian diuraikan menggunakan BeautifulSoup. Objek yang diuraikan kemudian diiterasi untuk menemukan semua elemen jangkar (yaitu, <a>tag) dan mencetak nilai hrefatribut mereka yang berisi hyperlink.
Namun, skrip yang tampaknya sederhana ini mungkin bukan yang paling mudah dijalankan pada mesin yang tidak memenuhi 
persyaratannya. The README.mdFile menyarankan bagaimana menjalankannya, jadi mari kita mencobanya:

Ketika kami mencoba menjalankannya sebagai skrip, kami mendapatkan Permission deniedkesalahan. Mari kita periksa izin saat ini pada file ini:
Izin ini -rw-r--r--menunjukkan bahwa skrip tidak dapat dieksekusi. Kami dapat mengubahnya dengan menjalankan chmod a+x linkextractor.pyatau menjalankannya sebagai program Python alih-alih skrip yang dijalankan sendiri seperti yang diilustrasikan di bawah ini:

Bergantung pada mesin dan sistem operasi tempat Anda mencoba menjalankan skrip ini, perangkat lunak apa yang sudah 
diinstal, dan seberapa banyak akses yang Anda miliki, Anda mungkin menghadapi beberapa kesulitan potensial:

1. Apakah skrip dapat dieksekusi?
2. Apakah Python diinstal pada mesin?
3. Bisakah Anda menginstal perangkat lunak pada mesin?
4. Sudah pipterpasang?
5. Apakah requestsdan beautifulsoup4pustaka Python diinstal?
Di sinilah alat kontainerisasi aplikasi seperti Docker berguna. Pada langkah selanjutnya kita akan mencoba membuat wadah 
script ini dan membuatnya lebih mudah untuk dieksekusi.

Langkah 1: Script Extractor Tautan Kontainer
Periksa step1cabang dan daftar file di dalamnya.
Kami telah menambahkan satu file baru (yaitu, Dockerfile) pada langkah ini. Mari kita lihat isinya:
Dengan ini Dockerfilekita bisa menyiapkan gambar Docker untuk skrip ini. Kita mulai dari pythongambar Docker resmi yang 
berisi lingkungan run-time Python serta alat yang diperlukan untuk menginstal paket dan dependensi Python. Kami kemudian
menambahkan beberapa metadata sebagai label (langkah ini tidak penting, tetapi merupakan praktik yang baik). Berikutnya 
dua instruksi menjalankan pip installperintah untuk menginstal dua paket pihak ketiga yang diperlukan agar skrip berfungsi dengan baik. Kami kemudian membuat direktori yang berfungsi /app, menyalin linkextractor.pyfile di dalamnya, dan mengubah izinnya untuk membuatnya menjadi skrip yang dapat dieksekusi. Akhirnya, kami menetapkan skrip sebagai titik masuk untuk gambar.
Sejauh ini, kami baru saja menggambarkan bagaimana kami ingin gambar Docker kami menjadi seperti, tetapi tidak benar-benar
membangun satu. Jadi mari kita lakukan:

 Langkah 2: Tautkan Modul Extractor dengan URI Lengkap dan Anchor Text
 Periksa step2cabang dan daftar file di dalamnya.
 Pada langkah ini linkextractor.pyskrip diperbarui dengan perubahan fungsional berikut:

1. Jalur dinormalisasi ke URL lengkap
1. Melaporkan tautan dan teks jangkar
2. Dapat digunakan sebagai modul di skrip lain
Mari kita lihat skrip yang diperbarui:

Sejauh ini, kami telah mempelajari cara membuat wadah naskah dengan dependensi yang diperlukan untuk membuatnya lebih 
portabel. Kami juga telah belajar cara membuat perubahan dalam aplikasi dan membangun berbagai varian gambar Docker yang 
dapat hidup berdampingan. Pada langkah berikutnya kita akan membangun layanan web yang akan memanfaatkan skrip ini dan 
akan membuat layanan berjalan di dalam wadah Docker.

Langkah 3: Layanan Link Extractor API
Periksa step3cabang dan daftar file di dalamnya.
Perubahan berikut telah dilakukan pada langkah ini:

Menambahkan skrip server main.pyyang menggunakan modul ekstraksi tautan yang ditulis pada langkah terakhir
1. Itu Dockerfilediperbarui untuk merujuk ke main.pyfile sebagai gantinya
2. Server dapat diakses sebagai API WEB di http://<hostname>[:<prt>]/api/<url>
3. Ketergantungan dipindahkan ke requirements.txtfile
4. Membutuhkan pemetaan port untuk membuat layanan dapat diakses di luar wadah ( Flaskserver yang digunakan di sini mendengarkan port 5000secara default)
Pertama mari kita lihat Dockerfileuntuk perubahan:

Langkah 4: Link Extractor API dan Layanan Front Web
Periksa step4cabang dan daftar file di dalamnya.
Pada langkah ini perubahan berikut telah dilakukan sejak langkah terakhir:
1. Layanan tautan extractor JSON API (ditulis dengan Python) dipindahkan dalam ./apifolder terpisah yang memiliki kode yang sama persis seperti pada langkah sebelumnya
2. Aplikasi front-end web ditulis dalam PHP di bawah ./wwwfolder yang berbicara dengan API JSON
3. Aplikasi PHP dipasang di dalam php:7-apachegambar Docker resmi untuk modifikasi yang lebih mudah selama pengembangan
4. Aplikasi web dapat diakses di http://<hostname>[:<prt>]/?url=<url-encoded-url>
5. Variabel lingkungan API_ENDPOINTdigunakan di dalam aplikasi PHP untuk mengkonfigurasinya untuk berbicara dengan server JSON API
6. Sebuah docker-compose.ymlfile ditulis untuk membangun berbagai komponen dan lem mereka bersama-sama

Langkah 5: Layanan Redis untuk Caching
Periksa step5cabang dan daftar file di dalamnya.
Beberapa perubahan nyata dari langkah sebelumnya adalah sebagai berikut:

Lain Dockerfileditambahkan dalam ./wwwfolder 
1. untuk aplikasi web PHP untuk membangun gambar mandiri dan menghindari pemasangan file langsung
2. Wadah Redis ditambahkan untuk caching menggunakan gambar Redis Docker resmi
3. Layanan API berbicara dengan layanan Redis untuk menghindari mengunduh dan mem-parsing halaman yang sudah dikerik sebelumnya
4. Sebuah REDIS_URLvariabel lingkungan ditambahkan ke layanan API untuk memungkinkan untuk terhubung ke cache Redis
Pertama-tama mari kita periksa yang baru ditambahkan di Dockerfilebawah ./wwwfolder:

Sekarang, cobalah untuk mengekstrak tautan dari beberapa halaman web menggunakan antarmuka web dan lihat perbedaan dalam 
entri log Redis untuk halaman yang tergores pertama kali dan yang diulang. Sebelum melanjutkan dengan tutorial, hentikan 
monitorstreaming interaktif sebagai akibat dari hal di atasredis-cli perintah di dengan menekan Ctrl + Ctombol saat 
terminal interaktif dalam fokus.
Sekarang kami tidak memasang /wwwfolder di dalam wadah, perubahan lokal seharusnya tidak tercermin dalam layanan yang 
berjalan:

Langkah 6: Tukar Layanan API Python dengan Ruby
Periksa step6cabang dan daftar file di dalamnya.
Beberapa perubahan signifikan dari langkah sebelumnya meliputi:

1. Layanan API yang ditulis dengan Python diganti dengan implementasi Ruby yang serupa
2. The API_ENDPOINTvariabel lingkungan diperbarui ke titik ke layanan API Ruby baru
3. Kejadian cache ekstraksi tautan (HIT / MISS) dicatat dan dipertahankan menggunakan volume
Perhatikan bahwa ./apifolder tersebut tidak mengandung skrip Python, sebaliknya, sekarang memiliki file Ruby dan Gemfileuntuk mengelola dependensi.

Kesimpulan
Kami memulai tutorial ini dengan skrip Python sederhana yang mengikis tautan dari URL laman web pemberian. Kami menunjukkan
berbagai kesulitan dalam menjalankan skrip. Kami kemudian mengilustrasikan betapa mudahnya menjalankan dan membuat skrip
menjadi mudah dibawa kemas. Pada langkah selanjutnya kami secara bertahap mengembangkan skrip menjadi tumpukan aplikasi 
multi-layanan. Dalam proses tersebut kami mengeksplorasi berbagai konsep arsitektur layanan mikro dan bagaimana alat Docke
dapat membantu dalam menyusun tumpukan multi-layanan. Akhirnya, kami mendemonstrasikan kemudahan pertukaran komponen layanan mikro dan kegigihan data.

