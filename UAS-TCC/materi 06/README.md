Kubernetes untuk Pemula

Mulai cluster
Langkah pertama adalah menginisialisasi gugus di terminal pertama:

kubeadm init --apiserver-advertise-address $(hostname -i)

Itu akan memakan waktu beberapa menit, selama waktu itu Anda akan melihat banyak aktivitas di terminal.
Anda akan melihat sesuatu seperti ini di akhir:
  Your Kubernetes master has initialized successfully!
  To start using your cluster, you need to run (as a regular user):

    mkdir -p $HOME/.kube
    sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
    sudo chown $(id -u):$(id -g) $HOME/.kube/config

  You should now deploy a pod network to the cluster.
  Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
    http://kubernetes.io/docs/admin/addons/

  You can now join any number of machines by running the following on each node
  as root:

  kubeadm join --token SOMETOKEN SOMEIPADDRESS --discovery-token-ca-cert-hash SOMESHAHASH

  Salin seluruh baris yang dimulai kubeadm joindari terminal pertama dan rekatkan ke terminal kedua. Anda harus melihat sesuatu seperti ini:

  Itu berarti Anda hampir siap untuk pergi. Terakhir, Anda hanya perlu menginisialisasi jaringan cluster Anda di terminal pertama:

   kubectl apply -n kube-system -f \
    "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 |tr -d '\n')"

    Anda akan melihat output seperti ini:

  kubectl apply -n kube-system -f \
  >     "https://cloud.weave.works/k8s/net?k8s-version=$(kubectl version | base64 |tr -d '\n')"
  serviceaccount "weave-net" created
  clusterrole "weave-net" created
  clusterrolebinding "weave-net" created
  role "weave-net" created
  rolebinding "weave-net" created
  daemonset "weave-net" created
Cluster Anda sudah diatur!

Apa aplikasi ini?
Ini adalah penambang DockerCoin! 💰🐳📦🚢

Tidak, Anda tidak dapat membeli kopi dengan DockerCoins

Cara kerja DockerCoins:

workermeminta untuk rngmenghasilkan beberapa byte acak

worker mengumpankan byte ini ke hasher

dan ulangi selamanya!

setiap detik, workerpembaruan redismenunjukkan berapa banyak loop dilakukan

webuipertanyaan redis, dan menghitung dan mengekspos "hashing speed" di browser Anda

Mendapatkan kode sumber aplikasi
Kami telah membuat contoh aplikasi untuk dijalankan di sebagian bengkel. Aplikasi ini ada dalam repositori dockercoins .

Mari kita lihat tata letak umum dari kode sumber:

ada file Tulis docker-compose.yml...

… Dan 4 layanan lainnya, masing-masing dalam direktori sendiri:

rng = layanan web menghasilkan byte acak

hasher = hash komputasi layanan web dari data POSTed

worker= proses latar belakang menggunakan rngdanhasher

webui = antarmuka web untuk menonton kemajuan

Kami akan mengkloning repositori GitHub

Repositori juga berisi skrip dan alat yang akan kami gunakan melalui workshop

git clone https://github.com/dockersamples/dockercoins
(Anda juga dapat melakukan fork repositori di GitHub dan mengkloning fork Anda jika Anda menginginkannya.)

Menjalankan aplikasi
Buka direktori dockercoins, di repo hasil kloning:

cd ~/dockercoins
Gunakan Tulisan untuk membuat dan menjalankan semua wadah:

docker-compose up
Compose memberi tahu Docker untuk membuat semua gambar kontainer (menarik gambar dasar yang sesuai), lalu memulai semua kontainer, dan menampilkan log agregat.

Banyak log
Aplikasi terus menerus menghasilkan log

Kita dapat melihat layanan pekerja membuat permintaan ke rng dan hasher

Mari kita letakkan itu di latar belakang

Menghubungkan ke UI web
The webuiwadah mengekspos dashboard web; mari kita melihatnya

Dengan browser web, sambungkan ke node1 sini di port 8000 (dibuat saat Anda menjalankan aplikasi)

Membersihkan
Sebelum melanjutkan, mari matikan semuanya dengan mengetik Ctrl-C.

Konsep Kubernetes
Kubernetes adalah sistem manajemen wadah

Ini berjalan dan mengelola aplikasi kemas pada sebuah cluster

