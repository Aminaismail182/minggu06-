Bagian 1: Apa itu Orkestrasi
orkestrasi mungkin paling baik digambarkan menggunakan contoh. Katakanlah Anda memiliki aplikasi yang memiliki lalu 
lintas tinggi bersama dengan persyaratan ketersediaan tinggi. Karena persyaratan ini, Anda biasanya ingin menggunakan
setidaknya 3+ mesin, sehingga jika tuan rumah gagal, aplikasi Anda masih dapat diakses dari setidaknya dua lainnya. Jelas,
ini hanya sebuah contoh dan kasus penggunaan Anda kemungkinan akan memiliki persyaratan sendiri, tetapi Anda mendapatkan 
idenya.

Menyebarkan aplikasi Anda tanpa Orkestrasi biasanya sangat memakan waktu dan rawan kesalahan, karena Anda harus secara
manual SSH ke setiap mesin, mulai aplikasi Anda, dan kemudian terus mengawasi hal-hal untuk memastikan itu berjalan seperti
yang Anda harapkan.

Tetapi, dengan Perkakas orkestrasi, Anda biasanya dapat melepas sebagian besar pekerjaan manual ini dan membiarkan otomatisasi
melakukan pekerjaan berat. Salah satu fitur keren Orkestrasi dengan Docker Swarm, adalah Anda dapat menggunakan aplikasi 
di banyak host dengan hanya satu perintah (setelah mode Swarm diaktifkan). Plus, jika salah satu node pendukung mati di
Docker Swarm Anda, node lain akan secara otomatis mengambil beban, dan aplikasi Anda akan terus bersenandung seperti biasa.

Jika Anda biasanya hanya menggunakan docker rununtuk menyebarkan aplikasi Anda, maka Anda mungkin benar-benar mendapat 
manfaat dari menggunakan Docker Compose, mode Docker Swarm, atau keduanya Docker Compose dan Swarm.

Bagian 2: Konfigurasikan Mode Gerombolan
Aplikasi dunia nyata biasanya digunakan di beberapa host seperti yang dibahas sebelumnya. Ini meningkatkan kinerja 
aplikasi dan ketersediaan, serta memungkinkan komponen aplikasi individu untuk skala secara mandiri. Docker memiliki alat
asli yang tangguh untuk membantu Anda melakukan ini.

Contoh menjalankan berbagai hal secara manual dan pada satu host adalah membuat wadah baru pada node1 dengan menjalankan 
docker run -dt ubuntu sleep infinity.

Perintah ini akan membuat wadah baru berdasarkan ubuntu:latestgambar dan akan menjalankan sleepperintah untuk menjaga
wadah berjalan di latar belakang. Anda dapat memverifikasi wadah contoh kami sudah habis dengan berjalan docker psdi
node1 .

Tapi, ini hanya pada satu node. Apa yang terjadi jika simpul ini turun? Nah, aplikasi kita baru saja mati dan tidak pernah restart. Untuk memulihkan layanan, kita harus masuk secara manual ke mesin ini, dan mulai mengubah hal-hal untuk membuatnya kembali dan berjalan. Jadi, akan sangat membantu jika kita memiliki beberapa jenis sistem yang memungkinkan kita untuk menjalankan aplikasi / layanan "tidur" ini di banyak mesin.

Di bagian ini Anda akan mengkonfigurasi Mode Swarm . Ini adalah mode opsional baru di mana beberapa host Docker membentuk kelompok mesin yang mengatur sendiri disebut swarm . Mode Swarm memungkinkan fitur baru seperti layanan dan bundel yang membantu Anda menyebarkan dan mengelola aplikasi multi-kontainer di beberapa host Docker.

Anda akan menyelesaikan yang berikut:

Konfigurasikan mode Swarm
Jalankan aplikasi
Skala aplikasi
Kuras node untuk perawatan dan penjadwalan ulang kontainer
Untuk sisa lab ini kita akan merujuk ke pengelompokan asli Docker sebagai mode Swarm . Koleksi mesin Docker yang 
dikonfigurasi untuk mode Swarm akan disebut sebagai swarm .

