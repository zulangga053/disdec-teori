Nama Zul Ikhwanul Anggara
Nim 255410043

# Nama : Zul Ikhwanul Anggara

# Nim : 255410043

# Materi Minggu 02: Proses dan Komunikasi Antar Proses pada Sistem Terdistribusi

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Proses dan Komunikasi Antar Proses pada Sistem Terdistribusi.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Format penyusunan rangkuman pada materi, serta sebagian isi" pada materi yang di susun tersebut.

---

# Rangkuman Materi

## 1. Pendahuluan dan Konsep Dasar Proses

### 1.1 Definisi Proses dan Manajemen Proses

Dalam konteks sistem operasi dan komputasi dasar, **proses** didefinisikan sebagai program yang sedang dalam tahap eksekusi. Ketika sebuah aplikasi dijalankan, instruksi program dari disk diinstansiasi ke dalam RAM dan diabstraksi oleh sistem operasi melalui struktur data seperti _Process Control Block_ (PCB), alokasi memori, registrator, stack, serta thread. Penjadwal CPU (_CPU Scheduler_) kemudian mengatur eksekusi proses melalui mekanisme _context switch_ dan _preemption_.

Sistem operasi mengelola proses melalui tiga aktivitas utama:

1. **_Process Creation_**: Pembuatan proses baru berdasarkan permintaan aplikasi/pengguna, pengalokasian sumber daya (memori, file), dan inisialisasi lingkungan eksekusi.
2. **_Process Migration_**: Memindahkan proses yang sedang berjalan dari satu CPU atau node ke node lain untuk menyeimbangkan beban (_load balancing_), meningkatkan kinerja, atau menangani kegagalan sistem (_fault handling_).
3. **_Process Monitoring_**: Melacak status proses (seperti PID, status _running_/_sleeping_, persentase penggunaan CPU, dan memori) untuk memastikan sistem berjalan dengan baik.

### 1.2 Proses pada Sistem Terdistribusi

Sistem terdistribusi terdiri dari sekumpulan komputer atau _nodes_ independen yang saling berkoordinasi untuk bekerja sebagai satu kesatuan logis. Di dalam arsitektur terdistribusi:

- Setiap node menjalankan prosesnya sendiri.
- Proses-proses tersebut berada pada node yang terpisah secara fisik.
- **Tidak ada memori terbagi (_shared memory_)** secara fisik di antara node-node yang berbeda.

Oleh karena itu, agar komputer-komputer tersebut dapat berkoordinasi secara harmonis sebagai satu sistem tunggal, diperlukan mekanisme khusus **Komunikasi Antar Proses** (_Inter-Process Communication_ / IPC) yang dirancang untuk lingkungan terdistribusi melalui jaringan.

---

## 2. Model Komunikasi Pesan dan Jenis IPC

### 2.1 Taksonomi IPC

Secara umum, terdapat 4 jenis dasar mekanisme IPC:

1. **Shared Memory**: Pemrosesan melalui area memori bersama. Metode ini tidak cocok untuk sistem terdistribusi karena tidak ada akses memori fisik langsung antar node yang terpisah.
2. **Message Passing**: Pertukaran data antar proses secara langsung menggunakan pesan melalui saluran komunikasi jaringan.
3. **Remote Procedure Call (RPC)**: Pemanggilan fungsi/prosedur yang dieksekusi pada server jarak jauh.
4. **Middleware**: Infrastruktur perantara yang mengelola pertukaran pesan secara asinkron.

### 2.2 Model Komunikasi Pesan (Direct, Indirect, dan Pub/Sub)

Penyampaian pesan antar node dalam sistem terdistribusi dapat dikategorikan ke dalam tiga model utama:

| Model Komunikasi                  | Prinsip Kerja                                                                                                                                                            | Contoh Implementasi                         |
| :-------------------------------- | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------ |
| **Langsung (_Direct_)**           | Komunikasi poin-ke-poin secara langsung antar proses menggunakan abstraksi _socket_.                                                                                     | Socket Programming (TCP/UDP)                |
| **Tidak Langsung (_Indirect_)**   | Komunikasi melalui perantara seperti _mailboxes_ atau antrean pesan (_message queues_).                                                                                  | Perintah `write` pada Linux / Message Queue |
| **Publish / Subscribe (Pub/Sub)** | _Publisher_ mengirimkan pesan (_event_) ke suatu _Topic_ tanpa mengenal siapa penerimanya, dan _Subscriber_ yang mendaftar ke _Topic_ tersebut akan menerima notifikasi. | Pub/Sub messaging middleware                |

Contoh penggunaan soket langsung dalam Python untuk komunikasi server dan client:

