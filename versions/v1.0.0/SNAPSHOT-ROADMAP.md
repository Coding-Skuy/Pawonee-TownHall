> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# SNAPSHOT-ROADMAP v1.0.0 — Salinan Beku

Salinan beku janji v1.0.0 pada 10 Okt 2026. Tidak diubah lagi. Perubahan masa depan dicatat di `roadmap/` living dan dirilis sebagai versi baru.

## Janji Beku

- Bank inti: 12 resep grade-2 Indonesia, evaluasi 12/12 lolos tiap rilis.
- Modul `:shared:pantry-resep` milik Pawonee versi 1.0.0; uji 32 kasus hijau di KMP dan web.
- Rekomendasi: maksimal 5 item, luring 800 ms, serapan mingguan minimal 1.500 g, rasio minimal 80 persen, waste maksimal 300 g.
- Data: DB `pawonee` Postgres; JWT akses 15 menit dan refresh 7 hari audiens `pawonee`.
- Sinyal: topik `sinyal.pedaree.v1` terbit tiap 15 menit untuk Pedaree.
- Beta: 20 keluarga hari 1 sampai 30, 100 keluarga hari 31 sampai 60, lepas beta hari 61 sampai 90. Syarat lulus: serapan rata-rata minimal 300 g per keluarga per minggu, 80 persen rekomendasi dibuka, crash-free 99,5 persen.
- Web: `pawonee-web` tercatat Next.js 15 sebagai drift; target SvelteKit dan Bun terbaru adalah rencana migrasi pada fase coding, bukan eksekusi v1.0.0.

## Sumber

- Dibekukan dari isi lama `resep/`, `preferensi/`, `produk/`, `platform/`, dan `metrik/` yang sudah dipindah dengan `git mv` dan dihapus dari lokasi asal.

## Batasan

Batasan dokumen ini: hanya salinan janji saat v1.0.0 disetujui. Tidak menjadi acuan operasional terkini; acuan terkini ada di `roadmap/TIMELINE.md` dan `roadmap/MILESTONE.md`.
