# A2. Stakeholder dan Peta Power/Interest

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->
## 1. Stakeholder Register

Berdasarkan hasil wawancara dengan 5 responden (3 Mahasiswa dari berbagai angkatan, 1 Dosen Pengampu, dan 1 Staf Biro Akademik) serta keterlibatan tim teknis, berikut adalah *Stakeholder Register* untuk Layanan Presensi Kuliah:

| Stakeholder | Kelompok | Kebutuhan Utama | Ekspektasi Kualitas | Prioritas |
| :--- | :--- | :--- | :--- | :--- |
| **Mahasiswa (Maba, Sem 3, Sem 5)** | Pengguna Akhir | Melakukan presensi dengan cepat di kelas, melihat status konfirmasi kehadiran secara *real-time*, antarmuka yang sederhana, serta adanya toleransi/sinkronisasi saat koneksi internet kelas melambat. | *Usability* tinggi, *responsiveness* (< 2 detik), *offline resilience* (penyimpanan lokal sementara), dan kejelasan navigasi UI. | Tinggi |
| **Dosen Pengampu** | Pengguna Akhir | Membuka/menutup sesi perkuliahan tanpa *loading* lama, memantau kehadiran mahasiswa di kelas, serta menyetujui/menolak (*approval*) permohonan koreksi presensi. | *Reliability* tinggi, antarmuka sederhana (*ease of use*), dan alur persetujuan koreksi presensi yang efisien. | Tinggi |
| **Staf Biro Akademik** | Manajer Bisnis | Menerima rekapitulasi data kehadiran yang akurat, tidak dapat dimanipulasi, dan siap diekspor sebagai syarat kelayakan mahasiswa mengikuti ujian akhir semester. | *Accuracy*, *integrity*, *auditability*, dan kemudahan eksportasi laporan rekapitulasi. | Tinggi |
| **Tim Pengembang (PDTI UMI)** | Tim Pengembang | Membangun modul presensi yang terintegrasi rapi dengan basis data SIAKAD UMI dan mudah dirawat. | *Maintainability*, *code reusability*, dan kemudahan integrasi API dengan sistem *legacy*. | Sedang |
| **Tim Operasional Server** | Tim Operasional | Memastikan server tetap stabil dan tidak *down* saat terjadi lonjakan akses serentak di menit-menit awal jam perkuliahan. | *High Availability* (99.9%) dan *Performance/Scalability* yang baik pada jam puncak. | Tinggi |
| **Auditor Mutu Internal UMI** | Pihak Eksternal | Memastikan kesesuaian jumlah tatap muka perkuliahan dan keabsahan bukti kehadiran sesuai standar penjaminan mutu kampus. | *Auditability* dan keberadaan jejak audit (*audit trail*) yang aman. | Sedang |

---

## 2. Peta Power/Interest (Mendelow) dan Strategi

Posisi tiap stakeholder pada Peta *Power/Interest* digambarkan pada diagram `diagram/A2-peta-power-interest.png`. Berikut adalah pembagian kuadran dan strategi pengelolaannya:
![Diagram modul](../diagram/A2-peta-power-interest.pgn)
