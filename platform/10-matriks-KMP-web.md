# Matriks KMP vs Web Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Menjawab satu pertanyaan: logika apa tinggal di KMP bersama, apa di aplikasi mobile, apa di web Bun+Svelte.

## 1. Matriks keputusan

| Kemampuan | KMP bersama (`pantry-resep`) | Mobile (Compose + Navigation3) | Web (Bun 1.4 + SvelteKit 2) |
|---|---|---|---|
| Bank 24 resep + harga Lumbung | Pemilik (`ResepBawaan.kt`) | Baca saja | Impor salinan JSON, uji paritas |
| Skor rekomendasi tahap 1–4 | Pemilik (`RekomendasiEngine`) | Panggil langsung (luring) | Port TS, hasil identik |
| Skala porsi 1–8 jiwa | Pemilik (`PorsiScaler`) | Panggil langsung | Port TS, hasil identik |
| Substitusi bahan | Pemilik (`Substitusi`) | Panggil langsung | Port TS |
| Baca stok Pedaree | Model `StokItem` saja | Sinkron via REST + cache DataStore | Proksi `GET /api/v1/stok` ke Pedaree |
| Profil keluarga | Model + validasi | DataStore + UI editor | Tabel Postgres + UI editor |
| Navigasi layar | Tidak tahu navigasi | Navigation3 (lihat `platform/20-navigasi3.md`) | SvelteKit routing (`/`, `/stok`, `/resep/[id]`) |
| Notifikasi layu cepat | Hitung bonus skor | Worker harian + notifikasi | Cron Bun harian + email |
| Analitik serapan | Hitung per rekomendasi | Kirim kejadian `serapan_dihitung` | Agregasi + dasbor metrik |

## 2. Aturan anti-duplikasi

1. Rumus skor dan faktor porsi hanya ditulis sekali di Kotlin. Web mem-port, bukan menemukan ulang; perbedaan > 0,01 pada uji paritas memblokir rilis web.
2. Teks nama resep, langkah, dan alergen hanya ditulis sekali di `ResepBawaan.kt`; web menariknya lewat `GET /api/v1/resep` saat build (`bun run sinkron-resep`).
3. Validasi rentang (`jiwa` 1–8, `gram` ≥ 0) diduplikasi di Zod web dan Kotlin secara sadar agar kesalahan tertangkap di tepi; tabel rentangnya sama.

## 3. Mode luring dan daring

- Mobile: penuh luring untuk rekomendasi dan porsi; daring hanya untuk sinkron stok/resep/profil. Banner "luring" muncul bila sinkron > 24 jam.
- Web: butuh daring (aplikasi pendamping). Bila API Pedaree mati, halaman stok menampilkan cache 60 detik terakhir dengan penanda "data 1 jam lalu".

## 4. Kaitan ke modul lain

- Rincian mobile di `platform/40-mobile-KMP.md`; rincian web di `platform/50-web-bun-svelte.md`; navigasi mobile di `platform/20-navigasi3.md`.
