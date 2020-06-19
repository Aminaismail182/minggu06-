Bagian # 1 - Dasar-Dasar Jaringan
Langkah 1: Perintah Jaringan Docker
The docker networkperintah adalah perintah utama untuk mengkonfigurasi dan mengelola jaringan kontainer. Jalankan docker
networkperintah dari terminal pertama.

Output perintah menunjukkan cara menggunakan perintah serta semua docker networksub-perintah. Seperti yang dapat Anda 
lihat dari output, docker networkperintah ini memungkinkan Anda untuk membuat jaringan baru, daftar jaringan yang ada, 
memeriksa jaringan, dan menghapus jaringan. Ini juga memungkinkan Anda untuk menghubungkan dan memutuskan wadah dari 
jaringan.

Langkah 2: Daftar jaringan
Jalankan docker network lsperintah untuk melihat jaringan kontainer yang ada pada host Docker saat ini.
Output di atas menunjukkan jaringan kontainer yang dibuat sebagai bagian dari instalasi standar Docker.
Jaringan baru yang Anda buat juga akan muncul di output docker network lsperintah.
Anda dapat melihat bahwa setiap jaringan mendapat unik IDdan NAME. Setiap jaringan juga dikaitkan dengan satu driver. 
Perhatikan bahwa jaringan "jembatan" dan jaringan "host" memiliki nama yang sama dengan driver masing-masing.

Langkah 3: Periksa jaringan
The docker network inspectPerintah ini digunakan untuk melihat rincian konfigurasi jaringan. Rincian ini meliputi; nama, 
ID, driver, driver IPAM, info subnet, wadah terhubung, dan banyak lagi.
Gunakan docker network inspect <network>untuk melihat detail konfigurasi jaringan kontainer pada host Docker Anda.
Perintah di bawah ini menunjukkan perincian jaringan yang disebut bridge.

Langkah 4: Daftar plugin driver jaringan
The docker infoperintah menunjukkan banyak informasi menarik tentang instalasi Docker.

Jalankan docker infoperintah dan temukan daftar plugin jaringan.
Output di atas menunjukkan bahwa jaringan jembatan dikaitkan dengan driver jembatan . Penting untuk dicatat bahwa 
jaringan dan driver terhubung, tetapi tidak sama. Dalam contoh ini jaringan dan driver memiliki nama yang sama - tetapi
mereka bukan hal yang sama!
Output di atas juga menunjukkan bahwa jaringan jembatan dicakup secara lokal. Ini berarti bahwa jaringan hanya ada pada 
host Docker ini. Ini berlaku untuk semua jaringan yang menggunakan driver jembatan - driver jembatan menyediakan jaringan 
host tunggal.
Semua jaringan yang dibuat dengan driver jembatan didasarkan pada jembatan Linux (alias saklar virtual).
Instal brctlperintah dan gunakan untuk mendaftar jembatan Linux di host Docker Anda. Anda dapat melakukan ini dengan 
menjalankan sudo apt-get install bridge-utils.

Kemudian, daftarkan jembatan pada host Docker Anda, dengan menjalankan brctl show.

Output di atas menunjukkan jembatan Linux tunggal yang disebut docker0 . Ini adalah jembatan yang secara otomatis dibuat 
untuk jaringan jembatan . Anda dapat melihat bahwa saat ini tidak ada antarmuka yang terhubung.
Anda juga dapat menggunakan ip aperintah untuk melihat detail jembatan docker0 .

Langkah 2: Hubungkan wadah
Jaringan jembatan adalah jaringan default untuk wadah baru. Ini berarti bahwa kecuali Anda menentukan jaringan yang
berbeda, semua kontainer baru akan terhubung ke jaringan jembatan .

Buat wadah baru dengan menjalankan docker run -dt ubuntu sleep infinity.
Perintah ini akan membuat wadah baru berdasarkan ubuntu:latestgambar dan akan menjalankan sleepperintah untuk menjaga 
wadah berjalan di latar belakang. Anda dapat memverifikasi wadah contoh kami sudah habis dengan menjalankan docker ps.
Karena tidak ada jaringan yang ditentukan pada docker runperintah, wadah akan ditambahkan ke jaringan jembatan .

Jalankan brctl showperintah lagi.

Perhatikan bagaimana jembatan docker0 sekarang memiliki antarmuka yang terhubung. Antarmuka ini menghubungkan jembatan 
docker0 ke wadah baru yang baru saja dibuat.
Anda dapat memeriksa jaringan jembatan lagi, dengan menjalankan docker network inspect bridge, untuk melihat wadah baru 
yang melekat padanya.

