# A4. Kebutuhan Signifikan (ASR)

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

Berikut adalah 3 Kebutuhan Signifikan Secara Arsitektural (*Architecturally Significant Requirements* / ASR) pada Layanan Presensi Kuliah:

### 1. ASR 1: Kinerja dan Transaksi Bersamaan (Performance & Concurrency)
* **Persyaratan:** Saat jam pergantian perkuliahan puncak (misal pukul 08.00 WITA), sistem harus mampu menangani hingga 3.000 permintaan absensi masuk secara bersamaan (*concurrent requests*) dengan waktu tanggap (*response time*) halaman kurang dari 2 detik.
* **Alasan Memengaruhi Arsitektur:** Lonjakan akses serentak ini menuntut perancangan arsitektur yang tidak membebani basis data secara langsung. Oleh karena itu, sistem memerlukan komponen antrean pesan (*message queue*) dan *in-memory cache* untuk mengelola lalu lintas data yang padat agar server tidak mengalami *overhead* atau *crash*.

### 2. ASR 2: Integritas Data dan Keamanan Anti-Kecurangan (Data Integrity & Anti-Fraud)
* **Persyaratan:** Sistem harus memiliki mekanisme keamanan yang ketat untuk mencegah praktik kecurangan seperti pemalsuan lokasi (*fake GPS*) atau penitipan absen menggunakan tangkapan layar (*screenshot*) QR code milik teman.
* **Alasan Memengaruhi Arsitektur:** Persyaratan keamanan ini mengharuskan arsitektur mendukung modul validasi ganda, yaitu kombinasi pemindaian QR code dinamis (yang diperbarui otomatis setiap beberapa detik di layar kelas) serta verifikasi kesesuaian koordinat GPS perangkat mahasiswa dengan lokasi radius kelas.

### 3. ASR 3: Ketahanan terhadap Jaringan Tidak Stabil (Resilience / Low-Connectivity Support)
* **Persyaratan:** Jika koneksi internet di dalam gedung perkuliahan mengalami gangguan atau tidak stabil, data absensi mahasiswa yang dikirimkan dari perangkat bergerak harus dapat disimpan sementara di penyimpanan lokal (*local storage/SQLite*) pada perangkat klien, lalu disinkronkan secara otomatis begitu jaringan pulih.
* **Alasan Memengaruhi Arsitektur:** Kebutuhan ini mendikte perancangan arsitektur berbasis *client-side caching* serta *idempotent API endpoints* pada sisi server untuk memastikan tidak terjadinya duplikasi data saat proses sinkronisasi ulang dilakukan.
