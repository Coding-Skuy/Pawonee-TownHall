> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# FSD 20 — Model Data

## Entitas Inti

Sumber isi lama: `produk/30-modul-KMP-bersama.md`, dipindah dengan `git mv` ke file ini. Koreksi versi ini: `:shared:pantry-resep` adalah milik Pawonee (koordinat Gradle `:shared:pantry-resep`, paket `id.chefgenie.pawonee.pantryresep`, artefak `id.chefgenie.pawonee:pantry-resep:1.0.0`); Pedaree hanya mengonsumsi keluarannya.

- Resep: id `R-01` sampai `R-12` memetakan 12 resep bank (`tempe-tumis-bawang` sampai `capcay-kuah`), nama, kelompok enum kuah, tumis, sambal, lauk, kukus, waktu menit integer, kisaran biaya rupiah integer, bahan berisi nama, gram dasar 4 porsi integer, kode grade enum G2-V, G2-U, G2-S, NON-G2, dan flag opsional, langkah list string, alergen list string, alat wajib list string.
- ProfilKeluarga: id `KEL-NNNN`, jumlah jiwa 1 sampai 8, komposisi dewasa, anak, balita, lansia, alergi dan pantangan list string, budget per makan rupiah integer, pedas 0 sampai 3, alat list string, sumber belanja enum.
- StokItem: dipakai ulang dari Pedaree lewat alias tipe (nama bahan, gram integer, kedaluwarsa hari integer, kode grade); Pawonee tidak mendefinisikan ulang.
- Rekomendasi: rujukan resep, skor total double, alasan string 2 kalimat, bahan terskala berisi nama dan gram integer, belanja tambahan berisi nama, gram integer, dan perkiraan rupiah integer.
- RiwayatMasak: resep id, jiwa, gram G2 terserap integer dari takaran terskala, tanggal. Laporan busuk dicatat di Pedaree.

## Aturan Angka

- Berat selalu gram integer, uang selalu rupiah integer, waktu selalu menit integer. Harga Lumbung G2 per 100 g: wortel 1.200, kentang 1.500, tomat 1.800, kangkung 1.000, cabai 4.000.
- DB `pawonee` (Postgres): tabel `profil_keluarga` (kolom alergi dan pantangan array teks), tabel `riwayat_masak`, tabel `sinyal_pedaree` untuk topik `sinyal.pedaree.v1`. Retensi riwayat minimal 2 tahun.

## Batasan

Batasan dokumen ini: hanya definisi entitas, kunci, enum, dan aturan angka. Serialisasi JSON dan endpoint ada di `30-kontrak.md`. Perubahan skema butuh persetujuan Tech Lead dan migrasi teruji.
