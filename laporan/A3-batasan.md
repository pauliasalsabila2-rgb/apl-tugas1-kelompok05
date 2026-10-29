# A3. Batasan Sistem

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

Berikut adalah batasan-batasan yang ditetapkan pada perancangan arsitektur layanan presensi kuliah (*Attendance Management*):

## 1. Batasan Teknologi (Technology Constraints)
* **Penggunaan Infrastruktur Basis Data Kampus yang Ada:** Sistem harus terintegrasi dengan basis data akademik terpusat yang sudah berjalan di universitas dan tidak diizinkan menggunakan sistem basis data eksternal yang terpisah.
  * **Dampak pada Arsitektur:** Perangkat lunak harus dirancang menggunakan lapisan akses data (*data access layer*) yang kompatibel dengan skema SQL universitas, serta menerapkan pembatasan koneksi (*connection pooling*) untuk menghindari kelebihan beban pada server basis data pusat.
* **Standar Akses Protokol Keamanan Web (HTTPS):** Seluruh komunikasi data antara perangkat pengguna dan server wajib menggunakan protokol HTTPS terenkripsi.
  * **Dampak pada Arsitektur:** Arsitektur jaringan harus menyertakan sertifikat SSL/TLS yang valid serta komponen pengarah (*reverse proxy*) untuk menangani enkripsi dan dekripsi lalu lintas data secara aman.

## 2. Batasan Bisnis (Business Constraints)
* **Kepatuhan pada Kalender Akademik dan Tenggat Waktu:** Layanan presensi dan rekapitulasi kehadiran harus siap digunakan penuh sebelum perkuliahan semester berjalan dimulai dan harus ditutup sesuai jadwal evaluasi Biro Akademik.
  * **Dampak pada Arsitektur:** Prioritas pengembangan difokuskan pada modul inti presensi (pembukaan sesi dan absen mahasiswa) terlebih dahulu, sehingga fitur pendukung non-esensial dapat dikerjakan pada fase berikutnya.
* **Keterbatasan Anggaran dan Sumber Daya Pengembangan:** Proyek pengembangan dikerjakan oleh tim internal universitas dengan sumber daya komputasi awan (*cloud*) kampus yang terbatas.
  * **Dampak pada Arsitektur:** Desain sistem harus efisien, menghindari penggunaan layanan berbayar pihak ketiga yang mahal, dan memaksimalkan pemanfaatan sumber daya server internal yang sudah ada.

## 3. Batasan Operasional (Operational Constraints)
* **Tim Pemeliharaan yang Terbatas:** Operasional harian dan pemeliharaan sistem dikelola oleh tim IT (PDTI) yang memiliki jumlah personel terbatas dengan jam dukungan teknis reguler.
  * **Dampak pada Arsitektur:** Sistem harus memiliki struktur modul yang bersih dan mudah dipelihara (*maintainability*), serta dilengkapi sistem pencatatan (*logging*) otomatis untuk memudahkan deteksi dini gangguan tanpa intervensi manual yang rumit.
* **Keterbatasan Bandwidth Jaringan Kampus pada Jam Puncak:** Saat pergantian jam kuliah, ribuan mahasiswa mengakses jaringan secara bersamaan, yang berpotensi menyebabkan hambatan akses lokal.
  * **Dampak pada Arsitektur:** Perancangan arsitektur harus menerapkan strategi pengelolaan beban, seperti penggunaan mekanisme *cache* ringan pada data jadwal perkuliahan untuk mengurangi frekuensi kueri langsung ke basis data utama.

