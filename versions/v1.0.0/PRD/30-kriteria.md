> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# PRD 30 — Kriteria Keberhasilan

## Cerita Pengguna dan Acceptance

- US-001 Sebagai juru masak keluarga saya menerima 5 rekomendasi sehingga bisa memilih masakan hari ini. Acceptance: maksimal 5 item, tiap item memuat skor, alasan 2 kalimat, bahan terskala, dan belanja tambahan; diversifikasi maksimal 2 sekelompok ditegakkan.
- US-002 Sebagai pengelola profil saya menyunting alergi sehingga rekomendasi berbahaya hilang. Acceptance: resep beralergen tidak pernah tampil; perubahan profil menghitung ulang dalam 1 detik di KMP dan 1 respons API di web.
- US-003 Sebagai juru masak saya membuka detail per jiwa sehingga takaran benar. Acceptance: `jiwa` 1 sampai 8 diterima, di luar itu kode `JIWA_DI_LUAR_RENTANG`; bahan memakai faktor dan pembulatan BRD porsi.
- US-004 Sebagai pengguna web saya memakai dasbor pendamping sehingga bisa mencetak dan berbagi. Acceptance: tautan bagi membuka halaman publik tanpa login; tombol buka di aplikasi memicu deep link; port TypeScript selisih skor di bawah 0,01 pada 32 kasus.
- US-005 Sebagai admin konten saya memicu evaluasi bank sehingga mutu terjaga. Acceptance: 12/12 lolos grade-2 tercatat di `eval/metrics.md`; rilis diblokir bila gagal.
- US-006 Sebagai keluarga beta saya menandai dimasak sehingga serapan tercatat. Acceptance: tanda ganda hari dan resep yang sama dihitung sekali; gram memakai takaran terskala; buka detail di bawah 300 ms dan rekomendasi di bawah 800 ms.

## Non-Goals v1.0.0

- Tanpa perluasan bank ke resep ke-13; tanpa pencatatan stok inventori baru; tanpa perhitungan harga di klien web; tanpa eksekusi migrasi `pawonee-web` (hanya rencana terdokumentasi).

## Batasan

Batasan dokumen ini: hanya kriteria produk yang dapat diuji. Rincian teknis API dan skema ada di FSD. Klaim sukses tanpa skor mingguan serapan, rasio, dan waste dinyatakan tidak berlaku.
