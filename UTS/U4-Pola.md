# U4. Pola Arsitektur (Architectural Patterns)

## 1. Pendahuluan

Pola arsitektur merupakan solusi umum yang dapat digunakan untuk menyelesaikan permasalahan berulang dalam perancangan struktur dan interaksi sistem perangkat lunak. Pemilihan pola arsitektur harus mempertimbangkan kebutuhan fungsional, karakteristik kualitas, serta batasan yang telah diidentifikasi pada tahap sebelumnya.

Pada layanan Presensi Kuliah LMS KALAM UMI, pemilihan pola arsitektur didasarkan pada tiga karakteristik pendorong utama, yaitu Performance (Kinerja), Security (Keamanan), dan Maintainability (Kemudahan Perawatan).

Berdasarkan kebutuhan tersebut, dipilih dua pola arsitektur dari kategori yang berbeda, yaitu Layered Architecture sebagai pola struktural dan Event-Driven Architecture sebagai pola komunikasi antarkomponen. Kedua pola tersebut digunakan secara saling melengkapi untuk mendukung pengelolaan proses presensi, penanganan lonjakan akses, serta pemeliharaan sistem.

## 2. Pola Arsitektur yang Dipilih

### 2.1 Layered Architecture (Pola Berlapis)

**Kategori:** Structural Architectural Pattern.

#### A. Pengertian

Layered Architecture merupakan pola arsitektur yang membagi sistem perangkat lunak ke dalam beberapa lapisan berdasarkan tanggung jawab dan fungsi masing-masing. Setiap lapisan memiliki tugas tertentu dan berkomunikasi dengan lapisan lainnya melalui antarmuka yang telah ditentukan.

Pola ini digunakan agar fungsi tampilan, pengelolaan proses, aturan bisnis, dan penyimpanan data tidak tercampur dalam satu bagian sistem.

#### B. Penerapan pada Layanan Presensi LMS KALAM UMI

Pola Layered Architecture diterapkan dengan membagi layanan presensi menjadi empat lapisan utama sebagai berikut.

1. **Presentation Layer (Lapisan Tampilan)**

   Lapisan ini berfungsi sebagai antarmuka antara pengguna dan sistem. Mahasiswa menggunakan lapisan ini untuk melakukan presensi dan melihat riwayat kehadiran, sedangkan dosen menggunakannya untuk membuka sesi, memantau kehadiran, dan melakukan koreksi presensi.

2. **Application Layer (Lapisan Koordinasi)**

   Lapisan ini bertanggung jawab mengatur alur proses layanan presensi, seperti menerima permintaan pengguna, mengoordinasikan validasi, memproses permintaan presensi, dan menghubungkan fungsi antarmodul.

3. **Domain Layer (Lapisan Aturan Bisnis)**

   Lapisan ini berisi aturan dan logika utama layanan presensi, seperti validasi sesi perkuliahan, pemeriksaan status kehadiran, ketentuan koreksi presensi, validasi QR Code, dan aturan hak akses pengguna.

4. **Data Layer (Lapisan Data)**

   Lapisan ini bertanggung jawab mengelola penyimpanan, pengambilan, pembaruan, dan pengelolaan data presensi melalui integrasi dengan basis data utama SIAKAD UMI.

#### C. Alasan Pemilihan

Layered Architecture dipilih karena layanan presensi memiliki beberapa fungsi yang membutuhkan pemisahan tanggung jawab secara jelas. Proses tampilan mahasiswa dan dosen perlu dipisahkan dari aturan bisnis presensi serta pengelolaan basis data.

Pola ini juga sesuai dengan struktur modul yang telah dirancang pada B3 Tugas Kelompok I. Pemisahan empat lapisan membantu tim pengembang PDTI UMI melakukan perubahan pada aturan bisnis tanpa harus mengubah keseluruhan sistem.

#### D. Kelebihan

- Memiliki pembagian tanggung jawab yang jelas pada setiap lapisan.
- Memudahkan proses pemeliharaan dan pengembangan sistem.
- Mengurangi ketergantungan langsung antara antarmuka pengguna dan basis data.
- Memudahkan pengujian setiap lapisan secara terpisah.
- Mendukung penerapan keamanan melalui pengelolaan autentikasi dan otorisasi pada lapisan yang sesuai.

#### E. Kekurangan

- Permintaan pengguna harus melewati beberapa lapisan sehingga dapat menambah waktu pemrosesan.
- Perubahan yang melibatkan beberapa lapisan membutuhkan koordinasi pengembangan.
- Apabila tidak dirancang dengan baik, komunikasi antarlapisan dapat menambah kompleksitas sistem.
- Pemisahan lapisan saja tidak secara otomatis menyelesaikan permasalahan lonjakan akses secara bersamaan.

