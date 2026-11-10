
# U1. Konteks dan ASR (Architecturally Significant Requirements)

## 1. Ringkasan Layanan

Layanan Presensi Kuliah pada LMS KALAM UMI merupakan bagian dari layanan akademik digital yang digunakan untuk mengelola dan mencatat kehadiran mahasiswa selama kegiatan perkuliahan. Layanan ini mendukung beberapa aktivitas utama, yaitu pembukaan sesi presensi oleh dosen, pelaksanaan presensi oleh mahasiswa, pemantauan status kehadiran, koreksi data presensi, serta penyediaan laporan rekapitulasi kehadiran.

Mahasiswa menggunakan layanan ini untuk melakukan presensi sesuai sesi perkuliahan yang telah dibuka oleh dosen dan memeriksa riwayat kehadiran masing-masing. Dosen bertanggung jawab membuka sesi presensi, memantau kehadiran mahasiswa, serta melakukan koreksi apabila terdapat ketidaksesuaian data atau kendala jaringan. Staf Biro Akademik memanfaatkan data rekapitulasi kehadiran untuk kebutuhan administrasi akademik, termasuk pemeriksaan persentase kehadiran sebagai salah satu persyaratan mengikuti Ujian Akhir Semester (UAS).

Berdasarkan hasil analisis dan wawancara, layanan presensi masih menghadapi beberapa permasalahan, terutama perlambatan akses ketika banyak mahasiswa melakukan presensi secara bersamaan, ketidaksesuaian data kehadiran, serta gangguan koneksi jaringan di lingkungan perkuliahan. Oleh karena itu, perancangan arsitektur layanan perlu memperhatikan aspek kinerja, keamanan, integritas data, dan kemudahan pemeliharaan sistem.

## 2. Konteks Arsitektur

Analisis konteks arsitektur dilakukan melalui empat perspektif, yaitu teknis, siklus hidup proyek, bisnis, dan profesional.

| Konteks | Deskripsi |
|---|---|
| Teknis | Layanan presensi membutuhkan mekanisme autentikasi pengguna, pengelolaan sesi perkuliahan, pencatatan kehadiran, validasi data, serta integrasi dengan basis data utama SIAKAD UMI. Sistem juga perlu mendukung akses secara bersamaan, validasi lokasi, dan sinkronisasi data ketika jaringan mengalami gangguan. |
| Siklus Hidup Proyek | Pengembangan layanan perlu mencakup analisis kebutuhan, perancangan arsitektur, implementasi, pengujian, penerapan, dan pemeliharaan. Arsitektur harus bersifat modular agar perubahan aturan presensi dan perbaikan sistem dapat dilakukan tanpa mengganggu layanan akademik lainnya. |
| Bisnis | Layanan presensi bertujuan membantu mahasiswa, dosen, dan pengelola akademik dalam mencatat, memantau, mengoreksi, serta merekap kehadiran secara efisien. Data presensi harus akurat karena berkaitan dengan pemantauan aktivitas perkuliahan dan persyaratan administrasi akademik mahasiswa. |
| Profesional | Pengelolaan layanan harus memperhatikan kerahasiaan data mahasiswa, pembatasan hak akses, dan tanggung jawab setiap pengguna. Setiap perubahan data presensi perlu memiliki catatan aktivitas agar dapat ditelusuri dan dipertanggungjawabkan. |

## 3. Stakeholder dan Kebutuhannya

Berdasarkan hasil identifikasi stakeholder dan lima wawancara yang dilakukan pada Tugas Kelompok I, terdapat enam kelompok pemangku kepentingan yang memiliki kebutuhan terhadap layanan presensi kuliah.

| Stakeholder | Kebutuhan dan Kepentingan |
|---|---|
| Mahasiswa | Melakukan presensi dengan cepat, memperoleh konfirmasi kehadiran secara langsung, melihat riwayat presensi, dan mendapatkan kepastian pencatatan kehadiran. |
| Dosen Pengampu | Membuka dan menutup sesi presensi, memantau kehadiran mahasiswa, serta melakukan koreksi presensi apabila terjadi kendala atau ketidaksesuaian data. |
| Staf Biro Akademik | Memperoleh data kehadiran yang akurat, konsisten, dan dapat digunakan untuk kebutuhan administrasi serta laporan kelayakan mengikuti UAS. |
| Tim Pengembang (PDTI UMI) | Mengembangkan dan memelihara layanan presensi yang terintegrasi dengan basis data SIAKAD UMI serta mudah diperbarui. |
| Tim Operasional Server | Menjaga kestabilan dan ketersediaan layanan, khususnya ketika terjadi peningkatan akses secara bersamaan pada jam perkuliahan. |
| Auditor Mutu Internal UMI | Memastikan keabsahan data kehadiran, kesesuaian pelaksanaan perkuliahan, dan ketersediaan riwayat perubahan data sebagai bukti audit. |

