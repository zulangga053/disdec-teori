# Nama   : Zul Ikhwanul Anggara
# Nim    : 255410043

# Materi Minggu 01: Pengenalan Sistem Terdistribusi dan Terdesentralisasi

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Pengenalan Sistem Terdistribusi dan Terdesentralisasi.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Hasil rangkuman ini dibuat secara otomatis oleh AI berdasarkan sumber materi perkuliahan `01--pengenalan.pdf`. Dokumen ini belum mengalami penyuntingan manual dan masih perlu ditinjau serta disesuaikan kembali sesuai dengan pemahaman dan catatan perkuliahan individu.

---

# Rangkuman Materi

## 1. Pendahuluan

### 1.1 Latar Belakang dan Arti Penting

Perkembangan teknologi informasi saat ini diwarnai oleh pertumbuhan volume data dan jumlah pengguna yang sangat masif. Platform modern skala besar seperti Google, Netflix, YouTube, dan jaringan Blockchain tidak akan mungkin beroperasi tanpa landasan sistem terdistribusi dan terdesentralisasi. Layanan komputasi masa kini menuntut tingkat skalabilitas yang tinggi, kemampuan toleransi terhadap kegagalan (_fault-tolerant_), serta performa dan ketersediaan sistem (_availability_) secara terus-menerus selama 24 jam sehari dan 7 hari seminggu.

Dua konsep utama yang menjadi pilar dalam mengatasi tantangan skala ini adalah:

- **Skalabilitas**: Kemampuan suatu sistem, jaringan, proses, atau organisasi untuk berkembang dan menangani peningkatan beban kerja, jumlah pengguna, atau permintaan tanpa mengorbankan kinerja maupun kualitas layanan.
- **Sistem _Fault-Tolerant_**: Kemampuan sistem untuk tetap beroperasi secara normal meskipun terjadi kegagalan atau malfungsi pada sebagian komponennya, baik pada perangkat keras (_hardware_) maupun perangkat lunak (_software_).

### 1.2 Tujuan Pembelajaran

Mata kuliah Sistem Terdistribusi dan Terdesentralisasi (Kode: FIF25002T, 3 SKS) pada Program Studi Informatika ini memiliki beberapa tujuan pembelajaran utama:

1. Memahami dan menjelaskan konsep dasar sistem terdistribusi.
2. Memahami dan menjelaskan konsep dasar sistem terdesentralisasi.
3. Membedakan secara mendasar antara sistem terdistribusi dan sistem terdesentralisasi.
4. Mengidentifikasi berbagai contoh penerapan nyata dari kedua jenis sistem tersebut di industri modern.
5. Mengenali tantangan teknis serta tren perkembangan teknologi di masa depan.

---

## 2. Konsep Dasar dan Arsitektur Sistem Terdistribusi

### 2.1 Pengertian Sistem Terdistribusi

Menurut definisi klasik dari Andrew S. Tanenbaum dan Maarten van Steen (2007), **sistem terdistribusi** didefinisikan sebagai _“a collection of independent computers that appears to its users as a single coherent system”_ (kumpulan komputer independen yang tampak bagi penggunanya sebagai satu sistem tunggal yang koheren).

Sistem terdistribusi terdiri dari komponen-komponen komputer independen yang saling terhubung dan berkomunikasi satu sama lain melalui jaringan komputer. Pengguna melihat dan berinteraksi dengan sistem tersebut seolah-olah hanya berhadapan dengan satu komputer utuh, karena adanya aspek **transparansi** (seperti transparansi lokasi, akses, replikasi, dan kegagalan) yang menyembunyikan kompleksitas fisik jaringan di belakang layar.

### 2.2 Karakteristik Utama Sistem Terdistribusi

Sistem terdistribusi memiliki lima karakteristik inti yang membedakannya dari sistem terpusat konvensional :

1. **Transparansi Lokasi**: Pengguna dan aplikasi tidak perlu mengetahui lokasi fisik sumber daya atau server yang memproses permintaan mereka.
2. **Skalabilitas**: Sistem dapat diperbesar (ditambah node/komputer) untuk menangani kenaikan beban kerja secara fleksibel.
3. **Toleransi Kegagalan (_Fault Tolerance_)**: Sistem tetap dapat berjalan meskipun beberapa node atau komponen mengalami kegagalan fungsi.
4. **Kinerja Tinggi (_High Performance_)**: Pembagian beban kerja ke banyak komputer memungkinkan pemrosesan data dilakukan secara paralel sehingga meningkatkan throughput [2, 5].
5. **Heterogenitas**: Komponen-komponen dalam sistem dapat terdiri dari berbagai variasi perangkat keras, sistem operasi, dan protokol jaringan yang berbeda namun tetap dapat saling berinteraksi .

