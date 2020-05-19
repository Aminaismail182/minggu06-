Klon Repo GitHub Lab
Gunakan perintah berikut untuk mengkloning repo lab dari GitHub (Anda dapat mengklik perintah atau mengetiknya 
secara manual). Ini akan membuat salinan repo lab di sub-direktori baru bernama linux_tweet_app.

    git clone https://github.com/dockersamples/linux_tweet_app
Pastikan Anda memiliki DockerID
Jika Anda tidak memiliki DockerID (login gratis yang digunakan untuk mengakses Docker Hub), silakan kunjungi Docker 
Hub dan mendaftar untuk itu. Anda akan membutuhkan ini untuk langkah selanjutnya.

Tugas 1: Jalankan beberapa wadah Docker sederhana
Ada berbagai cara untuk menggunakan wadah. Ini termasuk:

Untuk menjalankan satu tugas: Ini bisa berupa skrip shell atau aplikasi khusus.
Interaktif: Ini menghubungkan Anda ke wadah yang mirip dengan cara Anda SSH ke server jauh.
Di latar belakang: Untuk layanan jangka panjang seperti situs web dan basis data.
Di bagian ini Anda akan mencoba masing-masing opsi tersebut dan melihat bagaimana Docker mengelola beban kerja.

Jalankan satu tugas dalam wadah Alpine Linux
Pada langkah ini kita akan memulai sebuah wadah baru dan memerintahkannya untuk menjalankan hostnameperintah. 
Wadah akan mulai, jalankan hostnameperintah, lalu keluar.

Jalankan perintah berikut di konsol Linux Anda.

 docker container run alpine hostname
Output di bawah ini menunjukkan bahwa alpine:latestgambar tidak dapat ditemukan secara lokal. Ketika ini terjadi,
 Docker secara otomatis menariknya dari Docker Hub.

Setelah gambar ditarik, nama host penampung ditampilkan ( 888e89a3b36bdalam contoh di bawah).

 Unable to find image 'alpine:latest' locally
 latest: Pulling from library/alpine
 88286f41530e: Pull complete
 Digest: sha256:f006ecbb824d87947d0b51ab8488634bf69fe4094959d935c0c103f4820a417d
 Status: Downloaded newer image for alpine:latest
 888e89a3b36b
Docker menjaga wadah berjalan selama proses itu dimulai di dalam wadah masih berjalan. Dalam hal ini hostnameproses keluar segera setelah output ditulis. Ini artinya wadah berhenti. Namun, Docker tidak menghapus sumber daya secara default, sehingga wadah masih ada di Exitednegara.

Daftar semua wadah.

 docker container ls --all
Perhatikan bahwa wadah Alpine Linux Anda berada di Exitednegara bagian.

 CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS            PORTS               NAMES
 888e89a3b36b        alpine              "hostname"          50 seconds ago      Exited (0) 49 seconds ago                       awesome_elion
Catatan: ID wadah adalah nama host yang ditampilkan oleh wadah tersebut. Pada contoh di atas itu 888e89a3b36b.

Wadah yang melakukan satu tugas dan kemudian keluar bisa sangat berguna. Anda bisa membuat gambar Docker yang mengeksekusi skrip untuk mengkonfigurasi sesuatu. Siapa pun dapat menjalankan tugas itu hanya dengan menjalankan wadah - mereka tidak memerlukan skrip atau informasi konfigurasi yang sebenarnya.

Jalankan wadah Ubuntu interaktif
Anda dapat menjalankan sebuah wadah berdasarkan versi Linux yang berbeda dari yang dijalankan pada host Docker Anda.

Pada contoh berikut, kita akan menjalankan wadah Ubuntu Linux di atas host Alpine Linux Docker (Play With Docker
 menggunakan Alpine Linux untuk node-node-nya).

Jalankan wadah Docker dan akses cangkangnya.

 docker container run --interactive --tty --rm ubuntu bash
