# Nama : Zul Ikhwanul Anggara

# Nim : 255410043

# Materi Minggu 05: Fault Tolerance

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Fault Tolerance.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Format penyusunan rangkuman pada materi, serta sebagian isi" pada materi yang di susun tersebut.

---

# Rangkuman Materi

## 1. Pendahuluan dan Arti Penting Fault Tolerance

### 1.1 Pengertian Fault Tolerance

Dalam arsitektur sistem terdistribusi dan terdesentralisasi, **_Fault Tolerance_** (Toleransi Kesalahan) didefinisikan sebagai kemampuan suatu sistem untuk mempertahankan operasional secara tepat dan normal meskipun terjadi kegagalan atau kesalahan (_fault_/_failure_) pada satu atau lebih komponennya.

Sistem yang menerapkan toleransi kesalahan tidak akan langsung mengalami lumpuh (_crash_) total saat terjadi gangguan internal. Sebaliknya, sistem mampu mengisolasi kesalahan, menutupi kesalahan dari pengguna (_fault masking_), atau memulihkan diri (_self-healing_) sehingga fungsi utama aplikasi tetap berjalan sebagaimana mestinya.

### 1.2 Arti Penting pada Sistem Terdistribusi dan Terdesentralisasi

Sistem terdistribusi modern terdiri atas ratusan hingga ribuan komponen independen (server, jaringan, basis data, perangkat penyimpanan) yang saling terhubung. Karakteristik ini menimbulkan beberapa tantangan kritis:

1. **Kompleksitas Komponen dan Jaringan**: Semakin banyak komponen yang terlibat, semakin besar probabilitas terjadinya kegagalan pada salah satu komponen atau saluran komunikasi antar komponen.
2. **Kebutuhan _High Availability_ (HA)**: Semakin krusial suatu aplikasi (misalnya pada sistem _mission-critical_, _life-critical_, transaksi finansial, atau platform skala global), semakin tinggi tuntutan ketersediaan layanan 24/7 tanpa _downtime_.
3. **Pencegahan _Single Point of Failure_ (SPOF)**: Tanpa perancangan toleransi kesalahan yang matang, kegagalan pada satu node tunggal dapat merembet dan melumpuhkan seluruh ekosistem aplikasi (_cascading failure_).

---

## 2. Jenis-Jenis Fault dan Klasifikasi Berdasarkan Sumber

### 2.1 Taksonomi Jenis Fault

Kesalahan atau kegagalan pada sistem dapat dikategorikan berdasarkan pola kemunculannya:

- **_Transient Fault_**: Kesalahan yang bersifat sementara dan akan hilang dengan sendirinya setelah beberapa saat. Contohnya: _packet loss_ akibat kepadatan jaringan singkat atau lonjakan listrik akibat sambaran petir.
- **_Intermittent Fault_**: Kesalahan yang muncul dan hilang secara berulang kali tanpa pola yang pasti. Contohnya: ketidakstabilan konektor fisik atau penurunan kinerja server akibat suhu berlebih (_overheat_) pada perangkat keras.
- **_Permanent Fault_**: Kesalahan yang bersifat tetap dan tidak akan pulih sampai ada intervensi perbaikan fisik atau penggantian komponen. Contohnya: _disk crash_, modul RAM rusak, atau kabel serat optik terputus.
- **_Byzantine Fault_**: Kesalahan paling kompleks di mana sebuah komponen atau node memberikan respon yang tidak konsisten, acak, atau bahkan menyesatkan (misalnya akibat malfungsi logika atau adanya node pengkhianat/_traitor_ pada jaringan terdesentralisasi/blockchain).

### 2.2 Klasifikasi Fault Berdasarkan Sumber

Berdasarkan asal muasal penyebabnya, kesalahan dapat diklasifikasikan ke dalam 3 kategori utama:

