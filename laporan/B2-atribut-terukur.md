# B2. Atribut Kualitas yang Terukur

<!-- Penanggung jawab: [Putri Aulia Salsabila] -->

Pada bagian ini dirumuskan spesifikasi atribut kualitas terukur untuk 3 karakteristik pendorong utama (*drivers*) yang telah dipilih pada Bagian B1, yaitu **Performance (Kinerja)**, **Security (Keamanan)**, dan **Maintainability (Kemudahan Perawatan)**. Setiap atribut dilengkapi dengan angka target, kondisi pengukuran, dan cara mengujinya.

---

## 1. Atribut Kualitas Kinerja (Performance)
* **Kategori / Sifat:** Operasional / Eksplisit
* **Angka Target:** 
  * 95% permintaan absensi mahasiswa selesai diproses dan menampilkan status konfirmasi di aplikasi dalam waktu **< 2,0 detik**.
  * Server mampu menangani beban puncak hingga **3.000 permintaan absensi bersamaan (*concurrent requests*)** tanpa adanya *error* atau kegagalan koneksi (*0% failure rate*).
* **Kondisi Pengukuran:** Diukur pada jam beban puncak perkuliahan (misalnya pukul 08.00 WITA saat pergantian jadwal jam kuliah pertama) dengan kondisi seluruh sesi kelas dibuka serentak.
* **Cara Menguji:** Menggunakan alat pengujian beban (*load testing*) seperti *Apache JMeter* atau *Locust* untuk mensimulasikan 3.000 pengguna aktif yang mengirimkan data presensi secara bersamaan ke server *staging*.

---

## 2. Atribut Kualitas Keamanan (Security)
* **Kategori / Sifat:** Lintas Sektor / Implisit
* **Angka Target:** 
  * Masa berlaku token QR Code dinamis adalah **maksimal 5 detik** sebelum secara otomatis diperbarui di layar proyektor/perangkat dosen.
  * Akurasi verifikasi lokasi geofencing berada dalam radius **maksimal 15 meter** dari titik tengah area gedung perkuliahan UMI, dan sistem menolak **100% upaya manipulasi lokasi (*fake GPS*)** atau penggunaan tangkapan layar (*screenshot*) QR code lama.
* **Kondisi Pengukuran:** Diuji pada berbagai perangkat seluler (Android dan iOS) di dalam maupun di luar area gedung perkuliahan dengan memanfaatkan aplikasi *mock location* serta pemindaian foto QR code yang sudah *expired*.
* **Cara Menguji:** Melakukan *Penetration Testing* (Pen-testing) pada API presensi, pengujian pemindaian QR code yang di-capture sebelumnya, serta pengujian pemalsuan koordinat GPS menggunakan perangkat lunak simulator lokasi.

---

## 3. Atribut Kualitas Kemudahan Perawatan (Maintainability)
* **Kategori / Sifat:** Struktural / Implisit
* **Angka Target:** 
  * Perubahan atau pembaruan aturan bisnis presensi (misalnya perubahan batas waktu keterlambatan atau aturan persentase minimal kehadiran) dapat dilakukan hanya dengan memperbarui **maksimal 1 modul kode** tanpa menyebabkan perubahan atau kaskade *bug* pada modul SIAKAD lainnya.
  * Waktu yang dibutuhkan bagi pengembang baru untuk memahami dan melakukan modifikasi kecil pada fungsi presensi adalah **< 4 jam kerja**.
* **Kondisi Pengukuran:** Diukur saat dilakukan simulasi pembaruan aturan akademik kampus pada lingkungan pengembangan (*development environment*).
* **Cara Menguji:** Menjalankan *Unit Test* dan *Integration Test* dengan cakupan kode (*code coverage*) minimal 80%, serta melakukan *code review* dan analisis kompleksitas kode (*cyclomatic complexity*) menggunakan alat seperti *SonarQube*.

---

## 4. Tabel Pemetaan Ringkasan Atribut Kualitas

| Karakteristik Pendorong | Angka Target | Kondisi Pengukuran | Cara Menguji |
| :--- | :--- | :--- | :--- |
| **Performance** | 95% respons < 2 detik pada 3.000 *concurrent requests*. | Jam puncak perkuliahan (pukul 08.00 WITA). | *Load testing* menggunakan JMeter / Locust. |
| **Security** | Masa aktif QR 5 detik; 100% penolakan *fake GPS* & QR lama. | Pengujian di dalam dan luar radius 15 meter kelas. | *Penetration testing* & *mock location simulation*. |
| **Maintainability** | Perubahan aturan presensi hanya menyentuh ≤ 1 modul; *code coverage* ≥ 80%. | Simulasi perubahan logika bisnis di lingkungan *dev*. | *Static code analysis* (SonarQube) & *unit testing*. |

