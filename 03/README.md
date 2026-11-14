# Materi Minggu 03: Sinkronisasi dan Koordinasi pada Sistem Terdistribusi

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Sinkronisasi dan Koordinasi pada Sistem Terdistribusi.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Format penyusunan rangkuman pada materi, serta sebagian isi" pada materi yang di susun tersebut.

---

# Rangkuman Materi

## 1. Konsep Dasar Sinkronisasi dan Mengapa Sinkronisasi Penting

### 1.1 Definisi Sinkronisasi Terdistribusi

Dalam lingkungan sistem terdistribusi, **sinkronisasi** adalah proses mengoordinasikan eksekusi proses, pengelolaan data, dan pemanfaatan sumber daya (_resources_) di berbagai _node_ jaringan untuk memastikan operasi sistem berjalan secara konsisten, aman, dan dapat diandalkan (_reliable_).

Karena sistem terdistribusi terdiri atas banyak komputer independen yang beroperasi secara paralel tanpa memori fisik bersama (_shared memory_), sinkronisasi menjadi mekanisme vital untuk menyelaraskan _state_ (keadaan sistem) dan mencegah konflik saat terjadi eksekusi bersamaan (_concurrency_).

### 1.2 Urgensi dan Manfaat Sinkronisasi

Sinkronisasi pada sistem terdistribusi memiliki peran penting untuk:

1. **Menjaga Integritas Data**: Mencegah terjadinya _data race_ dan _race condition_ akibat akses simultan ke sumber daya yang sama.
2. **Sinkronisasi _State_**: Memastikan _state_ aplikasi tetap konsisten di seluruh _node_. Contohnya:
   - **Online Multiplayer Game**: Mengirimkan posisi pemain, tingkat kesehatan (_health_), dan inventaris terbaru ke semua _client_ secara real-time agar tampilan permainan tetap sinkron.
   - **Aplikasi Web E-Commerce**: Menyinkronkan data keranjang belanja (_shopping cart_) antara _browser_ pengguna dan basis data di berbagai perangkat dan sesi.
3. **Koordinasi Tugas (_Task Coordination_)**: Mengatur urutan eksekusi tugas yang terdistribusi di beberapa server.
4. **Pengelolaan Sumber Daya (_Resource Management_)**: Mengatur giliran akses terhadap sumber daya kritis (seperti basis data atau file terbagi).
5. **Dukungan Pemulihan (_Recovery Mechanism_)**: Memelihara catatan _state_ konsisten untuk memfasilitasi pemulihan jika terjadi kegagalan _node_.
6. **Efisiensi dan Utilisasi**: Mencegah operasi redundan dan mendukung _load balancing_ yang optimal.
7. **Pencegahan _Deadlock_**: Memastikan tidak ada proses yang saling mengunci sumber daya secara permanen.

### 1.3 Perbedaan Kritis: _Data Race_ vs _Race Condition_

Sering kali istilah _data race_ dan _race condition_ disamakan, padahal keduanya terjadi pada tingkatan abstraksi dan implikasi yang berbeda:

- **_Data Race_**: Terjadi pada tingkatan _low-level_ (instruksi CPU dan akses memori fisik) ketika minimal dua _thread_ mengakes lokasi memori yang sama secara bersamaan, di mana minimal satu akses berupa operasi penulisan (_write_), tanpa adanya mekanisme penguncian (_mutex/lock_).
- **_Race Condition_**: Terjadi pada tingkatan _high-level_ (alur logika bisnis aplikasi) ketika kebenaran hasil eksekusi program bergantung pada urutan dan waktu eksekusi instruksi yang tidak pasti.

