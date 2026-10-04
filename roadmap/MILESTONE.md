> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# MILESTONE Pawonee — Status Living

Legenda status: todo berarti belum mulai, doing berarti sedang berjalan, done berarti selesai terverifikasi.

## Milestone v1.0.0

- M-001 Bank 12 resep grade-2 dievaluasi 12/12 lolos — status: done. Bukti: `eval/metrics.md` di `pawonee-ai-models`.
- M-002 Modul `:shared:pantry-resep` milik Pawonee terbit 1.0.0 — status: done. Bukti: artefak `id.chefgenie.pawonee:pantry-resep:1.0.0` dan uji 32 kasus hijau.
- M-003 Mobile KMP beta 100 keluarga dengan rekomendasi luring di bawah 800 ms — status: doing. Target: 80 persen rekomendasi dibuka.
- M-004 Web pendamping Bun dan SvelteKit tayang dengan paritas skor di bawah 0,01 — status: doing. Bukti: `bun run test` 32 kasus hijau.
- M-005 Backend Rust menyajikan rekomendasi dan preferensi di atas DB `pawonee` — status: doing. Bukti: kontrak `GET /v1/resep/rekomendasi` lolos uji kontrak.
- M-006 Pipa menerbitkan `sinyal.pedaree.v1` tiap 15 menit — status: doing. Bukti: tabel `sinyal_pedaree` terisi dan dibaca Pedaree.
- M-007 Serapan rata-rata minimal 300 g G2 per keluarga per minggu pada beta — status: doing. Sumber: `riwayat_masak`.
- M-008 Migrasi `pawonee-web` Next.js 15 ke SvelteKit dan Bun terbaru direncanakan tuntas — status: todo. Syarat mulai: kontrak dibekukan dan fase coding disetujui TownHall.

## Aturan Pembaruan

- Status diubah hanya oleh Kepala Pawonee dengan bukti tanggal. Milestone yang sudah done tidak dihapus, hanya ditambah catatan verifikasi.

## Batasan

Batasan dokumen ini: hanya status milestone dan bukti ringkas. Rincian angka ada di `versions/v1.0.0/BRD/` dan `versions/v1.0.0/PRD/30-kriteria.md`. Dokumen ini tidak mengubah janji beku v1.0.0.