### 2.3 Arsitektur Sistem Dasar

Dalam praktiknya, arsitektur dasar sistem terdistribusi dapat dikelompokkan ke dalam tiga model utama [4]:

| Model Arsitektur       | Deskripsi dan Prinsip Kerja                                                                                                                                                                                                                                                                                                                     | Contoh Penerapan                                                |
| :--------------------- | :---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :-------------------------------------------------------------- |
| **Client-Server**      | Terdapat pemisahan peran yang jelas antara penyedia layanan (_server_) dan peminta layanan (_client_). Client mengirimkan permintaan melalui jaringan (misalnya REST API via API Gateway) ke mikroservis terpisah (seperti _Account Service_, _Inventory Service_, _Shipping Service_) yang masing-masing terhubung ke basis data terpisah [4]. | Aplikasi Web & Mobile (e-commerce, sistem informasi kampus) [4] |
| **Peer-to-Peer (P2P)** | Setiap komputer atau node dalam jaringan memiliki kedudukan yang setara (berperan sebagai client sekaligus server secara bersamaan) dan berkomunikasi langsung antar peer tanpa bergantung pada server pusat [4].                                                                                                                               | BitTorrent, Jaringan Kripto / P2P File Sharing [4, 6]           |
| **Hybrid**             | Menggabungkan model Client-Server dan P2P. Biasanya menggunakan _Supernodes_ atau server terpusat untuk fungsi koordinasi dan otentikasi awal, sedangkan transmisi data atau transmisi pesan dilakukan secara P2P antar node [4].                                                                                                               | Aplikasi perpesanan instan (WhatsApp, Telegram, Signal) [4]     |

---

## 3. Sistem Terdesentralisasi dan Perbandingannya

### 3.1 Pengertian Sistem Terdesentralisasi

Sistem terdesentralisasi lahir sebagai bentuk evolusi untuk mengatasi kelemahan sistem terpusat (_centralized system_). Pada sistem terpusat, seluruh kendali dan pemrosesan bergantung pada satu titik kontrol (_single point of failure_) . Jika titik pusat tersebut mengalami gangguan, maka seluruh sistem akan lumpuh .

Sebaliknya, pada **sistem terdesentralisasi**, kontrol dan wewenang pengambilan keputusan didistribusikan ke banyak node dalam jaringan . Tidak ada otoritas tunggal atau _single point of failure_ . Keputusan kolektif dalam sistem terdesentralisasi dicapai melalui mekanisme **konsensus** antar node yang berpartisipasi .

### 3.2 Perbedaan Utama: Sistem Terdistribusi vs Sistem Terdesentralisasi

Meskipun sering kali digunakan secara bersamaan, sistem terdistribusi dan terdesentralisasi memiliki fokus utama yang berbeda [5, 8]:

- **Sistem Terdistribusi**: Berfokus utama pada **pembagian beban kerja (_workload distribution_)** [5, 8]. Tujuan utamanya adalah meningkatkan performa, skalabilitas, dan keandalan pemrosesan komputasi [2, 5]. Contoh fisiknya meliputi _Google File System (GFS)_ dan _MinIO Object Storage_ (yang kemudian di-fork menjadi Silo) .
- **Sistem Terdesentralisasi**: Berfokus utama pada **pembagian kontrol (_control distribution_)** [5, 8]. Tujuan utamanya adalah menghilangkan ketergantungan pada titik kendali terpusat dan membangun kepercayaan bersama melalui konsensus . Contoh utamanya adalah jaringan _Blockchain_ [5, 6].

| Dimensi Pembanding        | Sistem Terdistribusi                                    | Sistem Terdesentralisasi                                        |
| :------------------------ | :------------------------------------------------------ | :-------------------------------------------------------------- |
| **Fokus Utama**           | Pembagian beban kerja (_workload_) [5, 8]               | Pembagian wewenang/kontrol (_control_) [5, 8]                   |
| **Titik Kontrol**         | Dapat berupa terpusat secara logis oleh satu organisasi | Tersebar di banyak entitas/node independen                      |
| **Pengambilan Keputusan** | Ditentukan oleh arsitektur internal sistem              | Menggunakan mekanisme konsensus kolektif                        |
| **Tujuan Utama**          | Efisiensi, skalabilitas, dan performa tinggi [2, 5]     | Otonomi, transparansi, dan pencegahan _single point of failure_ |
| **Contoh Sistem**         | Google File System (GFS), MinIO, HDFS [5, 6]            | Blockchain (Bitcoin, Ethereum), DNS, BitTorrent [5, 6]          |

