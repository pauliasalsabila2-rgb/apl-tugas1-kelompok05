
# U3. Taktik Arsitektur (Architectural Tactics)

## 1. Pendahuluan

Taktik arsitektur merupakan strategi desain yang digunakan untuk mencapai karakteristik kualitas tertentu dalam suatu sistem perangkat lunak. Taktik dipilih berdasarkan kebutuhan signifikan secara arsitektural (Architecturally Significant Requirements/ASR) dan skenario atribut kualitas yang telah dirumuskan pada U1 dan U2.

Pada layanan Presensi Kuliah LMS KALAM UMI, penerapan taktik arsitektur difokuskan pada tiga karakteristik utama, yaitu Performance (Kinerja), Security (Keamanan), dan Maintainability (Kemudahan Perawatan).

Pemilihan taktik ini bertujuan mengatasi permasalahan perlambatan akses ketika mahasiswa melakukan presensi secara bersamaan, mencegah manipulasi data kehadiran, serta mempermudah tim pengembang dalam melakukan perubahan dan pemeliharaan sistem.

## 2. Taktik untuk Performance (Kinerja)

### 2.1 Tujuan

Meningkatkan kemampuan layanan presensi dalam menangani permintaan mahasiswa secara bersamaan, khususnya pada jam pergantian perkuliahan, tanpa menyebabkan perlambatan akses maupun kegagalan pemrosesan.

### 2.2 Taktik yang Digunakan

**a. Manage Request (Pengelolaan Permintaan)**

Sistem mengatur permintaan presensi yang masuk agar tidak seluruhnya diproses secara langsung oleh basis data utama. Penggunaan Message Queue (RabbitMQ) membantu mengelola antrean permintaan dan meneruskan pekerjaan kepada background worker secara bertahap.

**b. Maintain Multiple Copies of Data (Menyediakan Salinan Data)**

Sistem menggunakan mekanisme in-memory caching melalui Redis untuk menyimpan data yang sering diakses, seperti informasi sesi perkuliahan dan status sesi presensi. Dengan demikian, permintaan pembacaan data yang berulang tidak selalu harus diteruskan ke basis data utama.

**c. Increase Resource Efficiency (Meningkatkan Efisiensi Sumber Daya)**

Pemrosesan permintaan dirancang agar penggunaan sumber daya server lebih efisien. Pemisahan proses penerimaan permintaan dan pemrosesan data melalui background worker membantu mengurangi beban pemrosesan secara langsung pada server aplikasi.

### 2.3 Hubungan dengan Skenario U2

Taktik Performance diterapkan untuk mendukung skenario lonjakan akses hingga 3.000 permintaan presensi secara bersamaan. Redis membantu mempercepat akses data yang sering digunakan, sedangkan RabbitMQ membantu mengatur antrean permintaan agar proses pencatatan kehadiran tidak membebani basis data secara berlebihan.

### 2.4 Ukuran Keberhasilan

- Sistem mampu menangani hingga 3.000 permintaan presensi secara bersamaan.
- Sebanyak 95% permintaan memperoleh respons kurang dari 2 detik.
- Tingkat kegagalan permintaan sebesar 0% pada kondisi pengujian yang ditetapkan.
- Penggunaan cache dan antrean pesan dapat mengurangi beban akses langsung ke basis data utama.

## 3. Taktik untuk Security (Keamanan)

### 3.1 Tujuan

Menjaga kerahasiaan, keaslian, dan integritas data kehadiran mahasiswa serta mencegah tindakan kecurangan seperti penitipan presensi.

### 3.2 Taktik yang Digunakan

**a. Authenticate Users (Autentikasi Pengguna)**

Setiap pengguna diwajibkan melakukan autentikasi sebelum mengakses layanan presensi. Sistem memverifikasi identitas pengguna melalui mekanisme login sehingga hanya pengguna yang terdaftar yang dapat menggunakan layanan.

**b. Authorize Users (Otorisasi Pengguna)**

Sistem menerapkan Role-Based Access Control (RBAC) untuk membatasi hak akses berdasarkan peran pengguna.

Pembagian hak akses meliputi:
- Mahasiswa: melakukan presensi dan melihat riwayat kehadiran miliknya sendiri.
- Dosen: membuka sesi presensi, memantau kehadiran mahasiswa, dan melakukan koreksi presensi pada kelas yang diampu.
- Admin: mengelola data pengguna dan administrasi presensi sesuai kewenangannya.

