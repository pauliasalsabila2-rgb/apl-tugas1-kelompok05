U7. Architectural Decision Records (ADR)
ADR-001: Penggunaan RabbitMQ untuk Pengiriman Pesan Asinkron
Status: Diterima (Accepted)

Context (Konteks):
Layanan Presensi Kuliah LMS KALAM UMI mengalami lonjakan trafik ekstrem (beban puncak hingga 3.000 permintaan bersamaan) pada menit-menit awal jam perkuliahan (misal pukul 08.00 WITA). Akses langsung yang bersifat sinkron ke basis data utama PostgreSQL SIAKAD UMI menyebabkan bottleneck, peningkatan response time, hingga potensi down pada sistem.

Decision (Keputusan):
Kami memutuskan untuk menerapkan pemrosesan asinkron menggunakan Message Queue dengan teknologi RabbitMQ. Setiap transaksi presensi yang valid dari API Gateway akan diubah menjadi event dan dimasukkan ke dalam antrean RabbitMQ, kemudian diproses secara bertahap oleh Background Worker Group sebelum ditulis ke basis data PostgreSQL.

Consequences (Dampak/Konsekuensi):

Positif:

Beban akses ke basis data relasional PostgreSQL berkurang secara drastis pada jam sibuk.

Waktu tanggap (response time) bagi pengguna menjadi sangat cepat (< 2 detik) karena pengguna tidak perlu menunggu proses database write selesai.   
U1-Konteks-dan-ASR.md
+ 1

Sistem memiliki tingkat ketahanan (fault tolerance) yang lebih baik apabila basis data mengalami perlambatan sementara.

Negatif (Harga yang Dibayar):

Meningkatkan kompleksitas infrastruktur dan pemeliharaan karena adanya komponen middleware baru.   
U4-Pola.md

Menerapkan Eventual Consistency, sehingga ada jeda waktu singkat (beberapa milidetik hingga detik) sebelum data presensi benar-benar tercatat di basis data utama SIAKAD UMI.

ADR-002: Implementasi Caching dengan Redis untuk Data Sesi Presensi
Status: Diterima (Accepted)

Context (Konteks):
Setiap kali mahasiswa melakukan pemindaian QR Code presensi, sistem harus memverifikasi status aktif sesi perkuliahan, validitas QR Code dinamis (yang diperbarui tiap 5 detik), dan data jadwal perkuliahan. Jika verifikasi ini selalu membaca basis data utama PostgreSQL, maka basis data akan menerima ribuan kueri read berulang dalam waktu bersamaan.

Decision (Keputusan):
Kami memutuskan untuk mengimplementasikan In-Memory Caching menggunakan Redis. Data sesi perkuliahan yang sedang aktif, token/payload QR Code dinamis, dan parameter geofencing lokasi akan disimpan sementara di dalam Redis Cache.

Consequences (Dampak/Konsekuensi):

Positif:

Proses pembacaan data sesi dan validasi QR Code menjadi jauh lebih cepat (latensi sangat rendah, < 10 ms).   
U3-Taktik.md

Mencegah query flooding ke basis data PostgreSQL untuk data yang bersifat sementara (ephemeral).   
U1-Konteks-dan-ASR.md
+ 1

Mendukung validasi QR Code dinamis 5 detik secara efisien dengan memanfaatkan fitur Time-To-Live (TTL) bawaan Redis.   
U2-Skenario-atribut-kualitas.md

Negatif (Harga yang Dibayar):

Memerlukan mekanisme invalidsi cache (cache invalidation) yang tepat ketika dosen menutup atau mengubah sesi perkuliahan secara manual.

Menambah kebutuhan kapasitas RAM pada infrastruktur server untuk menampung in-memory data.
