> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FSD 10 — Alur Sistem

## Urutan Luring Dahulu dan Pembagian Kerja

Sumber isi lama: `platform/10-matriks-KMP-web.md`, `platform/40-mobile-KMP.md`, dan `platform/50-web-bun-svelte.md`, dipindah dengan `git mv` lalu digabung di sini.

1. Mobile KMP bekerja luring dulu: `RekomendasiEngine` dan `PorsiScaler` berjalan on-device dari `:shared:pantry-resep` milik Pawonee; API jarak jauh hanya untuk sinkron stok, resep, dan profil. Rekomendasi 12 resep selesai dalam 800 ms pada HP RAM 3 GB.
2. Rumus skor dan faktor porsi hanya ditulis sekali di Kotlin; web mem-port ke TypeScript di `src/lib/server/rekomendasi.ts`, bukan menemukan ulang; selisih di atas 0,01 pada 32 kasus memblokir rilis web.
3. Cache dikunci: resep 7 hari, stok 24 jam, profil selamanya di DataStore; web memproksi stok Pedaree dengan cache 60 detik dan menandai data usang lewat header `X-Data-Usang: true` bila Pedaree mati.
4. Pekerja harian 07.00: sinkron stok Pedaree, hitung bonus layu G2-S, kirim notifikasi bila ada stok kedaluwarsa maksimal 1 hari. Banner luring muncul bila sinkron melebihi 24 jam.
5. Navigasi mobile memakai Navigation3 satu graf 6 layar (beranda, stok, rekomendasi, resepDetail, preferensi, riwayat); `jiwa` dibawa sebagai argumen navigasi; perubahan profil disiarkan via `StateFlow` dan menghitung ulang otomatis.
6. Web memakai SvelteKit routing (`/`, `/stok`, `/resep/[id]`, `/preferensi`, `/riwayat`); beranda memakai runes `$state` dan `$derived` dengan debounce 300 ms; cron Bun harian mengirim email 1 resep tercepat untuk stok layu.

## Batasan

Batasan dokumen ini: hanya urutan sistem dan aturan sinkron. Formula bisnis ada di FSD model data dan kontrak. Di luar batas: desain visual dan merek. Target mutu: uji 32 kasus hijau sebelum beta dan crash-free 99,5 persen sebelum produksi.