Berdasarkan kebutuhan tersebut, mahasiswa dan dosen menjadi pengguna utama yang berinteraksi langsung dengan layanan. Staf akademik, tim pengembang, tim operasional, dan auditor memiliki kepentingan terhadap keakuratan, keamanan, ketersediaan, serta keberlanjutan pengelolaan sistem.

## 4. Batasan Sistem

Perancangan arsitektur layanan presensi kuliah memperhatikan beberapa batasan yang telah diidentifikasi pada Tugas Kelompok I.

### 4.1 Batasan Teknologi

1. Sistem menggunakan basis data utama SIAKAD UMI berbasis PostgreSQL yang telah tersedia di lingkungan kampus.
2. Layanan dapat diakses melalui aplikasi perangkat bergerak atau web pengguna.
3. Mekanisme validasi lokasi menggunakan koordinat GPS dan geofencing pada area kampus UMI.

### 4.2 Batasan Bisnis

1. Pengembangan dan implementasi layanan ditargetkan selesai dalam waktu dua bulan sebelum semester baru dimulai.
2. Anggaran infrastruktur terbatas sehingga pengembangan tidak diarahkan pada penambahan server fisik baru khusus untuk layanan presensi.

### 4.3 Batasan Operasional

1. Dukungan teknis langsung dari tim IT kampus tersedia pada jam kerja operasional pukul 08.00–16.00 WITA.
2. Koreksi presensi memerlukan persetujuan resmi dari dosen pengampu maksimal tujuh hari setelah jadwal perkuliahan berlangsung.

Batasan tersebut memengaruhi pemilihan arsitektur, terutama dalam penggunaan komponen modular, integrasi dengan basis data yang sudah tersedia, pengelolaan hak akses, serta mekanisme penanganan gangguan jaringan.

## 5. Karakteristik Pendorong Arsitektur

Berdasarkan hasil identifikasi delapan kandidat karakteristik arsitektur pada bagian B1, ditetapkan tiga karakteristik pendorong utama (architecture drivers) yang menjadi dasar perancangan layanan presensi kuliah.

1. **Performance (Kinerja)**

   Sistem harus mampu memproses permintaan presensi secara cepat dan stabil ketika banyak mahasiswa mengakses layanan secara bersamaan. Karakteristik ini menjadi penting karena perlambatan pemuatan halaman pernah dialami mahasiswa pada menit-menit awal perkuliahan.

2. **Security (Keamanan)**

   Sistem harus melindungi informasi kehadiran mahasiswa dan mencegah tindakan kecurangan akademik, seperti penitipan presensi, penggunaan QR Code lama, dan manipulasi lokasi GPS.

3. **Maintainability (Kemudahan Perawatan)**

   Arsitektur perlu memiliki pembagian modul yang jelas agar tim pengembang PDTI UMI dapat memperbaiki kesalahan, memperbarui aturan presensi, dan mengembangkan fitur tanpa mengganggu subsistem akademik lainnya.

Ketiga karakteristik tersebut dipilih karena memiliki pengaruh langsung terhadap struktur arsitektur, kualitas operasional sistem, serta kebutuhan stakeholder.

## 6. Architecturally Significant Requirements (ASR)

Architecturally Significant Requirements (ASR) merupakan kebutuhan sistem yang memiliki pengaruh besar terhadap keputusan dan rancangan arsitektur perangkat lunak. Berdasarkan hasil identifikasi pada bagian A4, ditetapkan tiga kebutuhan signifikan secara arsitektural untuk layanan presensi kuliah.

### 6.1 ASR 1: Kinerja dan Transaksi Bersamaan (Performance & Concurrency)

**Persyaratan:**

Sistem harus mampu menangani hingga 3.000 permintaan presensi mahasiswa secara bersamaan (concurrent requests), khususnya pada jam pergantian perkuliahan, dengan waktu tanggap (response time) kurang dari 2 detik.

**Alasan Signifikansi Arsitektur:**

Lonjakan akses secara bersamaan dapat menyebabkan peningkatan beban pemrosesan pada server dan basis data. Apabila seluruh permintaan langsung diproses oleh basis data utama, sistem berpotensi mengalami perlambatan atau kegagalan layanan. Oleh karena itu, arsitektur perlu mempertimbangkan penggunaan in-memory cache dan message queue untuk membantu mengelola permintaan serta mengurangi beban pemrosesan.

**Kriteria Keberhasilan:**

- Sistem mampu menangani 3.000 permintaan bersamaan.
- Sebanyak 95% permintaan presensi memperoleh respons kurang dari 2 detik.
- Pengujian dilakukan menggunakan load testing pada kondisi beban puncak perkuliahan.

### 6.2 ASR 2: Integritas Data dan Keamanan Anti-Kecurangan (Data Integrity & Anti-Fraud)

**Persyaratan:**

Sistem harus memiliki mekanisme keamanan yang dapat mencegah kecurangan presensi, seperti penggunaan QR Code melalui tangkapan layar milik mahasiswa lain dan manipulasi koordinat lokasi menggunakan fake GPS.