Dalam contoh ini, kami memberikan Docker tiga parameter:

--interactive mengatakan Anda ingin sesi interaktif.
--tty mengalokasikan pseudo-tty.
--rm memberitahu Docker untuk terus maju dan menghapus wadah ketika sudah selesai mengeksekusi.
Dua parameter pertama memungkinkan Anda untuk berinteraksi dengan wadah Docker.

Kami juga memberi tahu kontainer untuk menjalankannya bashsebagai proses utamanya (PID 1).

Saat wadah mulai Anda akan jatuh ke bash shell dengan prompt default root@<container id>:/#. Docker telah menempel
 pada shell di wadah, menyampaikan input dan output antara sesi lokal Anda dan sesi shell di wadah.

Jalankan perintah berikut dalam wadah.

ls /akan mencantumkan isi direktur root dalam wadah, ps auxakan menunjukkan proses yang berjalan dalam wadah, cat /etc/issueakan menunjukkan distro Linux mana wadah berjalan, dalam hal ini Ubuntu 18.04.3 LTS.

ls /
ps aux
cat /etc/issue
Ketik exituntuk meninggalkan sesi shell. Ini akan menghentikan bashproses, menyebabkan wadah untuk keluar.

 exit
Catatan: Saat kami menggunakan --rmbendera saat memulai wadah, Docker menghapus wadah saat berhenti. 
Ini berarti jika Anda menjalankan yang lain docker container ls --allAnda tidak akan melihat wadah Ubuntu.

Untuk bersenang-senang, mari kita periksa versi host VM kami.

 cat /etc/issue
Anda harus melihat:

 Welcome to Alpine Linux 3.8
 Kernel \r on an \m (\l)
Perhatikan bahwa host VM kami menjalankan Alpine Linux, namun kami dapat menjalankan wadah Ubuntu. 
Seperti disebutkan sebelumnya, distribusi Linux di dalam wadah tidak perlu cocok dengan distribusi Linux 
yang berjalan di host Docker.

Namun, wadah Linux mengharuskan host Docker menjalankan kernel Linux. Misalnya, wadah Linux tidak dapat berjalan 
langsung di host Windows Docker. Hal yang sama berlaku untuk kontainer Windows - mereka harus dijalankan pada host
 Docker dengan kernel Windows.

Wadah interaktif berguna ketika Anda menyusun gambar Anda sendiri. Anda dapat menjalankan sebuah wadah dan
 memverifikasi semua langkah yang Anda butuhkan untuk menggunakan aplikasi Anda, dan menangkapnya di Dockerfile.

Anda dapat melakukan wadah untuk membuat gambar dari itu - tetapi Anda harus menghindari itu sedapat mungkin. 
Jauh lebih baik menggunakan Dockerfile berulang untuk membangun gambar Anda. Anda akan segera melihatnya.

Jalankan latar belakang wadah MySQL
Kontainer latar belakang adalah cara Anda menjalankan sebagian besar aplikasi. Berikut ini contoh sederhana 
menggunakan MySQL.

Jalankan wadah MySQL baru dengan perintah berikut.

 docker container run \
 --detach \
 --name mydb \
 -e MYSQL_ROOT_PASSWORD=my-secret-pw \
 mysql:latest
--detach akan menjalankan wadah di latar belakang.
--nameakan menamainya mydb .
-e akan menggunakan variabel lingkungan untuk menentukan kata sandi root (CATATAN: Ini tidak boleh dilakukan 
dalam produksi).
Karena gambar MySQL tidak tersedia secara lokal, Docker secara otomatis menariknya dari Docker Hub.

 Unable to find image 'mysql:latest' locallylatest: Pulling from library/mysql
 aa18ad1a0d33: Pull complete
 fdb8d83dece3: Pull complete
 75b6ce7b50d3: Pull complete
 ed1d0a3a64e4: Pull complete
 8eb36a82c85b: Pull complete
 41be6f1a1c40: Pull complete
 0e1b414eac71: Pull complete
 914c28654a91: Pull complete
 587693eb988c: Pull complete
 b183c3585729: Pull complete
 315e21657aa4: Pull complete
 Digest: sha256:0dc3dacb751ef46a6647234abdec2d47400f0dfbe77ab490b02bffdae57846ed
 Status: Downloaded newer image for mysql:latest
 41d6157c9f7d1529a6c922acb8167ca66f167119df0fe3d86964db6c0d7ba4e0
