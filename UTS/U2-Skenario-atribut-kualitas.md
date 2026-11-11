# U3. Taktik Arsitektur

## 1. Pendahuluan

Taktik arsitektur merupakan pendekatan teknis yang digunakan untuk memenuhi karakteristik kualitas sistem yang telah ditentukan pada U2. Pada layanan presensi kuliah LMS KALAM UMI, penerapan taktik difokuskan pada tiga karakteristik utama, yaitu kinerja (performance), keamanan (security), dan kemudahan perawatan (maintainability).

Pemilihan taktik didasarkan pada kebutuhan pengguna, hasil identifikasi stakeholder, serta permasalahan yang ditemukan pada layanan presensi. Setiap taktik dijelaskan berdasarkan cara kerjanya dan konsekuensi atau harga yang harus dibayar (trade-off) dalam penerapannya.

## 2. Taktik untuk Setiap Skenario

### 2.1 Skenario 1 – Kinerja (Performance)

**Taktik: Load Balancing dan Caching**

**Cara Kerja:**

1. Load balancing digunakan untuk mendistribusikan permintaan akses presensi dari mahasiswa ke beberapa server atau instance aplikasi sehingga beban tidak terpusat pada satu server.
2. Caching digunakan untuk menyimpan sementara informasi yang sering diakses, seperti jadwal perkuliahan dan informasi sesi presensi yang sedang berlangsung.
3. Ketika banyak mahasiswa mengakses layanan presensi secara bersamaan, permintaan dapat dilayani secara lebih merata sehingga mengurangi beban pemrosesan pada server dan basis data.
4. Sistem tetap melakukan validasi terhadap status sesi presensi sebelum menyimpan kehadiran mahasiswa agar data yang dicatat sesuai dengan sesi yang aktif.

**Harga yang Dibayar (Trade-off):**

- Membutuhkan sumber daya server tambahan untuk mendistribusikan beban permintaan.
- Konfigurasi dan pemeliharaan load balancer menambah kompleksitas infrastruktur.
- Data yang tersimpan dalam cache berpotensi tidak langsung mencerminkan perubahan terbaru sehingga diperlukan mekanisme pembaruan dan invalidasi cache.

**Hasil yang Diharapkan:**

Akses layanan presensi menjadi lebih responsif ketika terjadi lonjakan pengguna sehingga mahasiswa dapat melakukan presensi tanpa mengalami keterlambatan yang berlebihan.

### 2.2 Skenario 2 – Keamanan (Security)

**Taktik: Authentication, Role-Based Access Control (RBAC), dan Audit Logging**

**Cara Kerja:**

1. Authentication diterapkan untuk memastikan setiap pengguna harus melakukan login menggunakan akun yang telah terdaftar sebelum mengakses layanan presensi.
2. Role-Based Access Control (RBAC) digunakan untuk membatasi hak akses berdasarkan peran pengguna, yaitu mahasiswa, dosen, dan staf akademik.
3. Mahasiswa hanya dapat mengakses sesi presensi yang tersedia untuk perkuliahannya dan melakukan pencatatan kehadiran sesuai ketentuan.
4. Dosen memiliki hak akses untuk membuka dan menutup sesi presensi, memantau kehadiran mahasiswa, serta melakukan koreksi apabila ditemukan ketidaksesuaian data.
5. Staf akademik memperoleh akses terhadap informasi dan laporan presensi sesuai kewenangan administrasi yang diberikan.
6. Audit logging digunakan untuk mencatat aktivitas penting, seperti login, pembukaan sesi, perubahan data presensi, dan koreksi kehadiran sehingga aktivitas tersebut dapat ditelusuri kembali.
7. Komunikasi antara pengguna dan layanan dilindungi menggunakan HTTPS/TLS untuk mengurangi risiko penyadapan informasi selama proses pertukaran data.

**Harga yang Dibayar (Trade-off):**

- Proses autentikasi dan pemeriksaan hak akses menambah tahapan pemrosesan sehingga dapat sedikit memengaruhi waktu respons.
- Pengelolaan hak akses setiap peran membutuhkan konfigurasi dan pemeliharaan secara berkala.
- Audit logging membutuhkan ruang penyimpanan tambahan untuk mencatat aktivitas pengguna.
- Penerapan HTTPS/TLS membutuhkan pengelolaan sertifikat digital dan konfigurasi keamanan secara berkala.

