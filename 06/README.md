# Nama : Zul Ikhwanul Anggara

# Nim : 255410043

# Materi Minggu 06: Distributed File System

**AI yang digunakan:** Google NotebookLM

**Prompt:**
Berdasarkan materi tersebut, berikan saya poin" penting dalam materi tersebut untuk saya menyusun ringkasan" serta untuk di pelajari dalam materi : Distributed File System.

**Bagian yang disesuaikan dari keluaran prompt ada pada:**
Format penyusunan rangkuman pada materi, serta sebagian isi" pada materi yang di susun tersebut.

---

# Rangkuman Materi

## 1. Pendahuluan dan Konsep Dasar File System

### 1.1 Definisi File System

_File System_ (sering ditulis _filesystem_ atau disingkat FS) adalah metode dan struktur data yang digunakan oleh sistem operasi untuk mengelola, mengorganisasi, dan menyimpan berkas (_file_) serta direktori pada media penyimpan fisik (seperti _harddisk_, SSD, USB _flashdisk_, atau media penyimpanan lainnya) [2, 3].

Secara arsitektural pada sistem operasi Linux:

1. **Aplikasi Pengguna (_User Space App_)** melakukan panggilan sistem (_System Call Interface_) seperti `read()` atau `write()` [3].
2. Panggilan tersebut diterima oleh **Virtual File System (VFS)** di dalam _kernel space_. VFS menyediakan abstraksi antarmuka seragam bagi aplikasi tanpa memedulikan jenis _file system_ konkret yang digunakan [2, 3].
3. VFS meneruskan perintah ke **Driver File System** spesifik (misalnya ext4 atau XFS) [2, 3].
4. Driver berkomunikasi dengan **Device Drivers** penyimpanan untuk mengakses perangkat keras (_hardware_) [3].

### 1.2 Keanekaragaman File System Lokal

Setiap sistem operasi mendukung berbagai jenis _file system_ lokal yang disesuaikan dengan kebutuhan kinerja dan fiturnya [2]:

- **Linux**: `ext4`, `XFS`, `btrfs`, `ReiserFS`.
- **DOS / Microsoft Windows**: `FAT16`, `FAT32`, `NTFS`.
- **macOS**: `APFS` (_Apple File System_).

### 1.3 Perbedaan File System dan Hirarki File System

Penting untuk membedakan antara _file system_ fisik/logis dengan **Hirarki File System** [2]:

- **File System**: Berfokus pada mekanisme fisik pengelolaan blok data pada media penyimpan [2].
- **Hirarki File System**: Berfokus pada tata letak dan pengorganisasian direktori serta berkas dari sudut pandang pengguna dan sistem operasi (misalnya standar _Filesystem Hierarchy Standard_ / FHS pada UNIX/Linux yang mengatur direktori `/bin`, `/boot`, `/dev`, `/etc`, `/home`, `/lib`, `/root`, `/usr`, `/var`, dan lain-lain) [2, 3]. Pengguna berinteraksi dengan hirarki ini tanpa perlu memikirkan pengorganisasian fisik _file system_ di tingkat bawah [2].

---

## 2. Pengertian dan Karakteristik Distributed File System (DFS)

### 2.1 Definisi Distributed File System

**Distributed File System (DFS)** bukan sekadar sistem berkas yang tersebar di beberapa mesin terpisah. Secara konseptual, DFS adalah sistem abstraksi terkoordinasi dari sekumpulan media penyimpan heterogen yang terhubung melalui jaringan. DFS menyembunyikan kompleksitas heterogenitas perangkat keras, kegagalan _node_, dan akses konkurensi, sehingga menghasilkan jaringan penyimpanan terintegrasi, andal, dan berkinerja tinggi [3].

Dari sudut pandang pengguna, meskipun fisik data tersimpan di puluhan atau ribuan komputer (_node_) yang berbeda, seluruh media penyimpan tersebut tampak dan diakses sebagai satu kesatuan sistem berkas tunggal yang koheren (_single system image_) [3].

### 2.2 Perbandingan Arsitektur Penyimpanan Data

Untuk memahami posisi DFS dalam lanskap teknologi penyimpanan, berikut adalah perbandingan antara DAS, NAS, SAN, dan DFS [3, 4, 7]:

