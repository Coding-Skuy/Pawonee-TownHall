# Kontrak API KMP Mobile Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Lingkup: antarmuka Kotlin yang dipakai aplikasi mobile KMP + Compose Multiplatform (Android dan iOS). Backend REST di `https://api.pawonee.chefgenie.id/v1`. Versi kontrak: `v1`.

## 1. Prinsip

1. Mobile bekerja luring dulu: `RekomendasiEngine` dan `PorsiScaler` berjalan on-device dari modul `pantry-resep`. API jarak jauh hanya untuk sinkronisasi stok, resep, dan profil.
2. Semua berat dalam gram (bilangan bulat), uang dalam rupiah (bilangan bulat), waktu dalam menit.
3. Kesalahan memakai hasil `Hasil<T>` tertutup: `Berhasil(data)`, `Gagal(kode, pesan)`. Kode baku: `STOK_KOSONG`, `PROFIL_TIDAK_ADA`, `JARINGAN_GAGAL`, `RESEP_TIDAK_ADA`, `JIWA_DI_LUAR_RENTANG`.

## 2. Model data (Kotlin)

```kotlin
data class StokItem(val namaBahan: String, val gram: Int, val kedaluwarsaHari: Int, val kodeGrade: String)
// kodeGrade: "G2-V" | "G2-U" | "G2-S" | "NON-G2"

data class BahanResep(val namaBahan: String, val gramDasar4Porsi: Int, val kodeGrade: String, val opsional: Boolean)

data class Resep(
  val id: String, // "R-01".."R-24"
  val nama: String,
  val kelompok: String, // "kuah" | "tumis" | "sambal" | "lauk" | "kukus"
  val waktuMenit: Int,
  val kisaranBiayaRp: IntRange,
  val bahan: List<BahanResep>,
  val langkah: List<String>,
  val alergen: List<String>,
  val alatWajib: List<String>,
)

data class ProfilKeluarga(
  val idKeluarga: String, val jumlahJiwa: Int,
  val alergi: List<String>, val pantangan: List<String>,
  val budgetPerMakan: Int, val levelPedas: Int,
  val alatMasak: List<String>,
)

data class Rekomendasi(
  val resep: Resep,
  val skorTotal: Double,
  val alasan: String,
  val bahanTerskala: List<BahanTerskala>,
  val belanjaTambahan: List<BelanjaItem>,
)

data class BahanTerskala(val namaBahan: String, val gram: Int)
data class BelanjaItem(val namaBahan: String, val gram: Int, val perkiraanRp: Int)
```

## 3. Antarmuka repositori (dipakai ViewModel)

```kotlin
interface PawoneeRepository {
  suspend fun rekomendasi(profil: ProfilKeluarga, stok: List<StokItem>, jiwa: Int, waktuMaksimalMenit: Int): Hasil<List<Rekomendasi>>
  suspend fun detailResep(id: String, jiwa: Int): Hasil<Rekomendasi>
  suspend fun simpanProfil(profil: ProfilKeluarga): Hasil<Unit>
  suspend fun sinkronStok(): Hasil<List<StokItem>> // tarik dari Pedaree inventori
  suspend fun sinkronBankResep(): Hasil<List<Resep>> // tarik 24 resep, cache 7 hari
}
```

Aturan: `jiwa` wajib 1–8, selain itu kembalikan `Gagal(JIWA_DI_LUAR_RENTANG, ...)`. `rekomendasi` selalu mengembalikan maksimal 5 item sesuai alur di `produk/10-alur-rekomendasi.md`. `detailResep` memakai `PorsiScaler` dari `resep/20-standar-porsi.md`.

## 4. Endpoint REST pendamping (Ktor client)

| Metode | Path | Badan / Query | Balikan |
|---|---|---|---|
| GET | `/resep` | — | `{ "data": Resep[24] }`, cache 7 hari |
| GET | `/resep/{id}?jiwa=4` | — | `{ "data": Rekomendasi }` (bahan terskala) |
| POST | `/rekomendasi` | `{ profil, stok, jiwa, waktuMaksimalMenit }` | `{ "data": Rekomendasi[≤5] }` (dipakai bila perangkat RAM < 2 GB) |
| GET | `/stok?idKeluarga=KEL-0001` | — | `{ "data": StokItem[] }` (sumber Pedaree inventori) |
| PUT | `/profil/{idKeluarga}` | `ProfilKeluarga` | `{ "data": { "tersimpan": true } }` |

Header: `Authorization: Bearer <token>`, `X-Kontrak-Versi: v1`, `Content-Type: application/json`. Batas 30 permintaan per menit per token; jawaban `429` berisi `retrySetelahDetik: 60`.

## 5. Contoh payload

Permintaan `POST /rekomendasi`:

```json
{
  "profil": { "idKeluarga": "KEL-0001", "jumlahJiwa": 4, "alergi": [], "pantangan": ["halal-selalu"], "budgetPerMakan": 35000, "levelPedas": 1, "alatMasak": ["kompor", "wajan", "panci", "kukusan"] },
  "stok": [
    { "namaBahan": "kentang mini", "gram": 600, "kedaluwarsaHari": 5, "kodeGrade": "G2-U" },
    { "namaBahan": "wortel bengkok", "gram": 400, "kedaluwarsaHari": 4, "kodeGrade": "G2-V" }
  ],
  "jiwa": 4,
  "waktuMaksimalMenit": 30
}
```

Balikan (ringkas, 1 dari 5):

```json
{ "data": [{ "resep": { "id": "R-02", "nama": "Sayur Sop Tomat Belang + Wortel", "waktuMenit": 30 }, "skorTotal": 74.5, "alasan": "Memakai 400 g wortel bengkok dari stokmu.", "bahanTerskala": [{ "namaBahan": "wortel", "gram": 250 }], "belanjaTambahan": [{ "namaBahan": "ayam", "gram": 250, "perkiraanRp": 9000 }] }] }
```

## 6. Kaitan ke modul lain

- Implementasi logika ada di `produk/30-modul-KMP-bersama.md` (modul `pantry-resep`).
- Navigasi ke layar yang memanggil tiap fungsi diatur di `platform/20-navigasi3.md`.
- Versi web dari kontrak yang sama ada di `produk/21-kontrak-api-web-bun.md` dengan bentuk JSON identik.
