# A1. Analisis Empat Konteks Arsitektur

<!-- Penanggung jawab: [Annajhma Rasyid Sulaiman] -->

Berikut adalah analisis empat konteks arsitektur untuk layanan presensi kuliah pada subsistem *Attendance Management*:

| Konteks Arsitektur | Kondisi Subsistem / Penjelasan |
| :--- | :--- |
| **Teknis (Technical)** | Layanan presensi berbasis web dan mobile yang diakses secara bersamaan oleh ribuan mahasiswa menggunakan perangkat masing-masing melalui jaringan internet kampus, menuntut integrasi stabil dengan basis data akademik utama. |
| **Siklus Hidup Proyek (Project Lifecycle)** | Sistem dikembangkan dan dipelihara secara bertahap oleh tim pengembang internal PDTI UMI, dengan pembaruan fitur berkala disesuaikan dengan kalender akademik dan evaluasi semester. |
| **Bisnis (Business)** | Mendukung kebijakan operasional universitas dalam memvalidasi kehadiran mahasiswa secara akurat sebagai syarat administratif kelayakan mengikuti ujian semester. |
| **Profesional (Professional)** | Dikelola dengan standar pengembangan perangkat lunak yang membatasi hak akses berdasarkan peran (mahasiswa, dosen, dan admin) serta menjaga privasi data kehadiran dari pihak yang tidak berkepentingan. |