**c. Validate Input (Validasi Masukan)**

Sistem melakukan validasi terhadap setiap permintaan presensi sebelum data disimpan. Validasi mencakup identitas mahasiswa, status sesi perkuliahan, keabsahan QR Code, serta kesesuaian koordinat GPS dengan lokasi perkuliahan.

**d. Detect Attacks (Mendeteksi Serangan)**

Sistem melakukan pemeriksaan terhadap permintaan yang menunjukkan indikasi kecurangan, seperti penggunaan QR Code yang telah kedaluwarsa, pemindaian tangkapan layar QR Code lama, dan koordinat lokasi yang tidak sesuai dengan area perkuliahan.

### 3.3 Hubungan dengan Skenario U2

Taktik Security diterapkan untuk memastikan setiap transaksi presensi berasal dari pengguna yang sah dan memenuhi ketentuan perkuliahan. QR Code dinamis digunakan untuk mengurangi risiko penggunaan kode lama, sedangkan validasi GPS dan geofencing digunakan untuk memeriksa lokasi mahasiswa.

Penerapan autentikasi dan otorisasi juga memastikan bahwa perubahan data presensi hanya dapat dilakukan oleh pengguna yang memiliki kewenangan.

### 3.4 Ukuran Keberhasilan

- QR Code memiliki masa berlaku maksimal 5 detik sebelum diperbarui secara otomatis.
- Validasi lokasi menggunakan radius maksimal 15 meter dari titik tengah area perkuliahan.
- Sistem menolak seluruh upaya manipulasi QR Code dan fake GPS yang berhasil disimulasikan dalam pengujian.
- Hak akses koreksi presensi hanya diberikan kepada dosen dan admin sesuai kewenangannya.
- Setiap perubahan data presensi memiliki catatan aktivitas yang dapat ditelusuri.

## 4. Taktik untuk Maintainability (Kemudahan Perawatan)

### 4.1 Tujuan

Mempermudah tim pengembang dalam melakukan perbaikan kesalahan, pembaruan aturan bisnis, serta pengembangan fitur presensi tanpa mengganggu fungsi subsistem akademik lainnya.

### 4.2 Taktik yang Digunakan

**a. Increase Cohesion (Meningkatkan Kohesi Modul)**

Sistem membagi fungsi layanan presensi ke dalam modul-modul berdasarkan tanggung jawab masing-masing. Pemisahan ini bertujuan agar setiap modul memiliki fungsi yang jelas dan tidak menangani terlalu banyak tanggung jawab sekaligus.

**b. Reduce Coupling (Mengurangi Ketergantungan Antar-Modul)**

Setiap modul berkomunikasi melalui antarmuka atau API yang telah ditentukan. Modul tampilan tidak diperbolehkan mengakses basis data secara langsung, tetapi harus melalui modul pengelola proses dan aturan bisnis.

**c. Encapsulate (Enkapsulasi)**

Aturan bisnis presensi, seperti batas waktu keterlambatan dan ketentuan koreksi kehadiran, ditempatkan pada modul khusus. Dengan demikian, perubahan aturan dapat dilakukan tanpa mengubah keseluruhan struktur sistem.

**d. Support Testing (Mendukung Pengujian)**

Setiap modul dikembangkan agar dapat diuji secara terpisah melalui unit testing dan integration testing. Pengujian dilakukan untuk memastikan perubahan pada satu modul tidak menimbulkan kesalahan pada modul lainnya.

### 4.3 Hubungan dengan Skenario U2

Taktik Maintainability diterapkan dengan membagi layanan presensi menjadi beberapa modul yang memiliki tanggung jawab berbeda. Pemisahan modul memungkinkan tim pengembang memperbarui aturan bisnis tanpa melakukan perubahan menyeluruh terhadap sistem.

Penerapan unit testing, integration testing, dan code review juga membantu memastikan bahwa setiap perubahan tidak merusak fungsi yang sudah berjalan.

### 4.4 Ukuran Keberhasilan

- Perubahan aturan bisnis presensi hanya memerlukan pembaruan maksimal satu modul kode.
- Pengembang baru dapat memahami dan melakukan modifikasi kecil dalam waktu kurang dari 4 jam kerja.
- Pengujian memiliki code coverage minimal 80% untuk fungsi yang diuji.
- Perubahan pada modul presensi tidak menyebabkan gangguan terhadap subsistem akademik lainnya.

