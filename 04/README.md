# Nama : Zul Ikwanul Anggara

# Nim : 255410043

# Materi Minggu 04: Konsistensi dan Replikasi pada Sistem Terdistribusi

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Konsistensi dan Replikasi pada Sistem Terdistribusi.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Format penyusunan rangkuman pada materi, serta sebagian isi" pada materi yang di susun tersebut.

---

# Rangkuman Materi

## 1. Pendahuluan dan Konsep Konsistensi

### 1.1 Definisi Konsistensi pada Sistem Terdistribusi

Dalam arsitektur sistem terdistribusi, **konsistensi** (_consistency_) didefinisikan sebagai jaminan bahwa seluruh _node_ (komputer atau server) dalam sistem memiliki atau menampilkan data yang sama pada waktu tertentu, meskipun data tersebut disimpan secara terdistribusi di banyak lokasi fisik dan dapat diubah secara simultan oleh pengguna atau proses yang berbeda.

Ketika suatu aplikasi memperbarui salinan data pada satu server, sistem harus memiliki mekanisme terstruktur untuk memastikan bahwa seluruh server lainnya pada akhirnya memiliki data yang identik. Hal ini merupakan pilar utama dari konsep konsistensi data terdistribusi.

### 1.2 Urgensi Konsistensi dan Potensi Masalah

Pentingnya konsistensi dalam sistem terdistribusi didasari oleh beberapa alasan kritis:

- **Mencegah Anomali Data**: Tanpa jaminan konsistensi, pengguna yang mengakses _node_ berbeda akan melihat versi data yang bertentangan atau kadaluarsa.
- **Integritas Transaksi Finansial**: Sebagai contoh, pada transaksi perbankan ketika Nasabah mentransfer uang dari Rekening A ke Rekening B:
  - Jika Server 1 telah mengurangkan saldo Rekening A, namun Server 2 belum menambah saldo Rekening B, maka saldo Rekening B belum mencukupi saat nasabah ingin melakukan transaksi berikutnya.
  - Hal ini menciptakan _state_ yang tidak konsisten dan merugikan pengguna.
- **Menjamin Transparansi dan Kebenaran**: Tujuan utama konsistensi adalah agar pengguna selalu memperoleh informasi yang benar dan _up-to-date_ tanpa perlu memikirkan kompleksitas replikasi fisik di belakang layar.

---

## 2. Teorema CAP dan Trade-Off Latensi

### 2.1 Teorema CAP (Brewer's Theorem)

Dalam merancang sistem penyimpanan data terdistribusi, perancang sistem terikat pada **Teorema CAP** (_Brewer’s Theorem_). Teorema ini menyatakan bahwa sebuah sistem terdistribusi tidak mungkin menjamin ketiga properti berikut secara sempurna dan bersamaan:

1. **Consistency (C)**: Seluruh _node_ melihat data yang sama pada waktu yang sama (pembacaan mengembalikan hasil penulisan terbaru).
2. **Availability (A)**: Setiap permintaan (_request_) dari _client_ selalu menerima respon (berhasil/gagal) tanpa adanya jaminan bahwa respon tersebut berisi data paling baru.
3. **Partition Tolerance (P)**: Sistem tetap dapat terus beroperasi meskipun terjadi gangguan atau pemutusan saluran komunikasi antar _node_ jaringan (_network partition_).

Dalam dunia nyata, gangguan jaringan (_partition_) adalah hal yang tidak dapat dihindari. Oleh karena itu, ketika terjadi partisi jaringan, sistem terdistribusi harus memilih kompromi (_trade-off_):

- **Sistem AP (_Availability + Partition Tolerance_)**: Mengutamakan ketersediaan layanan. Pengguna tetap dapat melakukan operasi _read/write_, namun terdapat risiko sementara di mana stok atau data di berbagai wilayah tidak sinkron.
- **Sistem CP (_Consistency + Partition Tolerance_)**: Mengutamakan konsistensi data. Sistem akan menolak atau menunda transaksi sampai jaringan pulih agar tidak terjadi ketidakcocokan data, meskipun harus mengorbankan ketersediaan layanan (_availability_).

#### Contoh Kasus: Aplikasi E-Commerce Antar-Wilayah

Dua server e-commerce melayani wilayah terpisah: Server A (Indonesia Barat) dan Server B (Indonesia Timur). Jika koneksi antar-server terputus:

- Jika memilih **AP**: Pengguna di kedua wilayah tetap bisa berbelanja, namun stok barang bisa sementara tidak konsisten.
- Jika memilih **CP**: Sistem menolak transaksi pembelian sampai koneksi pulih demi menjaga keakuratan stok.

### 2.2 Trade-Off Antara Latensi dan Konsistensi

Selain Teorema CAP, terdapat kompromi langsung antara tingkat konsistensi dan respon waktu (_latency_):

$$\text{Konsistensi Tinggi} \uparrow \implies \text{Latensi} \uparrow$$
$$\text{Konsistensi Lemah} \downarrow \implies \text{Latensi} \downarrow$$

- **Strong Consistency**: Sistem harus menunggu seluruh _node_ selesai bersinkronisasi lintas jaringan sebelum memberikan respon balik ke pengguna. Akibatnya, latensi menjadi lebih tinggi.
- **Eventual Consistency**: Sistem langsung memberikan respon balik begitu data diperbarui di _node_ lokal, lalu menyebarkan pembaruan ke _node_ lain secara bertahap. Akibatnya, latensi sangat rendah, namun ada jendela waktu di mana data belum seragam.

---

## 3. Spektrum Model Konsistensi

Model konsistensi adalah aturan atau jaminan mengenai bagaimana dan kapan pembaruan data pada sistem terdistribusi dapat dilihat oleh _node_ atau _client_ lain. Model ini terbagi dari yang paling ketat (_strong_) hingga yang paling longgar (_weak_).

```
[Strong] -----------------------------------------------------> [Weak]
Linearizable -> Sequential -> Causal -> Read-your-writes -> Monotonic read -> Eventual
```

### 3.1 Model Konsistensi Berbasis Data (_Data-Centric Consistency_)

1. **Linearizability (Strict Consistency)**:
   - Varian paling kuat dari _strong consistency_. Setiap operasi dieksekusi secara atomik dalam urutan waktu nyata (_real-time global ordering_).
   - Sistem terdistribusi tampak seolah-olah hanya berupa satu _node_ tunggal. Setelah penulisan selesai, seluruh pembacaan berikutnya di _node_ mana pun pasti mengembalikan nilai baru tersebut.
   - Contoh Penggunaan: Pengecekan keunikan ID/username di **etcd**, **Apache ZooKeeper**, dan **Google Spanner**.
2. **Sequential Consistency**:
   - Urutan operasi bersifat konsisten untuk semua _node_, namun tidak harus mengikuti waktu nyata (_real-time_). Seluruh proses melihat eksekusi penulisan dalam urutan logis yang sama.
   - Contoh Penggunaan: Urutan posting di media sosial yang dilihat sama oleh semua pengguna, meskipun ada sedikit _delay_ pengiriman.
3. **Causal Consistency**:
   - Operasi-operasi yang memiliki hubungan sebab-akibat (_causally related_) harus dilihat dalam urutan yang sama oleh seluruh _node_. Namun, operasi yang independen (konkuren) dapat dilihat dalam urutan berbeda.
   - Contoh Penggunaan: Pada media sosial, komentar atau reaksi terhadap suatu _post_ tidak boleh muncul sebelum _post_ utamanya muncul.

### 3.2 Model Konsistensi Berbasis Klien (_Client-Centric Consistency_)

1. **Read-Your-Writes Consistency**:
   - Menjamin bahwa setelah seorang pengguna memperbarui data, pembacaan berikutnya oleh pengguna yang sama akan selalu melihat hasil pembaruannya tersebut.
   - Contoh Penggunaan: Saat mengirim pesan di aplikasi _chat_, pesan tersebut langsung muncul di layar pengirim, meskipun penerima belum menerimanya.
2. **Monotonic Read Consistency**:
   - Menjamin bahwa jika seorang pengguna telah membaca suatu nilai data versi tertentu, pembacaan berikutnya oleh pengguna tersebut tidak akan pernah mengembalikan versi data yang lebih lama (_tidak mundur_).
   - Contoh Penggunaan: _Timeline_ media sosial yang tidak kembali ke status lama saat di-_refresh_.

### 3.3 Model Konsistensi BASE dan Eventual Consistency