| Kategori Fault             | Sumber / Penyebab Utama                            | Contoh Kasus                                                                               |
| :------------------------- | :------------------------------------------------- | :----------------------------------------------------------------------------------------- |
| **_Hardware Faults_**      | Malfungsi pada perangkat keras fisik.              | _Disk crash_, kegagalan daya (_power failure_), pemprosesan CPU _overheat_.                |
| **_Software Faults_**      | Kesalahan pada alur program atau logika pengodean. | _Software bugs_, _memory leak_, kondisi pacuan (_race condition_), _unhandled exceptions_. |
| **_Communication Faults_** | Gangguan pada saluran transmisi jaringan komputer. | _Packet loss_, partisi jaringan (_network partition_), _latency jitter_ tinggi.            |

---

## 3. _Design Patterns_ untuk Mencapai Fault Tolerance

Untuk membangun sistem terdistribusi yang andal, industri perangkat lunak memanfaatkan beberapa pola desain (_design patterns_) utama:

### 3.1 Redundansi dan Replikasi (_Redundancy & Replication_)

Prinsip dasar pola ini adalah menyediakan lebih dari satu salinan sumber daya (_resources_) atau node di lokasi yang berbeda. Dengan mendistribusikan salinan data dan proses ke beberapa server, sistem dapat menghilangkan risiko _Single Point of Failure_ (SPOF). Jika salah satu node mengalami kegagalan, node replika dapat langsung mengambil alih tugas tersebut secara transparan.

### 3.2 _Load Balancing_

_Load balancing_ adalah teknik melakukan _scaling_ aplikasi menjadi beberapa _instance_ aktif (satu _instance_ merupakan satu proses aplikasi utuh yang berjalan). Pemeta beban (_load balancer_ seperti Nginx atau HAProxy) bertugas mengarahkan lalu lintas permintaan pengguna secara merata ke instance-instance tersebut. Jika salah satu instance aplikasi mengalami kegagalan, _load balancer_ akan mendeteksi dan secara otomatis menghentikan pengiriman _request_ ke instance yang rusak.

### 3.3 _Checkpointing and Rollback Recovery_

- **_Checkpointing_**: Proses menyimpan status (_state_) sistem secara berkala ke media penyimpanan non-volatile (seperti disk atau _object storage_).
- **_Rollback Recovery_**: Proses mengembalikan status sistem ke titik _checkpoint_ terakhir yang valid ketika terjadi kegagalan.
- **Manfaat**: Memungkinkan sistem untuk memulihkan proses tanpa harus memulai ulang eksekusi dari awal, sehingga meminimalkan kehilangan data dan menghemat waktu komputasi.

### 3.4 _Heartbeat and Failure Detection_

- **_Heartbeat_**: Sinyal periodik yang dikirimkan secara berkala antar komponen atau dari node pekerja (_worker_) ke node pemantau (_monitor_) untuk memberi tahu bahwa komponen tersebut dalam kondisi aktif.
- **_Failure Detection_**: Proses mengevaluasi sinyal _heartbeat_. Jika node pemantau tidak menerima sinyal _heartbeat_ dari suatu komponen dalam batas waktu tertentu (_timeout_), komponen tersebut dinyatakan gagal/mati (_dead/unresponsive_), sehingga sistem dapat memicu aksi pemulihan (_failover_).

### 3.5 _Retry Pattern_

Pola ini digunakan untuk mengatasi _transient fault_ (seperti kegagalan jaringan sementara atau _service busy_). Alih-alih langsung mengembalikan pesan error ke pengguna, aplikasi akan melakukan percobaan ulang (_retry_) pengiriman permintaan secara berkala dengan jeda waktu tertentu. Dalam ekosistem Python, pola ini dapat diimplementasikan dengan mudah menggunakan pustaka `tenacity`.

### 3.6 _Circuit Breaker Pattern_

Pola _Circuit Breaker_ dirancang khusus untuk arsitektur _microservices_ guna mencegah kegagalan berantai (_cascading failure_). Sirkuit bertindak sebagai saklar otomatis dengan 3 status (_state machine_):

