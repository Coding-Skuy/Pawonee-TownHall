> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# Pawonee-TownHall — Divisi AI Cooking PT ChefGenie

## Peran Pawonee

Pawonee adalah divisi AI Cooking PT ChefGenie: mengubah hortikultura grade-2 (cacat visual, ukuran tidak standar, surplus cepat layu) menjadi hidangan bernilai melalui rekomendasi resep berbasis stok nyata. Bank inti: 12 resep grade-2 di `pawonee-ai-models` (`models/bank_resep_grade2.json`), dievaluasi 12/12 lolos. Modul bersama `:shared:pantry-resep` adalah milik Pawonee (sumber: `pawonee-app-kmp/shared/pantry-resep`); repo lain dan Pedaree hanya mengonsumsi. Basis data: DB `pawonee` (Postgres). Autentikasi: JWT akses 15 menit ditambah refresh 7 hari dengan audiens `pawonee`, ditambah API key perangkat dan service key antar layanan. Sinyal lintas divisi: topik `sinyal.pedaree.v1` yang diproduksi `pawonee-data-pipeline` untuk dikonsumsi Pedaree.

## Peta Versi Aktif

- Versi aktif: v1.0.0 (disetujui). Isi beku ada di `versions/v1.0.0/`.
- `versions/v1.0.0/CHANGELOG.md` — ringkasan versi awal.
- `versions/v1.0.0/BRD/` — kebutuhan bisnis BR-001 dan seterusnya.
- `versions/v1.0.0/PRD/` — pengguna dan kriteria US-001 dan seterusnya.
- `versions/v1.0.0/FRD/` — kebutuhan fungsional FR-001 dan seterusnya.
- `versions/v1.0.0/FSD/` — rancangan alur, model data Resep, ProfilKeluarga, StokItem, Rekomendasi, dan kontrak API.
- `versions/v1.0.0/SNAPSHOT-ROADMAP.md` — salinan beku janji v1.0.0.
- Peta hidup lintas versi ada di `roadmap/`: `TIMELINE.md`, `MILESTONE.md`, `ROADMAP.md`.

## Cara Baca History

1. Mulai dari `versions/v1.0.0/CHANGELOG.md` untuk ringkasan versi.
2. Lanjut ke `versions/v1.0.0/BRD/00-ikhtisar.md` untuk konteks bisnis, lalu `PRD/10-pengguna.md` untuk peran.
3. Untuk janji waktu itu, baca `versions/v1.0.0/SNAPSHOT-ROADMAP.md` yang sudah dibekukan dan tidak diubah lagi.
4. Untuk kondisi terkini lintas versi, baca `roadmap/TIMELINE.md` dan `roadmap/MILESTONE.md`.
5. Riwayat perubahan antar versi dilacak lewat `git log` dan `CHANGELOG.md` tiap versi. File lama sengaja dipindah dengan `git mv` agar tidak ada dua sumber kebenaran.

## TownHall Lain dan Pedoman Induk

Pedoman induk: https://github.com/Coding-Skuy/ChefGenie-TownHall.

Pola yang ditiru persis dari template emas https://github.com/Coding-Skuy/Lumbung-TownHall: penamaan `versions/vX.Y.Z/BRD|PRD|FRD|FSD/`, file `NN-nama-kebab.md`, header versi satu baris, dan bagian Batasan di tiap file.

Lima TownHall lain yang memakai pola yang sama:

- https://github.com/Coding-Skuy/Lumbung-TownHall — agregasi dan pasokan grade-2, sumber harga Pawonee.
- https://github.com/Coding-Skuy/Pasaree-TownHall — pasar dan penjualan.
- https://github.com/Coding-Skuy/Pedaree-TownHall — inventori rumah dan konsumen sinyal Pawonee.
- https://github.com/Coding-Skuy/TitipO-TownHall — titip dan kemitraan.
- https://github.com/Coding-Skuy/Titeny-TownHall — ketelitian dan audit mutu.

## Batasan

Batasan ruang lingkup repo ini: hanya bank 12 resep grade-2, standar porsi, model keluarga dan preferensi, alur rekomendasi, modul `:shared:pantry-resep` milik Pawonee, kontrak API Pawonee, dan metrik serapan. Di luar batas: pencatatan stok inventori milik Pedaree, agregasi panen dan papan harga milik Lumbung, harga ecer pasar milik Pasaree, routing last-mile milik Pedaree, skema titip milik TitipO, dan audit independen milik Titeny.