Apa maksudnya itu?

Hal-hal dasar yang dapat kita minta dari Kubernet
Mulai 5 wadah menggunakan gambar atseashop/api:v1.3

Tempatkan penyeimbang beban internal di depan wadah ini

Mulai 10 wadah menggunakan gambar atseashop/webfront:v1.3

Tempatkan penyeimbang beban publik di depan wadah ini

Ini Black Friday (atau Natal), lonjakan lalu lintas, tumbuhkan cluster kami dan tambahkan kontainer

Rilis baru! Ganti wadah saya dengan gambar baruatseashop/webfront:v1.4

Pertahankan permintaan pemrosesan selama peningkatan; perbarui wadah saya satu per satu

Hal-hal lain yang bisa dilakukan Kubernet untuk kita
Autoscaling dasar

Biru / hijau, penyebaran kenari

Layanan berjalan lama, tetapi juga pekerjaan batch (sekali saja)

Komitmen berlebihan pada kluster kami dan usir pekerjaan prioritas rendah

Jalankan layanan dengan data stateful (database dll.)

Kontrol akses berbutir halus menentukan apa yang dapat dilakukan oleh siapa pada sumber daya mana

Mengintegrasikan layanan pihak ketiga (katalog layanan)

Mengotomatiskan tugas-tugas kompleks (operator)

Arsitektur Kubernetes
Arsitektur Kubernetes: master
Logika Kubernetes ("otak" -nya) adalah kumpulan layanan:

server API (titik masuk kami ke semuanya!)
layanan inti seperti scheduler dan manajer pengontrol
etcd (toko kunci / nilai yang sangat tersedia; "database" Kubernetes)
* Bersama-sama, layanan ini membentuk apa yang disebut "master"

Layanan ini dapat berjalan langsung pada host, atau dalam wadah (itu detail implementasi)

etcd dapat dijalankan pada mesin yang terpisah (skema pertama) atau co-located (skema kedua)

Kami membutuhkan setidaknya satu master, tetapi kami dapat memiliki lebih banyak (untuk ketersediaan tinggi)

Arsitektur Kubernetes: node
Node yang mengeksekusi kontainer kami menjalankan koleksi layanan lain:

Engine wadah (biasanya Docker)
kubelet ("agen simpul")
kube-proxy (komponen jaringan yang diperlukan tetapi tidak cukup)
Node sebelumnya disebut "pelayan"

Merupakan kebiasaan untuk tidak menjalankan aplikasi pada node yang menjalankan komponen master (Kecuali ketika menggunakan cluster pengembangan kecil)

Sumber daya Kubernetes
API Kubernetes mendefinisikan banyak objek yang disebut sumber daya

Sumber daya ini disusun berdasarkan jenis, atau Jenis (dalam API)

Beberapa jenis sumber daya umum adalah:

simpul (mesin - fisik atau virtual - di cluster kami)
pod (sekelompok kontainer yang berjalan bersama pada sebuah node)
layanan (titik akhir jaringan yang stabil untuk terhubung ke satu atau beberapa kontainer)
namespace (kelompok hal yang kurang lebih terisolasi)
rahasia (bundel data sensitif untuk diteruskan ke wadah)
Dan banyak lagi! (Kita dapat melihat daftar lengkap dengan menjalankan get kubectl)

Deklaratif vs imperatif
Orkestra kontainer kami sangat menekankan deklaratif

Deklaratif:

Saya mau secangkir teh.

Imperatif:

Rebus air. Tuangkan dalam teko. Tambahkan daun teh. Curam sebentar. Sajikan dalam cangkir.

Deklaratif tampaknya lebih sederhana pada awalnya ...

... Selama kamu tahu cara menyeduh teh

Akan seperti apa deklaratif itu:
Saya ingin secangkir teh, diperoleh dengan menuangkan infus daun teh ke dalam cangkir.

Infus diperoleh dengan membiarkan benda curam beberapa menit dalam air panas.

Cairan panas diperoleh dengan menuangkannya dalam wadah yang sesuai dan meletakkannya di atas kompor.

Ah, akhirnya, wadah! Sesuatu yang kita ketahui. Mari kita mulai bekerja, oke?

Ringkasan deklaratif vs imperatif
Sistem imperatif:

