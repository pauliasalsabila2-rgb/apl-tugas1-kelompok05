U7. Architectural Decision Records (ADR)
ADR-001: Penggunaan RabbitMQ untuk Pengiriman Pesan Asinkron
Status: Diterima (Accepted)

Context (Konteks):
Layanan Presensi Kuliah LMS KALAM UMI mengalami lonjakan trafik ekstrem (beban puncak hingga 3.000 permintaan secara bersamaan) pada menit-menit awal jam pergantian perkuliahan. Akses langsung yang bersifat pemrosesan sinkron ke basis data utama PostgreSQL SIAKAD UMI berpotensi menyebabkan hambatan pemrosesan (bottleneck), peningkatan waktu tanggap (response time), hingga penurunan ketersediaan layanan.

Decision (Keputusan):
Kami memutuskan untuk menerapkan pemrosesan asinkron menggunakan Message Queue dengan teknologi RabbitMQ. Setiap transaksi presensi yang terverifikasi di API Gateway diterbitkan sebagai event ke dalam antrean RabbitMQ, kemudian diproses secara bertahap oleh Background Worker Group sebelum dituliskan ke basis data relasional PostgreSQL.

Consequences (Dampak/Konsekuensi):

Positif:

Beban penulisan langsung ke basis data utama PostgreSQL berkurang secara signifikan saat jam sibuk perkuliahan.

Waktu tanggap (response time) bagi mahasiswa menjadi jauh lebih cepat (< 2 detik) karena pengguna tidak perlu menunggu proses penulisan basis data selesai.

Meningkatkan ketahanan sistem (fault tolerance) apabila basis data utama mengalami perlambatan pemrosesan sementara.

Negatif (Harga yang Dibayar):

Meningkatkan kompleksitas arsitektur infrastruktur dan perawatan karena penambahan komponen middleware.

Penerapan Eventual Consistency, sehingga terdapat jeda waktu singkat sebelum data presensi benar-benar tercatat secara permanen di basis data SIAKAD UMI.

ADR-002: Implementasi Caching dengan Redis untuk Data Sesi Presensi
Status: Diterima (Accepted)

Context (Konteks):
Setiap pemindaian QR Code presensi memerlukan verifikasi status aktif sesi perkuliahan, pengecekan keabsahan QR Code dinamis (yang diperbarui secara otomatis setiap 5 detik), serta validasi parameter geofencing lokasi. Apabila seluruh proses pemindaian melakukan kueri pembacaan langsung ke basis data PostgreSQL SIAKAD UMI, basis data akan mengalami lonjakan beban read query yang berlebihan.

Decision (Keputusan):
Kami memutuskan untuk mengimplementasikan In-Memory Caching menggunakan Redis. Data sesi perkuliahan aktif, token/payload QR Code dinamis, dan titik koordinat geofencing disimpan sementara pada Redis Cache.

Consequences (Dampak/Konsekuensi):

Positif:

Pemrosesan kueri pembacaan data sesi dan validasi QR Code dapat dilakukan secara instan dengan latensi sangat rendah (< 10 ms).

Mencegah penumpukan kueri (query flooding) ke basis data utama PostgreSQL untuk data yang bersifat sementara (ephemeral).

Mendukung mekanisme validasi QR Code dinamis 5 detik secara efisien dengan memanfaatkan fitur Time-To-Live (TTL) bawaan Redis.

Negatif (Harga yang Dibayar):

Memerlukan mekanisme pembatalan cache (cache invalidation) yang tepat ketika dosen melakukan penutupan atau perubahan sesi perkuliahan secara manual.

Menambah kebutuhan alokasi memori (RAM) pada infrastruktur server untuk menampung data in-memory.
