# Materi Minggu 01: Pengenalan Sistem Terdistribusi dan Terdesentralisasi

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Pengenalan Sistem Terdistribusi dan Terdesentralisasi.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Format penyusunan rangkuman pada materi, serta sebagian isi" pada materi yang di susun tersebut.

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
4. **Kinerja Tinggi (_High Performance_)**: Pembagian beban kerja ke banyak komputer memungkinkan pemrosesan data dilakukan secara paralel sehingga meningkatkan throughput.
5. **Heterogenitas**: Komponen-komponen dalam sistem dapat terdiri dari berbagai variasi perangkat keras, sistem operasi, dan protokol jaringan yang berbeda namun tetap dapat saling berinteraksi .

### 2.3 Arsitektur Sistem Dasar

Dalam praktiknya, arsitektur dasar sistem terdistribusi dapat dikelompokkan ke dalam tiga model utama:

| Model Arsitektur       | Deskripsi dan Prinsip Kerja                                                                                                                                                                                                                                                                                                                 | Contoh Penerapan                                            |
| :--------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | :---------------------------------------------------------- |
| **Client-Server**      | Terdapat pemisahan peran yang jelas antara penyedia layanan (_server_) dan peminta layanan (_client_). Client mengirimkan permintaan melalui jaringan (misalnya REST API via API Gateway) ke mikroservis terpisah (seperti _Account Service_, _Inventory Service_, _Shipping Service_) yang masing-masing terhubung ke basis data terpisah. | Aplikasi Web & Mobile (e-commerce, sistem informasi kampus) |
| **Peer-to-Peer (P2P)** | Setiap komputer atau node dalam jaringan memiliki kedudukan yang setara (berperan sebagai client sekaligus server secara bersamaan) dan berkomunikasi langsung antar peer tanpa bergantung pada server pusat.                                                                                                                               | BitTorrent, Jaringan Kripto / P2P File Sharing              |
| **Hybrid**             | Menggabungkan model Client-Server dan P2P. Biasanya menggunakan _Supernodes_ atau server terpusat untuk fungsi koordinasi dan otentikasi awal, sedangkan transmisi data atau transmisi pesan dilakukan secara P2P antar node.                                                                                                               | Aplikasi perpesanan instan (WhatsApp, Telegram, Signal)     |

---

## 3. Sistem Terdesentralisasi dan Perbandingannya

### 3.1 Pengertian Sistem Terdesentralisasi

Sistem terdesentralisasi lahir sebagai bentuk evolusi untuk mengatasi kelemahan sistem terpusat (_centralized system_). Pada sistem terpusat, seluruh kendali dan pemrosesan bergantung pada satu titik kontrol (_single point of failure_). Jika titik pusat tersebut mengalami gangguan, maka seluruh sistem akan lumpuh.

Sebaliknya, pada **sistem terdesentralisasi**, kontrol dan wewenang pengambilan keputusan didistribusikan ke banyak node dalam jaringan. Tidak ada otoritas tunggal atau _single point of failure_. Keputusan kolektif dalam sistem terdesentralisasi dicapai melalui mekanisme **konsensus** antar node yang berpartisipasi.

### 3.2 Perbedaan Utama: Sistem Terdistribusi vs Sistem Terdesentralisasi

Meskipun sering kali digunakan secara bersamaan, sistem terdistribusi dan terdesentralisasi memiliki fokus utama yang berbeda :

- **Sistem Terdistribusi**: Berfokus utama pada **pembagian beban kerja (_workload distribution_)**. Tujuan utamanya adalah meningkatkan performa, skalabilitas, dan keandalan pemrosesan komputasi. Contoh fisiknya meliputi _Google File System (GFS)_ dan _MinIO Object Storage_ (yang kemudian di-fork menjadi Silo).
- **Sistem Terdesentralisasi**: Berfokus utama pada **pembagian kontrol (_control distribution_)**. Tujuan utamanya adalah menghilangkan ketergantungan pada titik kendali terpusat dan membangun kepercayaan bersama melalui konsensus. Contoh utamanya adalah jaringan _Blockchain_.