```python
# Server Socket (TCP)
import socket

HOST = '127.0.0.1'
PORT = 65432

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.bind((HOST, PORT))
    s.listen()
    conn, addr = s.accept()
    with conn:
        print(f"Connected by {addr}")
        while True:
            data = conn.recv(1024)
            if not data:
                break
            conn.sendall(data) # Echo back data
```

```python
# Client Socket (TCP)
import socket

HOST = '127.0.0.1'
PORT = 65432

with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    s.connect((HOST, PORT))
    s.sendall(b"Hello, world!")
    data = s.recv(1024)

print(f"Received from server: {data.decode()}")
```

### 2.3 Message Passing Interface (MPI)

_Message Passing Interface_ (MPI) merupakan standar internasional untuk komunikasi pengiriman pesan pada sistem terdistribusi komputasi kinerja tinggi (_High Performance Computing_). Standardisasi MPI dikembangkan oleh MPI Forum (dengan versi terbaru versi 5.0 per Oktober 2026). Dalam praktiknya, MPI menyediakan fungsi primitif utama seperti `send(Q, message)` dan `receive(P, message)`. Di lingkungan bahasa Python, MPI diimplementasikan melalui pustaka `mpi4py`.

### 2.4 Remote Procedure Call (RPC)

RPC memisahkan kompleksitas komunikasi jaringan dengan memungkinkan suatu proses klien memanggil prosedur atau fungsi yang berada di server remote seolah-olah fungsi tersebut merupakan fungsi lokal.

Komponen utama RPC meliputi:

- **Client Stub**: Mengemas (_marshals_) parameter fungsi menjadi format pesan jaringan dan mengembangkannya ke server.
- **Server Stub**: Menerima pesan jaringan, mengurai (_unmarshals_) parameter, memanggil fungsi lokal di server, dan mengembalikan hasilnya kembali ke klien.

Salah satu framework RPC modern adalah **gRPC**, yang dikembangkan oleh Google menggunakan kerangka _Protocol Buffers_ (Protobuf) untuk mendefinisikan struktur pesan dan layanan antarbahasa (misalnya C++ Service berkomunikasi dengan Ruby atau Android-Java Client).

### 2.5 Message-Oriented Middleware (MOM)

MOM adalah infrastruktur perangkat lunak yang memfasilitasi pertukaran data antar aplikasi heterogen melalui pengiriman unit informasi mandiri yang disebut pesan (_message_) secara asinkron.

Beberapa standar dan protokol MOM populer yang digunakan di industri:

1. **AMQP** (_Advanced Message Queuing Protocol_): Protokol antrean pesan standar untuk aplikasi tingkat enterprise.
2. **MQTT** (_MQ Telemetry Transport_): Protokol pesan ringan yang dirancang khusus untuk perangkat dengan sumber daya terbatas (_resource-constrained devices_ / IoT).
3. **DDS** (_Data Distribution Service_ dari OMG): Protokol Pub/Sub untuk komunikasi real-time pada sistem terdistribusi.
4. **XMPP** (_eXtensible Messaging and Presence Protocol_): Protokol berbasis XML untuk komunikasi instan dan kehadiran.
5. **HLA IEEE 1516** (_High Level Architecture_): Standar arsitektur komunikasi simulasi terdistribusi dari IEEE dan SISO.
6. **XATMI** (_X/Open XATMI specification_): Spesifikasi pemrosesan transaksi terdistribusi.

---

## 3. Microservices serta Pola Komunikasi Sinkron vs Asinkron

### 3.1 Arsitektur Microservices

Arsitektur microservices membagi satu aplikasi besar menjadi sekumpulan layanan mandiri kecil yang masing-masing mengelola domain bisnis dan basis data (_database_) tersendiri. Sebagai contoh, pada platform transportasi online seperti Gojek atau Grab, _User Interface_ berinteraksi dengan berbagai layanan terpisah seperti _Driver Service_, _Location Tracing Service_, dan _Billing Service_ melalui antarmuka komunikasi.

### 3.2 Komunikasi Sinkron vs Asinkron

Dalam menghubungkan antar layanan/proses terdistribusi, terdapat dua pola interaksi utama:

- **Komunikasi Sinkron (_Synchronous_)**:
  - Klien mengirimkan permintaan (_request_) ke server dan menghentikan sementara eksekusi (_blocking_) untuk menunggu balasan (_response_) dari server sebelum melanjutkan proses.
  - Contoh: REST API over HTTP, gRPC.
  - **Kelebihan**: Alur eksekusi sederhana dan mudah dipahami.
  - **Kekurangan**: Memiliki ketergantungan waktu (_tight coupling_) dan rawan menyebabkan kaskade kegagalan jika server lambat atau crash.