Selama proses MySQL berjalan, Docker akan menjaga kontainer tetap berjalan di latar belakang.

Daftar wadah yang sedang berjalan.

 docker container ls
Perhatikan wadah Anda sedang berjalan.

 CONTAINER ID        IMAGE               COMMAND                  CREATED             STATUS              PORTS            NAMES
 3f4e8da0caf7        mysql:latest        "docker-entrypoint..."   52 seconds ago      Up 51 seconds       3306/tcp            mydb
Anda dapat memeriksa apa yang terjadi di wadah Anda dengan menggunakan beberapa perintah Docker bawaan: docker container logsdan docker container top.

 docker container logs mydb
Ini menunjukkan log dari wadah MySQL Docker.

   <output truncated>
   2017-09-29T16:02:58.605004Z 0 [Note] Executing 'SELECT * FROM INFORMATION_SCHEMA.TABLES;' to get a list of tables using the deprecated partition engine. You may use the startup option '--disable-partition-engine-check' to skip this check.
   2017-09-29T16:02:58.605026Z 0 [Note] Beginning of list of non-natively partitioned tables
   2017-09-29T16:02:58.616575Z 0 [Note] End of list of non-natively partitioned tables
Mari kita lihat proses yang berjalan di dalam wadah.

   docker container top mydb
Anda seharusnya melihat daemon MySQL ( mysqld) berjalan dalam wadah.

 PID                 USER                TIME                COMMAND
 2876                999                 0:00                mysqld
Meskipun MySQL sedang berjalan, ia terisolasi di dalam wadah karena tidak ada port jaringan yang telah dipublikasikan
 ke host. Lalu lintas jaringan tidak dapat mencapai kontainer dari host kecuali port diterbitkan secara eksplisit.

Daftar versi MySQL menggunakan docker container exec.

docker container execmemungkinkan Anda untuk menjalankan perintah di dalam sebuah wadah. Dalam contoh ini, kita akan 
gunakan docker container execuntuk menjalankan setara baris perintah mysql --user=root --password=$MYSQL_ROOT_PASSWORD --versiondi dalam wadah MySQL kita.

 docker exec -it mydb \
 mysql --user=root --password=$MYSQL_ROOT_PASSWORD --version
Anda akan melihat nomor versi MySQL, serta peringatan praktis.

 mysql: [Warning] Using a password on the command line interface can be insecure.
 mysql  Ver 14.14 Distrib 5.7.19, for Linux (x86_64) using  EditLine wrapper
Anda juga dapat menggunakan docker container execuntuk terhubung ke proses shell baru di dalam wadah yang sudah
 berjalan. Menjalankan perintah di bawah ini akan memberi Anda shell interaktif ( sh) di dalam wadah MySQL Anda.

 docker exec -it mydb sh
Perhatikan bahwa prompt shell Anda telah berubah. Ini karena cangkang Anda sekarang terhubung ke shproses yang 
berjalan di dalam wadah Anda.

Mari kita periksa nomor versi dengan menjalankan perintah yang sama lagi, hanya kali ini dari dalam sesi shell baru di 
wadah.

 mysql --user=root --password=$MYSQL_ROOT_PASSWORD --version
Perhatikan outputnya sama seperti sebelumnya.

Ketik exituntuk meninggalkan sesi shell interaktif.

 exit