Segerombolan terdiri dari satu atau lebih Node Manajer dan satu atau lebih Node Pekerja . Node manajer menjaga kondisi 
gerombolan dan menjadwalkan wadah aplikasi. Node pekerja menjalankan wadah aplikasi. Pada Docker 1.12, tidak ada backend 
eksternal, atau komponen pihak ke-3, yang diperlukan untuk segerombolan yang berfungsi penuh - semuanya built-in!

Di bagian demo ini Anda akan menggunakan ketiga node di lab Anda. node1 akan menjadi manajer Swarm, sedangkan node2 dan 
node3 akan menjadi node pekerja. Mode Swarm mendukung node manajer redundan yang sangat tersedia, tetapi untuk keperluan 
lab ini Anda hanya akan menggunakan node manajer tunggal.

Langkah 2.1 - Buat simpul Manajer
Pada langkah ini Anda akan menginisialisasi Swarm baru, bergabung dengan node pekerja tunggal, dan memverifikasi operasi 
berhasil.

Jalankan docker swarm initpada node1 .
Anda dapat menjalankan docker infoperintah untuk memverifikasi bahwa node1 berhasil dikonfigurasi sebagai node swarm 
manager.

Kawanan sekarang diinisialisasi dengan simpul1 sebagai satu-satunya simpul Manajer. Di bagian selanjutnya Anda akan menambahkan
node2 dan node3 sebagai node Worker .

Langkah 2.2 - Bergabung dengan node Pekerja ke Swarm
Anda akan melakukan prosedur berikut pada node2 dan node3 . Menjelang akhir prosedur, Anda akan beralih kembali ke node1 .
Sekarang, ambil seluruh docker swarm join ...perintah yang kita salin sebelumnya dari node1tempat itu ditampilkan sebagai 
terminal output. Kita perlu menempelkan perintah yang disalin ke terminal node2 dan node3 .
Seharusnya terlihat seperti ini untuk node2 . Ngomong-ngomong, jika docker swarm join ...perintah sudah bergulir dari 
layar Anda, Anda dapat menjalankan docker swarm join-token workerperintah pada simpul Manajer untuk mendapatkannya lagi.

Setelah Anda menjalankan ini pada node2 dan node3 , beralih kembali ke node1 , dan jalankan a docker node lsuntuk 
memverifikasi bahwa kedua node adalah bagian dari Swarm. Anda akan melihat tiga simpul, simpul 1 sebagai simpul Manajer 
dan simpul2 dan simpul3 sebagai simpul Pekerja.

The docker node lsperintah menunjukkan Anda semua node yang berada di swarm serta peran mereka dalam swarm. The
*mengidentifikasi node yang Anda mengeluarkan perintah dari.

Bagian 3: Menyebarkan aplikasi di beberapa host
Sekarang setelah Anda memiliki swarm up dan running, sekarang saatnya untuk menggunakan aplikasi tidur kami yang sangat sederhana.
Anda akan melakukan prosedur berikut dari node1 .

Langkah 3.1 - Menyebarkan komponen aplikasi sebagai layanan Docker
Kami sleepaplikasi menjadi sangat populer di internet (karena memukul Reddit dan HN). Orang suka itu. Jadi, Anda harus 
mengukur aplikasi Anda untuk memenuhi permintaan puncak. Anda harus melakukan ini di beberapa host untuk ketersediaan tinggi juga. Kami akan menggunakan konsep Layanan untuk mengukur aplikasi kami dengan mudah dan mengelola banyak wadah sebagai satu kesatuan.

Anda akan melakukan prosedur ini dari node1 .
Mari kita sebarkan sleepsebagai Layanan di Docker Swarm.
Verifikasi bahwa service createtelah diterima oleh manajer Swarm.
Status layanan dapat berubah beberapa kali hingga berjalan. Gambar sedang diunduh dari Docker Store ke mesin lain di Swarm.
Setelah gambar diunduh, wadah masuk ke keadaan berjalan di salah satu dari tiga node.