lebih sederhana

jika suatu tugas terganggu, kita harus memulai kembali dari awal

Sistem deklaratif:

jika tugas terganggu (atau jika kami muncul di pesta setengah jalan), kami dapat mencari tahu apa yang hilang dan hanya melakukan apa yang perlu

kita harus bisa mengamati sistem

... dan menghitung "perbedaan" antara apa yang kita miliki dan apa yang kita inginkan

Deklaratif vs imperatif di Kubernetes
Hampir semua yang kita buat di Kubernetes dibuat dari a spec

Tonton specbidang di file YAML nanti!

The specmenggambarkan bagaimana kita ingin hal menjadi

Kubernetes akan merekonsiliasi kondisi saat ini dengan spek (secara teknis, ini dilakukan oleh sejumlah pengendali )

Ketika kami ingin mengubah beberapa sumber, kami memperbarui spec

Kubernetes maka akan konvergen sumber daya yang

Model jaringan Kubernetes
TL, DR:

Cluster kami (node ​​dan pod) adalah satu jaringan IP flat besar.

Secara terperinci:

semua node harus dapat mencapai satu sama lain, tanpa NAT

semua pod harus dapat saling menjangkau, tanpa NAT

pod dan node harus dapat mencapai satu sama lain, tanpa NAT

setiap pod mengetahui alamat IP-nya (tidak ada NAT)

Kubernetes tidak mengamanatkan implementasi khusus apa pun

Model jaringan Kubernetes: yang baik
Semuanya bisa mencapai segalanya

Tidak ada terjemahan alamat

Tidak ada terjemahan port

Tidak ada protokol baru

Pod tidak dapat berpindah dari satu node ke node lain dan menjaga alamat IP-nya

Alamat IP tidak harus “portabel” dari satu simpul ke simpul lainnya (Kita dapat menggunakan misalnya subnet per simpul dan menggunakan topologi yang dialihkan sederhana)

Spesifikasi ini cukup sederhana untuk memungkinkan berbagai implementasi

Model jaringan Kubernetes: semakin kurang bagus
Semuanya bisa mencapai segalanya

jika Anda menginginkan keamanan, Anda perlu menambahkan kebijakan jaringan

implementasi jaringan yang Anda gunakan perlu mendukung mereka

Ada lusinan implementasi di luar sana (15 tercantum dalam dokumentasi Kubernetes)

Sepertinya Anda memiliki jaringan level 3, tetapi hanya level 4 (Spesifikasi ini membutuhkan UDP dan TCP, tetapi tidak rentang port atau paket IP yang sewenang-wenang)

kube-proxy ada di jalur data saat menghubungkan ke pod atau wadah, dan itu tidak terlalu cepat (bergantung pada proksi userland atau iptables)

Model jaringan Kubernetes: dalam praktiknya
Node yang kami gunakan telah diatur untuk menggunakan Weave

Kami tidak mendukung Weave dengan cara tertentu, itu hanya Bekerja Untuk Kami

Jangan khawatir tentang peringatan tentang kinerja kube-proxy

Kecuali kamu:

secara rutin menjenuhkan antarmuka jaringan 10G

hitung tarif paket dalam jutaan per detik

menjalankan VOIP lalu lintas tinggi atau platform game

lakukan hal-hal aneh yang melibatkan jutaan koneksi simultan (dalam hal ini Anda sudah terbiasa dengan penyetelan kernel)

Kontak pertama dengan kubectl
kubectl adalah (hampir) satu-satunya alat yang kita perlukan untuk berbicara dengan Kubernetes

Ini adalah alat CLI yang kaya di sekitar API Kubernetes (Semua yang dapat Anda lakukan dengan kubectl, Anda dapat melakukannya langsung dengan API)

Anda juga dapat menggunakan --kubeconfigbendera untuk meneruskan file konfigurasi

Atau langsung --server, --user, dll

kubectl dapat diucapkan "Cube CTL", "Cube cuttle", "Cube cuddle" ...

kubectl get
Mari kita lihat sumber Node kami dengan get kubectl!

Lihatlah komposisi cluster kami:

kubectl get node
Perintah-perintah ini setara

kubectl get no
kubectl get node
kubectl get nodes
Memperoleh hasil yang bisa dibaca mesin
kubectl get dapat menampilkan JSON, YAML, atau langsung diformat

