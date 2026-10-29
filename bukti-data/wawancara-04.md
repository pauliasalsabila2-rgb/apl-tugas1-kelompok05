# Ringkasan Wawancara [04]

- **Tanggal:** 26-10-2026
- **Responden (peran saja):** MABA (R4)
- **Pewawancara:** Annajhma Rasyid Sulaiman
- **Layanan yang dibahas:** Layanan presensi kuliah (absen mahasiswa)

## Pertanyaan dan ringkasan jawaban
| Pertanyaan | Ringkasan jawaban |
|---|---|
| Bagaimana pengalaman Anda saat melakukan absensi pada jam sibuk perkuliahan? | Sering mengalami penundaan (*lagging*) pemuatan halaman saat mengakses portal di menit-menit awal jam kuliah karena ribuan mahasiswa membuka sistem bersamaan. |
| Apakah tampilan dan navigasi fitur presensi mudah digunakan dari ponsel? | Secara antarmuka cukup sederhana, tetapi akan sangat membantu jika ada indikator status presensi yang langsung diperbarui secara otomatis. |

## Temuan yang dipakai pada laporan
- A4: Lonjakan akses bersamaan (*concurrent access*) pada jam puncak membutuhkan perancangan arsitektur yang tahan beban dengan batas waktu respons cepat.
- B2: Atribut kinerja menetapkan kriteria keberhasilan 95% halaman dapat dimuat < 3 detik saat diakses 3.000 pengguna bersamaan.
