
# BAB I. PENDAHULUAN

## 1.1 Gambaran Layanan Inti

Layanan Presensi Kuliah merupakan salah satu bagian dari subsistem *Attendance Management* dalam Smart Campus UMI yang berfungsi untuk membantu proses pencatatan dan pengelolaan kehadiran mahasiswa selama kegiatan perkuliahan berlangsung. Layanan ini dirancang untuk memudahkan mahasiswa dalam melakukan presensi, dosen dalam mengelola sesi kehadiran, serta pihak akademik dalam memantau dan memperoleh rekapitulasi kehadiran mahasiswa.

Dalam pelaksanaannya, layanan presensi kuliah melibatkan beberapa proses utama, yaitu mahasiswa melakukan absensi sesuai jadwal perkuliahan, dosen membuka sesi presensi, mahasiswa mengajukan koreksi apabila terdapat kesalahan pencatatan kehadiran, serta sistem menyediakan laporan atau rekapitulasi presensi.

Layanan ini memiliki peran penting dalam mendukung administrasi akademik karena data kehadiran dapat digunakan untuk memantau tingkat partisipasi mahasiswa dalam perkuliahan. Oleh karena itu, sistem presensi perlu memperhatikan keakuratan data, keamanan informasi, kemudahan penggunaan, serta kemampuan menangani akses pengguna secara bersamaan.

## 1.2 Ruang Lingkup

Ruang lingkup analisis Layanan Presensi Kuliah difokuskan pada proses utama yang berkaitan dengan pencatatan dan pengelolaan kehadiran mahasiswa. Terdapat lima kasus penggunaan (*use case*) yang menjadi fokus pembahasan, yaitu:

1. **Pembukaan Sesi Presensi oleh Dosen**

   Dosen membuka sesi presensi sesuai jadwal perkuliahan agar mahasiswa dapat melakukan pencatatan kehadiran.

2. **Pengisian Presensi oleh Mahasiswa**

   Mahasiswa melakukan presensi melalui layanan yang tersedia ketika sesi presensi telah dibuka oleh dosen.

3. **Pengecekan Status Kehadiran**

   Mahasiswa dapat melihat status kehadirannya untuk memastikan bahwa presensi telah tercatat dengan benar.

4. **Pengajuan dan Persetujuan Koreksi Presensi**

   Mahasiswa dapat mengajukan koreksi apabila terjadi kesalahan pencatatan kehadiran. Dosen kemudian melakukan pemeriksaan dan memberikan persetujuan atau penolakan terhadap permohonan tersebut.

5. **Pembuatan Laporan Kehadiran**

   Sistem menyediakan rekapitulasi atau laporan kehadiran mahasiswa yang dapat digunakan oleh dosen dan pihak akademik untuk kebutuhan pemantauan serta administrasi perkuliahan.

Analisis ini dibatasi pada proses pencatatan, pengelolaan, koreksi, dan pelaporan kehadiran mahasiswa. Pembahasan tidak mencakup layanan akademik lain seperti pengisian KRS, pengelolaan nilai, maupun pembayaran administrasi perkuliahan.

## 1.3 Sumber Data

Sumber data dalam penyusunan laporan analisis arsitektur Layanan Presensi Kuliah diperoleh melalui wawancara dengan lima responden yang memiliki keterlibatan dan pengalaman berbeda dalam penggunaan maupun pengelolaan layanan presensi kuliah. Wawancara dilaksanakan pada tanggal 26–28 Oktober 2026 untuk memperoleh informasi mengenai kebutuhan pengguna, kendala sistem, serta harapan terhadap pengembangan layanan presensi kuliah.

Adapun sumber data yang digunakan adalah sebagai berikut:

1. **Wawancara Mahasiswa Semester 5 (R1)**

   Wawancara dilakukan pada tanggal 26 Oktober 2026 oleh Annajhma Rasyid Sulaiman. Pembahasan berfokus pada kemudahan akses halaman presensi di KALAM saat jam perkuliahan dimulai. Responden menyampaikan bahwa halaman presensi sering mengalami perlambatan pemuatan karena diakses oleh banyak mahasiswa secara bersamaan. Responden juga mengharapkan adanya notifikasi instan dan riwayat kehadiran yang dapat diperbarui secara langsung.

2. **Wawancara Staf Administrasi Biro Akademik (R2)**

   Wawancara dilakukan pada tanggal 26 Oktober 2026 oleh Annajhma Rasyid Sulaiman. Pembahasan berfokus pada laporan rekapitulasi kehadiran dan persyaratan mengikuti Ujian Akhir Semester (UAS). Responden menjelaskan bahwa data presensi sangat penting untuk menentukan kelayakan mahasiswa mengikuti ujian. Selain itu, integritas basis data diperlukan agar tidak terjadi duplikasi maupun kehilangan data kehadiran.

3. **Wawancara Mahasiswa Semester 3 (R3)**

   Wawancara dilakukan pada tanggal 26 Oktober 2026 oleh Annajhma Rasyid Sulaiman. Pembahasan berfokus pada pemeriksaan rekapitulasi kehadiran mahasiswa. Responden menyampaikan bahwa rekap kehadiran biasanya diperiksa pada pertengahan dan akhir semester untuk memastikan terpenuhinya batas minimal kehadiran sebesar 75%. Kendala yang ditemukan adalah adanya kemungkinan perbedaan antara status kehadiran yang dicatat di kelas dengan data yang ditampilkan pada sistem.

4. **Wawancara Mahasiswa Baru (R4)**

   Wawancara dilakukan pada tanggal 26 Oktober 2026 oleh Annajhma Rasyid Sulaiman. Pembahasan berfokus pada pengalaman mahasiswa baru ketika melakukan presensi pada jam sibuk perkuliahan. Responden menyampaikan bahwa halaman portal terkadang mengalami keterlambatan saat diakses oleh banyak mahasiswa secara bersamaan. Dari sisi penggunaan, tampilan dan navigasi presensi dinilai cukup sederhana, tetapi diperlukan indikator status presensi yang dapat diperbarui secara otomatis.

5. **Wawancara Dosen (R5)**

   Wawancara dilakukan pada tanggal 28 Oktober 2026 oleh Putri Aulia Salsabila. Pembahasan berfokus pada pembukaan sesi presensi oleh dosen dan proses koreksi kehadiran mahasiswa. Responden menjelaskan bahwa pembukaan sesi presensi terkadang terhambat akibat jaringan lokal yang tidak stabil. Apabila mahasiswa mengalami kendala jaringan saat melakukan presensi, dosen dapat menggunakan fitur koreksi presensi manual dengan memberikan catatan penjelasan.

Hasil wawancara dari kelima responden tersebut digunakan sebagai dasar dalam mengidentifikasi kebutuhan dan permasalahan pada Layanan Presensi Kuliah. Informasi yang diperoleh menjadi bahan pendukung dalam analisis konteks arsitektur, identifikasi stakeholder, penentuan batasan sistem, penyusunan kebutuhan signifikan secara arsitektural (*Architecturally Significant Requirements*), serta penentuan karakteristik kualitas perangkat lunak.

Dokumentasi hasil wawancara secara lengkap disertakan pada bagian lampiran bukti data dalam repositori GitHub kelompok.