| Fitur / Parameter   | _Data Race_                                                                                                                                            | _Race Condition_                                                                             |
| :------------------ | :----------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------- |
| **Fokus Utama**     | Akses memori secara bersamaan (_concurrent memory access_).                                                                                            | Urutan dan waktu eksekusi instruksi (_timing & execution sequence_).                         |
| **Level Abstraksi** | _Low-level_ (Instrumen CPU / Akses Memori fisik).                                                                                                      | _High-level_ (Alur logika aplikasi / _Business logic_).                                      |
| **Syarat Terjadi**  | 1. Terdapat minimal 2 _thread_ mengakses memori sama.<br>2. Minimal 1 akses berupa operasi _write_.<br>3. Tanpa mekanisme sinkronisasi (_mutex/lock_). | Terjadi ketika kebenaran program bergantung pada urutan eksekusi yang tidak bisa diprediksi. |
| **Dampak Utama**    | _Undefined behavior_, korpsi memori, data tidak konsisten.                                                                                             | Output salah, _bug_ logika bisnis (misal: saldo minus, eksekusi ganda).                      |
| **Solusi**          | Gunakan _Mutex_ / _Lock_ atau tipe data _Atomic_.                                                                                                      | Desain ulang alur kontrol (misal: _Atomic Check-and-Act_, _Database Transaction Lock_).      |

#### Contoh Skenario _Race Condition_ pada E-Commerce

1. Produk X memiliki stok tersisa **10**.
2. User A dan User B secara bersamaan menekan tombol beli 1 unit Produk X.
3. Proses A membaca saldo Produk X = 10 (validasi aman).
4. Proses B membaca saldo Produk X = 10 (validasi aman).
5. Proses A memproses pembelian dan mengurangi stok menjadi **9** ($10 - 1$).
6. Proses B memproses pembelian dan mengurangi stok berdasarkan angka awal yang dibaca menjadi **9** ($10 - 1$).
7. **Hasil Akhir**: Seharusnya stok tersisa **8**, namun pada basis data tercatat **9** (terjadi kesalahan akibat _race condition_).

### 1.4 Contoh Implementasi Multithreading dalam Python

Berikut adalah contoh simulasi eksekusi multithreaded menggunakan pustaka `threading` di Python untuk menunjukkan eksekusi tugas konkuren:

```python
import threading
import time

def task(name, delay):
    print(f"Task {name} starting...")
    time.sleep(delay)  # Mensimulasikan operasi I/O-bound
    print(f"Task {name} finished after {delay}s.")

# 1. Membuat objek thread
t1 = threading.Thread(target=task, args=("A", 5))
t2 = threading.Thread(target=task, args=("B", 5))
t3 = threading.Thread(target=task, args=("C", 5))
t4 = threading.Thread(target=task, args=("D", 5))
t5 = threading.Thread(target=task, args=("E", 5))

# 2. Menjalankan thread
t1.start()
t2.start()
t3.start()
t4.start()
t5.start()

# 3. Menunggu semua thread selesai
t1.join()
t2.join()
t3.join()
t4.join()
t5.join()

print("All tasks completed.")
```

---

## 2. Tantangan Sinkronisasi pada Sistem Terdistribusi

Merancang mekanisme sinkronisasi yang efektif dalam jaringan terdistribusi dihadapkan pada beberapa tantangan teknis utama:

1. **_Network Latency_ dan Partisi Jaringan (_Partitioning_)**: Waktu tunda pengiriman pesan antar _node_ bersifat bervariasi (_jitter_) dan tidak dapat diprediksi secara pasti. Partisi jaringan juga dapat memutuskan hubungan komunikasi antar kelompok _node_.
2. **Skalabilitas**: Penambahan jumlah _node_ secara masif akan meningkatkan jumlah pesan sinkronisasi yang beredar di jaringan secara eksponensial.
3. **_Fault-Tolerant System_**: Sistem harus tetap mempertahankan sinkronisasi meskipun beberapa _node_ mengalami kegagalan fungsi (_crash_) atau pesan hilang di jalan.
4. **_Concurrency Control_**: Mengatur akses simultan dari banyak transaksi atau _thread_ agar tidak terjadi pembacaan/penulisan data yang tidak valid.
5. **_Data Consistency_**: Mempertahankan konsistensi data di seluruh _replica node_ tanpa mengorbankan performa sistem secara berlebihan.
6. **Sinkronisasi Waktu**: Menyelaraskan waktu jam lokal (_physical clock_) antar _node_ yang secara alami mengalami pergeseran (_drift_).