Beri kami info lebih lanjut tentang node:

kubectl get nodes -o wide
Mari kita memiliki beberapa YAML:

kubectl get no -o yaml
Lihat jenis itu: Daftar di akhir? Ini adalah jenis hasil kami!

(Ab) menggunakan kubectldanjq
Sangat mudah untuk membuat laporan khusus

Tampilkan kapasitas semua node kami sebagai aliran objek JSON:

kubectl get nodes -o json |
      jq ".items[] | {name:.metadata.name} + .status.capacity"
Apa yang tersedia
kubectl memiliki fasilitas introspeksi yang cukup bagus

Kami dapat mendaftar semua jenis sumber daya yang tersedia dengan menjalankan kubectl get

Kami dapat melihat detail tentang sumber daya dengan:
kubectl describe type/name
kubectl describe type name
Kami dapat melihat definisi untuk jenis sumber daya dengan:
kubectl explain type
Setiap kali, typebisa berupa nama tipe tunggal, jamak, atau disingkat.

Jasa
Suatu layanan adalah titik akhir yang stabil untuk terhubung ke "sesuatu" (Dalam proposal awal, mereka disebut "portal")

Daftarkan layanan di kluster kami:

kubectl get services
Ini juga akan berfungsi:

    kubectl get svc
Sudah ada satu layanan di kluster kami: API Kubernetes itu sendiri.

Layanan ClusterIP
Sebuah ClusterIPlayanan internal, tersedia dari cluster hanya

Ini berguna untuk introspeksi dari dalam wadah

Coba sambungkan ke API.

-k digunakan untuk melewati verifikasi sertifikat
Pastikan untuk mengganti 10.96.0.1 dengan CLUSTER-IP yang ditunjukkan oleh $ kubectl get svc
curl -k https://10.96.0.1
Kesalahan yang kami lihat diharapkan: API Kubernetes memerlukan otentikasi.

Mendaftar kontainer yang berjalan
Kontainer dimanipulasi melalui pod

Pod adalah sekelompok wadah:

berjalan bersama (pada node yang sama)

berbagi sumber daya (RAM, CPU; tetapi juga jaringan, volume)

Daftarkan pod di kluster kami:

kubectl get pods
Ini bukan pod yang Anda cari . Tapi di mana mereka?!?

Ruang nama
Ruang nama memungkinkan kami untuk memisahkan sumber daya

Daftar ruang nama pada kluster kami dengan salah satu dari perintah ini:

kubectl get namespaces
salah satu dari ini akan bekerja juga:

kubectl get namespace
kubectl get ns
Anda tahu apa ... Benda ini kube-systemterlihat mencurigakan.

Mengakses ruang nama
Secara default, kubectlgunakan defaultnamespace

Kita dapat beralih ke namespace yang berbeda dengan -nopsi

Daftar pod di kube-systemnamespace:

kubectl -n kube-system get pods
Ding ding ding ding ding!

Apa semua polong ini?
etcd adalah server etcd kami

kube-apiserver adalah server API

kube-controller-managerdan kube-schedulerkomponen master lainnya

kube-dns adalah komponen tambahan (tidak wajib tetapi sangat berguna, jadi ada di sana)

kube-proxy adalah komponen (per-simpul) yang mengelola pemetaan port dan semacamnya

weave adalah komponen (per-simpul) yang mengelola overlay jaringan
yang READYkolom menunjukkan jumlah kontainer di setiap pod

pod dengan nama yang diakhiri dengan -node1adalah komponen master (mereka secara khusus "disematkan" ke node master)

Menjalankan kontainer pertama kami di Kubernetes
Hal pertama yang pertama: kita tidak bisa menjalankan wadah

Kami akan menjalankan pod, dan di pod itu akan ada satu wadah

Dalam wadah itu di pod, kita akan menjalankan perintah ping sederhana

Kemudian kita akan memulai salinan pod tambahan

Mulai pod sederhana dengan kubectl run
Kita harus menentukan setidaknya nama dan gambar yang ingin kita gunakan

Mari kita ping 8.8.8.8, DNS publik Google

kubectl run pingpong --image alpine ping 8.8.8.8
Oke, apa yang baru saja terjadi?

