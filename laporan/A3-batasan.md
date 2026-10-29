# A3. Batasan Sistem

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->


Berikut adalah batasan teknologi, bisnis, dan operasional pada Layanan Presensi Kuliah beserta dampaknya terhadap arsitektur perangkat lunak:

## 1. Batasan Teknologi
* **Batasan 1:** Sistem wajib menggunakan basis data utama SIAKAD UMI berbasis *PostgreSQL* yang sudah ada di lingkungan kampus.
  * **Dampak Arsitektur:** Arsitektur harus menyediakan lapisan repositori (*adapter*) khusus untuk berkomunikasi dengan basis data *legacy* tanpa mengubah struktur atau skema tabel utama secara langsung.
* **Batasan 2:** Akses layanan dilakukan melalui aplikasi perangkat bergerak (*mobile app*) atau web pengguna dengan memanfaatkan validasi koordinat GPS/geofencing area kampus UMI.
  * **Dampak Arsitektur:** Membutuhkan komponen *Service Validator* khusus di sisi server untuk memverifikasi batas toleransi radius lokasi sebelum data presensi disimpan ke dalam sistem.

## 2. Batasan Bisnis
* **Batasan 1:** Batas waktu pengembangan dan implementasi layanan presensi adalah 2 bulan sebelum semester baru dimulai.
  * **Dampak Arsitektur:** Memakai arsitektur berbasis modul yang terdekopel (*decoupled modules*) agar komponen fungsional dapat dibangun secara paralel oleh tim pengembang.
* **Batasan 2:** Anggaran infrastruktur terbatas, sehingga kelompok tidak disarankan menambah server fisik baru khusus untuk modul presensi.
  * **Dampak Arsitektur:** Arsitektur memanfaatkan *caching layer* (seperti Redis) untuk mengoptimalkan kinerja dan mengurangi beban kueri langsung ke basis data utama saat jam sibuk.

## 3. Batasan Operasional
* **Batasan 1:** Tim IT kampus hanya tersedia untuk penanganan teknis langsung pada jam kerja operasional kantor (pukul 08.00–16.00 WITA).
  * **Dampak Arsitektur:** Membutuhkan mekanisme pencatatan *log error* otomatis serta rancangan pemulihan mandiri (*graceful degradation*) jika terjadi gangguan server di luar jam kerja.
* **Batasan 2:** Kebijakan koreksi presensi wajib membutuhkan persetujuan (*approval*) resmi dari Dosen Pengampu maksimal 7 hari setelah jadwal perkuliahan berlangsung.
  * **Dampak Arsitektur:** Membutuhkan pengelolaan alur kerja status presensi (*state machine*, misal: *Pending*, *Approved*, *Rejected*) di dalam modul backend.