---

## 4. Teorema CAP dan Konsistensi Data

### 4.1 Teorema CAP (Brewer's Theorem)

Dalam merancang sistem penyimpanan data terdistribusi (_distributed data store_), terdapat sebuah hukum fundamental yang dikenal sebagai **Teorema CAP** atau **Brewer’s Theorem**, yang diperkenalkan oleh Eric Brewer pada tahun 2000 [6].

Teorema CAP menyatakan bahwa sebuah sistem data terdistribusi hanya dapat menjamin maksimal **dua dari tiga** properti berikut secara bersamaan [6]:

1. **Consistency (C)**: Setiap operasi pembacaan data akan mengembalikan hasil penulisan data terbaru atau mengembalikan pesan kesalahan (_error_) [6].
2. **Availability (A)**: Setiap permintaan ke node yang tidak rusak (_non-failing node_) pasti menerima respon non-error, meskipun respon tersebut mungkin bukan data yang paling baru [6].
3. **Partition Tolerance (P)**: Sistem tetap dapat beroperasi dan berfungsi dengan baik meskipun komunikasi antar node terputus atau terfragmentasi (_network partition_) [6].

Karena potensi gangguan jaringan (_partition_) selalu ada dalam dunia nyata, properti _Partition Tolerance_ (P) wajib dimiliki oleh sistem terdistribusi [6]. Oleh karena itu, perancang sistem harus memilih _trade-off_ antara dua pendekatan utama ketika terjadi partisi jaringan [6]:

- **AP (_Availability + Partition Tolerance_)**: Sistem mengutamakan ketersediaan layanan [6]. Permintaan data akan selalu dilayani meskipun ada risiko data yang diterima pengguna belum tersinkronisasi (mengorbankan konsistensi) [6]. Contoh sistem AP adalah **MongoDB** [6].
- **CP (_Consistency + Partition Tolerance_)**: Sistem mengutamakan konsistensi data [6]. Permintaan baca maupun tulis akan ditolak atau dibatalkan jika terjadi partisi jaringan untuk memastikan tidak ada data yang tidak konsisten (mengorbankan ketersediaan) [6]. Contoh sistem CP adalah **Apache Cassandra** [6].

### 4.2 Model Konsistensi Data

Tingkat konsistensi data dalam sistem terdistribusi umumnya dibagi menjadi dua kategori utama [7]:

- **Strong Consistency**: Menjamin bahwa seluruh operasi pembacaan di node mana pun akan selalu menghasilkan data yang sama dan paling baru secara instan [7]. Model ini sangat krusial untuk aplikasi kritis seperti **Sistem Perbankan**, di mana saldo rekening harus selalu tepat dan konsisten [7].
- **Eventual Consistency**: Menjamin bahwa data di berbagai node akan menjadi konsisten secara bertahap seiring berjalannya waktu (_eventually_) [7]. Model ini cocok untuk aplikasi seperti **Media Sosial**, di mana keterlambatan beberapa detik dalam pembaruan postingan atau jumlah _like_ tidak berdampak kritis [7].

---

## 5. Studi Kasus Penerapan di Industri

Penerapan sistem terdistribusi dan terdesentralisasi telah menjadi tulang punggung berbagai platform digital terkemuka [2, 6]:

### 5.1 Studi Kasus Sistem Terdistribusi

- **Penyimpanan Data Skala Besar (_Big Data Store_)**: _Google File System (GFS)_ dan _Hadoop Distributed File System (HDFS)_ digunakan untuk menyimpan dan mengelola petabyte data secara terdistribusi di ribuan server [6].
- **Messaging & Event Streaming**: _Apache Kafka_ dan _Apache Pulsar_ mengelola alur data pesan dan pemrosesan kejadian (_event streaming_) berkecepatan tinggi secara terdistribusi [6].
- **Platform Streaming Video**: _Netflix, YouTube, dan Twitch_ memanfaatkan infrastruktur pemrosesan dan pengiriman konten terdistribusi untuk melayani jutaan penonton secara bersamaan tanpa _downtime_ [6].
- **Platform Media Sosial**: _Instagram, Threads, Facebook, dan TikTok_ mengelola lalu lintas data interaktif dan media berskala raksasa melalui arsitektur microservices terdistribusi [6].

