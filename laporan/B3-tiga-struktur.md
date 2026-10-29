# B3. Tiga Struktur Desain

<!-- Penanggung jawab: [Putri Aulia Salsabila] -->
# B3. Tiga Struktur Desain

Bagian ini menyajikan tiga struktur desain arsitektur untuk Layanan Presensi Kuliah (*Attendance Management*), yang terdiri dari struktur modul, struktur komponen dan konektor (C&C), serta struktur alokasi.

## 1. Struktur Modul
Struktur modul menunjukkan bagaimana basis kode program dibagi ke dalam unit-unit fungsional serta hubungan relasi "menggunakan" antar-modul.

![Struktur Modul](../diagram/B3-struktur-modul.png)

* **Keterangan:** Kode Layanan Presensi dibagi ke dalam empat lapisan (tampilan, koordinasi, aturan bisnis, dan data) untuk memastikan pembaruan logika bisnis hanya menyentuh modul spesifik tanpa merusak lapisan lain. Lapisan tampilan hanya memanggil modul pengelola absensi dan tidak mengakses data secara langsung.

## 2. Struktur Komponen dan Konektor (C&C)
Struktur C&C menunjukkan elemen-elemen perangkat lunak aktif yang berjalan saat sistem digunakan serta cara mereka berinteraksi secara waktu nyata.

![Struktur C&C](../diagram/B3-struktur-cc.png)

* **Keterangan:** Browser atau aplikasi mobile mengirimkan permintaan absensi ke Server Web Presensi, yang kemudian diteruskan ke antrean pesan (*message queue*) dan *cache*. *Background worker* memproses antrean secara bertahap ke basis data utama SIAKAD, membuat sistem lebih tahan terhadap lonjakan beban puncak (ASR 1).

## 3. Struktur Alokasi
Struktur alokasi menunjukkan penempatan komponen perangkat lunak pada lingkungan infrastruktur fisik/virtual serta pembagian penugasan modul kepada anggota tim pengembang.

![Struktur Alokasi](../diagram/B3-struktur-alokasi.png)

* **Keterangan:** Layanan presensi dijalankan pada server aplikasi di *cloud* kampus dan perangkat bergerak pengguna, sementara data tetap berada pada server basis data utama SIAKAD UMI. Selain itu, modul pekerjaan dibagi secara terstruktur ke dalam kelompok tugas agar pengembangan dapat berjalan secara paralel.