**Hasil yang Diharapkan:**

Layanan presensi memiliki pembatasan akses yang jelas berdasarkan peran pengguna. Data kehadiran dan perubahan presensi dapat dilindungi serta ditelusuri sehingga mengurangi risiko akses tidak sah dan manipulasi data.

### 2.3 Skenario 3 – Kemudahan Perawatan (Maintainability)

**Taktik: Modularity dan Separation of Concerns**

**Cara Kerja:**

1. Sistem dibagi menjadi beberapa modul berdasarkan tanggung jawab masing-masing, seperti modul autentikasi, modul pengelolaan sesi presensi, modul pencatatan kehadiran, modul koreksi presensi, dan modul laporan.
2. Setiap modul memiliki fungsi dan tanggung jawab yang jelas sehingga perubahan pada satu bagian tidak harus mengubah keseluruhan sistem.
3. Pemisahan antara tampilan pengguna, logika bisnis, dan akses basis data diterapkan agar perubahan antarmuka tidak langsung memengaruhi proses pencatatan presensi.
4. Aturan pengelolaan presensi, seperti ketentuan pembukaan sesi dan validasi kehadiran, ditempatkan pada modul logika bisnis sehingga lebih mudah diperbarui.
5. Konfigurasi yang dapat berubah, seperti durasi sesi presensi dan aturan operasional, dikelola secara terpisah dari kode program agar penyesuaian tidak selalu membutuhkan perubahan pada banyak modul.
6. Setiap modul dilengkapi dokumentasi dan pengujian agar pengembang dapat memahami fungsi sistem serta memastikan perubahan tidak merusak fungsi yang sudah berjalan.

**Harga yang Dibayar (Trade-off):**

- Perancangan modul membutuhkan waktu dan perencanaan struktur sistem yang lebih matang.
- Pemisahan modul menambah kebutuhan pengelolaan komunikasi dan integrasi antarmodul.
- Dokumentasi dan pengujian setiap modul membutuhkan waktu pemeliharaan tambahan.
- Pembagian modul yang terlalu banyak dapat meningkatkan kompleksitas pengelolaan sistem.

**Hasil yang Diharapkan:**

Sistem lebih mudah dipelihara dan dikembangkan. Perubahan pada aturan presensi atau kebutuhan administrasi dapat dilakukan pada modul yang berkaitan tanpa harus mengubah keseluruhan sistem.

## 3. Ringkasan Penerapan Taktik

| Skenario | Taktik | Manfaat Utama | Harga yang Dibayar |
|---|---|---|---|
| Performance | Load Balancing dan Caching | Meningkatkan respons akses ketika terjadi lonjakan pengguna | Penambahan sumber daya dan kompleksitas infrastruktur |
| Security | Authentication, RBAC, dan Audit Logging | Melindungi data serta membatasi akses berdasarkan peran | Tambahan proses validasi, penyimpanan log, dan pengelolaan hak akses |
| Maintainability | Modularity dan Separation of Concerns | Memudahkan perubahan, pengembangan, dan pemeliharaan sistem | Membutuhkan perencanaan modul, dokumentasi, dan pengujian |

## 4. Kesimpulan

Berdasarkan ketiga karakteristik pendorong tersebut, penerapan taktik arsitektur pada layanan presensi kuliah LMS KALAM UMI diarahkan untuk menghasilkan sistem yang responsif, aman, dan mudah dipelihara.

Pada aspek kinerja, load balancing dan caching digunakan untuk membantu mengatasi peningkatan beban akses presensi. Pada aspek keamanan, authentication, Role-Based Access Control, dan audit logging diterapkan untuk membatasi akses serta menjaga keterlacakan aktivitas pengguna. Pada aspek kemudahan perawatan, modularity dan separation of concerns digunakan untuk memisahkan tanggung jawab setiap bagian sistem sehingga perubahan dapat dilakukan secara lebih terarah.

Setiap taktik memiliki konsekuensi yang perlu dipertimbangkan dalam perancangan arsitektur. Oleh karena itu, pemilihan taktik harus memperhatikan keseimbangan antara kebutuhan kualitas sistem, sumber daya yang tersedia, dan kompleksitas penerapannya.
