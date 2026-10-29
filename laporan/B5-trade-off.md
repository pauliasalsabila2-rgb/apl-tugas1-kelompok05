# B5. Analisis Trade-off

<!-- Penanggung jawab: [Putri Aulia Salsabila] -->

Dalam perancangan arsitektur perangkat lunak, peningkatan pada satu karakteristik seringkali berdampak atau bertolak belakang dengan karakteristik lainnya. Pada bagian ini dianalisis minimal dua pasang karakteristik yang saling memengaruhi pada Layanan Presensi Kuliah, beserta keputusan prioritas dan alasannya.

## 1. Pasangan Trade-off 1: Keamanan (Security) vs. Kinerja (Performance)
* **Analisis Dampak:** 
  * Menambahkan lapisan keamanan berlapis—seperti enkripsi token QR dinamis yang diperbarui tiap 5 detik dan verifikasi koordinat GPS (*geofencing*) yang ketat pada setiap permintaan absensi—membutuhkan siklus komputasi tambahan di sisi server dan pemeriksaan validasi yang lebih panjang. Hal ini berpotensi meningkatkan waktu tanggap (*response time*) dan menurunkan efisiensi kinerja saat diakses ribuan pengguna bersamaan.
* **Keputusan Prioritas & Alasan:** 
  * **Keamanan didahulukan** untuk proses verifikasi identitas dan validasi lokasi awal demi mencegah kecurangan akademik (titip absen/fake GPS). Namun, untuk mengatasi penurunan kinerja, sistem mengombinasikannya dengan *in-memory cache* (Redis) dan antrean pesan (*message queue*) sehingga waktu tunggu di sisi mahasiswa tetap cepat (< 2 detik) tanpa mengurangi tingkat validitas keamanan data.

## 2. Pasangan Trade-off 2: Kinerja (Performance) vs. Kemudahan Perawatan (Maintainability)
* **Analisis Dampak:** 
  * Untuk mencapai kinerja tinggi dan menangani lonjakan trafik secara instan, pengembang sering kali tergoda melakukan optimasi kode tingkat rendah (*low-level optimization*), *hardcoding* konfigurasi tertentu, atau membuat struktur basis data yang terdenormalisasi. Hal ini justru membuat kode menjadi sulit dipahami, kompleks, dan menurunkan tingkat kemudahan perawatan (*maintainability*) serta fleksibilitas pembaruan kode ke depannya.
* **Keputusan Prioritas & Alasan:** 
  * **Kemudahan perawatan dan struktur modular tetap diutamakan** melalui penerapan pembagian lapisan arsitektur yang bersih (*clean architecture*). Kinerja tinggi tidak dicapai dengan merusak struktur kode, melainkan dengan memanfaatkan komponen infrastruktur eksternal seperti *Message Queue* (RabbitMQ) dan *Caching Layer* (Redis) yang terdekopel dari kode inti bisnis presensi.

---

## Ringkasan Keputusan Prioritas Arsitektur
| Pasangan Karakteristik | Karakteristik yang Didahulukan | Alasan Utama |
| :--- | :--- | :--- |
| **Keamanan vs. Kinerja** | Keamanan (dengan mitigasi *Caching* & *Queue*) | Integritas data absensi dan pencegahan kecurangan lebih krusial, diimbangi infrastruktur asinkron untuk menjaga kecepatan respons. |
| **Kinerja vs. Maintainability** | Maintainability & Pemisahan Modul | Stabilitas jangka panjang dan kemudahan perawatan kode oleh tim IT kampus lebih diutamakan; performa diatasi lewat infrastruktur pendukung, bukan kode yang rumit. |

