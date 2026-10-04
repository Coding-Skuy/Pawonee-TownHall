# Alur Rekomendasi Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Tujuan: dari stok Pedaree + pasokan Lumbung grade-2 menjadi 5 rekomendasi resep yang bisa dimasak hari ini.

## 1. Masukan

1. Stok Pedaree: daftar `StokItem { namaBahan, gram, tanggalKedaluwarsa, kodeGrade }` dari inventori rumah tangga Pedaree. Contoh: wortel G2-V 400 g, kentang mini G2-U 600 g, kangkung G2-S 200 g, telur 6 butir.
2. Pasokan Lumbung: daftar bahan G2 yang tersedia di agregator hari ini beserta harga per 100 g.
3. Profil keluarga: lihat `preferensi/10-model-keluarga.md`.
4. Parameter masak: `jiwa` (1–8) dan `waktuMaksimalMenit` (bawaan 30).

## 2. Lima tahap (urutan baku)

```
[1 FILTER KERAS] → [2 SKOR SERAPAN] → [3 SKOR KEMUDAHAN] → [4 RANKING + DIVERSIFIKASI] → [5 PENJELASAN + DAFTAR BELANJA]
```

### Tahap 1 — Filter keras (menghilangkan, bukan menurunkan skor)

- Buang resep yang mengandung alergen profil.
- Buang resep yang melanggar pantangan mutlak (`halal-selalu`, `vegetarian-nabati` bila tidak ada varian).
- Buang resep yang butuh alat yang tidak dimiliki.
- Buang resep dengan `waktuMenit > waktuMaksimalMenit`.

### Tahap 2 — Skor serapan grade-2 (bobot 50)

```
serapan = (gramG2Terpakai / gramG2TersediaDiStok) × 50
```

`gramG2Terpakai` adalah jumlah bahan G2 pada resep yang sudah dimiliki di stok (setelah diskala ke `jiwa`). Bonus +5 bila resep memakai bahan G2-S yang kedaluwarsa ≤ 2 hari (prioritas layu cepat). Maksimal 55, lalu dinormalkan ke 50.

### Tahap 3 — Skor kemudahan (bobot 50)

- Kecocokan stok 25 poin: `(bahanDimiliki / totalBahan) × 25`.
- Kecocokan budget 15 poin: 15 bila belanja tambahan ≤ 60% budget, 8 bila 60–100%, 0 bila melebihi budget (resep sebenarnya sudah dibuang bila melebihi 100%, jadi 0 tidak terjadi — aturan dipertahankan untuk audit).
- Kecocokan waktu 10 poin: 10 bila waktu ≤ 15 menit, 7 bila 16–30 menit, 4 bila 31–45 menit.

### Tahap 4 — Ranking dan diversifikasi

1. Jumlahkan skor (maksimal 100). Urutkan menurun.
2. Diversifikasi: 5 besar tidak boleh berisi lebih dari 2 resep sekelompok (misalnya maksimal 2 tumis). Bila pelanggaran, resep peringkat bawah digeser dengan resep sekelompok lain terbaik berikutnya.
3. Seri skor: menangkan yang `waktuMenit` lebih kecil, lalu yang `belanjaTambahanRp` lebih kecil.

### Tahap 5 — Penjelasan dan daftar belanja

Setiap rekomendasi mengembalikan: `skorTotal`, `alasan` (2 kalimat: bahan apa terserap + mengapa mudah), dan `belanjaTambahan` (bahan, gram, perkiraan harga Lumbung). Contoh alasan: "Memakai 600 g kentang mini dan 250 g wortel bengkok dari stokmu sehingga serapan 82%. Hanya perlu beli santan Rp6.000 dan waktu 35 menit."

## 3. Contoh ujung ke ujung

Stok: kentang mini 600 g, wortel bengkok 400 g, santan 0 ml. Profil Budi (4 jiwa, budget 35.000, alat lengkap tanpa oven).

- R-01 butuh kentang 500 g + wortel 250 g → serapan (750/1000)×50 = 37,5. Stok cocok 4/6 bahan = 16,7. Budget: beli santan 400 ml Rp6.000 → 15. Waktu 35 menit → 4. Total 73,2 → peringkat 1.
- R-06 butuh kangkung (tidak ada di stok) → serapan 0 → tersingkir dari 5 besar walau waktunya 12 menit.
- R-16 butuh kentang 600 g → serapan 30, cocok stok 3/4 → 18,75, budget 15, waktu 30 → 7. Total 70,75 → peringkat 2.

## 4. Kinerja dan batas

- KMP (on-device): 24 resep dinilai dalam ≤ 800 milidetik pada HP Android RAM 3 GB; tanpa internet setelah data stok dan resep tersinkron.
- Web (Bun): respons `GET /api/v1/rekomendasi` ≤ 400 milidetik pada 95 persen permintaan (diukur server).
- Bila stok kosong: kembalikan 5 resep termurah (R-08, R-09, R-19, R-07, R-13) dengan penanda `modeTanpaStok: true`.

## 5. Kaitan ke modul lain

- Fungsi `RekomendasiEngine.rekomendasikan()` di `produk/30-modul-KMP-bersama.md` adalah implementasi rujukan tahap 1–4.
- API mobile dan web hanya membungkus mesin yang sama; kontraknya di `produk/20-kontrak-api-KMP-mobile.md` dan `produk/21-kontrak-api-web-bun.md`.
- Serapan yang dihasilkan alur ini diukur agregatnya di `metrik/10-serapan-waste.md`.
