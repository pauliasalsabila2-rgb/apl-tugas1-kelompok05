
# U6. EVALUASI

## 1. Pendahuluan

Evaluasi arsitektur dilakukan untuk mengetahui apakah rancangan Layanan Presensi Kuliah LMS KALAM UMI telah memenuhi kebutuhan atribut kualitas yang telah ditentukan. Evaluasi ini menggunakan tiga metode, yaitu pohon utilitas mini (*Mini Utility Tree*), analisis *trade-off* berbobot terhadap dua alternatif arsitektur, dan analisis menggunakan pendekatan *Architecture Tradeoff Analysis Method* (ATAM).

Fokus utama evaluasi adalah mengatasi masalah perlambatan layanan ketika banyak mahasiswa melakukan presensi secara bersamaan, khususnya pada awal pergantian jam perkuliahan. Selain itu, evaluasi juga memperhatikan aspek keamanan dan kemudahan pemeliharaan sistem.

## 2. Pohon Utilitas Mini (Mini Utility Tree)

Pohon utilitas mini digunakan untuk menggambarkan kebutuhan atribut kualitas berdasarkan tiga karakteristik pendorong utama, yaitu *Performance*, *Security*, dan *Maintainability*. Setiap atribut dijelaskan melalui skenario dan ukuran keberhasilan yang dapat dievaluasi.

| Atribut Kualitas | Skenario | Ukuran Keberhasilan | Prioritas |
|---|---|---|---|
| Performance | Sistem mampu melayani hingga 3.000 permintaan presensi mahasiswa secara bersamaan pada jam sibuk. | 95% permintaan mendapatkan respons awal kurang dari 2 detik dan tingkat kegagalan 0% pada pengujian yang ditetapkan. | Tinggi |
| Security | Sistem menolak presensi menggunakan QR Code kedaluwarsa atau lokasi GPS di luar area perkuliahan. | QR Code berlaku maksimal 5 detik, radius geofencing maksimal 15 meter, dan seluruh percobaan manipulasi yang diuji berhasil ditolak. | Tinggi |
| Maintainability | Pengembang dapat melakukan perubahan aturan presensi tanpa mengubah banyak bagian sistem. | Perubahan aturan dapat dilakukan maksimal pada 1 modul, dipahami atau diselesaikan pengembang baru kurang dari 4 jam, dan cakupan pengujian minimal 80%. | Sedang |

Berdasarkan pohon utilitas tersebut, *Performance* menjadi prioritas utama karena berkaitan langsung dengan masalah perlambatan layanan presensi. *Security* dan *Maintainability* juga menjadi kebutuhan penting agar sistem tetap aman dan mudah dikembangkan.

## 3. Analisis Trade-off Berbobot

Analisis *trade-off* dilakukan dengan membandingkan dua alternatif arsitektur untuk menentukan rancangan yang paling sesuai dengan kebutuhan layanan Presensi Kuliah LMS KALAM UMI.

### 3.1 Alternatif Arsitektur

**Alternatif A: Layered Architecture dengan Pemrosesan Sinkron**

Alternatif ini menggunakan empat lapisan, yaitu Presentation, Application, Domain, dan Data Layer. Setiap permintaan presensi diproses secara langsung oleh server hingga data tersimpan ke PostgreSQL SIAKAD UMI.

**Alternatif B: Layered Architecture dengan Event-Driven Architecture**

Alternatif ini menggabungkan Layered Architecture dan Event-Driven Architecture. RabbitMQ digunakan untuk mengelola antrean permintaan presensi, Background Worker untuk memproses data secara asinkron, dan Redis untuk menyimpan data sesi presensi, token QR Code dinamis, serta parameter geofencing. Mekanisme penyimpanan lokal, retry, dan idempotency juga digunakan untuk mendukung kondisi jaringan yang tidak stabil.

### 3.2 Kriteria dan Bobot Penilaian

Penilaian dilakukan menggunakan skala 1–5 dengan keterangan:

- 1 = Sangat kurang
- 2 = Kurang
- 3 = Cukup
- 4 = Baik
- 5 = Sangat baik

Kriteria utama mengikuti tiga karakteristik pendorong pada U1. Resilience ditambahkan sebagai kriteria pendukung karena rancangan U3 dan U5 juga menerapkan penyimpanan lokal, retry, dan idempotency.