| Dimensi Pembanding        | Sistem Terdistribusi                                    | Sistem Terdesentralisasi                                        |
| :------------------------ | :------------------------------------------------------ | :-------------------------------------------------------------- |
| **Fokus Utama**           | Pembagian beban kerja (_workload_)                      | Pembagian wewenang/kontrol (_control_)                          |
| **Titik Kontrol**         | Dapat berupa terpusat secara logis oleh satu organisasi | Tersebar di banyak entitas/node independen                      |
| **Pengambilan Keputusan** | Ditentukan oleh arsitektur internal sistem              | Menggunakan mekanisme konsensus kolektif                        |
| **Tujuan Utama**          | Efisiensi, skalabilitas, dan performa tinggi            | Otonomi, transparansi, dan pencegahan _single point of failure_ |
| **Contoh Sistem**         | Google File System (GFS), MinIO, HDFS                   | Blockchain (Bitcoin, Ethereum), DNS, BitTorrent                 |

---

## 4. Teorema CAP dan Konsistensi Data

### 4.1 Teorema CAP (Brewer's Theorem)

Dalam merancang sistem penyimpanan data terdistribusi (_distributed data store_), terdapat sebuah hukum fundamental yang dikenal sebagai **Teorema CAP** atau **Brewer’s Theorem**, yang diperkenalkan oleh Eric Brewer pada tahun 2000.

Teorema CAP menyatakan bahwa sebuah sistem data terdistribusi hanya dapat menjamin maksimal **dua dari tiga** properti berikut secara bersamaan:

1. **Consistency (C)**: Setiap operasi pembacaan data akan mengembalikan hasil penulisan data terbaru atau mengembalikan pesan kesalahan (_error_).
2. **Availability (A)**: Setiap permintaan ke node yang tidak rusak (_non-failing node_) pasti menerima respon non-error, meskipun respon tersebut mungkin bukan data yang paling baru.
3. **Partition Tolerance (P)**: Sistem tetap dapat beroperasi dan berfungsi dengan baik meskipun komunikasi antar node terputus atau terfragmentasi (_network partition_).

Karena potensi gangguan jaringan (_partition_) selalu ada dalam dunia nyata, properti _Partition Tolerance_ (P) wajib dimiliki oleh sistem terdistribusi. Oleh karena itu, perancang sistem harus memilih _trade-off_ antara dua pendekatan utama ketika terjadi partisi jaringan:

- **AP (_Availability + Partition Tolerance_)**: Sistem mengutamakan ketersediaan layanan. Permintaan data akan selalu dilayani meskipun ada risiko data yang diterima pengguna belum tersinkronisasi (mengorbankan konsistensi). Contoh sistem AP adalah **MongoDB**.
- **CP (_Consistency + Partition Tolerance_)**: Sistem mengutamakan konsistensi data. Permintaan baca maupun tulis akan ditolak atau dibatalkan jika terjadi partisi jaringan untuk memastikan tidak ada data yang tidak konsisten (mengorbankan ketersediaan). Contoh sistem CP adalah **Apache Cassandra**.

### 4.2 Model Konsistensi Data

Tingkat konsistensi data dalam sistem terdistribusi umumnya dibagi menjadi dua kategori utama :

- **Strong Consistency**: Menjamin bahwa seluruh operasi pembacaan di node mana pun akan selalu menghasilkan data yang sama dan paling baru secara instan. Model ini sangat krusial untuk aplikasi kritis seperti **Sistem Perbankan**, di mana saldo rekening harus selalu tepat dan konsisten.
- **Eventual Consistency**: Menjamin bahwa data di berbagai node akan menjadi konsisten secara bertahap seiring berjalannya waktu (_eventually_). Model ini cocok untuk aplikasi seperti **Media Sosial**, di mana keterlambatan beberapa detik dalam pembaruan postingan atau jumlah _like_ tidak berdampak kritis.

---

## 5. Studi Kasus Penerapan di Industri

Penerapan sistem terdistribusi dan terdesentralisasi telah menjadi tulang punggung berbagai platform digital terkemuka :

### 5.1 Studi Kasus Sistem Terdistribusi

- **Penyimpanan Data Skala Besar (_Big Data Store_)**: _Google File System (GFS)_ dan _Hadoop Distributed File System (HDFS)_ digunakan untuk menyimpan dan mengelola petabyte data secara terdistribusi di ribuan server.
- **Messaging & Event Streaming**: _Apache Kafka_ dan _Apache Pulsar_ mengelola alur data pesan dan pemrosesan kejadian (_event streaming_) berkecepatan tinggi secara terdistribusi.
- **Platform Streaming Video**: _Netflix, YouTube, dan Twitch_ memanfaatkan infrastruktur pemrosesan dan pengiriman konten terdistribusi untuk melayani jutaan penonton secara bersamaan tanpa _downtime_.
- **Platform Media Sosial**: _Instagram, Threads, Facebook, dan TikTok_ mengelola lalu lintas data interaktif dan media berskala raksasa melalui arsitektur microservices terdistribusi.