### 5.2 Studi Kasus Sistem Terdesentralisasi

- **Teknologi Blockchain**: Platform seperti _Bitcoin, Ethereum, Algorand, Sui, Near, dan Aptos_ menggunakan jaringan terdesentralisasi berbasis konsensus untuk mencatat transaksi keuangan dan _smart contracts_ tanpa perantara lembaga keuangan pusat [6].
- **Domain Name System (DNS)**: Sistem pemetaan nama domain ke alamat IP yang terdistribusi dan terdesentralisasi secara hierarkis di seluruh dunia [5, 6].
- **BitTorrent**: Protokol berbagi file Peer-to-Peer terdesentralisasi yang memungkinkan pengguna mengunduh bagian-bagian file dari banyak peer secara bersamaan [5, 6].

---

## 6. Tantangan dan Tren Masa Depan

### 6.1 Tantangan Teknis dan Operasional

Mengembangkan dan mengelola sistem terdistribusi serta terdesentralisasi memiliki kompleksitas yang tinggi [7]:

1. **Latency dan Throughput**:
   - _Latency_: Waktu tunda yang dibutuhkan sistem untuk memproses satu permintaan [7].
   - _Throughput_: Jumlah total permintaan atau volume data yang dapat diproses oleh sistem dalam satu satuan waktu [7]. Meminimalkan latency sambil memaksimalkan throughput pada jaringan yang luas merupakan tantangan yang rumit [7].
2. **Keamanan Siber**:
   - **DDoS Attack**: Serangan yang membanjiri lalu lintas server hingga lumpuh [7].
   - **Sybil Attack**: Serangan siber di mana penyerang membuat banyak identitas palsu (akun, node, atau pseudonim hingga ribuan) secara bersamaan untuk mendominasi suara, reputasi, atau pengaruh dalam jaringan terdesentralisasi [7, 8]. Istilah ini diambil dari nama seorang wanita yang memiliki gangguan identitas dalam sebuah buku terbitan tahun 1973 [8]. Penyerang yang menyamar dapat memanipulasi konsensus dan merusak integritas sistem [8].
3. **Kompleksitas Manajemen Node**: Kesulitan dalam mengoordinasikan, memantau, dan memelihara ribuan node yang bersifat heterogen dan tersebar secara geografis [7].
4. **Tata Kelola (_Governance_)**: Pada sistem terdesentralisasi, pengambilan keputusan mengenai pembaruan protokol atau regulasi jaringan memerlukan mekanisme tata kelola berbasis komunitas yang kompleks [7].

### 6.2 Tren Masa Depan

Teknologi sistem terdistribusi dan terdesentralisasi terus berkembang menuju arah berikut [8]:

- **Cloud-Native Infrastructure**: Penggunaan teknologi kontainerisasi dan orkestrasi seperti _Docker_ dan _Kubernetes_ untuk membangun aplikasi terdistribusi yang sangat dinamis [8].
- **Edge & Fog Computing**: Pemrosesan data yang digeser dari pusat data (_cloud server_) ke dekat sumber data atau perangkat ujung (_edge devices_) untuk mengurangi latency [8].
- **Web3 & DeFi (_Decentralized Finance_)**: Pemanfaatan ekosistem berbasis blockchain untuk transaksi finansial terbuka dan aplikasi terdesentralisasi tanpa perantara [8].
- **Federated Data & Federated Learning**: Pendekatan pembelajaran mesin (_machine learning_) dan kecerdasan buatan (AI) terdistribusi di mana model dilatih di perangkat lokal tanpa perlu mengumpulkan data mentah pengguna ke satu server terpusat, sehingga menjaga privasi data [8].

---

## 7. Kesimpulan

Perkembangan sistem terdistribusi dan terdesentralisasi merupakan jawaban atas tuntutan skalabilitas, ketersediaan tinggi, dan keandalan layanan informasi modern [2, 8].

Secara ringkas, perbedaan fundamental dari kedua konsep ini terletak pada fokus utamanya: **sistem terdistribusi berfokus pada pembagian beban kerja (_workload_)**, sedangkan **sistem terdesentralisasi berfokus pada pembagian wewenang atau kontrol (_control_)** [5, 8]. Meskipun memiliki perbedaan fokus, keduanya saling melengkapi dan menjadi fondasi utama bagi arsitektur teknologi informasi di masa kini dan masa depan [8].
