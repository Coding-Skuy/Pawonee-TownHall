# Pawonee-TownHall — AI Cooking Assistant Grade-2

> Varian 1 · Bahasa: Indonesia · Status: disahkan TownHall.
> Peran: mengubah hortikultura grade-2 (cacat visual, ukuran tidak standar, surplus cepat layu) menjadi hidangan bernilai melalui rekomendasi resep berbasis stok nyata.

## 1. Peran dalam ekosistem ChefGenie

- **Pawonee (repo ini)**: otak resep dan rekomendasi. Menyimpan bank 24 resep, standar porsi, model keluarga, mesin rekomendasi, dan aplikasi mobile + web.
- **Pedaree (inventori)**: sumber stok rumah tangga. Pawonee membaca `StokItem` Pedaree dan tidak menduplikasi pencatatan stok. Repo: `https://github.com/Coding-Skuy/Pedaree-TownHall` (modul `inventori/`, endpoint `GET /stok`).
- **Lumbung (agregasi grade-2)**: sumber pasokan dan harga grade-2 hari ini. Pawonee memakai daftar dan harga Lumbung untuk menghitung belanja tambahan. Repo: `https://github.com/Coding-Skuy/Lumbung-TownHall` (modul `agregasi/`, daftar harga G2).

Aliran data: `Lumbung agregasi G2 → Pedaree inventori rumah → Pawonee rekomendasi resep → dapur keluarga → metrik serapan`.

## 2. Peta folder

```
Pawonee-TownHall/
  README.md
  resep/
    10-bank-resep-grade2.md   # 24 resep R-01–R-24 + substitusi
    20-standar-porsi.md       # dasar 4 jiwa + faktor 1–8 jiwa
  preferensi/
    10-model-keluarga.md      # profil, alergi, pantangan, budget, alat
  produk/
    10-alur-rekomendasi.md    # 5 tahap filter → skor → ranking
    20-kontrak-api-KMP-mobile.md  # antarmuka Kotlin + REST mobile
    21-kontrak-api-web-bun.md     # endpoint Bun/SvelteKit
    30-modul-KMP-bersama.md   # modul :shared:pantry-resep (milik bersama Pedaree)
  platform/
    10-matriks-KMP-web.md     # pembagian logika KMP vs web
    20-navigasi3.md           # 6 layar Navigation3 + deep link
    40-mobile-KMP.md          # aplikasi utama KMP + Compose
    50-web-bun-svelte.md      # web pendamping Bun + SvelteKit
  metrik/
    10-serapan-waste.md       # serapan ≥1500 g, rasio ≥80%, waste ≤300 g per minggu
```

## 3. Stack terkunci

- **Mobile utama**: Kotlin Multiplatform (Kotlin 2.1.20) + Compose Multiplatform 1.7.3 + Navigation3 1.0.0 (Android `minSdk 26`, iOS 16+). Logika luring penuh via modul `pantry-resep`.
- **Web pendamping**: Bun 1.4.3 + Svelte 5.28 + SvelteKit 2.20 + TypeScript 5.9.2 + Zod 3.23.8 + Postgres 16.
- **Berbagi dengan Pedaree**: modul KMP `:shared:pantry-resep` (`id.chefgenie.pawonee.pantryresep`); model `StokItem` dipakai ulang dari Pedaree; uji paritas 32 kasus Kotlin ↔ TypeScript.

## 4. Mulai cepat

1. Baca berurutan: `resep/10-bank-resep-grade2.md` → `resep/20-standar-porsi.md` → `preferensi/10-model-keluarga.md` → `produk/10-alur-rekomendasi.md`.
2. Implementasi mobile: `produk/30-modul-KMP-bersama.md` + `platform/40-mobile-KMP.md` + `platform/20-navigasi3.md`.
3. Implementasi web: `produk/21-kontrak-api-web-bun.md` + `platform/50-web-bun-svelte.md`, patuhi `platform/10-matriks-KMP-web.md`.
4. Ukur dampak: `metrik/10-serapan-waste.md`.

## 5. Tautan

- Pedaree inventori (stok): `https://github.com/Coding-Skuy/Pedaree-TownHall`
- Lumbung agregasi grade-2 (pasokan + harga): `https://github.com/Coding-Skuy/Lumbung-TownHall`
- API produksi: `https://api.pawonee.chefgenie.id/v1` · Web: `https://pawonee.chefgenie.id` · Deep link: `pawonee://resep/{id}?jiwa=4`