### 5.2 Studi Kasus Sistem Terdesentralisasi

- **Teknologi Blockchain**: Platform seperti _Bitcoin, Ethereum, Algorand, Sui, Near, dan Aptos_ menggunakan jaringan terdesentralisasi berbasis konsensus untuk mencatat transaksi keuangan dan _smart contracts_ tanpa perantara lembaga keuangan pusat.
- **Domain Name System (DNS)**: Sistem pemetaan nama domain ke alamat IP yang terdistribusi dan terdesentralisasi secara hierarkis di seluruh dunia.
- **BitTorrent**: Protokol berbagi file Peer-to-Peer terdesentralisasi yang memungkinkan pengguna mengunduh bagian-bagian file dari banyak peer secara bersamaan.

---

## 6. Tantangan dan Tren Masa Depan

### 6.1 Tantangan Teknis dan Operasional

Mengembangkan dan mengelola sistem terdistribusi serta terdesentralisasi memiliki kompleksitas yang tinggi :

1. **Latency dan Throughput**:
   - _Latency_: Waktu tunda yang dibutuhkan sistem untuk memproses satu permintaan.
   - _Throughput_: Jumlah total permintaan atau volume data yang dapat diproses oleh sistem dalam satu satuan waktu. Meminimalkan latency sambil memaksimalkan throughput pada jaringan yang luas merupakan tantangan yang rumit.
2. **Keamanan Siber**:
   - **DDoS Attack**: Serangan yang membanjiri lalu lintas server hingga lumpuh.
   - **Sybil Attack**: Serangan siber di mana penyerang membuat banyak identitas palsu (akun, node, atau pseudonim hingga ribuan) secara bersamaan untuk mendominasi suara, reputasi, atau pengaruh dalam jaringan terdesentralisasi. Istilah ini diambil dari nama seorang wanita yang memiliki gangguan identitas dalam sebuah buku terbitan tahun 1973. Penyerang yang menyamar dapat memanipulasi konsensus dan merusak integritas sistem.
3. **Kompleksitas Manajemen Node**: Kesulitan dalam mengoordinasikan, memantau, dan memelihara ribuan node yang bersifat heterogen dan tersebar secara geografis.
4. **Tata Kelola (_Governance_)**: Pada sistem terdesentralisasi, pengambilan keputusan mengenai pembaruan protokol atau regulasi jaringan memerlukan mekanisme tata kelola berbasis komunitas yang kompleks.

### 6.2 Tren Masa Depan

Teknologi sistem terdistribusi dan terdesentralisasi terus berkembang menuju arah berikut :

- **Cloud-Native Infrastructure**: Penggunaan teknologi kontainerisasi dan orkestrasi seperti _Docker_ dan _Kubernetes_ untuk membangun aplikasi terdistribusi yang sangat dinamis.
- **Edge & Fog Computing**: Pemrosesan data yang digeser dari pusat data (_cloud server_) ke dekat sumber data atau perangkat ujung (_edge devices_) untuk mengurangi latency.
- **Web3 & DeFi (_Decentralized Finance_)**: Pemanfaatan ekosistem berbasis blockchain untuk transaksi finansial terbuka dan aplikasi terdesentralisasi tanpa perantara.
- **Federated Data & Federated Learning**: Pendekatan pembelajaran mesin (_machine learning_) dan kecerdasan buatan (AI) terdistribusi di mana model dilatih di perangkat lokal tanpa perlu mengumpulkan data mentah pengguna ke satu server terpusat, sehingga menjaga privasi data.

---

## 7. Kesimpulan

Perkembangan sistem terdistribusi dan terdesentralisasi merupakan jawaban atas tuntutan skalabilitas, ketersediaan tinggi, dan keandalan layanan informasi modern.

Secara ringkas, perbedaan fundamental dari kedua konsep ini terletak pada fokus utamanya: **sistem terdistribusi berfokus pada pembagian beban kerja (_workload_)**, sedangkan **sistem terdesentralisasi berfokus pada pembagian wewenang atau kontrol (_control_)**. Meskipun memiliki perbedaan fokus, keduanya saling melengkapi dan menjadi fondasi utama bagi arsitektur teknologi informasi di masa kini dan masa depan.
