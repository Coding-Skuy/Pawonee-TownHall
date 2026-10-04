> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 10 — Bank Resep Grade-2 dan Standar Porsi

## Konteks

Bank resep adalah inti Pawonee: 12 resep Indonesia grade-2 yang toleran rupa, maksimal 45 menit, kompor tunggal, biaya porsi keluarga 4 jiwa Rp18.000 sampai Rp45.000. Sumber isi lama: `resep/10-bank-resep-grade2.md` dan `resep/20-standar-porsi.md`, dipindah dengan `git mv` lalu digabung di sini. Koreksi versi ini: jumlah dikunci 12 resep inti (daftar di `pawonee-ai-models/models/bank_resep_grade2.json`: tempe-tumis-bawang, sayur-bening-bayam, telur-balado, nasi-goreng-kampung, sop-ayam-sederhana, tumis-kangkung, semur-tahu, pecel-sayur, ayam-kecap-rumahan, sop-jagung-telur, sambal-goreng-tempe-kentang, capcay-kuah), bukan 24. Perluasan bank masuk v2.0.0.

## Kebutuhan Bisnis

- BR-101 Grade-2 berarti layak konsumsi dan hanya kalah rupa atau ukuran: G2-V cacat visual, G2-U ukuran tidak standar, G2-S surplus cepat layu. Setiap bahan utama resep wajib menandai kode grade yang diserap.
- BR-102 Prinsip formulasi dikunci: sayur G2-V dicacah, diulek, atau direbus; protein hewani opsional dan dapat diganti telur, tahu, atau tempe; bumbu dasar hanya putih, kuning, dan merah.
- BR-103 Aturan substitusi baku wajib tersedia: ayam 100 g setara telur 2 butir atau tahu 200 g atau tempe 150 g; santan 400 ml setara susu kedelai 400 ml ditambah minyak kelapa 10 ml; kentang 500 g setara ubi kuning kecil 500 g dengan tambah rebus 5 menit.
- BR-104 Porsi dasar 4 jiwa (2 dewasa dan 2 anak) dikunci: tumis kering 600 g matang, lauk hewani 400 g mentah, tahu 400 g atau tempe 300 g atau telur 4 butir, beras 360 g mentah, sambal 80 g per makan.
- BR-105 Faktor skala 1 sampai 8 jiwa dikunci: 1 jiwa 0,30; 2 jiwa 0,55; 3 jiwa 0,80; 4 jiwa 1,00; 5 jiwa 1,25; 6 jiwa 1,50; 8 jiwa 2,00. Bumbu memakai faktor bumbu yang lebih kecil. Di atas 6 jiwa tumis wajib 2 batch.
- BR-106 Aturan pembulatan belanja dikunci: sayur ke kelipatan 50 g, telur ke butir genap, santan ke kelipatan kemasan 200 ml. Stok G2-S layu cepat tidak boleh diskala untuk esok; sisanya dialihkan ke resep sapu stok.
- BR-107 Takaran gram tiap resep adalah takaran dasar 4 porsi; fungsi `PorsiScaler.skala` wajib memakai tabel faktor dan aturan pembulatan di atas.

## Metrik

- Evaluasi bank 12/12 lolos tiap rilis. Kecukupan 550 sampai 700 kkal per jiwa per makan berat. Selisih skor paritas Kotlin lawan TypeScript di bawah 0,01.

## Batasan

Batasan segmen ini: hanya bank resep, substitusi, dan standar porsi. Di luar batas: skor rekomendasi milik PRD alur, implementasi engine milik FSD, dan harga pasar milik Lumbung. Daftar harga Lumbung G2 per 100 g (wortel 1.200, kentang 1.500, tomat 1.800, kangkung 1.000, cabai 4.000) adalah acuan belanja, bukan isi bank ini.
