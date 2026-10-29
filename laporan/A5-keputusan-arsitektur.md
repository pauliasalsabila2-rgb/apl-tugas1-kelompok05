# A5. Keputusan Arsitektur dan ADR

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

Pada bagian ini dijelaskan keputusan arsitektur utama yang diambil untuk Layanan Presensi Kuliah, dokumentasi Architecture Decision Record (ADR), serta rencana komunikasi dan mitigasi *anti-pattern* keputusan arsitektur.

## 1. Ringkasan Keputusan Arsitektur (ADR)
Keputusan arsitektur utama pada layanan ini dicatat dalam bentuk dokumen terpisah di folder `/adr/`:
* **ADR-001:** Implementasi Redis Cache dan Message Queue (RabbitMQ) untuk mengatasi lonjakan akses presensi bersamaan pada jam sibuk perkuliahan.
* **ADR-002:** Penerapan kombinasi QR Code dinamis dan validasi koordinat GPS (*geofencing*) untuk mencegah kecurangan (titip absen/fake GPS).

## 2. Rencana Komunikasi ADR
* **Lokasi Penyimpanan:** Seluruh berkas ADR disimpan di dalam repositori GitHub kelompok pada folder `/adr/` dengan format Markdown (`ADR-001-caching-antrean.md` dan `ADR-002-qr-dinamis-geofencing.md`).
* **Mekanisme Pemberitahuan:** Setiap ada penambahan atau perubahan ADR, anggota tim diberi tahu melalui pesan singkat di grup koordinasi dengan melampirkan tautan langsung ke *commit* repositori, bukan mengirimkan isi dokumen penuh melalui email.

## 3. Strategi Menghindari Anti-Pattern Keputusan Arsitektur
Kelompok menghindari tiga *anti-pattern* utama dalam pengambilan keputusan arsitektur sebagai berikut:
1. **Menghindari Covering Your Assets (CYA):** Penulisan ADR dan keputusan didasarkan pada analisis teknis yang obyektif, untung-rugi yang riil, serta kebutuhan sistem, bukan sekadar mencari aman atau menyalahkan pihak lain.
2. **Menghindari Groundhog Day:** Keputusan yang telah berstatus *Accepted* dicatat dengan konteks yang jelas dan mengikat. Perdebatan berulang terhadap keputusan yang sama tidak diperkenankan kecuali terdapat perubahan batasan atau kondisi sistem yang signifikan.
3. **Menghindari Email-Driven Architecture:** Seluruh diskusi teknis, argumen, dan penetapan keputusan wajib didokumentasikan secara transparan di dalam repositori Git atau berkas ADR, dan tidak dibiarkan tersembunyi di dalam utas email (*email thread*) atau pesan pribadi.

