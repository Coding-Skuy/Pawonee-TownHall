> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FRD 10 — Kebutuhan Fungsional

Dokumen ini menyatakan apa yang wajib dilakukan sistem, tanpa menyatakan cara implementasi. Sumber: `produk/10-alur-rekomendasi.md`, `produk/20-kontrak-api-KMP-mobile.md`, `produk/21-kontrak-api-web-bun.md`, dan `produk/30-modul-KMP-bersama.md`.

## Resep

- FR-001 Sistem wajib menyimpan bank 12 resep grade-2 berisi id, nama, kelompok, waktu menit, kisaran biaya, bahan per grade, langkah, alergen, dan alat wajib.
- FR-002 Sistem wajib menerapkan substitusi baku bila bahan hilang sesuai tabel BRD resep.
- FR-003 Sistem wajib menskala bahan ke 1 sampai 8 jiwa memakai faktor dan pembulatan BRD porsi, dan menolak jiwa di luar rentang dengan kode `JIWA_DI_LUAR_RENTANG`.

## Preferensi

- FR-101 Sistem wajib menyimpan profil keluarga berisi jiwa, komposisi, alergi, pantangan, budget, pedas, alat, dan sumber belanja.
- FR-102 Sistem wajib menyaring mutlak resep beralergen, pelanggar pantangan mutlak, dan butuh alat yang tidak dimiliki sebelum memberi skor.
- FR-103 Sistem wajib menghitung ulang rekomendasi tersimpan setiap profil berubah.

## Rekomendasi

- FR-201 Sistem wajib menilai 12 resep lewat lima tahap (filter, serapan 50, kemudahan 50, ranking dan diversifikasi, penjelasan dan belanja) dan mengembalikan maksimal 5 item.
- FR-202 Sistem wajib mengembalikan mode hemat 5 resep termurah bervariabel `modeTanpaStok: true` bila stok kosong.
- FR-203 Sistem wajib mencatat `riwayat_masak` (resep, jiwa, gram terserap, tanggal) setiap tanda dimasak dan mengabaikan tanda ganda hari dan resep yang sama.
- FR-204 Sistem wajib menerbitkan agregat sinyal `sinyal.pedaree.v1` tiap 15 menit untuk dikonsumsi Pedaree.

## Lintas Segmen

- FR-301 Sistem wajib menegakkan hak peran: keluarga hanya datanya sendiri, admin konten hanya bank dan agregat.
- FR-302 Sistem wajib mencatat setiap aksi rekomendasi dan profil dengan pembuat, waktu, dan versi kontrak `v1`.
- FR-303 Sistem wajib memakai DB `pawonee` dan JWT audiens `pawonee` pada semua endpoint berjarak jauh.

## Batasan

Batasan dokumen ini: hanya kebutuhan fungsional. Bahasa pemrograman, pustaka, basis data lokal, dan pola navigasi tidak diatur di sini dan hanya boleh muncul di FSD. Setiap kebutuhan di atas wajib punya uji penerimaan di PRD/30-kriteria.md.
