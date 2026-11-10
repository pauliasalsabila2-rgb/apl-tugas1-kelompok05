# U2. Skenario Atribut Kualitas

## 1. Pendahuluan

Skenario atribut kualitas disusun berdasarkan tiga karakteristik pendorong utama (*architecture drivers*) yang telah ditetapkan pada U1, yaitu Performance (Kinerja), Security (Keamanan), dan Maintainability (Kemudahan Perawatan).

Ketiga karakteristik tersebut dipilih karena memiliki pengaruh langsung terhadap kualitas layanan presensi kuliah LMS KALAM UMI. Performance berfokus pada kemampuan sistem menangani permintaan presensi secara bersamaan, Security berfokus pada perlindungan data dan pencegahan kecurangan presensi, sedangkan Maintainability berfokus pada kemudahan pengembangan dan pemeliharaan sistem.

Setiap skenario dirumuskan menggunakan enam komponen, yaitu sumber stimulus, stimulus, lingkungan, artefak, respons, dan ukuran respons.

## 2. Skenario Atribut Kualitas

### 2.1 Skenario 1: Performance (Kinerja)

**Tujuan:** Memastikan layanan presensi mampu menangani lonjakan permintaan mahasiswa secara bersamaan tanpa mengalami perlambatan yang mengganggu proses presensi.

| Komponen | Deskripsi |
|---|---|
| Sumber Stimulus | Mahasiswa dan dosen yang mengakses layanan presensi LMS KALAM UMI. |
| Stimulus | Sebanyak 3.000 mahasiswa mengirimkan permintaan presensi secara bersamaan pada waktu pergantian perkuliahan. |
| Lingkungan | Kondisi beban puncak perkuliahan, khususnya sekitar pukul 08.00 WITA ketika beberapa sesi presensi dibuka secara bersamaan. |
| Artefak | Server aplikasi, layanan presensi, Redis Cache, RabbitMQ, dan basis data PostgreSQL SIAKAD UMI. |
| Respons | Sistem menerima dan memproses permintaan presensi secara efisien. Redis Cache membantu mengurangi beban akses berulang, sedangkan RabbitMQ mengelola antrean permintaan agar pemrosesan tidak membebani server dan basis data secara berlebihan. |
| Ukuran Respons | Sistem mampu menangani 3.000 permintaan bersamaan, dengan 95% permintaan presensi memperoleh respons kurang dari 2 detik dan tingkat kegagalan permintaan sebesar 0% pada pengujian beban. |

**Penjelasan:**

Skenario Performance berfokus pada permasalahan perlambatan akses yang terjadi ketika banyak mahasiswa melakukan presensi secara bersamaan. Kondisi tersebut membutuhkan arsitektur yang mampu mengelola lonjakan permintaan tanpa menyebabkan penurunan kinerja layanan. Penggunaan Redis Cache dan RabbitMQ menjadi alternatif solusi untuk membantu mengurangi beban server dan mengatur pemrosesan permintaan presensi. Pengujian dilakukan menggunakan *load testing* dengan Apache JMeter atau Locust untuk mensimulasikan 3.000 permintaan secara bersamaan.

### 2.2 Skenario 2: Security (Keamanan)

**Tujuan:** Menjamin keamanan dan integritas data kehadiran mahasiswa serta mencegah tindakan kecurangan dalam proses presensi.

| Komponen | Deskripsi |
|---|---|
| Sumber Stimulus | Mahasiswa yang melakukan presensi, dosen yang membuka sesi, dan pengguna yang mencoba melakukan manipulasi presensi. |
| Stimulus | Mahasiswa mencoba melakukan presensi menggunakan tangkapan layar QR Code milik mahasiswa lain, QR Code yang sudah kedaluwarsa, atau koordinat lokasi yang dimanipulasi menggunakan aplikasi fake GPS. |
| Lingkungan | Proses presensi berlangsung ketika dosen telah membuka sesi perkuliahan dan mahasiswa mengakses layanan melalui perangkat bergerak di dalam maupun di luar area gedung perkuliahan UMI. |
| Artefak | Modul autentikasi, Role-Based Access Control (RBAC), QR Code dinamis, validasi GPS dan geofencing, API presensi, serta basis data kehadiran. |
| Respons | Sistem memverifikasi identitas dan hak akses pengguna, memeriksa masa berlaku QR Code, serta mencocokkan koordinat GPS dengan lokasi perkuliahan. Permintaan yang terindikasi menggunakan QR Code kedaluwarsa atau lokasi palsu ditolak sebelum data kehadiran disimpan. |
| Ukuran Respons | QR Code diperbarui secara otomatis dengan masa berlaku maksimal 5 detik. Validasi lokasi menggunakan radius maksimal 15 meter dari titik tengah area gedung perkuliahan UMI. Sistem harus menolak 100% upaya penggunaan QR Code kedaluwarsa dan manipulasi lokasi yang diuji. Hak akses koreksi presensi hanya diberikan kepada pengguna yang memiliki kewenangan sesuai perannya. |