Tugas 2: Paket dan menjalankan aplikasi khusus menggunakan Docker
Pada langkah ini Anda akan belajar cara mengemas aplikasi Anda sendiri sebagai gambar Docker menggunakan Dockerfile .

Sintaks Dockerfile sangat mudah. Dalam tugas ini, kita akan membuat situs web NGINX sederhana dari Dockerfile.

Bangun gambar situs web sederhana
Mari kita lihat Dockerfile yang akan kita gunakan, yang membangun situs web sederhana yang memungkinkan Anda
 mengirim tweet.

Pastikan Anda berada di linux_tweet_appdirektori.

 cd ~/linux_tweet_app
Tampilkan konten Dockerfile.

 cat Dockerfile
 FROM nginx:latest

 COPY index.html /usr/share/nginx/html
 COPY linux.png /usr/share/nginx/html

 EXPOSE 80 443     

 CMD ["nginx", "-g", "daemon off;"]
Mari kita lihat apa yang dilakukan masing-masing baris ini di Dockerfile.

FROM menentukan gambar dasar untuk digunakan sebagai titik awal untuk gambar baru yang Anda buat ini. Untuk contoh ini 
kita mulai dari nginx:latest.
SALIN menyalin file dari host Docker ke dalam gambar, di lokasi yang diketahui. Dalam contoh ini, COPYdigunakan untuk
 menyalin dua file ke dalam gambar: index.html. dan grafik yang akan digunakan di halaman web kami.
TAMPILKAN dokumen yang port aplikasi gunakan.
CMD menentukan perintah apa yang harus dijalankan ketika sebuah wadah dimulai dari gambar. Perhatikan bahwa kita dapat 
menentukan perintah, serta argumen run-time.
Untuk membuat perintah berikut ini lebih ramah copy / paste, ekspor variabel lingkungan yang berisi DockerID Anda 
(jika Anda tidak memiliki DockerID, Anda bisa mendapatkannya secara gratis melalui Docker Hub ).

Anda harus mengetikkan perintah ini secara manual karena membutuhkan DockerID unik Anda.

export DOCKERID=<your docker id>

Gema nilai variabel kembali ke terminal untuk memastikan itu disimpan dengan benar.

 echo $DOCKERID
Gunakan docker image buildperintah untuk membuat gambar Docker baru menggunakan instruksi di Dockerfile.

--tagmemungkinkan kita untuk memberi gambar nama khusus. Dalam hal ini terdiri dari DockerID kami, nama aplikasi, 
dan versi. Memiliki ID Docker yang terlampir pada nama akan memungkinkan kami untuk menyimpannya di Docker Hub 
di langkah selanjutnya
. memberitahu Docker untuk menggunakan direktori saat ini sebagai konteks pembangunan
Pastikan untuk memasukkan titik ( .) di akhir perintah.

 docker image build --tag $DOCKERID/linux_tweet_app:1.0 .
Output di bawah ini menunjukkan daemon Docker yang mengeksekusi setiap baris di Dockerfile

 Sending build context to Docker daemon  32.77kB
 Step 1/5 : FROM nginx:latest
 latest: Pulling from library/nginx
 afeb2bfd31c0: Pull complete
 7ff5d10493db: Pull complete
 d2562f1ae1d0: Pull complete
 Digest: sha256:af32e714a9cc3157157374e68c818b05ebe9e0737aac06b55a09da374209a8f9
 Status: Downloaded newer image for nginx:latest
 ---> da5939581ac8
 Step 2/5 : COPY index.html /usr/share/nginx/html
 ---> eba2eec2bea9
 Step 3/5 : COPY linux.png /usr/share/nginx/html
 ---> 4d080f499b53
 Step 4/5 : EXPOSE 80 443
 ---> Running in 47232cb5699f
 ---> 74c968a9165f
 Removing intermediate container 47232cb5699f
 Step 5/5 : CMD nginx -g daemon off;
 ---> Running in 4623761274ac
 ---> 12045a0df899
 Removing intermediate container 4623761274ac
 Successfully built 12045a0df899
 Successfully tagged <your docker ID>/linux_tweet_app:latest