- **Eventual Consistency**: Menjamin bahwa jika tidak ada pembaruan data baru dalam jangka waktu tertentu, pada akhirnya (_eventually_) seluruh salinan data di semua _node_ akan menjadi identik.
- **Prinsip BASE**:
  - **Basically Available**: Sistem menjamin ketersediaan akses _read/write_ meskipun terjadi kegagalan sebagian.
  - **Soft-state**: Keadaan sistem dapat berubah seiring waktu tanpa adanya interaksi pengguna (akibat proses sinkronisasi latar belakang).
  - **Eventual consistency**: Konsistensi dicapai secara bertahap seiring berjalannya waktu.
  - Contoh Penggunaan: **Amazon DynamoDB** dan **Apache Cassandra**.

---

## 4. Konsep, Tujuan, dan Jenis Replikasi Data

### 4.1 Definisi dan Tujuan Replikasi

**Replikasi data** adalah proses membuat dan memelihara salinan data yang identik pada beberapa _node_ atau server dalam sistem terdistribusi.

Tujuan utama replikasi meliputi:

1. **Ketersediaan (_Availability_)**: Jika salah satu server mengalami kegagalan (_crash_), server lain masih memiliki salinan data aktif.
2. **Kinerja (_Performance_)**: Pengguna dapat membaca data dari server terdekat secara geografis untuk mengurangi latensi.
3. **Reliabilitas (_Reliability_)**: Meminimalkan risiko kehilangan data karena terdapat banyak cadangan (_backup_).
4. **Skalabilitas**: Beban pembacaan data (_read load_) dapat dibagi secara merata ke beberapa server (_load balancing_).

### 4.2 Jenis-Jenis Replikasi Data

| Klasifikasi Replikasi              | Jenis Replikasi                      | Karakteristik dan Cara Kerja                                                                                                                                                            | Contoh Teknologi                 |
| :--------------------------------- | :----------------------------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------- |
| **Berdasarkan Peran Node**         | **Master-Slave (Primary-Secondary)** | Satu server (_Primary_) melayani operasi _Read-Write_, sedangkan server _Slave_ hanya melayani operasi _Read_. Beban _write_ terpusat pada Master.                                      | PostgreSQL Streaming Replication |
|                                    | **Multi-Master (Peer-to-Peer)**      | Seluruh _node_ memiliki peran setara dan dapat melayani operasi _Read-Write_. Fleksibel, namun rawan konflik penulisan.                                                                 | Apache Ignite, Apache Cassandra  |
| **Berdasarkan Waktu Sinkronisasi** | **Synchronous Replication**          | Transaksi _write_ baru dianggap selesai (_committed_) setelah berhasil ditulis ke _Primary_ dan seluruh _Replica_. Konsisten penuh tetapi latensi tinggi.                               | Google Spanner                   |
|                                    | **Asynchronous Replication**         | Transaksi _write_ langsung dianggap selesai di _Primary_, lalu data dikirim ke _Replica_ secara terpisah. Sangat cepat, namun ada risiko kehilangan data jika _Primary_ mendadak crash. | MySQL Async Replication          |
|                                    | **Semi-Synchronous Replication**     | _Primary_ menunggu minimal satu _Replica_ mengonfirmasi penerimaan data sebelum menyelesaikan transaksi. Keseimbangan antara performa dan konsistensi.                                  | MySQL Semi-Sync                  |

---

## 5. Strategi Sinkronisasi dan Penanganan Konflik

### 5.1 Strategi Sinkronisasi Replika

1. **Arah Sinkronisasi**:
   - _Master-Slave_: Akses _write_ di Master, dipropagasi ke Slave (_read-only_).
   - _Master-Master Symmetric_: Seluruh _node_ memiliki sumber daya dan peran _read-write_ yang setara.
   - _Master-Master Asymmetric_: Peran _read-write_ sama, namun dengan alokasi sumber daya atau peran khusus yang berbeda.
2. **Cara Sinkronisasi**:
   - **Pull-Based**: _Replica_ memeriksa pembaruan secara periodik (_polling_) ke server _Source_.
   - **Push-Based**: Server _Source_ aktif mengirimkan pembaruan data secara _real-time_ ke _Replica_ menggunakan mekanime _Change Data Capture_ (CDC), WebSockets, atau _Server-Sent Events_ (SSE).
   - **Hybrid**: Kombinasi pendekatan _push_ dan _pull_.

### 5.2 Teknik Penanganan Konflik (_Conflict Resolution_)

Ketika terjadi pembaruan simultan (_concurrent write_) pada arsitektur Multi-Master, diperlukan mekanisme penanganan konflik:

- **Last Write Wins (LWW)**: Menggunakan _timestamp_ terbaru untuk menentukan data yang valid. Data dengan _timestamp_ lebih lama akan ditimpa dan diabaikan.
- **Vector Clock**: Menggunakan vektor angka untuk melacak hubungan kausalitas dan mendeteksi konflik penulisan secara presisi.
- **Conflict-free Replicated Data Types (CRDT)**: Struktur data khusus yang dirancang sedemikian rupa sehingga pembaruan dari berbagai _node_ dapat digabungkan (_merged_) secara otomatis tanpa konflik.
  - _Contoh CRDT_: Pustaka **Yjs** yang menggunakan aturan _Max_ untuk kolaborasi teks atau _collaborative editor_ (seperti Google Docs).
- **Operational Transformation (OT)**: Mengubah dan mengondisikan indeks operasi lokal berdasarkan informasi pesan dari _node_ lain agar isi dokumen akhir tetap seragam.

---

## 6. Protokol Replikasi Terdistribusi

### 6.1 Primary-Based & Chain Replication

- **Primary-Based**: Seluruh operasi penulisan diarahkan ke satu _Primary Node_, yang kemudian mempropagasi perubahan ke _Replica Nodes_.
- **Chain Replication**: _Node-node_ disusun berurutan dalam bentuk rantai (_chain_):
  - Permintaan _Write_ dikirim ke _Head Node_, lalu diteruskan secara berurutan hingga _Tail Node_.
  - _Tail Node_ mengembalikan sinyal konfirmasi (_ACK_) ke _client_ dan melayani seluruh permintaan _Read_.

### 6.2 Quorum-Based Replication

Operasi _write_ atau _read_ dianggap berhasil jika memperoleh konfirmasi dari sejumlah _node_ minimal (_quorum_).

$$W + R > N$$

Dimana:

- $N$ = Total jumlah _node_ replika.
- $W$ = Jumlah _node_ minimal yang harus mengonfirmasi operasi penulisan (_write_).
- $R$ = Jumlah _node_ minimal yang harus mengonfirmasi operasi pembacaan (_read_).

#### Skenario Contoh ($N = 3, W = 2, R = 2$):

Syarat terenuhi karena $2 + 2 = 4 > 3$.

- **Operasi Write**: Klien menulis $x = 100$. Permintaan dikirim ke Node A, B, dan C. Node A dan B berhasil memproses dan memberikan ACK, sedangkan Node C mengalami _timeout_. Karena $W = 2$ terpenuhi, operasi _write_ dinyatakan **sukses**.
- **Operasi Read**: Klien membaca nilai $x$ dari $R = 2$ node (Node A dan C). Node A memberikan $x = 100$ dan Node C memberikan nilai lama. Karena irisan $W$ dan $R$ pasti memuat minimal satu _node_ terbaru (Node A), sistem dapat mengidentifikasi dan mengembalikan nilai $x = 100$ yang paling baru.

### 6.3 State Machine Replication (SMR)

SMR menjaga _high-availability_ dengan memastikan seluruh replika mengeksekusi urutan perintah yang sama dari _log_ yang terurut.

1. _Leader_ menerima permintaan _client_ (misal: `SET x = 20`), memasukkannya ke _log_, dan memberikan nomor urut.
2. _Leader_ mengirimkan entri _log_ ke seluruh replika.
3. Setelah menerima konfirmasi dari mayoritas replika, _Leader_ menyatakan perintah tersebut ter-_commit_.
4. Seluruh replika mengeksekusi perintah dari _log_ ke penyimpanan lokal masing-masing secara seragam.

### 6.4 Gossip Protocol

Digunakan pada basis data terdistribusi seperti **Apache Cassandra**:

- Setiap detik, sebuah _node_ memilih 1–3 _node_ lain secara acak untuk saling bertukar informasi status (_state_).
- Dalam beberapa putaran "gossip", seluruh _node_ dalam _cluster_ dapat mengetahui perubahan status atau kegagalan _node_ lain secara cepat dan efisien.

---

## 7. Studi Kasus Penerapan di Industri

### 7.1 PostgreSQL 18 Streaming Replication (Master-Slave / Primary-Replica)

PostgreSQL 18 menerapkan replikasi _Master-Slave_ berbasis _Physical Log_ (Write-Ahead Logging / WAL) secara asinkron atau semi-sinkron.

#### Konfigurasi Utama (`docker-compose.yaml`):