| Kriteria | Bobot |
|---|---:|
| Performance | 35% |
| Security | 30% |
| Maintainability | 20% |
| Resilience (pendukung) | 15% |
| **Total** | **100%** |

### 3.3 Hasil Perbandingan Alternatif

| Kriteria | Bobot | Alternatif A | Nilai A | Alternatif B | Nilai B |
|---|---:|---:|---:|---:|---:|
| Performance | 35% | 2 | 0,70 | 5 | 1,75 |
| Security | 30% | 4 | 1,20 | 4 | 1,20 |
| Maintainability | 20% | 4 | 0,80 | 4 | 0,80 |
| Resilience | 15% | 2 | 0,30 | 4 | 0,60 |
| **Total** | **100%** | | **3,00** | | **4,35** |

Keterangan: Nilai akhir diperoleh dari hasil perkalian bobot dengan skor setiap kriteria, kemudian dijumlahkan. Skor merupakan penilaian analitis kelompok terhadap rancangan, bukan hasil pengujian sistem secara langsung.

### 3.4 Hasil Evaluasi Trade-off

Berdasarkan hasil perhitungan, Alternatif A memperoleh nilai 3,00, sedangkan Alternatif B memperoleh nilai 4,35.

Alternatif A memiliki kelebihan berupa struktur sistem yang lebih sederhana dan proses yang mudah dipahami. Namun, pemrosesan secara sinkron berpotensi menyebabkan penumpukan permintaan ketika ribuan mahasiswa melakukan presensi secara bersamaan.

Alternatif B memperoleh nilai lebih tinggi karena penggunaan RabbitMQ membantu mengatur antrean permintaan, Background Worker memproses data secara bertahap, dan Redis mempercepat pembacaan data sesi serta informasi validasi. Penggunaan penyimpanan lokal dan mekanisme retry juga membantu menghadapi jaringan yang tidak stabil.

Dengan demikian, **Alternatif B dipilih** karena lebih sesuai untuk memenuhi kebutuhan Performance dan Resilience tanpa menghilangkan keunggulan pemisahan lapisan pada Layered Architecture.

## 4. Analisis ATAM (Architecture Tradeoff Analysis Method)

Analisis ATAM digunakan untuk mengidentifikasi risiko, non-risiko, *sensitivity point*, dan *trade-off point* yang muncul dari rancangan arsitektur terpilih.

### 4.1 Risiko (Risk)

**Risiko 1: Penumpukan Antrean RabbitMQ**

Ketika jumlah permintaan presensi yang masuk lebih besar daripada kemampuan Background Worker dalam memproses pesan, antrean RabbitMQ dapat menumpuk. Kondisi ini menyebabkan keterlambatan penyimpanan data dan konfirmasi akhir kepada mahasiswa.

Mitigasi:

- Menyesuaikan jumlah Background Worker dengan beban permintaan.
- Memantau jumlah pesan yang menumpuk pada antrean.
- Menggunakan Dead Letter Queue untuk menangani pesan yang gagal diproses berulang kali.
- Melakukan pengujian beban menggunakan JMeter atau Locust.

**Risiko 2: Ketidakakuratan Validasi GPS dan Geofencing**

Koordinat GPS perangkat dapat mengalami ketidakakuratan akibat kondisi lingkungan gedung atau perangkat yang digunakan. Hal ini berpotensi menyebabkan mahasiswa yang berada di area perkuliahan ditolak oleh sistem.

Mitigasi:

- Melakukan pengujian akurasi GPS di lingkungan kampus.
- Menerapkan pencatatan log terhadap hasil validasi lokasi.
- Menyediakan prosedur pengajuan koreksi presensi melalui persetujuan dosen.
- Menguji mekanisme penolakan terhadap simulasi manipulasi lokasi.

### 4.2 Non-Risiko (Non-Risk)

**Pemisahan Lapisan pada Layered Architecture**

Penerapan empat lapisan, yaitu Presentation, Application, Domain, dan Data Layer, merupakan keputusan yang mendukung keteraturan struktur sistem. Setiap lapisan memiliki tanggung jawab yang jelas sehingga perubahan pada satu bagian tidak langsung memengaruhi seluruh sistem.

Selama batas tanggung jawab dan antarmuka setiap lapisan diterapkan secara konsisten, struktur ini tidak menjadi risiko utama bagi rancangan. Sebaliknya, pemisahan tersebut mendukung kemudahan pemeliharaan dan pengujian sistem.

### 4.3 Sensitivity Point