1. **_Closed_** (Normal): Semua permintaan diizinkan lewat. Sistem memantau rasio kegagalan. Jika rasio kegagalan melewati ambang batas (_threshold_), sirkuit berubah ke status _Open_.
2. **_Open_** (Memblokir): Semua permintaan langsung ditolak (_fail-fast_) tanpa menghubungi layanan yang rusak, memberikan waktu bagi layanan tujuan untuk memulihkan diri.
3. **_Half-Open_** (Uji Coba): Setelah jeda waktu tertentu (_reset timeout_), sirkuit mengizinkan sejumlah kecil permintaan uji coba. Jika uji coba berhasil, sirkuit kembali ke status _Closed_; jika gagal, sirkuit kembali ke status _Open_.

Dalam ekosistem Python, pola ini diimplementasikan menggunakan pustaka `pybreaker`.

### 3.7 _Consensus and Coordination_

- **Konsensus**: Proses di mana beberapa node terdistribusi menyepakati satu nilai, status, atau urutan keputusan bersama di tengah potensi kegagalan jaringan. Contoh algoritma konsensus meliputi **Raft** dan **Paxos**.
- **Koordinasi**: Pengelolaan interaksi antar node agar bekerja secara teratur.
- **Contoh Penerapan**:
  - _Database Leader Election_: Memilih node utama (_leader_) untuk mengelola transaksi penulisan (_write_).
  - _Cloud Storage File Updates_: Memastikan perubahan file (misalnya pada Google Drive) di-commit ke mayoritas server sebelum dinyatakan sukses.
  - _Blockchain_: Validasi transaksi terdesentralisasi tanpa otoritas terpusat.

---

## 4. _Actor Model_ untuk Supervisi Proses

### 4.1 Konsep dasar Actor Model

_Actor Model_ adalah model matematis untuk komputasi konkuren di mana **Actor** menjadi unit dasar komputasi. Setiap Actor bersifat independen, mengelola status internalnya sendiri, dan berkomunikasi dengan Actor lain hanya melalui pertukaran pesan asinkron (_mailbox_).

### 4.2 Pola Supervisor-Worker

Untuk mencapai _fault tolerance_, _Actor Model_ menerapkan hirarki **Supervisor-Worker**:

- _Supervisor Actor_ bertugas memantau kondisi kesehatan (_lifecycle_) dari _Worker Actor_ di bawahnya.
- Jika terjadi kegagalan/kesalahan pada _Worker Actor_, kesalahan diisolasi sehingga tidak merusak keseluruhan aplikasi.
- _Supervisor_ menangani kegagalan tersebut dengan strategi pemulihan (misalnya melakukan _restart_ otomatis pada worker yang rusak).

### 4.3 Implementasi di Berbagai Bahasa Pemrograman

| Bahasa Pemrograman  | Peranti / Pustaka Support Fault Tolerance & Actor Model                                                                                                |
| :------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Erlang / Elixir** | Memiliki dukungan _native/builtin_ untuk _Actor Model_ dan arsitektur _Supervisor-Worker_ (OTP framework).                                             |
| **Pony**            | Bahasa pemrograman yang dirancang khusus dari awal berbasis _Actor Model_ (ponylang.io).                                                               |
| **Python**          | Menggunakan pustaka pihak ketiga seperti `Xoscar` (xoscar.dev) untuk _Actor Model_, `tenacity` untuk _Retry_, dan `pybreaker` untuk _Circuit Breaker_. |
| **Java**            | Menggunakan `Apache Pekko` (atau Akka) untuk _Actor Model_, serta `Resilience4j` untuk _Circuit Breaker_ dan _Fault Tolerance_.                        |

---

## 5. Praktikum & Studi Kasus: Load Balancing Aplikasi Python BlackSheep dengan Nginx dan Docker

Untuk memahami penerapan _Load Balancing_ sebagai strategi _Fault Tolerance_ dan _High Availability_, dilakukan simulasi skenario penyebaran (_deployment_) aplikasi web ASGI Python menggunakan framework **BlackSheep**, dipadu dengan **Nginx** sebagai _Reverse Proxy / Load Balancer_, serta dikelola menggunakan **Docker Compose**.

### 5.1 Arsitektur Praktikum

