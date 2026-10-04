# Standar Porsi Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Acuan: porsi dasar 4 jiwa (2 dewasa + 2 anak). Semua angka gram adalah berat bersih siap masak (setelah kupas/cuci).

## 1. Porsi dasar 4 jiwa (sekali makan)

| Kelompok | Berat total siap masak | Contoh |
|---|---|---|
| Sayur berkuah (isi + kuah) | Isi 800 g + kuah 800 ml | R-01, R-02, R-03 |
| Tumis/oseng kering | 600 g matang | R-06–R-10 |
| Lauk protein hewani | 400 g mentah (jadi ±320 g matang) | ayam pada R-04, R-10 |
| Lauk nabati (tahu/tempe/telur) | Tahu 400 g / tempe 300 g / telur 4 butir | R-07, R-19, R-20 |
| Karbohidrat pokok | Beras 360 g mentah (jadi ±900 g nasi) atau kentang 600 g | pendamping semua resep |
| Sambal | 80 g per makan (20 g per jiwa) | R-11, R-12 |
| Buah/jus | 600 ml jus atau 400 g buah potong | R-15 |

Kecukupan: standar ini memenuhi 550–700 kkal per jiwa per makan berat dengan protein 18–25 g bila memakai lauk hewani, atau 12–16 g bila nabati penuh.

## 2. Faktor skala 1–8 jiwa

Kalikan berat dasar 4 jiwa dengan faktor berikut. Bumbu (bawang, garam, gula, minyak) memakai faktor bumbu yang lebih kecil agar tidak keasinan.

| Jiwa | Faktor bahan | Faktor bumbu | Faktor air/kuah | Contoh: kentang R-16 (600 g dasar) |
|---|---|---|---|---|
| 1 | 0,30 | 0,35 | 0,35 | 180 g |
| 2 | 0,55 | 0,60 | 0,60 | 330 g |
| 3 | 0,80 | 0,80 | 0,85 | 480 g |
| 4 | 1,00 | 1,00 | 1,00 | 600 g |
| 5 | 1,25 | 1,15 | 1,20 | 750 g |
| 6 | 1,50 | 1,30 | 1,40 | 900 g |
| 8 | 2,00 | 1,60 | 1,80 | 1.200 g |

Batas alat: di atas 6 jiwa, masak dalam 2 batch untuk tumis (R-06–R-10) agar sayur tidak berair. Kuah (R-01–R-05) boleh 1 panci maksimal 5 liter; di atas itu bagi 2 panci.

## 3. Aturan pembulatan belanja

- Sayur: bulatkan ke atas ke kelipatan 50 g (contoh: hitungan 480 g menjadi 500 g).
- Telur: bulatkan ke atas ke butir genap (contoh: 3,2 butir menjadi 4 butir).
- Bumbu: garam 4 g, gula 6 g, minyak 12 ml per porsi dasar 4 jiwa; skala dengan faktor bumbu.
- Santan: kemasan 200 ml; bulatkan ke kelipatan 200 ml (contoh: kebutuhan 300 ml menjadi 400 ml, sisa 100 ml untuk R-19 keesokan hari).

## 4. Penyesuaian khusus

1. Balita (1–4 tahun) dihitung 0,6 jiwa untuk sayur dan 0,5 jiwa untuk sambal/lauk pedas; lauk pedas dipisah sebelum dicampur cabai.
2. Lansia dengan pantangan lunak: pilih R-21, R-22, R-23; potong bahan 1 cm dan tambah waktu rebus 5 menit tanpa mengubah faktor.
3. Bekal sekolah: tambah 0,15 faktor karbohidrat per anak bekal (nasi lebih banyak 50 g per anak).
4. Stok G2-S layu cepat (kangkung, bayam, tauge) tidak boleh diskala untuk besok; masak hari ini walau faktor dibulatkan ke bawah, sisanya alihkan ke R-24 capcay sapu stok.

## 5. Contoh hitung

Keluarga 5 jiwa memasak R-01: kentang 500 g × 1,25 = 625 g dibulatkan 650 g; wortel 250 g × 1,25 = 313 g dibulatkan 350 g; santan 400 ml × 1,20 = 480 ml dibulatkan 600 ml (3 kemasan 200 ml, sisa 120 ml untuk esok). Bumbu kuning: bawang merah 60 g × 1,15 = 69 g dibulatkan 70 g.

## 6. Kaitan ke modul lain

- Fungsi `PorsiScaler.skala(resep, jiwa)` di modul `pantry-resep` (`produk/30-modul-KMP-bersama.md`) wajib memakai tabel faktor pada bagian 2 dan aturan pembulatan bagian 3.
- API mobile dan web menerima parameter `jiwa` (1–8) dan mengembalikan `bahanTerskala` sesuai standar ini; lihat `produk/20-kontrak-api-KMP-mobile.md` dan `produk/21-kontrak-api-web-bun.md`.