Langkah 3: Uji konektivitas jaringan
Output ke docker network inspectperintah sebelumnya menunjukkan alamat IP wadah baru. Dalam contoh sebelumnya ini adalah 
"172.17.0.2" tetapi milik Anda mungkin berbeda.
Ping alamat IP wadah dari prompt shell host Docker Anda dengan menjalankan ping -c5 <IPv4 Address>. Ingatlah untuk 
menggunakan IP wadah di lingkungan Anda .

Balasan di atas menunjukkan bahwa host Docker dapat melakukan ping kontainer melalui jaringan jembatan . Namun, kami 
juga dapat memverifikasi bahwa wadah tersebut dapat terhubung ke dunia luar juga. Mari masuk ke dalam wadah, instal 
pingprogramnya, lalu ping www.github.com.
Pertama, kita perlu mendapatkan ID wadah dimulai pada langkah sebelumnya. Anda bisa lari docker psuntuk mendapatkannya.

Selanjutnya, mari kita jalankan shell di dalam wadah ubuntu itu, dengan menjalankan docker exec -it <CONTAINER ID> /bin/bash.
Selanjutnya, kita perlu menginstal program ping. Jadi, ayo lari apt-get update && apt-get install -y iputils-ping.
Mari ping www.github.com dengan menjalankan ping -c5 www.github.com
Akhirnya, mari kita lepaskan shell kita dari wadah, dengan menjalankan exit.
Kita juga harus menghentikan wadah ini sehingga kita membersihkan semuanya dari tes ini, dengan berlari docker stop
<CONTAINER ID>.

Ini menunjukkan bahwa kontainer baru dapat melakukan ping ke internet dan karenanya memiliki konfigurasi jaringan 
yang valid dan berfungsi.

Langkah 4: Konfigurasikan NAT untuk konektivitas eksternal
Mulai wadah baru berdasarkan gambar NGINX resmi dengan menjalankan docker run --name web1 -d -p 8080:80 nginx.
Tinjau status kontainer dan pemetaan port dengan menjalankan docker ps.

Jika karena alasan tertentu Anda tidak dapat membuka sesi dari web broswer, Anda dapat terhubung dari host Docker Anda 
menggunakan curl 127.0.0.1:8080perintah.

Bagian # 3 - Overlay Networking
Langkah 1: Dasar-Dasarnya
Pada langkah ini Anda akan menginisialisasi Swarm baru, bergabung dengan node pekerja tunggal, dan memverifikasi operasi 
berhasil.

Lari docker swarm init --advertise-addr $(hostname -i).
Di terminal pertama salin seluruh docker swarm join ...perintah yang ditampilkan sebagai bagian dari output dari output 
terminal Anda. Kemudian, rekatkan perintah yang disalin ke terminal kedua.
Nilai IDdan HOSTNAMEmungkin berbeda di lab Anda. Yang penting untuk diperiksa adalah bahwa kedua node telah bergabung 
dengan Swarm dan siap dan aktif .

Langkah 2: Buat jaringan overlay
Buat jaringan overlay baru yang disebut "overnet" dengan menjalankan docker network create -d overlay overnet.
Gunakan docker network lsperintah untuk memverifikasi bahwa jaringan berhasil dibuat.
Jaringan "overnet" yang baru ditunjukkan pada baris terakhir dari output di atas. Perhatikan bagaimana hal itu dikaitkan 
dengan driver overlay dan dicakup ke seluruh Swarm.

Jalankan docker network lsperintah yang sama dari terminal kedua.
Gunakan docker network inspect <network>perintah untuk melihat informasi lebih rinci tentang jaringan "overnet". Anda 
perlu menjalankan perintah ini dari terminal pertama.

Langkah 3: Buat layanan
Sekarang kami memiliki jaringan Swarm yang diinisialisasi dan overlay, saatnya untuk membuat layanan yang menggunakan
jaringan.
Jalankan perintah berikut dari terminal pertama untuk membuat layanan baru bernama myservice pada jaringan overnet 
dengan dua tugas / replika.
Verifikasi bahwa layanan dibuat dan kedua replika sudah berjalan docker service ls.
The 2/2dalam REPLICASmenunjukkan kolom yang kedua tugas di layanan yang berdiri dan berjalan.

