> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# BRD 20 — Preferensi dan Model Keluarga

## Konteks

Satu profil keluarga mengendalikan filter alergi, pantangan, budget, dan peralatan sehingga rekomendasi selalu aman dan bisa dimasak. Sumber isi lama: `preferensi/10-model-keluarga.md`, dipindah dengan `git mv` ke file ini.

## Kebutuhan Bisnis

- BR-201 Profil wajib memuat: `idKeluarga`, `jumlahJiwa` 1 sampai 8, komposisi dewasa, anak, balita, lansia, daftar alergi, daftar pantangan, `budgetPerMakan` rupiah, `levelPedas` 0 sampai 3, daftar `alatMasak`, dan `sumberBelanja` (`pedaree-stok`, `lumbung-g2`, atau `campuran`).
- BR-202 Nilai bawaan keluarga baru dikunci: 4 jiwa (2 dewasa dan 2 anak), budget Rp35.000, pedas 1, alat kompor, wajan, dan panci, sumber campuran.
- BR-203 Daftar alergi resmi: `udang`, `telur`, `kacang-tanah`, `kedelai`, `susu-sapi`, `ikan`, `gluten`. Resep yang mengandung alergen langsung disaring keluar, bukan diturunkan skornya.
- BR-204 Daftar pantangan resmi: `halal-selalu` (menyaring mutlak), `vegetarian-nabati`, `tanpa-santan`, `rendah-garam`, `rendah-gula`, `lunak-lansia`. Daftar alat resmi: `kompor`, `wajan`, `panci`, `kukusan`, `blender`, `oven`, `air-fryer`.
- BR-205 Aturan pakai dikunci: filter keras dulu (alergi, pantangan mutlak, alat), lalu skor. Belanja tambahan tidak boleh melebihi budget. Resep bercabai di atas 50 g per 4 porsi hanya tampil bila `levelPedas` minimal 2. Perubahan profil langsung membatalkan rekomendasi tersimpan dan menghitung ulang.
- BR-206 Penyimpanan dikunci: mobile KMP memakai DataStore `profil_keluarga.json` (1 aktif dan maksimal 4 cadangan); web memakai tabel `profil_keluarga` di DB `pawonee` dengan kolom array teks.

## Metrik

- Nol rekomendasi alergen lolos filter pada uji 32 kasus. Hitung ulang profil dalam 1 detik di KMP dan 1 respons API di web.

## Batasan

Batasan segmen ini: hanya model profil dan aturan pakainya. Di luar batas: algoritma skor milik PRD alur, skema API milik FSD kontrak, dan tampilan editor milik aplikasi. Balita dihitung 0,6 jiwa untuk sayur; lauk pedas dipisah sebelum dicampur cabai.