**Masa Berlaku QR Code Dinamis Maksimal 5 Detik**

Masa berlaku QR Code merupakan titik sensitivitas karena perubahan nilainya dapat memengaruhi keamanan dan kemudahan penggunaan sistem.

Apabila masa berlaku QR Code diperpanjang, risiko penggunaan ulang tangkapan layar QR Code dapat meningkat. Sebaliknya, apabila masa berlaku terlalu singkat, mahasiswa dapat mengalami kesulitan melakukan pemindaian akibat keterlambatan jaringan atau proses pemindaian.

Oleh karena itu, masa berlaku QR Code ditetapkan maksimal 5 detik agar keseimbangan antara keamanan dan kemudahan penggunaan tetap terjaga.

### 4.4 Trade-off Point

**Penggunaan RabbitMQ untuk Pemrosesan Asinkron**

Penggunaan RabbitMQ memberikan keuntungan dalam mengatur lonjakan permintaan presensi karena data dapat diproses secara bertahap oleh Background Worker. Hal ini membantu mengurangi beban pemrosesan langsung pada PostgreSQL.

Namun, terdapat konsekuensi berupa keterlambatan konfirmasi akhir karena proses penyimpanan dilakukan secara asinkron. Mahasiswa dapat menerima status awal `202 Accepted` sebelum data benar-benar tersimpan di basis data.

Dengan demikian, sistem perlu membedakan status permintaan yang sedang diproses dan status presensi yang sudah berhasil disimpan. Keputusan ini merupakan pertukaran antara peningkatan kemampuan menangani beban sistem dan kecepatan konfirmasi akhir kepada pengguna.

## 5. Evaluasi Penyelesaian Masalah Utama

Masalah utama pada layanan Presensi Kuliah LMS KALAM UMI adalah perlambatan sistem ketika banyak mahasiswa melakukan presensi secara bersamaan, terutama pada awal pergantian jam perkuliahan.

Berdasarkan hasil evaluasi, rancangan Alternatif B menggunakan kombinasi Layered Architecture dan Event-Driven Architecture untuk mengatasi permasalahan tersebut.

Beberapa mekanisme yang diterapkan adalah:

1. **RabbitMQ** digunakan untuk menampung dan mengatur antrean permintaan presensi agar tidak seluruhnya diproses secara bersamaan ke basis data.
2. **Background Worker** digunakan untuk memproses permintaan presensi secara bertahap.
3. **Redis Cache** digunakan untuk mempercepat pembacaan data sesi, token QR Code dinamis, dan parameter geofencing sehingga mengurangi kueri berulang ke PostgreSQL.
4. **SQLite atau Local Storage** digunakan untuk menyimpan data presensi sementara ketika jaringan terputus, kemudian mengirimkannya kembali saat koneksi pulih.

Berdasarkan mekanisme tersebut, rancangan arsitektur sudah memberikan solusi terhadap penyebab utama perlambatan layanan pada tingkat desain. Namun, masalah belum dapat dinyatakan sepenuhnya teratasi karena rancangan masih harus dibuktikan melalui implementasi dan pengujian beban.

Pengujian perlu memastikan bahwa sistem mampu menangani 3.000 permintaan secara bersamaan, mencapai waktu respons awal kurang dari 2 detik untuk 95% permintaan, serta tidak mengalami kegagalan pada skenario pengujian yang ditetapkan.

## 6. Kesimpulan

Berdasarkan hasil evaluasi, Alternatif B yang menggabungkan Layered Architecture dan Event-Driven Architecture memperoleh nilai tertinggi sebesar 4,35 dibandingkan Alternatif A sebesar 3,00.

Hasil analisis ATAM menunjukkan dua risiko utama, yaitu penumpukan antrean RabbitMQ dan ketidakakuratan validasi GPS. Selain itu, terdapat satu non-risiko berupa pemisahan lapisan arsitektur, satu *sensitivity point* berupa masa berlaku QR Code dinamis maksimal 5 detik, serta satu *trade-off point* berupa penggunaan pemrosesan asinkron yang dapat memperlambat konfirmasi akhir.

Secara keseluruhan, rancangan arsitektur yang dipilih telah memberikan solusi pada tingkat desain terhadap masalah perlambatan presensi ketika terjadi lonjakan akses. Meskipun demikian, keberhasilan penyelesaian masalah tetap harus dibuktikan melalui implementasi dan pengujian sesuai target atribut kualitas yang telah ditetapkan.