- **Komunikasi Asinkron (_Asynchronous_)**:
  - Klien mengirimkan pesan lalu dapat langsung melanjutkan tugas lain (_continue working_) tanpa menunggu respon balasan secara langsung. Respon akan ditangani secara terpisah saat tersedia.
  - Contoh: Message Broker (RabbitMQ, Apache Kafka) berbasis MOM.
  - **Kelebihan**: Komunikasi bersifat _decoupled_, lebih toleran terhadap kegagalan, dan meningkatkan respon sistem.

---

## 4. Masalah Utama dan Protokol Komunikasi

### 4.1 Tantangan dalam Komunikasi Antar Proses

Komunikasi jaringan pada sistem terdistribusi dihadapkan pada 4 masalah utama:

1. **_Reliability_ (Keandalan)**: Memastikan pesan terkirim secara lengkap dan berurutan meskipun terjadi kegagalan jaringan atau server crash.
   - **Solusi**: Penggunaan sinyal konfirmasi (_acknowledgement_), mekanisme transmisi ulang (_retransmission_), dan protokol transpor andal seperti TCP.
2. **_Ordering_ (Pengurutan Pesan)**: Memastikan pesan diproses sesuai urutan logis yang diharapkan.
   - **Solusi**: Penggunaan antrean FIFO (_Pipes_), _message queues_, sockets, dan RPC.
3. **_Security_ (Keamanan)**: Melindungi data dari penyadapan atau manipulasi.
   - **Solusi**: Penerapan enkripsi (SSL/TLS) dan mekanisme autentikasi (OAuth2).
4. **_Performance_ (Kinerja)**: Meminimalkan _latency_ (waktu tunda pengiriman) dan memaksimalkan _bandwidth_ (kapasitas transfer data).

### 4.2 Protokol Komunikasi Terdistribusi

Struktur protokol komunikasi dibagi menjadi protokol lapisan bawah/transportasi dan protokol tingkat aplikasi:

- **Protokol Lapisan Bawah (_Transport Layer_)**:
  - **TCP**: Mengutamakan keandalan dan pengurutan data.
  - **UDP**: Mengutamakan kecepatan tanpa overhead koneksi.
  - **HTTP/2 & HTTP/3 (QUIC)**: HTTP/2 menggunakan TCP dengan multiplexing, sedangkan HTTP/3 berjalan di atas protokol **QUIC** berbasis UDP untuk mengurangi latensi koneksi.
- **Protokol Aplikasi / Lapisan Atas**:
  - **gRPC**, **GraphQL**, dan **REST**: Berjalan di atas HTTP sebagai protokol transpor dasar untuk pertukaran data antar mikroservis.

---

## 5. Sinkronisasi Jam dan Logical Clock

### 5.1 Masalah Sinkronisasi Waktu Fisik (_Physical Clock_)

Setiap node dalam sistem terdistribusi memiliki jam (_clock_) lokal tersendiri. Akibat keterbatasan perangkat keras dan variasi suhu, jam fisik mengalami pergeseran (_clock drift_). Selain itu, adanya latensi jaringan yang dinamis menyebabkan waktu fisik lokal di setiap komputer tidak pernah benar-benar sama.

Untuk menyelaraskan jam fisik, digunakan metode:

- **Sinkronisasi Eksternal**: Memanfaatkan jam atom (_Atomic Clock_) yang menghasilkan _International Atomic Time_ (TAI), GPS, dan UTC.
- **Protokol Jaringan**: Network Time Protocol (**NTP**) dan Precision Time Protocol (**PTP**), di mana PTP memberikan tingkat presisi yang lebih tinggi daripada NTP.

### 5.2 Lamport Logical Clock

Ketika jam fisik tidak dapat diandalkan untuk menentukan urutan kejadian (_event ordering_), Leslie Lamport memperkenalkan konsep **Logical Clock**. Logical clock tidak bergantung pada waktu dunia nyata, melainkan pada urutan mekanis kejadian (_causal ordering_).

**Aturan Kerja Lamport Logical Clock**:

1. Setiap kejadian internal pada suatu node menambahkan angka _logical clock_ sebanyak **+1**.
2. Ketika mengirimkan pesan, proses melampirkan nilai _logical clock_ terbarunya pada pesan tersebut.
3. Ketika menerima pesan, proses penerima membandingkan nilai _logical clock_ lokal dengan nilai _logical clock_ yang terdapat pada pesan, mengambil angka terbesar ( $\max(\text{clock}_{\text{lokal}}, \text{clock}_{\text{pesan}})$ ), lalu menambahkan **+1**.