Gunakan docker container runperintah untuk memulai wadah baru dari gambar yang Anda buat.

Karena kontainer ini akan menjalankan server web NGINX, kami akan menggunakan --publishflag untuk 
menerbitkan port 80 di dalam container ke port 80 pada host. Ini akan memungkinkan lalu lintas masuk ke host 
Docker pada port 80 untuk diarahkan ke port 80 dalam wadah. Format --publishbendera adalah host_port: container_port.

 docker container run \
 --detach \
 --publish 80:80 \
 --name linux_tweet_app \
 $DOCKERID/linux_tweet_app:1.0
Setiap lalu lintas eksternal yang masuk ke server pada port 80 sekarang akan diarahkan ke wadah pada port 80.

Pada langkah selanjutnya Anda akan melihat cara memetakan lalu lintas dari dua port yang berbeda - 
ini diperlukan ketika dua kontainer menggunakan port yang sama untuk berkomunikasi karena Anda hanya dapat mengekspos
 port tersebut satu kali pada host.

Klik di sini untuk memuat situs web yang seharusnya berjalan.

Setelah Anda mengakses situs web Anda, matikan dan hapus.

 docker container rm --force linux_tweet_app
Catatan: Kami menggunakan --forceparameter untuk menghapus wadah yang sedang berjalan tanpa mematikannya. 
Ini tidak akan mematikan wadah dan menghapusnya secara permanen dari host Docker.

Dalam lingkungan produksi, Anda mungkin ingin menggunakannya docker container stopuntuk menghentikan wadah 
dengan anggun dan membiarkannya di host. Anda kemudian dapat menggunakannya docker container rmuntuk menghapusnya 
secara permanen.

Tugas 3: Memodifikasi situs web yang sedang berjalan
Ketika Anda secara aktif mengerjakan aplikasi, tidak nyaman harus menghentikan wadah, membangun kembali gambar, 
dan menjalankan versi baru setiap kali Anda membuat perubahan pada kode sumber Anda.

Salah satu cara untuk merampingkan proses ini adalah dengan memasang direktori kode sumber pada mesin lokal
 ke wadah yang sedang berjalan. Ini akan memungkinkan setiap perubahan yang dilakukan pada file pada host segera
  tercermin dalam wadah.

Kami melakukan ini menggunakan sesuatu yang disebut bind mount .

Saat Anda menggunakan bind mount, file atau direktori pada mesin host dipasang ke wadah yang berjalan di host yang sama.

Mulai aplikasi web kami dengan bind mount
Mari mulai aplikasi web dan pasang direktori saat ini ke dalam wadah.

Dalam contoh ini kita akan menggunakan --mountbendera untuk memasang direktori saat ini di host ke /usr/share/nginx/htmlda
am wadah.

Pastikan untuk menjalankan perintah ini dari dalam linux_tweet_appdirektori pada host Docker Anda.

 docker container run \
 --detach \
 --publish 80:80 \
 --name linux_tweet_app \
 --mount type=bind,source="$(pwd)",target=/usr/share/nginx/html \
 $DOCKERID/linux_tweet_app:1.0
Ingat dari Dockerfile, usr/share/nginx/htmladalah tempat file html disimpan untuk aplikasi web.

Situs web harus dijalankan.

Ubah situs web yang berjalan
Bind mounts berarti bahwa setiap perubahan yang dilakukan pada sistem file lokal segera tercermin dalam wadah yang berjalan.

Salin yang baru index.htmlke dalam wadah.

Repo Git yang Anda tarik sebelumnya berisi beberapa versi file index.html yang berbeda. Anda dapat secara manual 
menjalankan lsperintah dari dalam ~/linux_tweet_appdirektori untuk melihat daftar mereka. Pada langkah ini kita akan 
menggantinya index.htmldengan index-new.html.

 cp index-new.html index.html