```yaml
services:
  postgres_primary:
    image: postgres:18.3-alpine3.23
    command: |
      postgres
      -c wal_level=replica
      -c hot_standby=on
      -c max_wal_senders=10
      -c max_replication_slots=10

  postgres_replica:
    image: postgres:18.3-alpine3.23
    environment:
      PGUSER: replicator
      PGPASSWORD: replicator_password
      PGDATA: /var/lib/postgresql/18/docker
    command: |
      bash -c "
      until pg_basebackup --pgdata=/var/lib/postgresql/18/docker -R --slot=replication_slot --host=postgres_primary --port=5432 -X stream; do
        sleep 5
      done
      postgres
      "
```

#### Pengujian dan Pemantauan Status:

1. **Di Server Primary**: Mengecek status aliran replikasi:
   ```sql
   SELECT * FROM pg_stat_replication;
   SELECT * FROM pg_replication_slots;
   ```
2. **Di Server Replica**: Verifikasi status _standby_ (_read-only_):
   ```sql
   SELECT pg_is_in_recovery(); -- Mengembalikan nilai 't' (true)
   ```
3. **Mekanisme Failover (High Availability)**: Jika server _Primary_ mengalami kegagalan (_down_), _Replica_ dapat dipromosikan menjadi _Primary_ baru yang mendukung operasi _Read-Write_ menggunakan perintah:
   ```sql
   SELECT pg_promote();
   ```

### 7.2 Apache Ignite 3.1.0 Cluster (Master-Master / Peer-to-Peer)

Apache Ignite 3 menggunakan arsitektur _Multi-Master_ (Peer-to-Peer) terdistribusi di mana seluruh _node_ dapat menerima operasi _Read-Write_.

#### Inisialisasi Cluster via CLI:

```bash
# Inisialisasi cluster 3-node dengan metastorage group
cluster init --name=ignite3 --metastorage-group=node1,node2,node3
```

#### Replikasi dan Distribusi Data via SQL:

Apache Ignite memungkinkan pengaturan strategi replikasi data per tabel melalui pembagian _Zone_:

```sql
-- Zone Replikasi Penuh (3 replika di seluruh node)
CREATE ZONE IF NOT EXISTS ChinookReplicated WITH replicas=3, partitions=25, storage_profiles='default';

-- Zone Terpartisi (Colocated)
CREATE ZONE IF NOT EXISTS Chinook WITH replicas=2, storage_profiles='default';

-- Pembuatan Tabel Replikasi
CREATE TABLE MediaType (
    MediaTypeId INT NOT NULL,
    Name VARCHAR(120),
    PRIMARY KEY (MediaTypeId)
) ZONE ChinookReplicated;
```

Ketika data diisikan di `node1` (REST API port 10300), perubahan secara otomatis direplikasi ke `node2` (port 10301) dan `node3` (port 10302), sehingga _query analytics_ pada _node_ mana pun akan mengembalikan hasil yang konsisten.

---

## 8. Kesimpulan

Konsistensi dan replikasi merupakan dua pilar utama dalam membangun sistem terdistribusi yang andal, berkinerja tinggi, dan tahan terhadap kegagalan (_fault-tolerant_).

Rangkuman poin inti pembelajaran minggu 04:

1. **Konsistensi Data** menjamin bahwa seluruh salinan data di berbagai lokasi fisik menampilkan informasi yang seragam. Perancangan konsistensi harus memperhitungkan batasan **Teorema CAP** ($AP$ vs $CP$) serta _trade-off_ latensi.
2. **Model Konsistensi** membentang dari _Linearizability_ (paling ketat, _real-time_) hingga _Eventual Consistency_ (paling longgar, berorientasi pada ketersediaan tinggi dan performa).
3. **Replikasi Data** dilakukan untuk meningkatkan _Availability_, _Performance_, dan _Reliability_. Replikasi dapat diklasifikasikan menjadi _Master-Slave_ vs _Multi-Master_, serta _Synchronous_ vs _Asynchronous_.
4. **Protokol Replikasi** seperti _Quorum-Based_ ($W + R > N$), _State Machine Replication_, _Chain Replication_, dan _Gossip Protocol_ memberikan landasan matematis dan struktural untuk menjaga integritas data klaster.
5. **Penerapan Industri Real** seperti _Streaming Replication_ pada PostgreSQL 18 melayani skenario _Master-Slave_ dengan dukungan _failover_ (`pg_promote`), sedangkan Apache Ignite 3.1.0 menyediakan solusi _Multi-Master In-Memory Database Cluster_ untuk pemrosesan paralel skala raksasa.
