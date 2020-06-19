langkah - langkah pembuatan untuk docker dan begginers Linux

# Klon Repo Github Lab
didunakan perintah berikut untuk mengkloning repo tab dan Github  (Anda dapat mengklik perintah atau mengetiknya secara manual). 
Ini akan membuat salinan repo laboratorium di sub-direktori baru bernama linux_tweet_app.
git clone https://github.com/dockersamples/linux_tweet_app

Pastikan Anda memiliki DockerID
Jika Anda tidak memiliki DockerID (login gratis yang digunakan untuk mengakses Docker Hub), silakan kunjungi Docker
Hub dan mendaftar untuk itu. Anda akan membutuhkan ini untuk langkah selanjutnya.

Tugas 1: Jalankan beberapa wadah Docker sederhana
Ada berbagai cara untuk menggunakan wadah. Ini termasuk:

1. Untuk menjalankan satu tugas: Ini bisa berupa skrip shell atau aplikasi khusus.
2. Interaktif: Ini menghubungkan Anda ke wadah yang mirip dengan cara Anda SSH ke server jauh.
3. latar belakang: Untuk layanan jangka panjang seperti situs web dan basis data.

Di bagian ini Anda akan mencoba masing-masing opsi tersebut dan melihat bagaimana Docker mengelola beban kerja.

Jalankan satu tugas dalam wadah Alpine Linux
Pada langkah ini kita akan memulai sebuah wadah baru dan memerintahkannya untuk menjalankan hostnameperintah. 
Wadah akan mulai, jalankan hostnameperintah, lalu keluar.

1. Jalankan perintah berikut di konsol Linux Anda
   (docker container run alpine hostname)

Output di bawah ini menunjukkan bahwa alpine:latestgambar tidak dapat ditemukan secara lokal. Ketika ini terjadi, 
Docker secara otomatis menariknya dari Docker Hub.
Setelah gambar ditarik, nama host penampung ditampilkan ( 888e89a3b36bdalam contoh di bawah).

2. Docker menjaga wadah berjalan selama proses itu dimulai di dalam wadah masih berjalan. 
   Dalam hal ini hostnameproses keluar segera setelah output ditulis. Ini artinya wadah berhenti. Namun, Docker tidak 
   menghapus sumber daya secara default, sehingga wadah masih ada di Exitednegara.

Daftar semua wadah.
 (docker container ls --all)
 Perhatikan bahwa wadah Alpine Linux Anda berada di Exitednegara bagian.

 Wadah yang melakukan satu tugas dan kemudian keluar bisa sangat berguna. Anda bisa membuat gambar Docker yang 
 mengeksekusi skrip untuk mengkonfigurasi sesuatu. Siapa pun dapat menjalankan tugas itu hanya dengan menjalankan 
 wadah - mereka tidak memerlukan skrip atau informasi konfigurasi yang sebenarnya.

Jalankan wadah Ubuntu interaktif
Anda dapat menjalankan sebuah wadah berdasarkan versi Linux yang berbeda dari yang dijalankan pada host Docker Anda.

Dalam contoh berikut, kita akan menjalankan wadah Ubuntu Linux di atas host Alpine Linux Docker (Play With Docker 
menggunakan Alpine Linux untuk node-node-nya).
1. Jalankan wadah Docker dan akses cangkangnya
    (docker container run --interactive --tty --rm ubuntu bash)
    Saat wadah mulai Anda akan jatuh ke bash shell dengan prompt default root@<container id>:/#. Docker telah menempel 
    pada shell di wadah, menyampaikan input dan output antara sesi lokal Anda dan sesi shell di wadah.
 
 2. Jalankan perintah berikut dalam wadah.
    ls /akan mencantumkan isi direktur root dalam wadah, ps auxakan menunjukkan proses yang berjalan dalam wadah, 
    cat /etc/issueakan menunjukkan distro Linux mana wadah berjalan, dalam hal ini Ubuntu 18.04.3 LTS.