| Jenis Storage                        | Karakteristik Utama                                                                                                                                | Interface / Protokol                         | Ringkasan Penggunaan                                                                   |
| :----------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------------- | :------------------------------------------- | :------------------------------------------------------------------------------------- |
| **DAS** (_Direct-Attached Storage_)  | Penyimpanan yang terhubung langsung ke satu komputer/server tanpa melalui jaringan (misal: SATA/NVMe internal) [4].                                | Bus internal (SATA, PCIe, NVMe) [4]          | Komputer pribadi, server lokal tunggal [4].                                            |
| **NAS** (_Network-Attached Storage_) | Perangkat penyimpanan bersama yang terhubung ke jaringan lokal (_shared network_), mengelola data pada level berkas (_file-level_) [4, 7].         | NFS, SMB/CIFS (berbasis TCP/IP) [7]          | Berbagi berkas (_file sharing_), _global file locking_ [7].                            |
| **SAN** (_Storage Area Network_)     | Jaringan penyimpanan khusus (_dedicated network_) berkecepatan tinggi yang menyajikan data sebagai blok-blok fisik (_block-level devices_) [4, 7]. | Fibre Channel (FC), iSCSI [7]                | _Backend_ basis data skala besar, sistem virtualisasi enterprise, _video editing_ [4]. |
| **DFS** (_Distributed File System_)  | Abstraksi perangkat lunak yang mengelola berkas dan direktori terdistribusi di banyak _node_ (biasanya memanfaatkan DAS pada tiap _node_) [4].     | Antarmuka khusus (HDFS API, POSIX, REST) [7] | Pemrosesan _Big Data_, komputasi awan, analisis data terdistribusi [5, 7].             |

---

## 3. Ragam Implementasi DFS dan Studi Kasus HDFS

### 3.1 Ragam Implementasi DFS Populer

Beberapa implementasi DFS yang luas digunakan di industri meliputi [5]:

- **Google File System (GFS)** & **Colossus**: Sistem berkas terdistribusi milik Google (Colossus merupakan generasi penerus GFS yang digunakan pada layanan YouTube, Gmail, dan Google Docs) [5].
- **CephFS**: Bagian dari platform Ceph yang menyediakan sistem berkas terdistribusi berkinerja tinggi [5, 7].
- **GlusterFS**: Sistem berkas terdistribusi _open-source_ berbasis klaster [5].
- **Lustre**: DFS untuk lingkungan _High-Performance Computing_ (HPC) dan superkomputer [5].
- **Apache Hadoop Distributed File System (HDFS)**: DFS _open-source_ yang menjadi standar _de facto_ untuk ekosistem _Big Data_ [5].

### 3.2 Arsitektur dan Komponen Utama HDFS

HDFS dirancang khusus untuk mengelola himpunan data (_datasets_) berukuran sangat besar (_petabytes_) yang berjalan di atas perangkat keras komoditas (_commodity hardware_) — yaitu perangkat keras komputer massal yang relatif murah dan mudah diganti [5].

Arsitektur HDFS menerapkan pola _Master-Slave_ [6]:

1. **NameNode (Master)**:
   - Mengelola _namespace_ sistem berkas (direktori dan nama berkas) [6].
   - Menyimpan _metadata_ seluruh sistem berkas, termasuk pemetaan berkas ke blok-blok data fisik serta lokasi replikanya [6].
   - Mengatur operasi dasar seperti pembukaan, penutupan, dan pengubahan nama berkas/direktori [6].
2. **DataNode (Slave)**:
   - Menyimpan blok-blok data fisik aktual pada _node_ klaster [6].
   - Memproses operasi pembacaan (_read_) dan penulisan (_write_) sesuai instruksi dari NameNode [6].
   - Secara berkala mengirimkan laporan blok (_Blockreport_) dan sinyal keandalan (_Heartbeat_) ke NameNode [6].
3. **Secondary NameNode**:
   - Bertugas melakukan pemrosesan _checkpointing_ secara periodik untuk menggabungkan catatan transaksi (_edits log_) dengan gambaran sistem berkas (_fsimage_), guna meringankan beban kerja NameNode utama [13].
