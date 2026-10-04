> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# PRD 20 — Alur Produk

## Alur Rekomendasi Lima Tahap

Sumber isi lama: `produk/10-alur-rekomendasi.md`, dipindah dengan `git mv` ke file ini.

1. Masukan disiapkan: stok Pedaree (`namaBahan`, gram, kedaluwarsa, kode grade), pasokan dan harga Lumbung G2 hari ini, profil keluarga, parameter `jiwa` 1 sampai 8 dan `waktuMaksimalMenit` bawaan 30.
2. Tahap 1 filter keras: buang resep beralergen, pelanggar pantangan mutlak, butuh alat yang tidak dimiliki, dan waktu melebihi batas. Filter menghilangkan, bukan menurunkan skor.
3. Tahap 2 skor serapan bobot 50: serapan sama dengan gram G2 terpakai dibagi gram G2 tersedia dikali 50, ditambah bonus 5 bila memakai G2-S kedaluwarsa maksimal 2 hari, dinormalkan ke 50.
4. Tahap 3 skor kemudahan bobot 50: kecocokan stok 25 poin, kecocokan budget 15 poin (15 bila belanja di bawah 60 persen budget, 8 bila 60 sampai 100 persen), kecocokan waktu 10 poin (10 bila maksimal 15 menit, 7 bila 16 sampai 30 menit, 4 bila 31 sampai 45 menit).
5. Tahap 4 ranking dan diversifikasi: jumlahkan maksimal 100, urut menurun, 5 besar maksimal 2 resep sekelompok, seri dimenangkan waktu terkecil lalu belanja terkecil.
6. Tahap 5 penjelasan dan belanja: tiap rekomendasi mengembalikan `skorTotal`, `alasan` 2 kalimat, dan `belanjaTambahan` berisi bahan, gram, dan perkiraan harga Lumbung.

## Contoh Nyata

Stok kentang mini 600 g dan wortel bengkok 400 g untuk profil Budi 4 jiwa: resep kentang lodeh memakai 750 g sehingga serapan 37,5, cocok stok 16,7, budget 15, waktu 4, total 73,2 dan peringkat 1. Resep tumis kangkung tanpa stok kangkung tersingkir dari 5 besar walau waktunya 12 menit.

## Batasan

Batasan alur ini: hanya masukan, lima tahap, ranking, dan keluaran penjelasan. Di luar batas: pengolahan dapur fisik, penjualan ecer, dan pengantar ke rumah konsumen. Bila stok kosong, kembalikan 5 resep termurah dengan penanda `modeTanpaStok: true`.