## 5. Taktik Pendukung untuk Resilience (Ketahanan terhadap Gangguan Jaringan)

Selain tiga karakteristik utama, layanan presensi juga memperhatikan kebutuhan ketahanan terhadap jaringan internet yang tidak stabil.

Taktik pendukung yang digunakan meliputi:

**a. Maintain Data at Client (Menyimpan Data pada Sisi Klien)**

Data presensi yang dikirimkan mahasiswa dapat disimpan sementara pada perangkat menggunakan local storage atau SQLite apabila koneksi internet mengalami gangguan.

**b. Retry (Mengulangi Permintaan)**

Sistem menyediakan mekanisme pengiriman ulang permintaan presensi ketika koneksi internet kembali tersedia. Mekanisme ini membantu memastikan data yang belum berhasil dikirim dapat diproses kembali.

**c. Prevent Duplicate Processing (Mencegah Pemrosesan Ganda)**

Sistem menggunakan mekanisme idempotent API endpoints untuk memastikan pengiriman ulang data presensi tidak menyebabkan pencatatan kehadiran ganda.

**d. Recover (Pemulihan Sistem)**

Ketika koneksi kembali stabil, sistem melakukan sinkronisasi data presensi yang sebelumnya tersimpan sementara pada perangkat menuju server utama.

Taktik pendukung ini bertujuan menjaga keberlangsungan proses presensi ketika mahasiswa mengalami kendala koneksi internet di lingkungan perkuliahan.

## 6. Tabel Ringkasan Taktik Arsitektur

| No. | Karakteristik | Taktik Arsitektur | Tujuan Penerapan |
|---|---|---|---|
| 1 | Performance | Manage Request | Mengatur antrean permintaan presensi menggunakan Message Queue. |
| 2 | Performance | Maintain Multiple Copies of Data | Mempercepat akses data menggunakan Redis Cache. |
| 3 | Performance | Increase Resource Efficiency | Mengoptimalkan penggunaan sumber daya server. |
| 4 | Security | Authenticate Users | Memastikan identitas pengguna sebelum mengakses sistem. |
| 5 | Security | Authorize Users | Membatasi hak akses berdasarkan peran pengguna. |
| 6 | Security | Validate Input | Memvalidasi QR Code, identitas, sesi, dan lokasi presensi. |
| 7 | Security | Detect Attacks | Mendeteksi indikasi kecurangan presensi. |
| 8 | Maintainability | Increase Cohesion | Memisahkan fungsi sistem berdasarkan tanggung jawab modul. |
| 9 | Maintainability | Reduce Coupling | Mengurangi ketergantungan antar-modul. |
| 10 | Maintainability | Encapsulate | Memisahkan aturan bisnis dari fungsi lainnya. |
| 11 | Maintainability | Support Testing | Mempermudah pengujian setiap modul secara terpisah. |
| 12 | Resilience | Maintain Data at Client | Menyimpan sementara data ketika jaringan terganggu. |
| 13 | Resilience | Retry | Mengirim ulang permintaan ketika koneksi pulih. |
| 14 | Resilience | Prevent Duplicate Processing | Mencegah pencatatan presensi ganda. |
| 15 | Resilience | Recover | Memulihkan dan menyinkronkan data presensi. |

## 7. Kesimpulan

Berdasarkan hasil identifikasi kebutuhan dan skenario atribut kualitas, penerapan taktik arsitektur pada layanan Presensi Kuliah LMS KALAM UMI difokuskan pada Performance, Security, dan Maintainability.

Taktik Performance menggunakan pengelolaan antrean permintaan dan mekanisme caching untuk membantu mengatasi lonjakan akses mahasiswa. Taktik Security menerapkan autentikasi, otorisasi, validasi masukan, serta pendeteksian indikasi kecurangan untuk menjaga integritas data kehadiran. Taktik Maintainability menerapkan pemisahan modul, pengurangan ketergantungan, enkapsulasi, dan pengujian agar sistem lebih mudah dipelihara.

Selain itu, taktik pendukung Resilience diterapkan untuk mengantisipasi gangguan jaringan melalui penyimpanan sementara, pengiriman ulang, dan sinkronisasi data.

Seluruh taktik tersebut menjadi dasar dalam menentukan pola arsitektur yang akan dibahas pada U4, sehingga struktur arsitektur yang dipilih dapat mendukung kebutuhan kualitas layanan presensi secara konsisten.