- **Aplikasi Web**: Diinstal menggunakan `blacksheep-cli` berbasis Python 3.14 (dikendalikan via manajer lingkungan `uv`).
- **Multi-Instance Scaling**: Aplikasi di-scale menjadi 2 _instance_ kontainer terpisah (`bs_app-1` dan `bs_app-2`).
- **Reverse Proxy / Load Balancer**: Nginx menerima seluruh lalu lintas dari luar pada port 80 dan mendistribusikan permintaan secara otomatis ke kelompok _backend_ instance aplikasi.

### 5.2 Struktur File dan Konfigurasi

#### 1. Konfigurasi Nginx (`nginx/nginx.conf`)

Konfigurasi berikut menentukan grup _upstream_ bernama `backend` yang mengarah ke layanan `bs_app` pada port 80 internal kontainer:

```nginx
events {
    worker_connections 4096;
}

http {
    # Definisi grup backend upstream
    upstream backend {
        server bs_app:80;
    }

    server {
        listen 80;
        location / {
            proxy_pass http://backend;
        }
    }
}
```

#### 2. Konfigurasi Orchestration (`docker-compose.yml`)

File orchestration mengarahkan kompilasi kontainer aplikasi dan kontainer Nginx:

```yaml
services:
  # Aplikasi ASGI BlackSheep yang akan di-scale
  bs_app:
    build: ./blacksheep_lb

  # Proxy Nginx Load Balancer
  nginx:
    container_name: nginx
    build: ./nginx
    ports:
      - "80:80"
    depends_on:
      - bs_app
```

### 5.3 Eksekusi dan Verifikasi Load Balancing

Proses _scaling_ dan pembukaan layanan dijalankan melalui perintah Docker Compose:

```bash
sudo docker-compose up --build -d --scale bs_app=2
```

Saat dilakukan pengujian pengaksesan alamat `http://localhost`, Nginx menerima permintaan pada port 80 dan meneruskannya secara bergantian ke instance `load-balancing-bs_app-1` dan `load-balancing-bs_app-2`.

Melalui pemantauan log kontainer (`docker logs`), terlihat bahwa lalu lintas permintaan pengguna terbagi secara otomatis. Apabila salah satu instance aplikasi mengalami kegagalan, Nginx akan secara transparan mengalihkan seluruh lalu lintas ke instance yang masih sehat, sehingga ketersediaan layanan (_High Availability_) tetap terjaga tanpa interupsi bagi pengguna akhir.

---

## 6. Kesimpulan

_Fault Tolerance_ merupakan fondasi utama dalam pembangunan sistem terdistribusi dan terdesentralisasi modern. Mengingat kompleksitas jaringan dan banyaknya komponen yang terlibat, kegagalan sistem (_fault_) adalah hal yang tidak terelakkan. Oleh karena itu, pendekatan utama dalam merancang sistem bukan lagi menghindari kegagalan sepenuhnya, melainkan membangun sistem yang mampu mentoleransi, mengisolasi, dan memulihkan diri dari kegagalan secara otomatis.

Poin-poin utama kesimpulan pembelajaran minggu 05:

1. **Jenis & Klasifikasi Fault**: Kesalahan dapat bersifat _transient_, _intermittent_, _permanent_, atau _Byzantine_, yang bersumber dari kegagalan _hardware_, _software_, maupun saluran komunikasi jaringan.
2. **_Design Patterns_ Utama**: Penerapan kombinasi pola redundansi, _load balancing_, _checkpointing_, _heartbeat/failure detection_, _retry_, _circuit breaker_, dan algoritma konsensus terdistribusi terbukti dapat mencegah _Single Point of Failure_ dan kegagalan berantai.
3. **_Actor Model & Supervisi_**: Penggunaan pola _Supervisor-Worker_ mengisolasi kesalahan pada tingkat proses individu dan menyediakan mekanisme pemulihan berbasis _restart_ otomatis.
4. **Implementasi Praktis**: Kombinasi _Load Balancer_ Nginx dengan aplikasi terkontainerisasi berbasis Docker memungkinkan pencapaian _High Availability_ secara efektif pada infrastruktur modern.
