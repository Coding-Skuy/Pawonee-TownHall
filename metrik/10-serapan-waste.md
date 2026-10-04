# Metrik Serapan dan Waste Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Satu tujuan ukur: membuktikan hortikultura grade-2 terserap menjadi nilai, bukan terbuang.

## 1. Definisi

- **Serapan G2**: gram bahan berkode `G2-V/G2-U/G2-S` yang dimasak (ditandai lewat "Tandai dimasak" di mobile atau web). Sumber: `riwayat_masak`.
- **Waste dapur**: gram bahan G2 yang dibuang karena busuk (dilaporkan di halaman stok Pedaree dengan alasan `busuk`). Pawonee tidak mengukur waste kebun, hanya waste dapur.
- **Nilai**: rupiah yang dihemat = `gramG2Terserap × harga pasar per gram bahan setara non-G2 × 0,6` (faktor 0,6 karena G2 dihargai 60% dari harga normal).

Harga pasar acuan per 100 g: wortel 2.000, kentang 2.500, tomat 3.000, kangkung 1.800, cabai 7.000, santan 5.000 per 200 ml.

## 2. Tiga metrik utama (dimonitor mingguan)

| Metrik | Rumus | Target Varian 1 (per keluarga per minggu) | Sumber |
|---|---|---|---|
| Serapan G2 mingguan | Σ gram G2 dimasak dalam 7 hari | ≥ 1.500 g | `riwayat_masak` |
| Rasio serapan | gram G2 dimasak / (gram G2 dimasak + gram G2 busuk) | ≥ 80% | `riwayat_masak` + laporan busuk Pedaree |
| Waste dapur mingguan | Σ gram G2 busuk dalam 7 hari | ≤ 300 g | laporan busuk Pedaree |

Agregat program (100 keluarga percontohan): serapan ≥ 150 kg per minggu, rasio ≥ 80%, nilai hemat ≥ Rp9.000.000 per minggu (150.000 g × rata-rata Rp100 per gram × 0,6).

## 3. Cara ukur (tanpa tebakan)

1. Tiap "Tandai dimasak" menulis baris `riwayat_masak { resepId, jiwa, gramG2Terserap (dihitung dari bahan terskala), tanggal }`.
2. Tiap laporan busuk menulis baris `laporan_busuk { namaBahan, gram, kodeGrade, tanggal }` di Pedaree.
3. Dasbor web (halaman `riwayat`) menampilkan: batang serapan 8 minggu terakhir, rasio bulan berjalan, dan 3 bahan paling sering busuk beserta saran resep tercepat (misalnya kangkung sering busuk → tawarkan R-06).
4. Periode ukur: Senin 00:00–Minggu 23:59 WIB. Keluarga baru mulai diukur minggu kedua (minggu pertama adalah adaptasi).

## 4. Aturan anti-curang

- "Tandai dimasak" yang sama (`resepId` + hari sama) dihitung sekali; penekanan ganda diabaikan.
- Gram serapan memakai takaran terskala (`PorsiScaler`), bukan klaim pengguna, sehingga tidak bisa digelembungkan.
- Keluarga dengan serapan 0 selama 2 minggu berturut-turut menerima kunjungan edukasi, bukan penalti.

## 5. Kaitan ke modul lain

- Peristiwa yang dicatat (`resep_dimasak`, `serapan_dihitung`) didefinisikan di `platform/40-mobile-KMP.md` dan `platform/50-web-bun-svelte.md`.
- Rekomendasi yang mengoptimalkan metrik ini dirinci di `produk/10-alur-rekomendasi.md` (bobot serapan 50 + bonus layu).
