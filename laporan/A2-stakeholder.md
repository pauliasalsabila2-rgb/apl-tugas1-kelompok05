# A2. Stakeholder dan Peta Power/Interest

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

## 1. Register Stakeholder Layanan Presensi Kuliah
Berikut adalah daftar minimal 6 stakeholder yang terlibat dalam sistem layanan presensi kuliah (*Attendance Management*):

| No | Stakeholder | Kelompok | Kebutuhan Utama & Ekspektasi Kualitas | Power / Interest | Strategi |
| :--- | :--- | :--- | :--- | :--- | :--- |
| 1 | **Mahasiswa** | Pengguna akhir | Melakukan absen dengan cepat, mudah melalui perangkat seluler, serta melihat rekap kehadiran secara real-time. | Rendah / Tinggi | Keep informed (Berikan informasi) |
| 2 | **Dosen Pengampu** | Pengguna akhir | Membuka sesi presensi kuliah dengan mudah, melakukan koreksi presensi mahasiswa yang valid, dan mengunduh laporan kehadiran. | Sedang / Tinggi | Keep informed / Manage closely |
| 3 | **Biro Akademik** | Manajer bisnis | Memastikan data rekap kehadiran akurat dan terintegrasi penuh sebagai syarat administratif kelayakan ujian mahasiswa. | Tinggi / Tinggi | Manage closely (Kelola dengan erat) |
| 4 | **Tim IT / PDTI** | Tim operasional | Menjaga ketersediaan sistem, menangani lonjakan akses saat jam perkuliahan dimulai, dan memelihara infrastruktur server. | Tinggi / Tinggi | Manage closely (Kelola dengan erat) |
| 5 | **Bagian Keuangan / Unit Terkait** | Manajer bisnis / internal | Memanfaatkan data kehadiran untuk keperluan audit kedisiplinan atau evaluasi kegiatan akademik. | Sedang / Rendah | Keep satisfied (Puaskan) |
| 6 | **Auditor Mutu Kampus** | Pihak eksternal | Memastikan proses pencatatan absensi transparan, akurat, dan dapat diaudit sewaktu-waktu sesuai standar mutu institusi. | Tinggi / Rendah | Keep satisfied (Puaskan) |

## 2. Analisis Peta Power/Interest (Mendelow)
* **Manage Closely (Pengaruh Tinggi, Kepentingan Tinggi):** Biro Akademik dan Tim IT (PDTI). Kedua pihak ini memegang kendali atas kebijakan operasional dan infrastruktur teknis sehingga harus dilibatkan secara aktif dalam setiap keputusan arsitektur.
* **Keep Satisfied (Pengaruh Tinggi, Kepentingan Rendah):** Auditor Mutu Kampus. Mereka memiliki kewenangan mengevaluasi tetapi tidak berdampak langsung pada operasional harian, sehingga cukup dipastikan sistem memenuhi standar kepatuhan.
* **Keep Informed (Pengaruh Rendah, Kepentingan Tinggi):** Mahasiswa dan Dosen Pengampu. Mereka adalah pengguna utama yang paling merasakan dampak sistem, sehingga perlu rutin diberi informasi pembaruan dan dimintai umpan balik.
* **Monitor (Pengaruh Rendah, Kepentingan Rendah):** Pihak eksternal atau utilitas pendukung lain yang hanya memerlukan pemantauan berkala.
  [diagram modul](../diagram/A2-peta-power-interest.png)
