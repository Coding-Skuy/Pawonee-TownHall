# Web Bun + Svelte Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Web pendamping (bukan pengganti mobile): dasbor keluarga di desktop dan berbagi resep lewat tautan. Bun 1.4.x + Svelte 5 (runes) + SvelteKit 2 + TypeScript 5.9.x.

## 1. Versi kunci dan perintah

- Bun 1.4.3, Svelte 5.28.0, SvelteKit 2.20.0, TypeScript 5.9.2, Zod 3.23.8, Postgres 16 (tabel `profil_keluarga`, `riwayat_masak`), Vitest 3.2.0.
- Perintah baku: `bun install`, `bun run dev` (port 5173), `bun run sinkron-resep` (tarik `GET /resep` ke `src/lib/data/resep.json`), `bun run test` (vitest paritas 32 kasus), `bun run build`.

## 2. Struktur kode

```
web/
  src/routes/
    +page.svelte (beranda: 5 rekomendasi + stok menipis)
    stok/+page.svelte (proksi Pedaree)
    resep/[id]/+page.svelte (detail + pemilih jiwa 1–8)
    preferensi/+page.svelte (editor profil)
    riwayat/+page.svelte (30 masakan terakhir)
    api/v1/resep/+server.ts, api/v1/resep/[id]/+server.ts,
    api/v1/rekomendasi/+server.ts, api/v1/stok/+server.ts,
    api/v1/profil/[idKeluarga]/+server.ts, api/v1/kesehatan/+server.ts
  src/lib/server/rekomendasi.ts (port RekomendasiEngine + PorsiScaler)
  src/lib/data/resep.json (24 resep, hasil sinkron-resep)
  src/lib/stores/keluarga.svelte.ts (runes: profil, jiwa, waktuMaks)
```

## 3. Halaman dan perilaku (Svelte 5 runes)

- Beranda memakai `$state` untuk profil/jiwa dan `$derived` untuk daftar rekomendasi (fetch `POST /api/v1/rekomendasi` saat stok atau jiwa berubah, debounce 300 ms).
- Halaman `resep/[id]`: `load()` memanggil `GET /api/v1/resep/[id]?jiwa=`; pemilih jiwa memakai stepper; tombol "Cetak" membuka versi cetak 1 halaman (bahan + langkah).
- Halaman `preferensi`: formulir terikat ke skema Zod yang sama dengan server; simpan memakai `PUT /api/v1/profil/[idKeluarga]`.
- Berbagi: tiap resep punya tombol salin tautan `https://pawonee.chefgenie.id/resep/R-06?jiwa=4` yang membuka halaman publik (tanpa login) + tombol "Buka di aplikasi" (deep link `pawonee://resep/R-06?jiwa=4`).

## 4. Server Bun (SvelteKit endpoints)

- `POST /api/v1/rekomendasi`: validasi Zod → panggil `rekomendasikan()` dari `src/lib/server/rekomendasi.ts` → respons 200. Waktu server p95 ≤ 400 ms.
- `GET /api/v1/stok`: proksi ke Pedaree inventori dengan cache memori 60 detik; bila Pedaree mati, kembalikan cache + header `X-Data-Usang: true`.
- Cron Bun (`bun run cron:layu`, tiap 07:00): memindai stok G2-S ≤ 1 hari dan mengirim email ringkas 1 resep tercepat (contoh R-06/R-09).

## 5. Uji paritas dan rilis

- `bun run test` menjalankan 32 kasus dari `kasus-uji.json` (milik KMP) terhadap port TS; selisih skor > 0,01 menggagalkan build.
- Rilis: `bun run build` → artefak Node-Bun di VPS; pratinjau tiap PR via URL sementara; produksi di `https://pawonee.chefgenie.id`.

## 6. Kaitan ke modul lain

- Kontrak endpoint di `produk/21-kontrak-api-web-bun.md`; pembagian kerja KMP vs web di `platform/10-matriks-KMP-web.md`; metrik yang ditampilkan di dasbor web dirinci di `metrik/10-serapan-waste.md`.
