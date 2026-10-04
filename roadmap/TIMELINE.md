> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# TIMELINE Pawonee — Garis Waktu Hidup Lintas Versi

Dokumen living: diperbarui tiap ada versi baru. Salinan beku v1.0.0 ada di `versions/v1.0.0/SNAPSHOT-ROADMAP.md` dan tidak diubah lagi.

## Garis Waktu

- 26 Sep 2026 sampai 09 Okt 2026 — Persiapan bank. Kurasi 12 resep grade-2, kunci standar porsi 4 jiwa, kunci model keluarga, bangun `RekomendasiEngine` dan `PorsiScaler` di `:shared:pantry-resep` milik Pawonee. Sumber isi lama: `resep/10-bank-resep-grade2.md` dan `preferensi/10-model-keluarga.md`.
- 10 Okt 2026 — v1.0.0 disetujui. Struktur versi BRD, PRD, FRD, FSD dibekukan mengikuti template emas Lumbung. Kunci: bank 12 resep, DB `pawonee`, JWT audiens `pawonee`, pemilik `pantry-resep` adalah Pawonee, sinyal `sinyal.pedaree.v1` tiap 15 menit.
- Hari 1 sampai 30 beta — Operasi kecil. 20 keluarga, rekomendasi luring, stok dibaca dari Pedaree, serapan ditandai manual. Target: buka rekomendasi 70 persen.
- Hari 31 sampai 60 beta — Skala penuh. 100 keluarga, web pendamping tayang, pipa sinyal berjalan, dasbor serapan mingguan terbit.
- Hari 61 sampai 90 beta — Kesiapan lepas beta. Audit konten 12 resep, uji paritas 32 kasus, crash-free 99,5 persen. Syarat lulus: serapan rata-rata minimal 300 g per keluarga per minggu dan 80 persen rekomendasi dibuka.
- Setelah beta — Skala dan versi berikutnya. Rencana migrasi `pawonee-web` Next.js 15 ke SvelteKit dan Bun terbaru dieksekusi pada fase coding. Rencana rinci menunjuk `ROADMAP.md` untuk v1.1.0 dan v2.0.0.

## Keterkaitan Versi

- v1.0.0 menjadi acuan awal. Perubahan jadwal pada versi baru dicatat di sini dengan tanggal dan nomor versi, tanpa mengubah snapshot beku.

## Batasan

Batasan dokumen ini: hanya mencatat tonggak waktu dan fase. Detail kebutuhan tetap di `versions/v1.0.0/BRD/`, detail kriteria lulus di `versions/v1.0.0/PRD/30-kriteria.md`, dan detail janji beku di `SNAPSHOT-ROADMAP.md`. Dokumen ini tidak mengatur tarif, grade, atau kontrak API.