---

## 3. Sinkronisasi Waktu Fisik (_Physical Clock_)

### 3.1 Pengertian dan Standar Waktu Dunia

Sinkronisasi waktu fisik adalah proses penyelarasan jam nyata (_real-time clock_) dari komputer-komputer dalam sistem terdistribusi dengan standar waktu umum. Hal ini sangat penting untuk korelasi berkas log (_log correlation_), penjadwalan _event_, dan audit transaksi.

Beberapa standar waktu internasional yang digunakan meliputi:

- **GMT (_Greenwich Mean Time_)**: Standar waktu lama berbasis astronomis (posisi matahari di Greenwich), kini sudah tidak digunakan sebagai standar ilmiah.
- **UTC (_Coordinated Universal Time_)**: Standar waktu resmi dunia yang didefinisikan oleh ITU (_International Telecommunication Union_) dan IAU sejak tahun 1963/1970. Singkatan UTC dipilih sebagai kompromi antara bahasa Inggris (_CUT_) dan bahasa Prancis (_TUC_).
- **TAI (_Temps Atomique International_)**: Standar waktu berbasis detik atomik murni yang dihasilkan oleh jam atom (_atomic clock_) di seluruh dunia. UTC dihitung berbasis TAI dengan penyesuaian _leap second_.
- **LTC (_Coordinated Lunar Time_)**: Standar waktu eksplorasi bulan yang saat ini sedang dalam tahap pengembangan oleh lembaga antariksa internasional.

### 3.2 Sumber Waktu dan Infrastruktur Jam Atom (GPS)

Komputer dalam sistem terdistribusi mendapatkan referensi waktu presisi tinggi dari infrastruktur berbasis perangkat keras _Atomic Clock_ dan GPS:

- **Satelit GPS**: Membawa jam atom di dalamnya. Terdapat sekitar 31 satelit GPS di MEO (_Medium Earth Orbit_) yang dikelola oleh _U.S. Space Force_, serta misi ilmiah seperti _Deep Space Atomic Clock_ (DSAC) oleh NASA dan _Atomic Clock Ensemble in Space_ (ACES) di ISS.
- **Mekanisme Sinkronisasi**:
  $$\text{Master Atomic Clock} \rightarrow \text{Atomic Clock Satelit GPS} \rightarrow \text{Sinyal GPS} \rightarrow \text{GPS Dongle (/dev/ttyACM0)} \rightarrow \text{NTP/PTP Server} \rightarrow \text{Client System}$$

### 3.3 Protokol Sinkronisasi Waktu Fisik

#### 1. Network Time Protocol (NTP)

- **Pengembang & Standar**: Dikembangkan oleh David L. Mills (RFC 5905), berjalan pada Application Layer.
- **Arsitektur Stratum**: Menggunakan hierarki tingkat _Stratum_ (Stratum 0 = sumber jam atom/GPS; Stratum 1 = server yang terhubung langsung ke Stratum 0; Stratum 2 = server yang menerima waktu dari Stratum 1, dan seterusnya).
- **Akurasi**: Berkisar antara beberapa milidetik (ms) melalui Internet hingga puluhan mikrodetik ($\mu s$) pada jaringan lokal (LAN).
- **Implementasi Software**: `chrony` (daemon `chronyd` dan CLI `chronyc`) merupakan salah satu implementasi NTP modern yang dirancang responsif terhadap perubahan suhu dan koneksi jaringan yang tidak stabil.

#### 2. Precision Time Protocol (PTP / IEEE 1588)