3. Ketik exituntuk meninggalkan sesi shell. Ini akan menghentikan bashproses, menyebabkan wadah untuk keluar.
4. Untuk bersenang-senang, mari kita periksa versi host VM kami.
    (cat /etc/issue)

Perhatikan bahwa host VM kami menjalankan Alpine Linux, namun kami dapat menjalankan wadah Ubuntu. Seperti disebutkan sebelumnya,
distribusi Linux di dalam wadah tidak perlu cocok dengan distribusi Linux yang berjalan di host Docker.
Namun, wadah Linux mengharuskan host Docker menjalankan kernel Linux. Misalnya, wadah Linux tidak dapat berjalan secara
langsung di host Windows Docker. Hal yang sama berlaku untuk kontainer Windows - mereka harus dijalankan pada host Docker dengan kernel Windows.
Wadah interaktif berguna ketika Anda menyusun gambar Anda sendiri. Anda dapat menjalankan sebuah wadah dan memverifikasi
semua langkah yang Anda butuhkan untuk menggunakan aplikasi Anda, dan menangkapnya di Dockerfile.

Jalankan latar belakang wadah MySQL
Kontainer latar belakang adalah cara Anda menjalankan sebagian besar aplikasi. Berikut ini contoh sederhana menggunakan 
MySQL.
1. Jalankan wadah MySQL baru dengan perintah berikut
(docker container run \
 --detach \
 --name mydb \
 -e MYSQL_ROOT_PASSWORD=my-secret-pw \
 mysql:latest)

 (detach akan menjalankan wadah di latar belakang.
 nameakan menamainya mydb .
e akan menggunakan variabel lingkungan untuk menentukan kata sandi root (CATATAN: Ini tidak boleh dilakukan dalam produksi).
Karena gambar MySQL tidak tersedia secara lokal, Docker secara otomatis menariknya dari Docker Hub.)

2. Daftar wadah yang sedang berjalan
 (docker container ls)
3. Anda dapat memeriksa apa yang terjadi di wadah Anda dengan menggunakan beberapa perintah Docker bawaan: docker container 
 logsdan docker container top.
 Mari kita lihat proses yang berjalan di dalam wadah
  (docker container top mydb)

  Meskipun MySQL sedang berjalan, ia terisolasi di dalam wadah karena tidak ada port jaringan yang telah dipublikasikan 
  ke host. Lalu lintas jaringan tidak dapat mencapai kontainer dari tuan rumah kecuali port diterbitkan secara eksplisit.

4. Daftar versi MySQL menggunakan docker container exec.
docker container execmemungkinkan Anda untuk menjalankan perintah di dalam sebuah wadah. Dalam contoh ini, kita akan 
gunakan docker container execuntuk menjalankan setara baris perintah mysql --user=root --password=$MYSQL_ROOT_PASSWORD
 --versiondi dalam wadah MySQL kita.

 # docker exec -it mydb \ #
 # mysql --user=root --password=$MYSQL_ROOT_PASSWORD --version #

 5. Anda juga dapat menggunakan docker container execuntuk terhubung ke proses shell baru di dalam wadah yang sudah 
 berjalan. Menjalankan perintah di bawah ini akan memberi Anda shell interaktif ( sh) di dalam wadah MySQL Anda.

 Tugas 2: Paket dan menjalankan aplikasi khusus menggunakan Docker
Bangun gambar situs web sederhana
Mari kita lihat Dockerfile yang akan kita gunakan, yang membangun situs web sederhana yang memungkinkan Anda mengirim 
tweet.
1. Pastikan Anda berada di linux_tweet_appdirektori.
2. Tampilkan konten Dockerfile.
3. Untuk membuat perintah berikut ini lebih ramah salin / tempel, ekspor variabel lingkungan yang berisi DockerID Anda
 (jika Anda tidak memiliki DockerID, Anda bisa mendapatkannya secara gratis melalui Docker Hub ).
4. Gema nilai variabel kembali ke terminal untuk memastikan itu disimpan dengan benar.
5. Gunakan docker image buildperintah untuk membuat gambar Docker baru menggunakan instruksi di Dockerfile.