#### F. Hubungan dengan Karakteristik Kualitas

| Karakteristik | Kontribusi Layered Architecture |
|---|---|
| Performance | Membantu mengatur alur pemrosesan permintaan agar setiap fungsi memiliki tanggung jawab yang jelas. |
| Security | Memudahkan penerapan autentikasi, otorisasi, dan validasi aturan bisnis pada lapisan yang sesuai. |
| Maintainability | Memudahkan perubahan dan pengujian modul tanpa harus mengubah seluruh sistem. |

---

### 2.2 Event-Driven Architecture (Pola Berbasis Peristiwa)

**Kategori:** Communication and Interaction Architectural Pattern.

#### A. Pengertian

Event-Driven Architecture merupakan pola arsitektur yang memungkinkan komponen sistem berkomunikasi melalui peristiwa (event) yang terjadi selama proses operasional. Suatu komponen dapat menghasilkan event, kemudian event tersebut diterima dan diproses oleh komponen lain tanpa harus melakukan komunikasi langsung secara terus-menerus.

Pola ini mendukung pemrosesan secara asinkron sehingga penerimaan permintaan dapat dipisahkan dari proses pengolahan data lebih lanjut.

#### B. Penerapan pada Layanan Presensi LMS KALAM UMI

Event-Driven Architecture diterapkan untuk membantu menangani permintaan presensi mahasiswa yang masuk secara bersamaan.

Alur penerapannya adalah sebagai berikut.

1. Mahasiswa mengirimkan permintaan presensi melalui aplikasi atau browser LMS KALAM UMI.
2. Server Web Presensi menerima dan melakukan validasi awal terhadap identitas, sesi perkuliahan, QR Code, dan informasi lokasi.
3. Setelah permintaan dinyatakan valid, sistem menghasilkan event presensi dan memasukkannya ke dalam Message Queue menggunakan RabbitMQ.
4. Background Worker mengambil event dari antrean untuk diproses secara bertahap.
5. Data kehadiran disimpan ke basis data utama SIAKAD UMI.
6. Sistem memberikan informasi hasil pemrosesan kepada pengguna setelah transaksi berhasil diselesaikan.

Redis Cache digunakan sebagai komponen pendukung untuk menyimpan data yang sering diakses, seperti informasi sesi presensi, sehingga permintaan pembacaan data tidak selalu membebani basis data utama.

#### C. Alasan Pemilihan

Event-Driven Architecture dipilih karena proses presensi dapat menghasilkan lonjakan permintaan dalam waktu yang hampir bersamaan, khususnya ketika dosen membuka sesi perkuliahan.

Pemrosesan secara asinkron melalui RabbitMQ memungkinkan sistem mengelola permintaan melalui antrean sebelum diproses oleh background worker. Dengan demikian, beban pemrosesan dapat diatur secara lebih terstruktur dan tidak seluruhnya diteruskan secara langsung ke basis data utama.

Pola ini juga mendukung pemisahan komponen sehingga perubahan pada proses pemrosesan event dapat dilakukan tanpa harus mengubah keseluruhan fungsi layanan presensi.

#### D. Kelebihan

- Mendukung pemrosesan permintaan secara asinkron.
- Membantu menangani lonjakan permintaan pengguna secara bersamaan.
- Mengurangi ketergantungan komunikasi langsung antarkomponen.
- Memungkinkan pemrosesan dilakukan secara bertahap melalui antrean pesan.
- Memudahkan penambahan komponen pemroses baru sesuai kebutuhan sistem.

#### E. Kekurangan

- Pengelolaan antrean pesan menambah kompleksitas sistem.
- Hasil pemrosesan tidak selalu tersedia secara langsung karena terdapat proses antrean.
- Membutuhkan mekanisme penanganan event gagal agar data tidak hilang.
- Berpotensi terjadi pemrosesan ganda apabila mekanisme idempotensi tidak diterapkan.
- Pemantauan dan penelusuran alur event membutuhkan mekanisme logging yang baik.

#### F. Hubungan dengan Karakteristik Kualitas

| Karakteristik | Kontribusi Event-Driven Architecture |
|---|---|
| Performance | Membantu mengelola lonjakan permintaan melalui antrean pesan dan pemrosesan asinkron. |
| Security | Mendukung pemisahan proses validasi awal dan pemrosesan event, tetapi keamanan tetap harus diterapkan melalui autentikasi, otorisasi, dan validasi pesan. |
| Maintainability | Memungkinkan komponen pemroses event dikembangkan dan diperbarui secara lebih terpisah. |