- **Standar**: IEEE 1588-2002, IEEE 1588-2008 (PTPv2), dan IEEE 1588-2019. Adaptasi PTP untuk _Time-Sensitive Networking_ (TSN) dikenal sebagai IEEE 802.1AS (gPTP).
- **Akurasi**: Mencapai tingkat **sub-mikrodetik** atau nanodetik.
- **Syarat Utama**: Memerlukan kartu jaringan (_Network Interface Card_ / NIC) yang mendukung **_Hardware Timestamping_** (`PTP Hardware Clock`).
- **Implementasi Software**: Pustaka `linuxptp` yang menyediakan program `ptp4l` (mengelola komunikasi PTP) dan `phc2sys` (menyelaraskan jam perangkat keras PTP dengan jam sistem OS).

---

## 4. Sinkronisasi Waktu Logis dan Vector Clocks

### 4.1 Prinsip Waktu Logis (_Logical Clock_)

Dalam sistem terdistribusi, waktu fisik sering kali tidak cukup presisi untuk menentukan urutan kejadian. Leslie Lamport memperkenalkan konsep **Waktu Logis** (_Logical Clock_) yang tidak bergantung pada waktu fisik dunia nyata, melainkan pada **hubungan urutan kausalitas (_causal order of events_)**.

Hubungan _happens-before_ ($\rightarrow$) memiliki sifat transitif:
$$\text{Jika } A \rightarrow B \text{ dan } B \rightarrow C, \text{ maka } A \rightarrow C$$

### 4.2 Vector Clocks

Untuk mengatasi keterbatasan _scalar clock_ Lamport yang tidak dapat membedakan antara kejadian yang berhubungan kausal dengan kejadian independen (_concurrent_), digunakan **Vector Clocks**.

#### Aturan dan Cara Kerja Vector Clocks

Misalkan terdapat $N$ proses dalam sistem. Setiap proses $i$ menyimpan sebuah vektor jam $V_i$ berukuran $N$, di mana $V_i[j]$ merepresentasikan pengetahuan proses $i$ tentang clock proses $j$.

1. **Inisialisasi**: Seluruh elemen vektor diisi angka 0: $V_i = [0, 0, \dots, 0]$.
2. **_Internal Event_**: Sebelum mengeksekusi kejadian internal, proses $i$ menambah nilainya sendiri:
   $$V_i[i] \leftarrow V_i[i] + 1$$
3. **_Send Message_**: Proses $i$ menambah nilai lokalnya $V_i[i] \leftarrow V_i[i] + 1$, lalu melampirkan vektor $V_i$ ke dalam pesan.
4. **_Receive Message_**: Saat proses $i$ menerima pesan berisikan vektor $V_{msg}$ dari proses lain, proses $i$ memperbarui vektor lokalnya menggunakan nilai maksimum per elemen (_element-wise max_), kemudian menambah nilai lokalnya sendiri:
   $$V_i[j] \leftarrow \max(V_i[j], V_{msg}[j]) \quad \forall j$$
   $$V_i[i] \leftarrow V_i[i] + 1$$

#### Aturan Perbandingan Hubungan Kausal:

- **$A$ Happens Before $B$ ($A \rightarrow B$)**: Terjadi jika seluruh elemen $V_A[k] \le V_B[k]$ dan terdapat minimal satu elemen di mana $V_A[k] < V_B[k]$.
- **Independen / Konkuren ($A \parallel B$)**: Terjadi jika tidak dapat dibuktikan $A \rightarrow B$ maupun $B \rightarrow A$.

### 4.3 Implementasi Python Vector Clock (`vclocks.py`)

Berikut adalah implementasi berorientasi objek untuk _Vector Clock_ dalam bahasa Python:

```python
class VectorClock:
    def __init__(self, num_processes, process_id):
        self.clocks = [0] * num_processes  # Inisialisasi semua clock ke 0
        self.process_id = process_id        # ID proses saat ini

    def increment(self):
        """Menambah clock lokal untuk proses saat ini."""
        self.clocks[self.process_id] += 1

    def send_message(self):
        """Menyiapkan vector clock sebelum dikirim bersama pesan."""
        self.increment()  # Increment clock lokal sebelum pengiriman
        return list(self.clocks)

    def receive_message(self, received_clocks):
        """Memperbarui vector clock saat menerima pesan."""
        for i in range(len(self.clocks)):
            self.clocks[i] = max(self.clocks[i], received_clocks[i])
        self.increment()  # Increment clock lokal setelah penggabungan

    def __str__(self):
        return f"P{self.process_id}: {self.clocks}"

    def happens_before(self, other_vector_clock):
        """Menentukan apakah vector clock ini terjadi sebelum (happens before) clock lain."""
        if self.clocks == other_vector_clock.clocks:
            return False

        all_le = True  # Semua elemen <=
        any_lt = False # Minimal satu elemen <

        for i in range(len(self.clocks)):
            if self.clocks[i] > other_vector_clock.clocks[i]:
                return False
            if self.clocks[i] < other_vector_clock.clocks[i]:
                any_lt = True

        return all_le and any_lt

    def is_concurrent(self, other_vector_clock):
        """Menentukan apakah dua vector clock bersifat konkuren (independen)."""
        return not (self.happens_before(other_vector_clock) or
                    other_vector_clock.happens_before(self))

# Simulasi sistem dengan 3 proses
vc0 = VectorClock(num_processes=3, process_id=0)
vc1 = VectorClock(num_processes=3, process_id=1)
vc2 = VectorClock(num_processes=3, process_id=2)

# Kejadian internal pada P0
vc0.increment()

# P0 mengirim pesan ke P1
msg_from_p0 = vc0.send_message()
vc1.receive_message(msg_from_p0)

# P2 melakukan kejadian internal
vc2.increment()

# P1 mengirim pesan ke P2
msg_from_p1 = vc1.send_message()
vc2.receive_message(msg_from_p1)

# Evaluasi kausalitas
print(f"VC0 happens before VC1: {vc0.happens_before(vc1)}")  # Output: True
print(f"VC1 happens before VC0: {vc1.happens_before(vc0)}")  # Output: False
print(f"VC0 is concurrent with VC2: {vc0.is_concurrent(vc2)}") # Evaluasi hubungan konkuren
```

---

## 5. Tantangan Mutual Exclusion pada Sistem Terdistribusi

### 5.1 Konsep dan Syarat _Mutual Exclusion_

_Mutual Exclusion_ (Mutex) adalah prinsip yang menjamin bahwa hanya ada **satu proses** yang dapat memasuki dan mengeksekusi bagian kode kritis (_Critical Section_ / CS) untuk mengakses sumber daya bersama pada satu waktu.

Empat syarat utama algoritma _Mutual Exclusion_ yang baik:

1. **_Mutual Exclusion_**: Tidak boleh ada lebih dari satu proses yang berada di _Critical Section_ secara bersamaan.
2. **_Progress_**: Jika tidak ada proses di CS dan ada proses yang ingin masuk, maka izin akses harus segera diberikan.
3. **_Bounded Waiting_**: Tidak ada proses yang harus menunggu batas waktu tak terhingga (_starvation_) untuk masuk ke CS.
4. **_Fairness_**: Permintaan akses dilayani sesuai urutan kedatangan yang adil.

### 5.2 Algoritma-Algoritma Mutual Exclusion Terdistribusi

| Algoritma                 | Prinsip Kerja                                                                                                                                                 | Kelebihan                                                  | Kekurangan                                                  |
| :------------------------ | :------------------------------------------------------------------------------------------------------------------------------------------------------------ | :--------------------------------------------------------- | :---------------------------------------------------------- |
| **Centralized Algorithm** | Satu node bertindak sebagai koordinator terpusat yang mengatur izin masuk ke CS via pesan _Request_, _Grant_, dan _Release_.                                  | Sederhana, jumlah pesan minimal per entri CS (3 pesan).    | _Single point of failure_, berpotensi menjadi _bottleneck_. |
| **Ricart - Agrawala**     | Algoritma terdistribusi penuh tanpa koordinator. Proses mengirim pesan _Request_ ber-timestamp ke semua node dan harus menunggu balasan _OK_ dari $N-1$ node. | _Fault-tolerant_, adil (_fairness_) berdasarkan timestamp. | Overhead pesan tinggi: $2 \times (N-1)$ pesan per entri CS. |
| **Token-Based Algorithm** | Sebuah _token_ unik beredar di antara node-node jaringan. Node hanya boleh masuk CS jika memegang token tersebut.                                             | Hanya butuh 1 pesan saat transfer token (efisien).         | Deteksi dan pemulihan token yang hilang sangat rumit.       |

