> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FSD 30 — Kontrak API, Event, dan Galat

## Basis dan Autentikasi

Sumber isi lama: `produk/20-kontrak-api-KMP-mobile.md` dan `produk/21-kontrak-api-web-bun.md`, dipindah dengan `git mv` lalu digabung di sini.

- Basis mobile dan backend: `https://api.pawonee.chefgenie.id/v1` dengan header `X-Kontrak-Versi: v1`. Basis web pendamping: `https://web.pawonee.chefgenie.id/api/v1` dengan bentuk JSON identik. Sehat: `GET /v1/kesehatan` dan `GET /api/v1/kesehatan` kembali status baik dan versi kontrak `v1`.
- Autentikasi dikunci: JWT akses 15 menit, refresh 7 hari, audiens `pawonee`, API key perangkat terdaftar, service key server-ke-server rotasi 90 hari. Batas 30 permintaan per menit per token; jawaban `429` berisi `retrySetelahDetik: 60`.
- Semua berat gram integer, uang rupiah integer, waktu menit integer. Kesalahan tertutup `Hasil<T>`: `Berhasil(data)` atau `Gagal(kode, pesan)` dengan kode baku `STOK_KOSONG`, `PROFIL_TIDAK_ADA`, `JARINGAN_GAGAL`, `RESEP_TIDAK_ADA`, `JIWA_DI_LUAR_RENTANG`, `VALIDASI_GAGAL`, `BATAS_LAJU`.

## Kontrak per Konsumen

- Mobile KMP 5 operasi: `GET /resep` (24 resep di dokumen lama dikoreksi menjadi 12 resep bank, cache 7 hari), `GET /resep/{id}?jiwa=4` (bahan terskala), `POST /rekomendasi` (maksimal 5 item, dipakai bila RAM di bawah 2 GB), `GET /stok?idKeluarga=KEL-0001` (sumber Pedaree inventori), `PUT /profil/{idKeluarga}` (simpan profil).
- Web Bun 6 rute di `src/routes/api/v1/`: `resep`, `resep/[id]`, `rekomendasi`, `stok` (proksi Pedaree cache 60 detik), `profil/[idKeluarga]`, `kesehatan`. Validasi Zod: `jiwa` 1 sampai 8, `waktuMaksimalMenit` 10 sampai 45, `gram` minimal 0, `kodeGrade` salah satu dari G2-V, G2-U, G2-S, NON-G2. CORS hanya mengizinkan `https://pawonee.chefgenie.id`.
- Deep link bersama: `pawonee://resep/{id}?jiwa=4` membuka layar detail langsung; tautan web bagi `https://pawonee.chefgenie.id/resep/{id}?jiwa=4` membuka halaman publik tanpa login.

## Event dan Galat

- Event: `rekomendasi.dihitung`, `resep.dimasak`, `serapan.dihitung`, `profil.disimpan`, `sinyal.diterbitkan`. Setiap event membawa id keluarga, waktu, dan versi kontrak.
- Idempotensi: `PUT profil` memakai versi profil; kirim ulang sama kembali 200 tanpa duplikat. Galat baku: 400 validasi, 404 resep tidak ada, 429 batas laju, 500 kesalahan server.
- Konsumen dilarang menghitung harga di klien web; semua angka belanja memakai harga server berbasis harga Lumbung G2.

## Batasan

Batasan dokumen ini: hanya kontrak, event, dan galat. Implementasi server ada di repo Pawonee-Backend. Port TypeScript wajib lolos uji paritas 32 kasus sebelum rilis web.