---

## 3. Perbandingan Kedua Pola Arsitektur

| Aspek | Layered Architecture | Event-Driven Architecture |
|---|---|---|
| Kategori | Struktural | Komunikasi dan interaksi |
| Fokus utama | Pembagian tanggung jawab sistem ke dalam lapisan. | Komunikasi dan pemrosesan berdasarkan peristiwa. |
| Mekanisme komunikasi | Melalui pemanggilan antarlapisan. | Melalui pengiriman dan penerimaan event. |
| Penerapan | Presentation, Application, Domain, dan Data Layer. | Message Queue RabbitMQ dan Background Worker. |
| Karakteristik utama yang didukung | Maintainability dan Security. | Performance dan Maintainability. |
| Tantangan | Penambahan waktu pemrosesan akibat komunikasi antarlapisan. | Kompleksitas antrean, sinkronisasi, dan penanganan event gagal. |

Kedua pola tersebut memiliki fungsi yang berbeda tetapi saling melengkapi. Layered Architecture mengatur struktur dan pembagian tanggung jawab sistem, sedangkan Event-Driven Architecture mengatur komunikasi serta pemrosesan permintaan secara asinkron.

Kombinasi keduanya diharapkan dapat menghasilkan layanan presensi yang lebih terstruktur, mudah dipelihara, dan mampu menangani peningkatan jumlah permintaan mahasiswa.

## 4. Perbedaan Architectural Style dan Architectural Pattern

Architectural Style dan Architectural Pattern merupakan dua konsep yang berkaitan dalam perancangan arsitektur perangkat lunak, tetapi memiliki penekanan yang berbeda.

**Architectural Style (Gaya Arsitektur)** merupakan pendekatan umum yang menentukan bentuk dasar struktur sistem, jenis komponen, hubungan antarkomponen, serta aturan komunikasi yang digunakan.

Sementara itu, **Architectural Pattern (Pola Arsitektur)** merupakan solusi desain yang dapat digunakan kembali untuk menyelesaikan permasalahan arsitektur tertentu dalam konteks sistem yang memiliki kebutuhan serupa.

Perbedaan keduanya dapat dilihat pada tabel berikut.

| Aspek | Architectural Style | Architectural Pattern |
|---|---|---|
| Pengertian | Pendekatan umum dalam mengorganisasi struktur sistem. | Solusi desain untuk permasalahan arsitektur tertentu. |
| Fokus | Struktur dan aturan umum sistem. | Penyelesaian masalah desain berdasarkan konteks tertentu. |
| Cakupan | Menentukan bentuk umum organisasi komponen. | Memberikan cara penerapan solusi pada bagian sistem. |
| Contoh | Layered Style, Client-Server Style, dan Event-Driven Style. | Layered Pattern, Event-Driven Pattern, dan Pipe-and-Filter Pattern. |
| Penerapan | Menjadi pedoman umum dalam membangun sistem. | Digunakan untuk menyelesaikan kebutuhan desain tertentu. |

Pada layanan presensi LMS KALAM UMI, pendekatan berlapis digunakan untuk mengorganisasi fungsi sistem, sedangkan pola berbasis peristiwa digunakan untuk mengatur komunikasi dan pemrosesan permintaan presensi. Keduanya diterapkan sesuai kebutuhan layanan dan karakteristik kualitas yang ingin dicapai.

## 5. Kesimpulan

Berdasarkan hasil analisis, Layered Architecture dan Event-Driven Architecture dipilih sebagai pola arsitektur yang saling melengkapi dalam perancangan layanan Presensi Kuliah LMS KALAM UMI.

Layered Architecture mendukung pemisahan tanggung jawab sistem ke dalam empat lapisan, yaitu Presentation, Application, Domain, dan Data Layer. Pola ini membantu meningkatkan keteraturan struktur, keamanan, dan kemudahan pemeliharaan sistem.

Event-Driven Architecture mendukung pengelolaan komunikasi dan pemrosesan permintaan presensi secara asinkron melalui Message Queue dan Background Worker. Pola ini membantu mengatasi lonjakan akses mahasiswa pada waktu perkuliahan.

Kombinasi kedua pola tersebut menjadi dasar dalam penyusunan desain arsitektur dan diagram pada U5. Diagram yang dibuat harus menunjukkan penerapan empat lapisan, hubungan antarkomponen, serta mekanisme komunikasi melalui RabbitMQ dan Redis sesuai rancangan pada B3.

