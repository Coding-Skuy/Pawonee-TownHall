# Navigasi Navigation3 Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Pustaka: Navigation3 (`androidx.navigation3:navigation3-runtime:1.0.0` + `navigation3-ui`) di atas Compose Multiplatform. Satu graf, dua platform (Android + iOS).

## 1. Destinasi (6 layar)

| Rute | Parameter | Sumber data | Aksi keluar |
|---|---|---|---|
| `beranda` | — | 5 rekomendasi teratas + stok menipis | ke `stok`, `rekomendasi`, `resepDetail(id)`, `preferensi` |
| `stok` | — | `sinkronStok()` Pedaree | kembali; edit stok membuka editor Pedaree |
| `rekomendasi` | `jiwa`, `waktuMaksimalMenit` | `rekomendasi(...)` | ke `resepDetail(id)` |
| `resepDetail` | `id` (R-01–R-24), `jiwa` | `detailResep(id, jiwa)` | ke `riwayat` (tandai dimasak) |
| `preferensi` | `idKeluarga` | `simpanProfil(...)` | kembali + hitung ulang rekomendasi |
| `riwayat` | — | log masak lokal | ke `resepDetail(id)` |

Deep link: `pawonee://resep/{id}?jiwa=4` membuka `resepDetail` langsung; dipakai dari notifikasi layu cepat dan dari web pendamping (QR).

## 2. Graf dan back stack

```
beranda ─┬─ stok
         ├─ rekomendasi ── resepDetail ── riwayat
         ├─ resepDetail (dari kartu beranda)
         └─ preferensi (dialog penuh, kembali menghitung ulang)
```

Aturan back: dari `resepDetail` kembali ke pemanggil (`beranda` atau `rekomendasi`), bukan selalu ke `beranda`. Dari `preferensi` yang dibuka dari `rekomendasi`, kembali menghitung ulang daftar sebelum pop.

## 3. Kode rujukan (Compose)

```kotlin
@Composable
fun PawoneeNav(pawoneeRepo: PawoneeRepository) {
  val navigator = rememberNavBackStack(Beranda)
  NavDisplay(
    backStack = navigator.backStack,
    onBack = { navigator.removeLast() },
    entryProvider = entryProvider {
      entry<Beranda> { BerandaLayar(keStok = { navigator.add(Stok) }, keResep = { id -> navigator.add(ResepDetail(id, jiwa = 4)) }) }
      entry<Stok> { StokLayar() }
      entry<Rekomendasi> { RekomendasiLayar(repo = pawoneeRepo, keResep = { id -> navigator.add(ResepDetail(id, jiwa = 4)) }) }
      entry<ResepDetail> { args -> ResepDetailLayar(id = args.id, jiwa = args.jiwa) }
      entry<Preferensi> { PreferensiLayar(padaSimpan = { navigator.removeLast() }) }
      entry<Riwayat> { RiwayatLayar() }
    }
  )
}
```

Rute dimodelkan sebagai `data object` / `data class` tertutup Kotlin (`Beranda`, `Stok`, `Rekomendasi(jiwa, waktu)`, `ResepDetail(id, jiwa)`, `Preferensi`, `Riwayat`), bukan string mentah.

## 4. Keadaan dan argumen

- `jiwa` dibawa sebagai argumen navigasi (bawaan dari profil) sehingga `resepDetail` selalu menampilkan takaran yang benar tanpa membaca ulang profil.
- Perubahan profil di `preferensi` disiarkan via `StateFlow`; `beranda` dan `rekomendasi` yang sedang terbuka menghitung ulang otomatis.
- Riwayat masak (`resepId`, `tanggal`, `jiwa`) disimpan lokal dan menjadi metrik serapan; lihat `metrik/10-serapan-waste.md`.

## 5. Kaitan ke modul lain

- Tiap layar memanggil tepat satu fungsi `PawoneeRepository` dari `produk/20-kontrak-api-KMP-mobile.md`.
- Implementasi platform (ViewModel, DataStore, worker) di `platform/40-mobile-KMP.md`.