Pastikan bahwa satu tugas (replika) berjalan di masing-masing dari dua node di Swarm dengan menjalankan docker service ps
myservice.
Nilai IDdan NODEmungkin berbeda dalam output Anda. Yang penting untuk diperhatikan adalah bahwa setiap tugas / replika berjalan pada node yang berbeda.
Sekarang simpul kedua menjalankan tugas pada jaringan "overnet", ia akan dapat melihat jaringan "overnet". Mari kita jalankan docker network lsdari terminal kedua untuk memverifikasi ini.
Kami juga dapat berjalan docker network inspect overnetdi terminal kedua untuk mendapatkan informasi lebih rinci tentang jaringan "overnet" dan mendapatkan alamat IP dari tugas yang berjalan di terminal kedua.
Anda harus mencatat bahwa pada Docker 1.12, docker network inspecthanya menampilkan wadah / tugas yang berjalan pada node lokal. Ini berarti bahwa 10.0.0.3ini adalah alamat IPv4 dari wadah yang berjalan pada node kedua. Catat alamat IP ini untuk langkah selanjutnya (alamat IP di lab Anda mungkin berbeda dari yang ditunjukkan di sini di panduan lab).

Langkah 4: Uji jaringan
Untuk menyelesaikan langkah ini, Anda memerlukan alamat IP dari tugas layanan yang berjalan pada node2 yang Anda lihat pada langkah sebelumnya ( 10.0.0.3).
Jalankan perintah berikut dari terminal pertama.
Perhatikan bahwa alamat IP yang tercantum untuk menjalankan tugas layanan (wadah) berbeda dengan alamat IP untuk tugas layanan yang berjalan pada node kedua. Perhatikan juga bahwa mereka berada di jaringan "overnet" yang sama.
Jalankan docker psperintah untuk mendapatkan ID tugas layanan sehingga Anda bisa masuk ke langkah berikutnya.

Masuk ke tugas layanan. Pastikan untuk menggunakan wadah IDdari lingkungan Anda karena akan berbeda dari contoh di bawah 
ini. Kita bisa melakukan ini dengan menjalankan docker exec -it <CONTAINER ID> /bin/bash.
Instal perintah ping dan ping tugas layanan yang berjalan pada node kedua di mana ia memiliki alamat IP 10.0.0.3dari docker network inspect overnetperintah.
Output di atas menunjukkan bahwa kedua tugas dari layanan myservice berada di jaringan overlay yang sama yang mencakup kedua node dan mereka dapat menggunakan jaringan ini untuk berkomunikasi.

Langkah 5: Uji penemuan layanan
Sekarang setelah Anda memiliki layanan yang berfungsi menggunakan jaringan overlay, mari kita uji penemuan layanan.
Jika Anda masih di dalam wadah, masuk kembali ke dalamnya dengan docker exec -it <CONTAINER ID> /bin/bashperintah.

Jalankan cat /etc/resolv.confformulir di dalam wadah.
Nilai yang kami minati adalah nameserver 127.0.0.11. Nilai ini mengirimkan semua permintaan DNS dari wadah ke resolver 
tertanam DNS yang berjalan di dalam wadah mendengarkan pada 127.0.0.11:53. Semua wadah Docker menjalankan server DNS 
tertanam di alamat ini.

Keluaran dengan jelas menunjukkan bahwa kontainer dapat melakukan ping myservicelayanan dengan nama. Perhatikan bahwa 
alamat IP yang dikembalikan adalah 10.0.0.2. Dalam beberapa langkah berikutnya kami akan memverifikasi bahwa alamat ini adalah IP virtual (VIP) yang ditetapkan untuk myservicelayanan ini.
Ketikkan exitperintah untuk meninggalkan execsesi kontainer dan kembali ke prompt shell host Docker Anda.

Periksa konfigurasi layanan "layanan saya" dengan menjalankan docker service inspect myservice. Mari kita verifikasi 
bahwa nilai VIP cocok dengan nilai yang dikembalikan oleh ping -c5 myserviceperintah sebelumnya .
Ke bagian bawah output, Anda akan melihat VIP dari layanan yang terdaftar. VIP di output di atas adalah 10.0.0.2tetapi 
nilainya mungkin berbeda dalam pengaturan Anda. Poin penting yang perlu diperhatikan adalah bahwa VIP yang tercantum di 
sini cocok dengan nilai yang dikembalikan oleh ping -c5 myserviceperintah.

Jangan ragu untuk membuat docker execsesi baru untuk tugas layanan (wadah) yang berjalan pada node2 dan melakukan 
ping -c5 serviceperintah yang sama . Anda akan mendapatkan respons dari VIP yang sama.

membersihkan
alankan docker service rm myserviceperintah untuk menghapus layanan yang disebut MyService .
Jalankan docker psperintah untuk mendapatkan daftar kontainer yang berjalan.
