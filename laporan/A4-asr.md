# A4. Kebutuhan Signifikan (ASR)

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

Berikut adalah tiga Kebutuhan Signifikan Secara Arsitektural (*Architecturally Significant Requirements* - ASR) untuk layanan presensi kuliah (*Attendance Management*):

## 1. ASR Atribut Kualitas (Kinerja & Lonjakan Beban Puncak)
* **Pernyataan ASR:** "Saat jam pergantian perkuliahan dimulai, ribuan mahasiswa melakukan absensi secara bersamaan (*concurrent access*), dan halaman/proses pencatatan absensi harus merespons dalam waktu kurang dari 3 detik."
* **Pengaruh pada Arsitektur:** Kebutuhan ini sangat memengaruhi desain arsitektur karena menuntut penerapan mekanisme penanganan beban tinggi (*load balancing*), penggunaan *caching* data sesi aktif, serta optimalisasi kueri basis data agar server tidak mengalami *bottleneck* atau penumpukan antrean koneksi.

## 2. ASR Fungsional (Keandalan dan Validasi Absensi)
* **Pernyataan ASR:** "Sistem harus memastikan bahwa mahasiswa hanya dapat melakukan absensi pada sesi kuliah yang aktif dan sesuai dengan jadwal akademik riil yang divalidasi oleh dosen pengampu."
* **Pengaruh pada Arsitektur:** Kebutuhan ini mengharuskan perancangan lapisan logika bisnis (*business logic layer*) yang ketat untuk melakukan validasi waktu nyata (*real-time validation*) terhadap status sesi kelas, serta mencegah manipulasi data absensi dari perangkat pengguna.

## 3. ASR Batasan (Keamanan dan Integritas Data Kehadiran)
* **Pernyataan ASR:** "Seluruh pencatatan riwayat kehadiran mahasiswa wajib dilindungi dari modifikasi ilegal dan mematuhi aturan privasi universitas tanpa mengekspos data pribadi sensitif."
* **Pengaruh pada Arsitektur:** Kebutuhan ini mendikte penerapan kontrol akses berbasis peran (*Role-Based Access Control* - RBAC) yang ketat pada tingkat API, serta pencatatan audit *log* otomatis untuk setiap perubahan atau koreksi presensi yang dilakukan oleh dosen maupun admin akademik.