---

## 6. Pemilihan Koordinator (_Leader Election_)

### 6.1 Latar Belakang dan Alasan Pemilihan Koordinator

Dalam sistem terdistribusi, sering kali diperlukan satu _node_ khusus yang bertindak sebagai **koordinator** atau _leader_ untuk mengelola sumber daya bersama, menyinkronkan transaksi, atau membuat keputusan global. Jika koordinator yang sedang bertugas mengalami kegagalan (_crash_), sistem harus melakukan mekanisme pemilihan pemimpin baru secara otomatis (_Leader Election_).

### 6.2 Algoritma Pemilihan Koordinator Modern

1. **Bully Algorithm**:
   - Digunakan ketika setiap _node_ mengetahui identitas/ID dari seluruh _node_ lain.
   - Ketika suatu _node_ menyadari koordinator mati, ia mengirim pesan _Election_ ke seluruh _node_ yang memiliki ID lebih tinggi.
   - _Node_ dengan ID tertinggi yang aktif akan "membully" _node_ ber-ID lebih rendah dan mengumumkan dirinya sebagai koordinator baru.
2. **Ring Algorithm**:
   - _Node-node_ dihubungkan secara logis dalam struktur cincin (_ring_).
   - Pesan pemilihan diteruskan mengelilingi cincin untuk mengumpulkan daftar _node_ aktif, dan _node_ dengan ID tertinggi dipilih sebagai koordinator.
3. **Algoritma Berbasis Konsensus Modern (Raft & Paxos)**:
   - **Apache ZooKeeper**: Menggunakan algoritma **Zab** (_ZooKeeper Atomic Broadcast_, varian Paxos) untuk mengelola koordinator terdistribusi.
   - **Kubernetes**: Menggunakan basis data **etcd** yang menerapkan algoritma konsensus **Raft** untuk pemilihan _leader_ dan sinkronisasi status kluster.

---

## 7. Kesimpulan

Sinkronisasi dan koordinasi merupakan mekanisme terpenting untuk menjaga konsistensi, keandalan, dan integritas data pada sistem terdistribusi. Tanpa sinkronisasi yang baik, sistem terdistribusi rawan mengalami _data race_, _race condition_, dan kerusakan data akibat akses konkuren.

Poin-poin utama kesimpulan pembelajaran minggu 03:

1. **Penyelarasan Waktu Fisik** dilakukan menggunakan standar UTC berbasis TAI/jam atom melalui protokol NTP (tingkat milidetik) atau PTP/IEEE 1588 (tingkat sub-mikrodetik dengan _hardware timestamping_).
2. **Penyelarasan Waktu Logis** memanfaatkan _Vector Clocks_ untuk melacak hubungan kausalitas (_happens-before_) dan mengidentifikasi kejadian konkuren tanpa bergantung pada jam fisik.
3. **_Mutual Exclusion_ Terdistribusi** menjamin keamanan _Critical Section_ melalui pendekatan terpusat (_Centralized_), terdistribusi (_Ricart-Agrawala_), maupun berbasis _Token_.
4. **Pemilihan Koordinator** (seperti _Bully_, _Ring_, serta kerangka modern berbasis konsensus Raft/Zab pada etcd dan ZooKeeper) memastikan sistem dapat memulihkan diri secara otomatis ketika pemimpin kluster mengalami kegagalan.