Di belakang layar kubectl run
Mari kita lihat sumber daya yang diciptakan oleh kubectl run

Daftarkan sebagian besar jenis sumber daya:

kubectl get all
Kita harus melihat hal-hal berikut:

deploy/pingpong( penyebaran yang baru saja kami buat)
rs/pingpong-xxxx( set replika yang dibuat oleh penyebaran)
po/pingpong-yyyy( pod yang dibuat oleh set replika)
Apa saja hal-hal yang berbeda ini?
Sebuah penyebaran adalah membangun tingkat tinggi

memungkinkan penskalaan, pembaruan bergulir, rollback

beberapa penyebaran dapat digunakan bersama untuk mengimplementasikan penyebaran kenari

mendelegasikan manajemen pod untuk set replika

Sebuah set replika adalah membangun tingkat rendah

memastikan bahwa sejumlah pod identik dijalankan

memungkinkan penskalaan

jarang digunakan secara langsung

Sebuah kontroler replikasi adalah (usang) pendahulu dari satu set replika

pingpongPenyebaran kami
kubectl runmenciptakan penyebaran ,deploy/pingpong

Penempatan itu menciptakan set replika ,rs/pingpong-xxxx

Itu replika set menciptakan pod ,po/pingpong-yyyy

Kita akan lihat nanti bagaimana orang-orang ini bermain bersama untuk:

scaling

ketersediaan tinggi

bergulir pembaruan

Melihat hasil wadah
Mari kita gunakan kubectl logsperintah

Kami akan melewati salah satu nama pod , atau tipe / nama (Misalnya jika kami menetapkan set penyebaran atau replika, itu akan mendapatkan pod pertama di dalamnya)

Kecuali ditentukan lain, itu hanya akan menampilkan log dari wadah pertama di pod (Untung hanya ada satu di kita!)

Lihat hasil dari perintah ping kami:

kubectl logs deploy/pingpong
Log streaming secara real time
Sama seperti docker logs, kubectl logsmendukung opsi yang mudah:

-f/--followuntuk melakukan streaming log secara real time (à la tail -f)

--tail untuk menunjukkan berapa banyak garis yang ingin Anda lihat (dari akhir)

--since untuk mendapatkan log hanya setelah cap waktu yang diberikan

Lihat log terbaru dari perintah ping kami:

kubectl logs deploy/pingpong --tail 1 --follow
Menskalakan aplikasi kita
Kami dapat membuat salinan tambahan dari wadah kami (atau lebih tepatnya pod kami) dengan kubectl scale

Skala penyebaran pingpong kami:

kubectl scale deploy/pingpong --replicas 8
Catatan: bagaimana jika kita mencoba skala rs/pingpong-xxxx? Kita bisa! Tetapi penyebaran akan segera menyadarinya, dan mengurangi ke tingkat awal.

Ketangguhan
Pingpong deployment menyaksikan set replika-nya

Set replika memastikan jumlah pod yang tepat berjalan

Apa yang terjadi jika polong hilang?

Di jendela terpisah, daftarkan pod, dan terus tonton:
kubectl get pods -w
Ctrl-C untuk mengakhiri menonton.

Jika Anda ingin menghancurkan pod, Anda akan menggunakan pola ini di mana yyyypengenal pod tertentu:
kubectl delete pod pingpong-yyyy
Bagaimana jika kita menginginkan sesuatu yang berbeda?
Bagaimana jika kita ingin memulai wadah "satu tembakan" yang tidak bisa dinyalakan kembali?

Kita bisa menggunakan kubectl run --restart=OnFailureataukubectl run --restart=Never

Perintah-perintah ini akan menciptakan pekerjaan atau pod bukannya penyebaran

Di bawah tenda, kubectl rungunakan "generator" untuk membuat deskripsi sumber daya

Kita juga bisa menulis sendiri deskripsi sumber daya ini (biasanya dalam YAML), dan membuatnya di cluster dengan kubectl apply -f(dibahas nanti)

Dengan kubectl run --schedule=…, kita juga dapat membuat cronjobs

Melihat log beberapa pod
Saat kami menentukan nama penggunaan, hanya satu log pod tunggal yang ditampilkan

Kami dapat melihat log beberapa pod dengan menentukan pemilih

Selector adalah ekspresi logika menggunakan label

