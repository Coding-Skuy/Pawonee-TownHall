> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 00 — Ikhtisar Pawonee

## Konteks

Divisi Pawonee adalah AI Cooking PT ChefGenie. Tugasnya mengubah hortikultura grade-2 menjadi hidangan bernilai lewat rekomendasi resep berbasis stok nyata dari Pedaree dan harga pasokan Lumbung. Sumber isi lama: `README.md` TownHall. Basis data tunggal: DB `pawonee` (Postgres). Modul bersama `:shared:pantry-resep` adalah milik Pawonee; Pedaree hanya mengonsumsi keluarannya.

## Kebutuhan Bisnis

- BR-001 Pawonee wajib memelihara bank 12 resep grade-2 inti di `pawonee-ai-models` dengan evaluasi 12/12 lolos grade-2 sebelum setiap rilis.
- BR-002 Setiap rekomendasi wajib memakai stok Pedaree sebagai masukan dan tidak menduplikasi pencatatan stok inventori.
- BR-003 Satu profil keluarga mengendalikan keamanan rekomendasi: alergi menyaring mutlak, pantangan `halal-selalu` dan `vegetarian-nabati` menyaring mutlak, alat masak yang tidak dimiliki menyembunyikan resep.
- BR-004 Sukses diukur sebagai serapan: serapan G2 minimal 1.500 g per keluarga per minggu, rasio serapan minimal 80 persen, waste dapur maksimal 300 g per minggu.
- BR-005 Rekomendasi on-device maksimal 5 item, dihitung dalam 800 ms pada HP RAM 3 GB tanpa internet setelah sinkron.
- BR-006 Harga acuan belanja tambahan memakai harga Lumbung G2 per 100 g; total belanja tambahan tidak boleh melebihi `budgetPerMakan` profil.
- BR-007 Autentikasi dikunci: JWT akses 15 menit, refresh 7 hari, audiens `pawonee`, API key perangkat terdaftar, service key server-ke-server, tanpa akun bersama.

## Metrik

- Serapan mingguan gram G2 per keluarga. Rasio serapan. Waste dapur. Persen rekomendasi dibuka. Skor paritas Kotlin lawan TypeScript di bawah 0,01.

## Batasan

Batasan dokumen ini: hanya menyatakan kebutuhan bisnis dan angka ambang. Cara pemenuhan diatur di PRD, FRD, dan FSD. Di luar batas: pencatatan inventori milik Pedaree, agregasi panen milik Lumbung, dan audit independen milik Titeny.