**Alasan Signifikansi Arsitektur:**

Data kehadiran mahasiswa merupakan informasi penting dalam administrasi akademik. Kesalahan atau manipulasi pencatatan dapat menyebabkan ketidaksesuaian rekapitulasi kehadiran dan memengaruhi kelayakan mahasiswa mengikuti ujian. Oleh karena itu, arsitektur perlu mendukung mekanisme validasi berlapis melalui QR Code dinamis dan verifikasi koordinat GPS.

**Kriteria Keberhasilan:**

- QR Code memiliki masa berlaku maksimal 5 detik sebelum diperbarui secara otomatis.
- Validasi lokasi menggunakan radius maksimal 15 meter dari titik tengah area gedung perkuliahan UMI.
- Sistem harus menolak upaya penggunaan QR Code yang sudah kedaluwarsa dan manipulasi lokasi.
- Hak akses koreksi data presensi dibatasi berdasarkan peran pengguna melalui mekanisme Role-Based Access Control (RBAC).

### 6.3 ASR 3: Ketahanan terhadap Jaringan Tidak Stabil (Resilience / Low-Connectivity Support)

**Persyaratan:**

Sistem harus mampu menangani kondisi jaringan internet yang tidak stabil ketika mahasiswa melakukan presensi. Data presensi yang dikirimkan melalui perangkat bergerak perlu memiliki mekanisme penyimpanan sementara pada penyimpanan lokal (local storage/SQLite), kemudian disinkronkan secara otomatis ketika koneksi jaringan kembali normal.

**Alasan Signifikansi Arsitektur:**

Gangguan jaringan di lingkungan gedung perkuliahan dapat menyebabkan mahasiswa mengalami kegagalan ketika melakukan presensi meskipun hadir secara langsung di kelas. Kondisi ini juga dapat menghambat proses pembukaan sesi dan pencatatan kehadiran. Oleh karena itu, arsitektur perlu mendukung mekanisme penyimpanan sementara, sinkronisasi data, dan pencegahan pencatatan ganda.

**Kriteria Keberhasilan:**

- Sistem menyediakan mekanisme penanganan ketika koneksi internet terputus.
- Data yang belum berhasil dikirim dapat disimpan sementara pada perangkat pengguna.
- Sinkronisasi dilakukan secara otomatis setelah koneksi kembali tersedia.
- Proses sinkronisasi tidak menyebabkan duplikasi data presensi melalui penerapan idempotent API endpoints.

## 7. Hubungan ASR dengan Karakteristik Pendorong

Ketiga ASR memiliki hubungan dengan karakteristik pendorong yang telah ditentukan pada bagian B1 dan B2.

| ASR | Kebutuhan Utama | Karakteristik yang Berkaitan |
|---|---|---|
| ASR 1 | Menangani 3.000 permintaan bersamaan dengan respons kurang dari 2 detik. | Performance |
| ASR 2 | Menjaga integritas data dan mencegah kecurangan presensi. | Security |
| ASR 3 | Menjaga keberlangsungan pencatatan presensi ketika jaringan tidak stabil. | Performance dan Security |

Selain ketiga karakteristik tersebut, Maintainability mendukung keseluruhan kebutuhan arsitektur melalui pembagian modul yang terstruktur sehingga proses pengembangan, pengujian, dan pemeliharaan dapat dilakukan secara lebih mudah.

## 8. Implikasi terhadap Perancangan Arsitektur

Berdasarkan konteks dan kebutuhan signifikan yang telah diidentifikasi, terdapat beberapa implikasi terhadap rancangan arsitektur layanan presensi kuliah.

1. Sistem perlu menggunakan pembagian lapisan atau modul yang jelas untuk memisahkan tampilan, koordinasi proses, aturan bisnis, dan pengelolaan data.
2. Penggunaan Redis Cache dan RabbitMQ dipertimbangkan untuk membantu mengatasi lonjakan permintaan presensi pada jam sibuk.
3. Mekanisme autentikasi dan Role-Based Access Control (RBAC) diperlukan untuk memastikan setiap pengguna hanya dapat mengakses fitur sesuai kewenangannya.
4. Validasi QR Code dinamis dan koordinat GPS diperlukan untuk meningkatkan keamanan serta mengurangi risiko kecurangan presensi.
5. Penyimpanan sementara dan mekanisme sinkronisasi diperlukan untuk menangani kondisi jaringan yang tidak stabil.
6. Integrasi dengan basis data utama SIAKAD UMI perlu dilakukan melalui lapisan adapter agar pengelolaan layanan presensi tidak mengganggu sistem yang sudah berjalan.

Implikasi tersebut menjadi dasar bagi pembahasan skenario atribut kualitas, pemilihan taktik arsitektur, pola arsitektur, desain dan diagram, evaluasi, serta dokumentasi keputusan arsitektur pada bagian U2 sampai U7.