Pada titik ini mungkin tidak tampak bahwa kami telah melakukan sesuatu yang sangat berbeda dari hanya menjalankan a docker
run .... Kami sekali lagi menggunakan satu wadah pada satu host. Perbedaannya di sini adalah bahwa wadah telah dijadwalkan pada gugus gerombolan.

Bagian 4: Skala aplikasi
Permintaannya gila! Semua orang menyukai sleepaplikasi Anda ! Sudah waktunya untuk meningkatkan skala.
Salah satu hal hebat tentang layanan adalah Anda dapat menaikkan dan menurunkannya untuk memenuhi permintaan. Pada langkah
ini Anda akan meningkatkan skala layanan dan kemudian kembali.

Anda akan melakukan prosedur berikut dari node1 .
Skala jumlah kontainer dalam layanan aplikasi-tidur ke 7 dengan docker service update --replicas 7 sleep-appperintah. replicasadalah
istilah yang kami gunakan untuk menggambarkan wadah identik yang menyediakan layanan yang sama.

Manajer Swarm menjadwalkan sehingga ada 7 sleep-appkontainer di cluster. Ini akan dijadwalkan secara merata di seluruh anggota Swarm.
Kami akan menggunakan docker service ps sleep-appperintah. Jika Anda melakukan ini cukup cepat setelah menggunakan --replicasopsi, Anda dapat melihat wadah muncul secara real time.

Perhatikan bahwa sekarang ada 7 kontainer yang terdaftar. Mungkin diperlukan beberapa detik untuk wadah baru dalam layanan untuk semua ditampilkan sebagai MENJALANKAN . The NODEKolom memberitahu kita di mana simpul wadah sedang berjalan.
Skala layanan kembali menjadi hanya empat kontainer dengan docker service update --replicas 4 sleep-appperintah.
Verifikasi bahwa jumlah kontainer telah dikurangi menjadi 4 menggunakan docker service ps sleep-appperintah.

Bagian 5: Kuras simpul dan jadwalkan ulang wadah
Aplikasi tidur Anda telah melakukan yang luar biasa setelah mencapai Reddit dan HN. Sekarang nomor 1 di App Store! Anda 
telah meningkatkan selama liburan dan turun selama musim yang lambat. Sekarang Anda sedang melakukan pemeliharaan di salah satu server Anda sehingga Anda harus dengan anggun mengeluarkan server dari kerumunan tanpa mengganggu layanan kepada pelanggan Anda.

Lihatlah status node lagi dengan menjalankan docker node lspada node1 .
Anda dapat melihat bahwa kami memiliki salah satu wadah aplikasi slepp berjalan di sini (output Anda mungkin terlihat berbeda).
Sekarang mari kita kembali ke node1 (manajer Swarm) dan mengambil node2 dari layanan. Untuk melakukannya, mari kita jalankan docker node lslagi.
Kita akan mengambil ID untuk node2 dan menjalankannya docker node update --availability drain yournodeid. Kami menggunakan ID host simpul2 sebagai input ke dalam perintah kami . Ganti yournodeid dengan id dari node2 .drain

Periksa status node
Node simpul2 sekarang dalam Drainkeadaan.
Beralih kembali ke node2 dan lihat apa yang sedang berjalan di sana dengan menjalankan docker ps.

node2 tidak memiliki kontainer yang berjalan di atasnya.
Terakhir, periksa lagi layanan pada node1 untuk memastikan bahwa kontainer dijadwal ulang. Anda harus melihat keempat kontainer berjalan di dua node yang tersisa.

Membersihkan
Jalankan docker service rm sleep-appperintah pada node1 untuk menghapus layanan yang disebut myservice .
Jalankan docker psperintah pada node1 untuk mendapatkan daftar kontainer yang berjalan.
Anda dapat menggunakan docker kill <CONTAINER ID>perintah pada node1 untuk membunuh wadah tidur yang kita mulai di awal.
Akhirnya, mari kita hapus node1, node2, dan node3 dari Swarm. Kita dapat menggunakan docker swarm leave --forceperintah untuk melakukan itu.

Mari kita jalankan docker swarm leave --forcedi node1 .
Kemudian, jalankan docker swarm leave --forcedi node2 
Akhirnya, jalankan docker swarm leave --forcedi node3 .
