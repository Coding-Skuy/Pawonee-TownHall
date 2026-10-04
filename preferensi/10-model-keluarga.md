# Model Keluarga Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Satu profil keluarga mengendalikan filter alergi, pantangan, budget, dan peralatan sehingga rekomendasi resep selalu aman dan bisa dimasak.

## 1. Struktur profil

```ts
type ProfilKeluarga = {
  idKeluarga: string;        // contoh: "KEL-0042"
  jumlahJiwa: number;        // 1–8, mengikuti resep/20-standar-porsi.md
  komposisi: { dewasa: number; anak: number; balita: number; lansia: number };
  alergi: Alergi[];          // daftar pasti, bukan tebakan
  pantangan: Pantangan[];    // agama, diet, medis
  budgetPerMakan: number;    // rupiah untuk 1 kali makan seluruh keluarga
  levelPedas: 0 | 1 | 2 | 3; // 0 tidak pedas, 3 sangat pedas
  alatMasak: AlatMasak[];    // yang benar-benar dimiliki
  sumberBelanja: "pedaree-stok" | "lumbung-g2" | "campuran";
};
```

Nilai awal bawaan untuk keluarga baru: `jumlahJiwa 4 (2 dewasa + 2 anak)`, `budgetPerMakan 35000`, `levelPedas 1`, `alatMasak [kompor, wajan, panci]`, `sumberBelanja campuran`.

## 2. Daftar nilai resmi

- Alergi: `udang`, `telur`, `kacang-tanah`, `kedelai`, `susu-sapi`, `ikan`, `gluten`. Pilihan ganda. Resep yang mengandung alergen langsung disaring keluar, bukan sekadar diturunkan skornya.
- Pantangan: `halal-selalu` (menyaring semua bahan non-halal secara mutlak), `vegetarian-nabati`, `tanpa-santan`, `rendah-garam`, `rendah-gula`, `lunak-lansia`.
- AlatMasak: `kompor`, `wajan`, `panci`, `kukusan`, `blender`, `oven`, `air-fryer`. R-14 membutuhkan `oven` atau penjemur; R-18 membutuhkan `kukusan`; bila alat tidak dimiliki, resep disembunyikan.

## 3. Contoh 4 profil baku

1. **Keluarga Budi (KEL-0001)**: 4 jiwa (2+2), alergi tidak ada, pantangan `halal-selalu`, budget Rp35.000, pedas 1, alat kompor-wajan-panci-kukusan, sumber campuran.
2. **Keluarga Sari balita (KEL-0002)**: 4 jiwa (2 dewasa + 1 anak + 1 balita), alergi `telur`, pantangan `halal-selalu` + `lunak-lansia` tidak, budget Rp30.000, pedas 0, alat kompor-wajan-panci-blender. R-22 menjadi prioritas, R-11/R-12 disaring keluar untuk porsi balita (dimasak terpisah).
3. **Keluarga vegetarian (KEL-0003)**: 3 jiwa, alergi tidak ada, pantangan `vegetarian-nabati`, budget Rp28.000, pedas 2. Semua resep berayam difilter ke varian tahu/tempe sesuai tabel substitusi bank resep.
4. **Warung Tegal mitra (KEL-0004)**: 8 jiwa setara, alergi tidak ada, pantangan `halal-selalu`, budget Rp90.000, pedas 3, alat lengkap termasuk oven. Faktor skala 8 dipakai; tumis wajib 2 batch.

## 4. Aturan pakai dalam rekomendasi

1. Filter keras dulu: alergi, pantangan `halal-selalu` dan `vegetarian-nabati`, alat masak. Resep lolos filter baru diberi skor.
2. Budget: total belanja tambahan (bahan yang tidak ada di stok Pedaree) tidak boleh melebihi `budgetPerMakan`. Harga acuan: harga Lumbung G2 per 100 g (wortel 1.200, kentang 1.500, tomat 1.800, kangkung 1.000, cabai 4.000 — rupiah).
3. Level pedas: resep dengan cabai di atas 50 g per 4 porsi hanya tampil bila `levelPedas >= 2`; R-11/R-12 hanya tampil bila `levelPedas >= 1`.
4. Perubahan profil (misalnya tambah alergi) langsung membatalkan rekomendasi tersimpan dan menghitung ulang dalam 1 detik (di KMP) atau 1 respons API (di web).

## 5. Penyimpanan

- Mobile KMP: tersimpan di DataStore `profil_keluarga.json`, 1 profil aktif + maksimal 4 profil cadangan (misalnya rumah ibu).
- Web Bun+Svelte: tersimpan di tabel `profil_keluarga` (Postgres), kolom `alergi` dan `pantangan` bertipe teks array.

## 6. Kaitan ke modul lain

- Alur rekomendasi memakai profil ini pada tahap filter; lihat `produk/10-alur-rekomendasi.md`.
- API menerima `idKeluarga` atau objek profil penuh; lihat kontrak API mobile dan web.
