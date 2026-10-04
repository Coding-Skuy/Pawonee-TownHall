> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# PRD 10 — Pengguna Pawonee

## Daftar Peran

- Juru masak keluarga: menerima 5 rekomendasi yang bisa dimasak hari ini, menandai dimasak, dan berbagi tautan resep. Kebutuhan: daftar rekomendasi, bahan terskala sesuai jiwa, dan daftar belanja tambahan. Sumber navigasi lama: `platform/20-navigasi3.md` (6 layar: beranda, stok, rekomendasi, resepDetail, preferensi, riwayat; deep link `pawonee://resep/{id}?jiwa=4`).
- Pengelola profil keluarga: menyunting alergi, pantangan, budget, dan alat; menyimpan memicu hitung ulang. Kebutuhan: editor profil dan 4 profil baku (Budi 4 jiwa, Sari balita, vegetarian 3 jiwa, warung 8 jiwa).
- Pengguna web pendamping: memakai dasbor desktop, mencetak 1 halaman resep, dan menyalin tautan bagi `https://pawonee.chefgenie.id/resep/{id}?jiwa=4`. Kebutuhan: halaman beranda, stok proksi Pedaree, detail per jiwa, preferensi, dan riwayat 30 masakan.
- Admin konten resep: memelihara bank 12 resep dan harga Lumbung; memicu evaluasi 12/12. Kebutuhan: arsip bank, hasil evaluasi, dan matriks paritas 32 kasus.

## Hak Akses

- Keluarga hanya melihat profil, stok, dan riwayatnya sendiri. Admin konten melihat bank dan metrik agregat tanpa nama bahan rinci per keluarga bila pengguna menolak analitik. Tidak ada akun bersama: 1 orang 1 akun.
- Autentikasi: JWT akses 15 menit, refresh 7 hari, audiens `pawonee`, PIN 6 digit diganti 90 hari, API key perangkat terdaftar, service key server-ke-server dirotasi 90 hari. Tiga salah PIN mengunci 15 menit.

## Batasan

Batasan dokumen ini: hanya peran, kebutuhan pandang, dan hak akses. Aturan bisnis rinci ada di BRD, langkah sistem ada di FSD. Di luar batas: peran pencatat inventori milik Pedaree dan peran agregator milik Lumbung.
