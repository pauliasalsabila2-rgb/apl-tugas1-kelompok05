# U7. Architectural Decision Records (ADR)

## 1. Pendahuluan

Architectural Decision Records (ADR) digunakan untuk mencatat keputusan-keputusan arsitektur penting yang diambil selama perancangan layanan Presensi Kuliah LMS KALAM UMI. Setiap rekaman mencakup konteks permasalahan, keputusan yang ditetapkan, serta konsekuensi (dampak positif dan harga yang harus dibayar) dari keputusan tersebut.

---

## 2. ADR-001: Penggunaan RabbitMQ untuk Pengiriman Pesan Asinkron

* **Title:** Penggunaan RabbitMQ untuk Pengiriman Pesan Asinkron dan Pengelolaan Antrean Presensi
* **Status:** Diterima (*Accepted*)

### Context (Konteks)
Layanan Presensi Kuliah LMS KALAM UMI menghadapi masalah lonjakan akses secara bersamaan (*concurrent requests*) hingga 3.000 mahasiswa pada menit-menit awal jam pergantian perkuliahan. Apabila seluruh permintaan presensi diproses secara langsung dan sinkron ke basis data utama PostgreSQL SIAKAD UMI, basis data akan mengalami peningkatan beban secara signifikan. Hal ini berpotensi menyebabkan perlambatan waktu tanggap (*response time* > 2 detik) atau bahkan kegagalan transaksi presensi.

### Decision (Keputusan)
Kami memutuskan untuk menerapkan pola *Event-Driven Architecture* menggunakan **RabbitMQ** sebagai *Message Broker* untuk pengiriman pesan secara asinkron. 

Setiap permintaan presensi yang telah melewati validasi awal di API Gateway akan diterbitkan (*publish*) sebagai *event* presensi ke dalam antrean RabbitMQ (`Queue: Attendance_Events`). Selanjutnya, kelompok *Background Worker* (*Attendance Worker Group*) akan mengambil dan memproses pesan dari antrean secara bertahap untuk dituliskan ke basis data PostgreSQL SIAKAD UMI.

### Consequences (Konsekuensi)

* **Dampak Positif:**
  * Menjaga stabilitas server dan basis data utama dengan cara mendistribusikan beban penulisan data presensi secara bertahap.
  * Memberikan waktu tanggap yang cepat bagi pengguna (< 2 detik) karena konfirmasi penerimaan presensi awal diberikan tanpa harus menunggu proses penulisan basis data selesai.
  * Meningkatkan ketahanan sistem (*resilience*); jika basis data relasional mengalami perlambatan sementara, pesan presensi tetap aman tersimpan di dalam antrean.

* **Harga yang Harus Dibayar (Dampak Negatif & Risiko):**
  * Memerlukan pengelolaan infrastruktur baru (penambahan *instance* RabbitMQ dan manajemen *worker*).
  * Sistem menerapkan konsistensi bertahap (*eventual consistency*), sehingga terdapat jeda waktu singkat sebelum data benar-benar tercatat di basis data utama.
  * Membutuhkan mekanisme penanganan pesan gagal (*Dead Letter Queue*) dan idempotensi API agar tidak terjadi pencatatan data presensi ganda.

---

## 3. ADR-002: Implementasi Caching dengan Redis untuk Data Sesi Presensi

* **Title:** Implementasi In-Memory Caching menggunakan Redis untuk Data Sesi dan Validasi QR Code Dinamis
* **Status:** Diterima (*Accepted*)

### Context (Konteks)
Proses validasi presensi memerlukan verifikasi berulang terhadap data sesi perkuliahan yang sedang aktif, keabsahan QR Code dinamis (yang diperbarui setiap 5 detik untuk mencegah kecurangan), dan koordinat *geofencing* lokasi perkuliahan. Jika setiap transaksi pemindaian QR Code dari ribuan mahasiswa harus melakukan kueri pembacaan langsung (*read query*) ke basis data utama PostgreSQL SIAKAD UMI, maka akan terjadi penumpukan kueri (*query flooding*) yang memicu perlambatan sistem.

### Decision (Keputusan)
Kami memutuskan untuk mengimplementasikan *In-Memory Caching* menggunakan **Redis** untuk menyimpan data sesi presensi yang sedang berlangsung, token/payload QR Code dinamis, serta parameter batas lokasi perkuliahan.

API Gateway dan lapisan validasi akan membaca data sesi dan memeriksa keabsahan QR Code langsung dari Redis Cache sebelum memasukkan transaksi ke antrean pemrosesan.

### Consequences (Konsekuensi)

* **Dampak Positif:**
  * Mempercepat proses validasi pembacaan data sesi dan keabsahan QR Code dengan latensi sangat rendah (< 10 ms).
  * Mengurangi beban kueri pembacaan berulang ke basis data PostgreSQL secara signifikan pada jam sibuk perkuliahan.
  * Memudahkan pengelolaan masa berlaku QR Code dinamis 5 detik dengan memanfaatkan fitur *Time-To-Live* (TTL) bawaan dari Redis.

* **Harga yang Harus Dibayar (Dampak Negatif & Risiko):**
  * Memerlukan mekanisme pembatalan cache (*cache invalidation*) yang konsisten apabila dosen mengubah jadwal atau menutup sesi presensi secara mendadak.
  * Membutuhkan alokasi memori (RAM) server yang memadai untuk menampung data *in-memory* selama jam operasional perkuliahan.
