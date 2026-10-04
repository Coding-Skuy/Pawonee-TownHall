> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# CHANGELOG v1.0.0 — Versi Awal Pawonee

## Ringkasan Isi

v1.0.0 adalah versi awal TownHall Pawonee yang dibekukan mengikuti template emas Lumbung. Seluruh isi lama dari folder `resep/`, `preferensi/`, `produk/`, `platform/`, dan `metrik/` dipecah dan dipindah dengan `git mv` ke struktur versi ini, lalu folder lama dihapus agar hanya ada satu sumber kebenaran. Koreksi kunci versi ini: bank dikunci 12 resep grade-2 inti (bukan 24), `:shared:pantry-resep` milik Pawonee (bukan milik bersama), DB `pawonee`, JWT audiens `pawonee`.

## Isi per Direktori

- BRD: `00-ikhtisar.md` memuat piagam, bank 12 resep, DB `pawonee`, dan JWT audiens `pawonee`. `10-resep-grade2.md` memuat grade G2-V, G2-U, G2-S, substitusi baku, porsi dasar 4 jiwa, dan faktor 1 sampai 8 jiwa. `20-preferensi-keluarga.md` memuat struktur profil, daftar alergi dan pantangan, dan aturan budget. `30-serapan-nilai.md` memuat serapan minimal 1.500 g, rasio minimal 80 persen, dan waste maksimal 300 g.
- PRD: `10-pengguna.md` memuat 4 peran: juru masak, pengelola profil, pengguna web, admin konten. `20-alur.md` memuat lima tahap filter sampai penjelasan. `30-kriteria.md` memuat US-001 dan seterusnya, acceptance, dan non-goals.
- FRD: `10-fungsional.md` memuat FR-001 dan seterusnya per segmen resep, preferensi, rekomendasi, tanpa cara implementasi.
- FSD: `10-alur.md` memuat luring dulu dan matriks KMP lawan web, `20-model-data.md` memuat entitas Resep, ProfilKeluarga, StokItem alias Pedaree, Rekomendasi, dan tabel DB `pawonee`, `30-kontrak.md` memuat kontrak mobile dan web, deep link, event, dan autentikasi JWT audiens `pawonee`.
- `SNAPSHOT-ROADMAP.md` memuat salinan beku janji beta 90 hari.

## Sumber Pemindahan

- `resep/10-bank-resep-grade2.md` dan `resep/20-standar-porsi.md` menjadi BRD resep grade-2. `preferensi/10-model-keluarga.md` menjadi BRD preferensi. `metrik/10-serapan-waste.md` menjadi BRD serapan.
- `produk/10-alur-rekomendasi.md` menjadi PRD alur. `platform/20-navigasi3.md` menjadi PRD pengguna. `produk/20-kontrak-api-KMP-mobile.md` dan `produk/21-kontrak-api-web-bun.md` menjadi FSD kontrak. `produk/30-modul-KMP-bersama.md` menjadi FSD model data. `platform/10-matriks-KMP-web.md`, `platform/40-mobile-KMP.md`, dan `platform/50-web-bun-svelte.md` menjadi FSD alur.

## Batasan

Batasan versi ini: hanya bank 12 resep grade-2, rekomendasi 5 item, DB `pawonee`, dan JWT audiens `pawonee`. Perubahan setelah ini wajib masuk v1.1.0 atau v2.0.0 dan dicatat di `roadmap/` living, bukan dengan mengubah file beku ini.
