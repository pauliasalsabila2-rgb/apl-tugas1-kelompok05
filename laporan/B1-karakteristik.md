# B1. Identifikasi dan Klasifikasi Karakteristik

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

Pada bagian ini diidentifikasi kandidat karakteristik arsitektur untuk Layanan Presensi Kuliah, diklasifikasikan ke dalam kategori dan sifatnya, serta dipilih tiga karakteristik pendorong (*driver*) utama berdasarkan tiga ciri karakteristik arsitektur.

## 1. Daftar Kandidat Karakteristik Arsitektur
Berikut adalah 8 kandidat karakteristik arsitektur yang diidentifikasi memengaruhi sistem presensi:

1. **Performance (Kinerja):** Kecepatan sistem dalam memproses transaksi absensi mahasiswa.
2. **Security (Keamanan):** Perlindungan data kehadiran dan pencegahan manipulasi lokasi/identitas (*anti-fraud*).
3. **Availability (Ketersediaan):** Jaminan sistem dapat diakses terus-menerus tanpa gangguan.
4. **Scalability (Skalabilitas):** Kemampuan sistem menangani lonjakan jumlah pengguna saat jam masuk kuliah.
5. **Maintainability (Kemudahan Perawatan):** Kemudahan bagi tim pengembang dalam memperbarui kode atau memperbaiki modul.
6. **Agility (Kelincahan):** Kecepatan merespons perubahan aturan bisnis akademik (misal perubahan toleransi waktu terlambat).
7. **Testability (Kemudahan Pengujian):** Kemudahan menguji fungsi modul presensi dan integrasinya dengan basis data.
8. **Fault Tolerance (Toleransi Kesalahan):** Kemampuan sistem tetap berfungsi atau pulih cepat saat terjadi kendala jaringan/server.

---

## 2. Klasifikasi Kandidat Karakteristik
Berikut adalah tabel klasifikasi kandidat karakteristik berdasarkan kategori (operasional, struktural, lintas sektor) serta sifatnya (implisit atau eksplisit):

| Kandidat Karakteristik | Kategori | Sifat | Alasan Pengelompokan |
| :--- | :--- | :--- | :--- |
| **Performance** | Operasional | Eksplisit | Sistem dituntut merespon cepat (< 2 detik) saat ribuan mahasiswa absen bersamaan. |
| **Security** | Lintas Sektor | Implisit | Melibatkan pengamanan data privasi, enkripsi token QR, dan pencegahan *fake GPS* di seluruh lapisan sistem. |
| **Availability** | Operasional | Eksplisit | Layanan harus aktif sedia penuh terutama pada jam-jam aktif perkuliahan kampus. |
| **Scalability** | Operasional | Implisit | Arsitektur harus mampu berkembang secara elastis menghadapi lonjakan trafik harian. |
| **Maintainability** | Struktural | Implisit | Memastikan struktur kode modular agar mudah dipelihara oleh tim pengembang PDTI UMI. |
| **Agility** | Struktural | Implisit | Diperlukan agar aturan bisnis akademik yang dinamis dapat diubah tanpa merombak sistem secara keseluruhan. |
| **Testability** | Struktural | Implisit | Memastikan setiap modul fungsi absensi dan integrasi API mudah diuji kualitas kodenya. |
| **Fault Tolerance** | Lintas Sektor | Implisit | Menjaga ketahanan sistem terhadap gangguan koneksi internet lokal atau kegagalan server parsial. |

---

## 3. Pemilihan Tiga Karakteristik Pendorong Utama (*Driver*)
Dengan menggunakan alat uji tiga ciri karakteristik arsitektur (menentukan struktur, memengaruhi kualitas operasional/struktural, serta krusial bagi pemangku kepentingan), dipilih **3 karakteristik pendorong utama** untuk Layanan Presensi Kuliah:

1. **Performance (Kinerja) - Kategori Operasional:**
   * *Alasan:* Menjadi pendorong utama karena sistem harus mampu melayani ribuan mahasiswa yang melakukan absensi secara serentak pada menit awal perkuliahan tanpa mengalami kelambatan.
2. **Security (Keamanan) - Kategori Lintas Sektor:**
   * *Alasan:* Sangat krusial untuk mencegah kecurangan akademik (titip absen atau *fake GPS*) serta menjaga integritas data kehadiran mahasiswa yang akan diaudit oleh penjaminan mutu.
3. **Maintainability (Kemudahan Perawatan) - Kategori Struktural:**
   * *Alasan:* Penting bagi tim pengembang kampus (PDTI) agar pembaruan aturan bisnis presensi dapat dilakukan secara modular tanpa mengganggu subsistem akademik lainnya di SIAKAD.

