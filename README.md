Mulai
Bagian ini mencakup berbagai opsi untuk mengatur dan menjalankan Kubernetes.

Berbagai solusi Kubernetes memenuhi persyaratan yang berbeda: kemudahan pemeliharaan, keamanan, kontrol, sumber daya yang 
tersedia, dan keahlian yang dibutuhkan untuk mengoperasikan dan mengelola sebuah cluster.
Anda bisa menggunakan kluster Kubernetes di mesin lokal, cloud, datacenter on-prem, atau memilih kluster Kubernetes yang 
dikelola. Anda juga dapat membuat solusi khusus di berbagai penyedia cloud, atau lingkungan bare metal.

Lingkungan belajar
Jika Anda mempelajari Kubernetes, gunakan solusi berbasis Docker: alat yang didukung oleh komunitas Kubernetes, atau alat
dalam ekosistem untuk mengatur kluster Kubernetes di mesin lokal.

Lingkungan produksi
Saat mengevaluasi solusi untuk lingkungan produksi, pertimbangkan aspek pengoperasian kluster Kubernetes (atau abstraksi )
mana yang ingin Anda kelola sendiri atau kirim ke penyedia.

Tutorial
Bagian dokumentasi Kubernetes ini berisi tutorial. Tutorial menunjukkan cara mencapai tujuan yang lebih besar dari satu
tugas . Biasanya tutorial memiliki beberapa bagian, yang masing-masing memiliki urutan langkah. Sebelum mempelajari setiap tutorial, Anda mungkin ingin membookmark halaman Standardized Glossary untuk referensi selanjutnya.

Dasar-dasar
* Kubernetes Basics adalah tutorial interaktif mendalam yang membantu Anda memahami sistem Kubernetes dan mencoba beberapa fitur dasar Kubernetes.
* Pengantar Kubernetes (edX)
* Halo Minikube

Konfigurasi
Mengkonfigurasi Redis Menggunakan ConfigMap
Aplikasi Stateless
* Mengekspos Alamat IP Eksternal untuk Mengakses Aplikasi dalam Cluster
* Contoh: Menyebarkan aplikasi Buku Tamu PHP dengan Redis

Aplikasi Stateful 
Aplikasi Stateful
Dasar-dasar StatefulSet

Contoh: WordPress dan MySQL dengan Volume Persisten

Contoh: Menyebarkan Cassandra dengan Set Stateful

Menjalankan ZooKeeper, Sistem Terdistribusi CP
Saluran Pipa CI / CD
Mengatur Saluran Pipa CI / CD dengan Kubernetes Bagian 1: Ikhtisar

Mengatur Pipeline CI / CD dengan Jenkins Pod di Kubernetes (Bagian 2)
Jalankan dan Skala Aplikasi Teka-teki Silang Terdistribusi dengan CI / CD di Kubernetes (Bagian 3)
Mengatur CI / CD untuk Aplikasi Teka-teki Silang Terdistribusi di Kubernetes (Bagian 4)

Cluster
AppArmor
Jasa
Menggunakan IP Sumber
Apa berikutnya
Jika Anda ingin menulis tutorial, lihat Tipe Halaman Konten untuk informasi tentang tipe halaman tutorial.

Dasar-Dasar Kubernetes
Tutorial ini memberikan panduan dasar-dasar sistem orkestrasi cluster Kubernetes. Setiap modul berisi beberapa informasi 
latar belakang tentang fitur dan konsep Kubernetes utama, dan termasuk tutorial online interaktif. Tutorial interaktif ini memungkinkan Anda mengelola kluster sederhana dan aplikasi kemas untuk Anda sendiri.

Menggunakan tutorial interaktif, Anda dapat belajar untuk:
Menyebarkan aplikasi kemas pada sebuah cluster.
Skala penyebaran.
Perbarui aplikasi kemas dengan versi perangkat lunak baru.
Debug aplikasi yang kemas.
Tutorial menggunakan Katacoda untuk menjalankan terminal virtual di browser web Anda yang menjalankan Minikube, sebuah 
penyebaran Kubernetes skala kecil yang dapat dijalankan di mana saja. Tidak perlu menginstal perangkat lunak apa pun atau
mengkonfigurasi apa pun; setiap tutorial interaktif berjalan langsung dari peramban web Anda sendiri.

Apa yang dapat dilakukan Kubernet untuk Anda?
Dengan layanan web modern, pengguna mengharapkan aplikasi tersedia 24/7, dan pengembang berharap untuk menyebarkan versi 
baru aplikasi tersebut beberapa kali sehari. Kontainer membantu paket perangkat lunak untuk melayani tujuan-tujuan ini, memungkinkan aplikasi untuk dirilis dan diperbarui dengan cara yang mudah dan cepat tanpa downtime. Kubernetes membantu Anda memastikan aplikasi kemas itu dijalankan di mana dan kapan pun Anda inginkan, dan membantu mereka menemukan sumber daya dan alat yang mereka butuhkan untuk bekerja. Kubernetes adalah platform open source yang siap-produksi yang dirancang dengan akumulasi pengalaman Google dalam orkestrasi wadah, dikombinasikan dengan ide-ide terbaik 
dari komunitas.

