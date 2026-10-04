# Mobile KMP Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Aplikasi utama Pawonee. Kotlin Multiplatform + Compose Multiplatform + Navigation3 untuk Android dan iOS dari satu basis kode.

## 1. Versi kunci

- Kotlin 2.1.20, Compose Multiplatform 1.7.3, Navigation3 1.0.0, Ktor Client 2.3.12, DataStore Preferences 1.1.1, WorkManager 2.9.0 (Android) / BGTaskScheduler (iOS via expect-actual).
- Android: `minSdk 26`, `targetSdk 35`, ukuran APK ≤ 45 MB.
- iOS: iOS 16 ke atas, SwiftUI hanya sebagai pembungkus `ComposeUIViewController`, tanpa logika di Swift.

## 2. Struktur kode aplikasi

```
composeApp/
  src/commonMain/kotlin/id/chefgenie/pawonee/
    App.kt (NavDisplay, tema)
    layar/Beranda.kt, Stok.kt, Rekomendasi.kt, ResepDetail.kt, Preferensi.kt, Riwayat.kt
    vm/BerandaVm.kt, RekomendasiVm.kt, ResepDetailVm.kt (StateFlow + PawoneeRepository)
    platform/Notifikasi.kt (expect), SinkronPekerja.kt (expect)
  src/androidMain/... (DataStore, WorkManager harian, notifikasi layu)
  src/iosMain/... (BGTask, UserNotifications)
shared/pantry-resep/ (modul bersama, lihat produk/30-modul-KMP-bersama.md)
```

## 3. Layar dan perilaku

1. **Beranda**: kartu "Masak hari ini" (3 rekomendasi teratas), strip "Segera layu" (stok G2-S ≤ 2 hari), tombol ke stok dan preferensi. Muat < 1 detik dari cache.
2. **Stok**: daftar dari Pedaree (baca saja) + tombol "Sinkron" + penanda umur stok (hijau > 3 hari, kuning 2–3 hari, merah ≤ 1 hari).
3. **Rekomendasi**: filter `waktuMaksimalMenit` (cip 15/30/45) + pemilih `jiwa` (stepper 1–8); daftar 5 kartu berisi skor, alasan, dan belanja tambahan.
4. **ResepDetail**: bahan terskala sesuai `jiwa`, langkah bernomor, tombol "Tandai dimasak" (menulis riwayat + mengurangi stok lokal), tombol bagikan deep link.
5. **Preferensi**: editor profil keluarga (lihat `preferensi/10-model-keluarga.md`); simpan memicu hitung ulang.
6. **Riwayat**: 30 masakan terakhir; tiap baris menampilkan gram G2 terserap.

## 4. Luring, sinkron, notifikasi

- Cache: resep 7 hari, stok 24 jam, profil selamanya (DataStore). Rekomendasi dihitung luring selalu.
- Pekerja harian 07:00: sinkron stok Pedaree, hitung bonus layu, kirim notifikasi bila ada G2-S ≤ 1 hari ("Kangkung layu besok — lihat R-06 12 menit").
- Izin: notifikasi opt-in; tanpa pelacakan lokasi; analitik hanya kejadian `serapan_dihitung` dan `resep_dimasak` (tanpa nama bahan rinci di luar perangkat bila pengguna menolak).

## 5. Kinerja dan rilis

- Rekomendasi on-device ≤ 800 ms (HP RAM 3 GB); buka `resepDetail` ≤ 300 ms; `allTests` hijau sebelum beta.
- Rilis: internal (10 penguji) → beta 500 keluarga (2 minggu) → produksi. Kriteria naik tahap: crash-free 99,5%, 80% rekomendasi dibuka, serapan rata-rata ≥ 300 g G2 per keluarga per minggu.

## 6. Kaitan ke modul lain

- Navigasi di `platform/20-navigasi3.md`; logika di `produk/30-modul-KMP-bersama.md`; metrik di `metrik/10-serapan-waste.md`.
