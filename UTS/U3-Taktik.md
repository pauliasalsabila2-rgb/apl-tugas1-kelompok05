# U3. Taktik Arsitektur

## 1. Pendahuluan

Taktik arsitektur merupakan pendekatan yang digunakan untuk memenuhi atribut kualitas yang telah ditentukan dalam skenario U2. Pada layanan presensi kuliah LMS KALAM UMI, penerapan taktik difokuskan pada peningkatan kinerja sistem, penyampaian notifikasi kehadiran, serta konsistensi data presensi mahasiswa.

Setiap taktik dipilih berdasarkan kebutuhan pengguna dan permasalahan utama layanan, khususnya ketika banyak mahasiswa mengakses presensi secara bersamaan. Penerapan taktik juga mempertimbangkan konsekuensi atau harga yang harus dibayar (*trade-off*), baik dari sisi sumber daya, kompleksitas sistem, maupun waktu pemrosesan.

## 2. Taktik untuk Setiap Skenario

### 2.1 Skenario 1 – Kinerja (Performance)

**Taktik: Load Balancing dan Caching**

**Cara Kerja:**

1. Load balancing digunakan untuk mendistribusikan permintaan akses presensi dari mahasiswa ke beberapa server atau instance aplikasi sehingga beban tidak terpusat pada satu server.
2. Caching digunakan untuk menyimpan sementara data yang sering diakses, seperti informasi jadwal kuliah dan informasi sesi presensi yang sedang berlangsung.
3. Ketika banyak mahasiswa mengakses layanan presensi secara bersamaan, permintaan dapat dilayani secara lebih merata dan mengurangi beban pemrosesan basis data.
4. Sistem tetap melakukan validasi terhadap status sesi presensi sebelum menyimpan kehadiran mahasiswa agar data yang dicatat sesuai dengan sesi yang aktif.

**Harga yang Dibayar (Trade-off):**

- Membutuhkan sumber daya server tambahan untuk mendistribusikan beban permintaan.
- Konfigurasi dan pemeliharaan load balancer menambah kompleksitas infrastruktur.
- Data yang disimpan dalam cache berpotensi tidak langsung mencerminkan perubahan terbaru sehingga diperlukan mekanisme pembaruan dan invalidasi cache.

**Hasil yang Diharapkan:**

Akses layanan presensi menjadi lebih responsif ketika terjadi lonjakan pengguna, sehingga mahasiswa dapat melakukan presensi tanpa mengalami keterlambatan yang berlebihan.

### 2.2 Skenario 2 – Notifikasi Kehadiran (Notification Responsiveness)

**Taktik: Asynchronous Messaging dan Message Queue**

**Cara Kerja:**

1. Setelah mahasiswa berhasil melakukan presensi, sistem terlebih dahulu menyimpan data kehadiran ke dalam basis data.
2. Sistem kemudian mengirimkan informasi keberhasilan presensi ke dalam antrean pesan (*message queue*).
3. Layanan notifikasi mengambil pesan dari antrean dan memproses pengiriman pemberitahuan kepada pengguna.
4. Pemrosesan notifikasi dilakukan secara terpisah dari proses utama pencatatan presensi sehingga pengiriman pemberitahuan tidak menghambat mahasiswa ketika melakukan presensi.
5. Apabila pengiriman notifikasi mengalami kegagalan sementara, pesan dapat dicoba kembali sesuai kebijakan percobaan ulang (*retry mechanism*).

**Harga yang Dibayar (Trade-off):**

- Membutuhkan komponen tambahan berupa message broker atau message queue.
- Pengelolaan antrean pesan menambah kompleksitas sistem.
- Notifikasi berpotensi mengalami sedikit keterlambatan karena diproses secara asynchronous.
- Diperlukan mekanisme pemantauan antrean dan penanganan pesan gagal agar notifikasi tidak hilang atau terkirim berulang kali.

**Hasil yang Diharapkan:**

Mahasiswa memperoleh pemberitahuan setelah presensi berhasil dicatat, sedangkan proses pencatatan kehadiran tetap dapat berjalan tanpa harus menunggu pengiriman notifikasi selesai.

### 2.3 Skenario 3 – Konsistensi Data Presensi (Data Consistency)

**Taktik: Database Transaction dan Audit Logging**

**Cara Kerja:**

1. Setiap proses pencatatan presensi dilakukan melalui transaksi basis data agar perubahan data tersimpan secara utuh.
2. Sistem memvalidasi identitas mahasiswa, sesi perkuliahan, dan status presensi sebelum data disimpan.
3. Penerapan batasan unik (*unique constraint*) pada kombinasi identitas mahasiswa dan sesi perkuliahan digunakan untuk mencegah pencatatan presensi ganda.
4. Setiap perubahan data presensi, termasuk koreksi yang dilakukan oleh dosen, dicatat melalui audit logging.
5. Riwayat perubahan menyimpan informasi mengenai data sebelum dan sesudah perubahan, waktu perubahan, serta pihak yang melakukan perubahan.
6. Data presensi yang telah diperbarui menjadi dasar bagi penyajian riwayat kehadiran dan laporan presensi.

**Harga yang Dibayar (Trade-off):**

- Proses transaksi dan validasi tambahan dapat meningkatkan waktu pemrosesan.
- Audit logging membutuhkan ruang penyimpanan tambahan.
- Pengelolaan riwayat perubahan membutuhkan kebijakan penyimpanan dan pengamanan data.
- Validasi yang lebih ketat dapat menyebabkan transaksi gagal apabila terjadi konflik perubahan data secara bersamaan.

**Hasil yang Diharapkan:**

Data kehadiran mahasiswa tersimpan secara konsisten, mengurangi risiko pencatatan ganda, serta memastikan setiap koreksi presensi dapat ditelusuri melalui riwayat perubahan.

## 3. Ringkasan Penerapan Taktik

| Skenario | Taktik | Manfaat Utama | Harga yang Dibayar |
|---|---|---|---|
| Kinerja (Performance) | Load Balancing dan Caching | Mengurangi beban server dan mempercepat akses presensi | Penambahan sumber daya dan kompleksitas infrastruktur |
| Notifikasi Kehadiran | Asynchronous Messaging dan Message Queue | Memisahkan proses notifikasi dari pencatatan presensi | Potensi keterlambatan notifikasi dan kebutuhan pengelolaan antrean |
| Konsistensi Data Presensi | Database Transaction dan Audit Logging | Menjaga keakuratan data dan menyediakan riwayat perubahan | Tambahan waktu pemrosesan dan kebutuhan penyimpanan |

## 4. Kesimpulan

Berdasarkan ketiga skenario tersebut, penerapan taktik arsitektur diarahkan untuk mendukung layanan presensi kuliah yang responsif, informatif, dan memiliki data yang konsisten. Load balancing dan caching digunakan untuk mengatasi peningkatan beban akses, asynchronous messaging mendukung penyampaian notifikasi tanpa menghambat proses utama, sedangkan database transaction dan audit logging menjaga keakuratan serta keterlacakan data presensi.

Setiap taktik memiliki konsekuensi yang perlu dipertimbangkan dalam perancangan arsitektur. Pemilihan taktik dilakukan dengan mengutamakan kebutuhan utama layanan presensi, terutama kelancaran akses mahasiswa, keberhasilan pencatatan kehadiran, dan keakuratan riwayat presensi.