#### Skenario Contoh: Logging 3 Mikroservis

Diberikan 3 layanan yang awalnya memiliki _logical clock_ = 0:

1. **Sales Service**: Mencatat transaksi penjualan $\rightarrow$ Clock lokal menjadi **1** ($0 + 1$). Mengirim pesan ke Product Service dengan timestamp **1**.
2. **Product Service**: Menerima pesan ber-timestamp **1**. Membandingkan clock lokal (0) dengan timestamp pesan (1) $\rightarrow$ $\max(0, 1) + 1 = \mathbf{2}$. Mengirim pesan ke Invoice Service dengan timestamp **2**.
3. **Invoice Service**: Menerima pesan ber-timestamp **2**. Membandingkan clock lokal (0) dengan timestamp pesan (2) $\rightarrow$ $\max(0, 2) + 1 = \mathbf{3}$. Faktur dicetak dengan timestamp logical **3**.

Mekanisme ini menjamin urutan kejadian yang konsisten secara logis: Sales Service (1) $\rightarrow$ Product Service (2) $\rightarrow$ Invoice Service (3).

### 5.3 Evolusi Algoritma Logical Clock

Sistem _scalar clock_ buatan Lamport kemudian dikembangkan lebih lanjut untuk menangani deteksi kausalitas yang lebih rinci:

- **Vector Clocks** (Colin Fidge & Friedemann Mattern, 1988)
- **Plausible Clocks** (Torres-Rojas & Ahamad, 1999)
- **Chain Clocks** (Agarwal & Garg, 2005)
- **Interval Tree Clocks** (Almeida et al., 2008)
- **Blum Clocks** (Lum Ramabaja, 2019)

---

## 6. Koordinasi Proses dan Tantangan Sistem Terdistribusi

### 6.1 Koordinasi Proses

**Koordinasi proses** adalah mekanisme yang memungkinkan beberapa proses independen di berbagai komputer untuk bekerja sama secara harmonis, memastikan mereka mencapai tujuan bersama tanpa terjadi konflik seperti _deadlock_ atau _race condition_.

Perangkat lunak koordinator (_coordination service_) yang umum digunakan di industri meliputi:

- **Apache ZooKeeper**, **etcd**, **Consul**, **ClickHouse Keeper**, **Oxia**, dan **KRaft**.

### 6.2 Ringkasan Tantangan dan Solusi Teknis

| Masalah / Tantangan       | Penyebab                              | Solusi Teknis                                       |
| :------------------------ | :------------------------------------ | :-------------------------------------------------- |
| **_Message Loss_**        | Kegagalan jaringan / paket hilang     | Retransmisi data dan sinyal _Acknowledgement_ (ACK) |
| **_Deadlock_**            | Saling menunggu sumber daya           | Mekanisme _Timeout_ dan _Detection Algorithm_       |
| **_Data Inconsistency_**  | Partisi jaringan / pembaruan simultan | Protokol Konsensus (misal: **Raft**, **Paxos**)     |
| **_Keamanan (Security)_** | Penyadapan dan akses ilegal           | Enkripsi SSL/TLS dan Autentikasi OAuth2             |

### 6.3 Isu Terkini (_Current Issues_)

Perkembangan terkini dalam ranah proses dan IPC terdistribusi mencakup:

- **Edge & Fog Computing**: Menggeser komputasi proses mendekati lokasi fisik sumber data.
- **Observability & Tracing (Telemetry)**: Pemantauan alur pesan dan jejak eksekusi (_tracing_) antar mikroservis terdistribusi.
- **Container Orchestration**: Pengelolaan dan migrasi otomatis kontainer proses menggunakan **Kubernetes** atau **Docker Swarm**.

---

## 7. Kesimpulan

Proses dan komunikasi antar proses (IPC) merupakan fondasi utama dari arsitektur sistem terdistribusi. Karena sistem terdistribusi tidak memiliki memori bersama (_shared memory_), seluruh koordinasi antar node harus dilakukan melalui pertukaran pesan melalui jaringan.

Pemilihan model IPC (seperti Socket, RPC/gRPC, atau Message-Oriented Middleware), pola komunikasi (sinkron vs asinkron), serta protokol yang tepat sangat menentukan keandalan, kinerja, dan keamanan sistem. Selain itu, penggunaan _Logical Clock_ (seperti Lamport Clock) dan layanan koordinasi proses menjadi kunci dalam menjaga konsistensi urutan kejadian dan mencegah konflik di tengah ketidakpastian jam fisik dan latensi jaringan terdistribusi.