**Penjelasan:**

Skenario Security bertujuan menjaga keabsahan data kehadiran mahasiswa agar tidak terjadi manipulasi atau penyalahgunaan layanan presensi. Penggunaan QR Code dinamis dan validasi lokasi melalui GPS menjadi mekanisme pengamanan untuk mengurangi risiko penitipan absen dan penggunaan lokasi palsu. Selain itu, penerapan Role-Based Access Control (RBAC) memastikan setiap pengguna hanya dapat mengakses fitur sesuai dengan kewenangannya. Pengujian dilakukan melalui *penetration testing*, simulasi pemindaian QR Code yang telah kedaluwarsa, dan pengujian koordinat GPS menggunakan aplikasi *mock location*.

### 2.3 Skenario 3: Maintainability (Kemudahan Perawatan)

**Tujuan:** Memudahkan tim pengembang dalam melakukan perubahan aturan bisnis, perbaikan kesalahan, dan pemeliharaan layanan presensi tanpa mengganggu subsistem akademik lainnya.

| Komponen | Deskripsi |
|---|---|
| Sumber Stimulus | Tim pengembang dan pengelola teknologi informasi PDTI UMI. |
| Stimulus | Terdapat perubahan aturan akademik, seperti penyesuaian batas waktu keterlambatan presensi atau persentase minimal kehadiran mahasiswa, sehingga diperlukan perubahan pada fungsi layanan presensi. |
| Lingkungan | Lingkungan pengembangan (*development environment*) ketika tim pengembang melakukan pembaruan aturan bisnis dan pengujian sebelum perubahan diterapkan pada sistem operasional. |
| Artefak | Modul layanan presensi, modul pengelolaan sesi, modul validasi kehadiran, API, dan lapisan integrasi dengan basis data SIAKAD UMI. |
| Respons | Sistem memiliki struktur modul yang terpisah sehingga perubahan aturan bisnis dapat dilakukan pada modul terkait tanpa mengubah keseluruhan sistem atau mengganggu fungsi subsistem akademik lainnya. Setiap perubahan diuji melalui unit testing dan integration testing sebelum diterapkan. |
| Ukuran Respons | Perubahan aturan bisnis presensi hanya memerlukan pembaruan maksimal 1 modul kode. Pengembang baru dapat memahami dan melakukan modifikasi kecil pada fungsi presensi dalam waktu kurang dari 4 jam kerja. Pengujian dilakukan dengan target code coverage minimal 80% untuk fungsi yang diuji. |

**Penjelasan:**

Skenario Maintainability berfokus pada kemudahan pemeliharaan dan pengembangan layanan presensi dalam jangka panjang. Pembagian sistem ke dalam modul yang terstruktur memungkinkan tim pengembang PDTI UMI melakukan perubahan aturan bisnis tanpa harus mengubah keseluruhan sistem. Pengujian unit dan integrasi diperlukan untuk memastikan perubahan tidak menimbulkan kesalahan pada fungsi yang sudah berjalan. Pengukuran dilakukan melalui simulasi perubahan aturan presensi, pemeriksaan struktur kode, *code review*, dan analisis menggunakan SonarQube.

## 3. Ringkasan Skenario Atribut Kualitas

| No. | Karakteristik Pendorong | Fokus Skenario | Ukuran Respons Utama |
|---|---|---|---|
| 1 | Performance | Penanganan lonjakan permintaan presensi secara bersamaan. | 3.000 permintaan bersamaan, 95% respons kurang dari 2 detik, dan 0% tingkat kegagalan. |
| 2 | Security | Perlindungan data kehadiran dan pencegahan kecurangan presensi. | QR Code maksimal 5 detik, radius lokasi maksimal 15 meter, dan penolakan 100% upaya manipulasi yang diuji. |
| 3 | Maintainability | Kemudahan perubahan dan pemeliharaan modul layanan presensi. | Perubahan maksimal 1 modul, waktu modifikasi kurang dari 4 jam kerja, dan code coverage minimal 80%. |

## 4. Kesimpulan

Berdasarkan ketiga skenario atribut kualitas yang telah dirumuskan, Performance, Security, dan Maintainability menjadi aspek utama yang perlu diperhatikan dalam perancangan arsitektur layanan presensi kuliah LMS KALAM UMI.

Skenario Performance menjadi prioritas karena secara langsung menjawab permasalahan perlambatan akses ketika mahasiswa melakukan presensi secara bersamaan. Skenario Security diperlukan untuk menjaga integritas data kehadiran dan mencegah kecurangan akademik, sedangkan Maintainability mendukung kemudahan pengembangan serta pemeliharaan sistem oleh tim pengelola teknologi informasi kampus.

Ketiga skenario tersebut menjadi dasar dalam menentukan taktik arsitektur pada U3, pemilihan pola arsitektur pada U4, serta penyusunan desain dan evaluasi arsitektur pada bagian selanjutnya.
