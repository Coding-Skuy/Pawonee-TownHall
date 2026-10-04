> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 30 — Serapan dan Nilai

## Konteks

Satu tujuan ukur: membuktikan hortikultura grade-2 terserap menjadi nilai, bukan terbuang. Sumber isi lama: `metrik/10-serapan-waste.md`, dipindah dengan `git mv` ke file ini.

## Kebutuhan Bisnis

- BR-301 Definisi dikunci: serapan G2 adalah gram berkode G2-V, G2-U, atau G2-S yang dimasak dan ditandai lewat `Tandai dimasak`; waste dapur adalah gram G2 yang dilaporkan busuk; nilai hemat sama dengan gram terserap dikali harga pasar per gram bahan setara dikali 0,6.
- BR-302 Tiga metrik mingguan per keluarga dikunci: serapan minimal 1.500 g, rasio serapan (dimasak dibagi dimasak ditambah busuk) minimal 80 persen, waste dapur maksimal 300 g. Agregat program 100 keluarga: serapan minimal 150 kg per minggu dan nilai hemat minimal Rp9.000.000 per minggu.
- BR-303 Cara ukur tanpa tebakan dikunci: tiap `Tandai dimasak` menulis `riwayat_masak` berisi resep, jiwa, gram terserap dari takaran terskala, dan tanggal; tiap laporan busuk menulis `laporan_busuk` di Pedaree. Periode Senin 00.00 sampai Minggu 23.59 WIB; keluarga baru diukur mulai minggu kedua.
- BR-304 Aturan anti-curang dikunci: tanda ganda resep dan hari yang sama dihitung sekali; gram memakai takaran `PorsiScaler`, bukan klaim pengguna; serapan nol 2 minggu berturut-turut menerima kunjungan edukasi, bukan penalti.

## Metrik

- Batang serapan 8 minggu terakhir, rasio bulan berjalan, dan 3 bahan paling sering busuk beserta saran resep tercepat tampil di halaman riwayat web dan mobile.

## Batasan

Batasan segmen ini: hanya definisi, rumus, dan aturan ukur serapan. Di luar batas: waste kebun, pembukuan PT induk, dan pajak. Pawonee tidak mengukur waste kebun, hanya waste dapur yang dilaporkan lewat Pedaree.