Buka situs web yang sedang berjalan dan segarkan halaman . Perhatikan bahwa situs telah berubah.

Jika Anda merasa nyaman dengan itu, viAnda dapat menggunakannya untuk memuat index.htmlfile lokal dan membuat perubahan 
tambahan. Itu juga akan tercermin ketika Anda memuat ulang halaman web. Jika Anda benar-benar berjiwa petualang, 
mengapa tidak mencoba menggunakan execuntuk mengakses wadah yang sedang berjalan dan memodifikasi file yang disimpan 
di sana.

Meskipun kami telah memodifikasi index.htmlsistem file lokal dan melihatnya tercermin dalam wadah yang sedang berjalan, 
kami belum benar-benar mengubah gambar Docker tempat wadah itu dimulai.

Untuk menunjukkan ini, hentikan wadah saat ini dan jalankan kembali 1.0gambar tanpa bind mount.

Hentikan dan hapus wadah yang sedang berjalan.

 docker rm --force linux_tweet_app
Jalankan kembali versi saat ini tanpa bind mount.

 docker container run \
 --detach \
 --publish 80:80 \
 --name linux_tweet_app \
 $DOCKERID/linux_tweet_app:1.0
Perhatikan situs web kembali ke versi aslinya.

Berhenti dan lepaskan wadah saat ini

docker rm --force linux_tweet_app
Perbarui gambar
Untuk mempertahankan perubahan yang Anda buat pada index.htmlfile ke dalam gambar, Anda perlu membuat versi 
gambar yang baru.

Bangun gambar baru dan beri tag sebagai 2.0

Ingatlah bahwa Anda sebelumnya memodifikasi index.htmlfile pada Docker host sistem file lokal. Ini berarti 
menjalankan docker image buildperintah lain akan membangun gambar baru dengan pembaruanindex.html

Pastikan untuk memasukkan titik ( .) di akhir perintah.

 docker image build --tag $DOCKERID/linux_tweet_app:2.0 .
Perhatikan seberapa cepat itu dibangun! Ini karena Docker hanya mengubah bagian gambar yang berubah vs membangun
 kembali seluruh gambar.

Mari kita lihat gambar pada sistem.

 docker image ls
Sekarang Anda memiliki kedua versi aplikasi web di host Anda.

 REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
 <docker id>/linux_tweet_app    2.0                 01612e05312b        16 seconds ago      108MB
 <docker id>/linux_tweet_app    1.0                 bb32b5783cd3        4 minutes ago       108MB
 mysql                          latest              b4e78b89bcf3        2 weeks ago         412MB
 ubuntu                         latest              2d696327ab2e        2 weeks ago         122MB
 nginx                          latest              da5939581ac8        3 weeks ago         108MB
 alpine                         latest              76da55c8019d        3 weeks ago         3.97MB
Uji versi baru
Jalankan wadah baru dari versi gambar yang baru.

 docker container run \
 --detach \
 --publish 80:80 \
 --name linux_tweet_app \
 $DOCKERID/linux_tweet_app:2.0
Periksa versi baru situs web ( Anda mungkin perlu menyegarkan peramban untuk mendapatkan versi baru untuk dimuat ).

Halaman web akan memiliki latar belakang oranye.

Kita dapat menjalankan kedua versi secara berdampingan. Satu-satunya hal yang perlu kita waspadai adalah bahwa kita
 tidak dapat memiliki dua kontainer menggunakan port 80 pada host yang sama.

Karena kita sudah menggunakan port 80 untuk wadah yang berjalan dari 2.0versi gambar, kita akan memulai wadah baru dan
 menerbitkannya di port 8080. Selain itu, kita perlu memberi nama unik wadah kita ( old_linux_tweet_app)

Jalankan wadah baru lain, kali ini dari versi lama gambar.