Mudah, ketika you kubectl run somename, objek terkait memiliki run=somenamelabel

Lihat baris log terakhir dari semua pod dengan run=pingponglabel:

kubectl logs -l run=pingpong --tail 1
Sayangnya, --followtidak dapat (belum) digunakan untuk melakukan streaming log dari banyak wadah.

Membersihkan
Bersihkan penyebaran Anda dengan menghapus pingpong

kubectl delete deploy/pingpong
Wadah terbuka
kubectl exposemenciptakan layanan untuk pod yang ada

Suatu layanan adalah alamat yang stabil untuk sebuah pod (atau sekelompok pod)

Jika kami ingin terhubung ke pod kami, kami perlu membuat layanan

Setelah layanan dibuat, kube-dnsakan memungkinkan kami untuk mengatasinya dengan nama (yaitu setelah membuat layanan hello, nama helloakan menyelesaikan sesuatu)

Ada berbagai jenis layanan, dirinci pada slide berikut:

ClusterIP, NodePort, LoadBalancer,ExternalName

Jenis layanan dasar
ClusterIP (tipe standar)

alamat IP virtual dialokasikan untuk layanan (dalam rentang internal, privat) alamat IP ini hanya dapat dijangkau dari dalam cluster (node ​​dan pod) kode kami dapat terhubung ke layanan menggunakan nomor port asli

NodePort

port dialokasikan untuk layanan (secara default, dalam kisaran 30000-32768) bahwa port tersedia pada semua node kami dan siapa pun dapat terhubung dengannya kode kami harus diubah untuk menyambung ke nomor port baru tersebut

Jenis layanan ini selalu tersedia.

Di bawah tenda: kube-proxymenggunakan proxy userland dan banyak iptablesaturan.

Jenis layanan lainnya
LoadBalancer
penyeimbang beban eksternal dialokasikan untuk layanan ini
penyeimbang beban dikonfigurasikan sesuai (misalnya: NodePortlayanan dibuat, dan penyeimbang beban mengirimkan lalu lintas ke port itu)
ExternalName

entri DNS yang dikelola oleh kube-dnshanya akan menjadi CNAMEcatatan yang disediakan
tidak ada port, tidak ada alamat IP, tidak ada yang dialokasikan
Menjalankan kontainer dengan port terbuka
Karena ping tidak memiliki apa pun untuk dihubungkan, kita harus menjalankan sesuatu yang lain

Mulai banyak wadah ElasticSearch:
kubectl run elastic --image=elasticsearch:2 --replicas=4
Lihat mereka dimulai:

kubectl get pods -w
The -wopsi “jam tangan” peristiwa yang terjadi pada sumber daya yang ditentukan.

Catatan: mohon JANGAN menelepon layanan search. Itu akan bertabrakan dengan TLD.

Mengungkap penyebaran kami
Kami akan membuat ClusterIPlayanan default

Buka port ElasticSearch HTTP API:

kubectl expose deploy/elastic --port 9200
Cari alamat IP yang dialokasikan:

kubectl get svc
Layanan adalah konstruksi lapisan 4
Anda dapat menetapkan alamat IP untuk layanan, tetapi mereka masih lapisan 4 (yaitu layanan bukan alamat IP; ini adalah alamat IP + protokol + port)

Ini disebabkan oleh implementasi saat ini kube-proxy(ini bergantung pada mekanisme yang tidak mendukung layer 3)

Akibatnya: Anda harus menunjukkan nomor port untuk layanan Anda

Menjalankan layanan dengan port sewenang-wenang (atau rentang port) memerlukan peretasan (mis. Mode jaringan host)

Menguji layanan kami
Kami sekarang akan mengirim beberapa permintaan HTTP ke pod ElasticSearch kami

Mari kita dapatkan alamat IP yang dialokasikan untuk layanan kami, secara terprogram :

IP=$(kubectl get svc elastic -o go-template --template '{{ .spec.clusterIP }}')
Kirim beberapa permintaan:

curl http://$IP:9200/
Permintaan kami dimuat seimbang di beberapa pod.

Membersihkan
Kami sudah selesai dengan elasticpenyebaran, jadi mari kita bersihkan

kubectl delete deploy/elastic
Aplikasi kami di Kube
Apa yang ada di menu?
Pada bagian ini, kita akan:

