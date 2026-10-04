# Modul KMP Bersama pantry-resep — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Modul `pantry-resep` adalah kode bersama antara Pawonee dan Pedaree: Pedaree memakai sisi stok, Pawonee memakai sisi stok + resep + rekomendasi.

## 1. Koordinat modul

- Nama Gradle: `:shared:pantry-resep`
- Package Kotlin: `id.chefgenie.pawonee.pantryresep`
- Artefak: `id.chefgenie.pawonee:pantry-resep:1.0.0`
- Target KMP: `androidTarget`, `iosX64`, `iosArm64`, `iosSimulatorArm64`, `jvm` (untuk uji dan backend).
- Dependensi: `ktor-client-core 2.3.12`, `kotlinx-serialization-json 1.7.3`, `kotlinx-datetime 0.6.1`, `datastore-preferences 1.1.1` (hanya di app, bukan di modul).

Struktur:

```
shared/pantry-resep/
  src/commonMain/kotlin/id/chefgenie/pawonee/pantryresep/
    model/StokItem.kt, Resep.kt, ProfilKeluarga.kt, Rekomendasi.kt
    engine/RekomendasiEngine.kt, PorsiScaler.kt, Substitusi.kt
    data/ResepBawaan.kt (24 resep), HargaLumbung.kt
    repo/PawoneeRepository.kt, Sinkronisasi.kt
  src/commonTest/kotlin/.../RekomendasiEngineTest.kt (24 kasus)
```

## 2. Batas dengan Pedaree

| Milik Pedaree (dipakai ulang, tidak diduplikasi) | Milik Pawonee (baru di modul ini) |
|---|---|
| `StokItem`, baca/tulis inventori, satuan gram, kode grade | `Resep`, `Rekomendasi`, `ProfilKeluarga` |
| API `GET /stok`, kamera catat stok | `RekomendasiEngine`, `PorsiScaler`, `ResepBawaan` 24 resep |

Aturan impor: Pawonee mengimpor `StokItem` dari Pedaree (`id.chefgenie.pedaree.inventori.StokItem`) melalui alias tipe, bukan mendefinisikan ulang. Bila Pedaree mengubah skema stok, Pawonee mengikuti dalam 1 rilis minor.

## 3. API publik modul (Kotlin)

```kotlin
object RekomendasiEngine {
  fun rekomendasikan(profil: ProfilKeluarga, stok: List<StokItem>, jiwa: Int, waktuMaksimalMenit: Int): List<Rekomendasi>
}

object PorsiScaler {
  fun skala(bahan: List<BahanResep>, jiwa: Int): List<BahanTerskala> // tabel faktor pada resep/20-standar-porsi.md
}

object Substitusi {
  fun ganti(bahanHilang: String): List<String> // tabel substitusi pada resep/10-bank-resep-grade2.md
}

val RESEP_BAWAAN: List<Resep> // 24 resep, dimuat dari ResepBawaan.kt, tanpa jaringan
val HARGA_LUMBUNG: Map<String, Int> // rupiah per 100 g: wortel 1200, kentang 1500, tomat 1800, kangkung 1000, cabai 4000, santan 3000/200ml
```

Semua fungsi murni (tanpa I/O, tanpa jam sistem) sehingga hasilnya identik di Android, iOS, dan uji JVM. Satu-satunya pengecualian: bonus kedaluwarsa memakai `kedaluwarsaHari` dari data, bukan tanggal hari ini.

## 4. Uji wajib (24 + 8 kasus)

- 24 kasus: satu per resep — stok tepat bahan resep menghasilkan resep itu di peringkat 1.
- 8 kasus tepi: alergi menyaring, alat hilang menyaring, waktu melebihi menyaring, stok kosong mengembalikan mode hemat, jiwa 0 dan 9 ditolak, bonus G2-S layu +5, diversifikasi maksimal 2 sekelompok, seri dimenangkan waktu terkecil.
- Perintah: `./gradlew :shared:pantry-resep:allTests` harus hijau di Android, iOS simulator, dan JVM sebelum rilis.

## 5. Paritas dengan web

Port TypeScript `src/lib/server/rekomendasi.ts` (Bun/SvelteKit) wajib menghasilkan skor yang sama persis (selisih < 0,01) untuk 32 kasus uji di atas. Berkas uji bersama: `shared/pantry-resep/src/commonTest/fixtures/kasus-uji.json` dipakai oleh `vitest` di web.

## 6. Kaitan ke modul lain

- Algoritma mengikuti `produk/10-alur-rekomendasi.md`; porsi mengikuti `resep/20-standar-porsi.md`.
- Konsumen mobile di `platform/40-mobile-KMP.md`; konsumen web di `platform/50-web-bun-svelte.md`; pembagian kerja di `platform/10-matriks-KMP-web.md`.