Perhatikan bahwa perintah ini memetakan wadah baru ke port 8080 pada host. Ini karena dua kontainer tidak dapat 
memetakan ke port yang sama pada satu host Docker.

 docker container run \
 --detach \
 --publish 8080:80 \
 --name old_linux_tweet_app \
 $DOCKERID/linux_tweet_app:1.0
Lihat versi lama situs web .

Dorong gambar Anda ke Docker Hub
Daftar gambar pada host Docker Anda.

 docker image ls -f reference="$DOCKERID/*"
Anda akan melihat bahwa Anda sekarang memiliki dua linux_tweet_appgambar - satu ditandai sebagai 1.0dan yang 
lainnya sebagai 2.0.

 REPOSITORY                     TAG                 IMAGE ID            CREATED             SIZE
 <docker id>/linux_tweet_app    2.0                 01612e05312b        3 minutes ago       108MB
 <docker id>/linux_tweet_app    1.0                 bb32b5783cd3        7 minutes ago       108MB
Gambar-gambar ini hanya disimpan di repositori lokal host Docker Anda. Host Docker Anda akan dihapus setelah lokakarya. 
Pada langkah ini kami akan mendorong gambar ke repositori publik sehingga Anda dapat menjalankannya dari mesin Linux 
dengan Docker.

Distribusi dibangun ke platform Docker. Anda dapat membuat gambar secara lokal dan mendorongnya ke registri publik
 atau pribadi , menjadikannya tersedia untuk pengguna lain. Siapa pun yang memiliki akses dapat menarik gambar itu dan 
 menjalankan wadah darinya. Perilaku aplikasi dalam wadah akan sama untuk semua orang, karena gambar berisi aplikasi 
 yang sepenuhnya dikonfigurasi - satu-satunya persyaratan untuk menjalankannya adalah Linux dan Docker.

Docker Hub adalah registri publik default untuk gambar Docker.

Sebelum Anda dapat mendorong gambar Anda, Anda harus masuk ke Docker Hub.

 docker login
Anda harus memberikan kredensial ID Docker Anda saat diminta.

 Username: <your docker id>
 Password: <your docker id password>
 Login Succeeded
Dorong versi 1.0aplikasi web Anda menggunakan docker image push.

 docker image push $DOCKERID/linux_tweet_app:1.0
Anda akan melihat progresnya ketika gambar didorong ke Docker Hub.

 The push refers to a repository [docker.io/<your docker id>/linux_tweet_app]
 910e84bcef7a: Pushed
 1dee161c8ba4: Pushed
 110566462efa: Pushed
 305e2b6ef454: Pushed
 24e065a5f328: Pushed
 1.0: digest: sha256:51e937ec18c7757879722f15fa1044cbfbf2f6b7eaeeb578c7c352baba9aa6dc size: 1363
Sekarang versi push 2.0.

 docker image push $DOCKERID/linux_tweet_app:2.0
Perhatikan bahwa beberapa baris output mengatakan Layer already exists. Ini karena Docker akan memanfaatkan 
lapisan read-only yang sama dengan lapisan gambar yang diunggah sebelumnya.

 The push refers to a repository [docker.io/<your docker id>/linux_tweet_app]
 0b171f8fbe22: Pushed
 70d38c767c00: Pushed
 110566462efa: Layer already exists
 305e2b6ef454: Layer already exists
 24e065a5f328: Layer already exists
 2.0: digest: sha256:7c51f77f90b81e5a598a13f129c95543172bae8f5850537225eae0c78e4f3add size: 1363
Anda dapat menelusuri https://hub.docker.com/r/<your docker id>/dan melihat gambar Docker yang baru saja Anda dorong.
 Ini adalah repositori publik, jadi siapa pun dapat menarik gambar - Anda bahkan tidak memerlukan Docker ID untuk menarik
  gambar publik. Docker Hub juga mendukung repositori pribadi.