membangun gambar untuk aplikasi kita,

kirimkan gambar-gambar ini dengan registri,

jalankan penyebaran menggunakan gambar-gambar ini,

mengekspos penyebaran ini sehingga mereka dapat berkomunikasi satu sama lain,

mengekspos UI web sehingga kami dapat mengaksesnya dari luar.

Rencana
Bangun di simpul kontrol kami ( node1)

Beri tag pada gambar sehingga diberi nama $USERNAME/servicename

Unggah mereka ke Docker Hub

Buat penyebaran menggunakan gambar

Paparkan (dengan a ClusterIP) layanan yang perlu dikomunikasikan

Paparkan (dengan a NodePort) WebUI

Mempersiapkan
Di terminal pertama, setel variabel lingkungan untuk nama pengguna Docker Hub Anda . Ini bisa menjadi nama pengguna Docker Hub yang sama dengan yang Anda gunakan untuk masuk ke terminal di situs ini.

export USERNAME=YourUserName
Pastikan Anda masih berada di dockercoinsdirektori.

pwd
Catatan tentang pendaftar
Untuk bengkel ini, kami akan menggunakan Docker Hub . Ada sejumlah opsi lain, termasuk dua yang disediakan oleh Docker.

Docker juga menyediakan:

Docker Trusted Registry yang menambahkan banyak fitur keamanan dan penyebaran termasuk pemindaian keamanan, dan kontrol akses berbasis peran.
Open Source Registry Docker .
Hub Docker
Docker Hub adalah registri default untuk Docker.

Nama gambar di Hub hanya $USERNAME/$IMAGENAMEatau $ORGANIZATIONNAME/$IMAGENAME.

Gambar resmi dapat disebut sebagai adil $IMAGENAME.

Untuk menggunakan Hub, pastikan Anda memiliki akun. Kemudian ketik docker loginterminal dan login dengan nama pengguna dan kata sandi Anda.

Menggunakan Docker Trusted Registry, Docker Open Source Registry sangat mirip.

Nama gambar pada pendaftar lain adalah $REGISTRYPATH/$USERNAME/$IMAGENAMEatau $REGISTRYPATH/$ORGANIZATIONNAME/$IMAGENAME.

Login menggunakan docker login $REGISTRYPATH.

Membangun dan mendorong gambar kita
Kami akan menggunakan fitur Docker Compose yang nyaman

Pergi ke stacksdirektori:

cd ~/dockercoins/stacks
Bangun dan dorong gambar:

docker-compose -f dockercoins.yml build
docker-compose -f dockercoins.yml push
Mari kita lihat file dockercoins.yml sementara ini sedang membangun dan mendorong.

version: "3"
services:
  rng:
    build: dockercoins/rng
    image: ${USERNAME}/rng:${TAG-latest}
    deploy:
      mode: global
  ...
  redis:
    image: redis
  ...
  worker:
    build: dockercoins/worker
    image: ${USERNAME}/worker:${TAG-latest}
    ...
    deploy:
      replicas: 10
Kalau-kalau Anda bertanya-tanya ... Docker "layanan" bukan "layanan" Kubernetes.

Menyebarkan semua hal
Kami sekarang dapat menggunakan kode kami (serta instance redis)

Sebarkan redis:

kubectl run redis --image=redis
Menyebarkan semuanya:

for SERVICE in hasher rng webui worker; do
  kubectl run $SERVICE --image=$USERNAME/$SERVICE -l app=$SERVICE
done
Apakah ini berhasil?
Setelah menunggu penyebaran selesai, mari kita lihat log!

(Petunjuk: gunakan kubectl get deploy -wuntuk menonton acara penempatan)

Lihatlah beberapa log:

kubectl logs deploy/rng
kubectl logs deploy/worker
🤔 rngbaik-baik saja ... Tapi tidak worker.

💡 Oh benar! Kami lupa expose.

Layanan terbuka
Mengekspos layanan secara internal
Tiga penyebaran harus dicapai oleh orang lain: hasher, redis,rng

worker tidak perlu diekspos

webui akan ditangani nanti

Ekspos setiap penyebaran, tentukan port yang tepat:

kubectl expose deployment redis --port 6379
kubectl expose deployment rng --port 80
kubectl expose deployment hasher --port 80