4. **Pembagian Blok dan Replikasi Data**:
   - HDFS membagi setiap berkas menjadi blok-blok besar (secara _default_ 128 MB atau 256 MB) [5].
   - Setiap blok direplikasi ke beberapa DataNode terpisah (faktor replikasi _default_ adalah 3) untuk menjamin toleransi kesalahan (_fault-tolerance_) dan ketersediaan tinggi (_high availability_) [5, 6].

---

## 4. Praktikum Instalasi dan Operasi HDFS pada Linux

### 4.1 Persyaratan Perangkat Lunak

Untuk menjalankan Apache Hadoop HDFS versi 3.5.0 pada sistem operasi Linux (misalnya Artix Linux atau distribusi Linux lainnya), diperlukan beberapa persyaratan dasar [8, 9]:

- **Java Development Kit (JDK)**: JDK versi 17 (diperlukan jika melakukan kompilasi dan digunakan untuk server) dan/atau JDK 21 (dapat digunakan untuk _client_) [8].
- **Apache Hadoop**: Versi 3.5.0 (rilis April 2026) [9].
- **pdsh** (_Parallel Distributed Shell_): Utilitas untuk mengeksekusi perintah secara paralel ke beberapa _node_ [9, 11].
- **SSH & SSH Daemon (`sshd`)**: Layanan SSH harus dalam keadaan aktif (`sudo dinitctl start sshd` pada Artix Linux + dinit, atau `sudo systemctl start ssh` pada systemd) [9].

### 4.2 Langkah-Langkah Instalasi dan Konfigurasi

#### 1. Ekstraksi dan Pengaturan Environment Variables

Ekstraksi berkas rilis Hadoop dan buat tautan simbolis (_symlink_), kemudian tambahkan direktori `bin` dan `sbin` ke dalam variabel lingkungan `PATH` [10]:

```bash
$ cd ~/software/storage/
$ tar -xvf ~/master/data-engineering/apache-hadoop/hadoop-3.5.0.tar.gz
$ ln -s hadoop-3.5.0 hadoop
```

Tambahkan perintah ekspor PATH pada berkas skrip shell (misalnya `~/env/bash/storage/hadoop.sh`) [10]:

```bash
export PATH="$HOME/software/storage/hadoop/bin:$HOME/software/storage/hadoop/sbin:$PATH"
```

Muat skrip lingkungan menggunakan perintah `source ~/env/bash/storage/hadoop.sh` [10].

#### 2. Instalasi `pdsh` dan Otentikasi SSH Bebas Akses

Instal `pdsh` (misalnya menggunakan `trizen -S pdsh` di Artix Linux/AUR) [11]. Atur tipe eksekusi _remote command_ ke SSH [11]:

```bash
$ export PDSH_RCMD_TYPE=ssh
```

Konfigurasikan penguncian kunci publik SSH agar komputer dapat melakukan koneksi SSH ke `localhost` tanpa interaksi manual [11].

#### 3. Konfigurasi Berkas XML pada Hadoop (`$HADOOP_HOME/etc/hadoop/`)

- **`hdfs-site.xml`**: Menentukan faktor replikasi berkas (diatur ke `1` untuk pengujian _single-node_) [12]:
  ```xml
  <configuration>
    <property>
      <name>dfs.replication</name>
      <value>1</value>
    </property>
  </configuration>
  ```
- **`core-site.xml`**: Menentukan URI titik akses utama HDFS [12]:
  ```xml
  <configuration>
    <property>
      <name>fs.defaultFS</name>
      <value>hdfs://localhost:9000</value>
    </property>
  </configuration>
  ```
- **`hadoop-env.sh`**: Mengatur lokasi instalisasi Java (`JAVA_HOME`) pada sistem [13].

#### 4. Formatisasi NameNode dan Menjalankan Daemon HDFS

Inisialisasi sistem berkas dilakukan **sekali saja** menggunakan perintah berikut (peringatan: melakukan _format_ ulang akan menghapus seluruh data yang ada di HDFS) [13]:

```bash
$ hdfs namenode -format
```

Jalankan daemon HDFS (NameNode, DataNode, dan Secondary NameNode) [13]:

```bash
$ start-dfs.sh
```

Pemantauan status klaster HDFS dapat diakses melalui antarmuka Web Browser di URL `http://localhost:9870` [13].

#### 5. Operasi Perintah Dasar CLI HDFS

Untuk mengelola berkas dan direktori pada HDFS, gunakan perintah antarmuka baris perintah (`hdfs dfs`) [14]:

```bash
# Menampilkan daftar berkas pada direktori root HDFS
$ hdfs dfs -ls /

# Membuat direktori baru secara rekursif
$ hdfs dfs -mkdir -p /user/bpdp

# Memeriksa isi direktori /user
$ hdfs dfs -ls /user
```

Untuk menghentikan seluruh daemon HDFS setelah selesai digunakan [14]:

```bash
$ stop-dfs.sh
```

---

## 5. Cloud Storage dan Hubungannya dengan DFS

### 5.1 Pengertian Cloud Storage

**Cloud Storage** adalah model layanan penyimpanan data daring di mana data disimpan, dikelola, dan diakses pada _logical storage pool_ yang berada di server jarak jauh (_remote servers_) melalui jaringan internet [6]. Infrastruktur fisik penyimpanannya dikelola dan dirawat oleh penyedia komputasi awan (_cloud provider_) seperti Amazon Web Services (AWS S3), Microsoft Azure Blob Storage, atau Google Cloud Storage (GCS) [6].

### 5.2 Relasi Cloud Storage dengan DFS

Secara konseptual, _Cloud Storage_ bertindak sebagai **media penyimpan fisik atau remote backend**, sedangkan _Distributed File System_ bertindak sebagai **pengorganisasi logika dan pengelola operasi sistem berkas** [7].

Sebagai contoh, layanan _object storage_ Amazon S3 dapat diintegrasikan sebagai media penyimpanan _backend_ untuk HDFS [7]. Pengintegrasian ini dilakukan melalui konfigurasi properti `fs.s3a.access.key`, `fs.s3a.secret.key`, dan `fs.s3a.endpoint` pada berkas `core-site.xml` [7].

### 5.3 Tipe-Tipe Cloud Storage

Penyimpanan awan diklasifikasikan menjadi tiga kategori utama [7]:

| Tipe Cloud Storage | Protokol Transpor             | Antarmuka Akses                     | Fitur & Skenario Penggunaan                                                                                            |
| :----------------- | :---------------------------- | :---------------------------------- | :--------------------------------------------------------------------------------------------------------------------- |
| **Block Storage**  | Fibre Channel (FC), iSCSI [7] | Direct-Attached atau SAN [7]        | Waktu tunda (_latency_) sangat rendah; sangat cocok untuk basis data terstruktur dan mesin virtual [7].                |
| **File Storage**   | TCP/IP [7]                    | NFS, SMB/CIFS [7]                   | Performa baik untuk berbagi berkas hirarkis (_file sharing_) dan _global file locking_ [7].                            |
| **Object Storage** | TCP/IP [7]                    | HTTP / REST API (Amazon S3 API) [7] | Skalabilitas masif tanpa batas hirarki; cocok untuk data tidak terstruktur (gambar, video, _backup_, _data lake_) [7]. |

Studi kasus teknologi _cloud storage_ modern meliputi: **Ceph**, **CubeFS** (sistem penyimpanan data tak terstruktur _cloud-native_ untuk Kubernetes), **RustFS**, **Silo** (fork MinIO kompatibel S3), dan **Rook** (_cloud-native storage orchestrator_ untuk Kubernetes) [7].

---

## 6. Kesimpulan

_Distributed File System_ (DFS) merupakan fondasi utama dari infrastruktur komputasi terdistribusi modern dan pemrosesan _Big Data_. Berbeda dengan _file system_ lokal yang mengelola media penyimpan tunggal, DFS menyajikan abstraksi sistem berkas tunggal yang koheren di atas sekumpulan mesin independen, menutupi kompleksitas kegagalan perangkat keras, jaringan, dan akses bersamaan.

Implementasi seperti **Apache Hadoop HDFS** menunjukkan bagaimana DFS mampu memanfaatkan perangkat keras komoditas (_commodity hardware_) untuk menyimpan data berskala sangat besar secara aman dan toleran terhadap kesalahan melalui pola arsitektur NameNode-DataNode serta mekanisme replikasi data. Dalam perkembangannya, DFS juga bersinergi secara erat dengan _Cloud Storage_ (khususnya _Object Storage_ berbasis antarmuka HTTP/REST) sebagai _backend_ penyimpanan yang scalable, dinamis, dan terdistribusi di era komputasi awan modern.
