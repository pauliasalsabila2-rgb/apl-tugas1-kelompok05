# A1. Analisis Empat Konteks Arsitektur

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

## Tabel Analisis Empat Konteks Arsitektur

| Konteks Arsitektur | Kondisi Subsistem / Layanan Presensi Kuliah |
| :--- | :--- |
| **Teknis** | Layanan presensi terintegrasi dengan portal web/aplikasi kampus (KALAM/SPADA) dan basis data akademik pusat, yang dirancang untuk menangani lonjakan akses bersamaan (*concurrent access*) dari ribuan mahasiswa pada menit-menit awal perkuliahan. |
| **Siklus Hidup Proyek** | Pengembangan, pemeliharaan, dan evaluasi layanan dikelola oleh tim IT/PDTI universitas, mencakup pengujian beban (*load testing*) sebelum awal semester serta pembaruan berkala berdasarkan umpan balik pengguna (mahasiswa dan staf akademik). |
| **Bisnis** | Layanan ini berfungsi menyediakan rekapitulasi kehadiran yang transparan dan akurat sebagai komponen utama penentu kepatuhan mutlak terhadap aturan akademik universitas, khususnya syarat minimal kehadiran untuk mengikuti Ujian Akhir Semester (UAS). |
| **Profesional** | Pengoperasian sistem berpedoman pada standar kualitas mutu perangkat lunak (seperti ISO/IEC 25010) untuk menjamin integritas data, mencegah duplikasi atau kehilangan data absensi, serta menjaga privasi data pengguna. |
