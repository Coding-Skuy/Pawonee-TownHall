# Kontrak API Web Bun Pawonee — Varian 1

> Status: disahkan Varian 1. Bahasa: Indonesia. Lingkup: backend pendamping Bun 1.4.x + SvelteKit 2 (TypeScript 5.9.x). Basis URL: `https://web.pawonee.chefgenie.id/api/v1`. Bentuk JSON identik dengan kontrak mobile.

## 1. Prinsip

1. Server Bun adalah bungkus tipis di atas logika yang sama dengan KMP: validasi, hitung rekomendasi, kembalikan JSON. Tidak ada logika resep ganda.
2. Rute SvelteKit ada di `src/routes/api/v1/` dan memanggil `src/lib/server/rekomendasi.ts` (port TypeScript dari `RekomendasiEngine`).
3. Autentikasi memakai Bearer token yang sama dengan mobile. CORS hanya mengizinkan `https://pawonee.chefgenie.id`.

## 2. Tabel endpoint

| Metode | Path SvelteKit | Fungsi | Cache |
|---|---|---|---|
| GET | `/api/v1/resep` | daftar 24 resep | `Cache-Control: public, max-age=604800` (7 hari) |
| GET | `/api/v1/resep/[id]?jiwa=4` | detail + bahan terskala | `max-age=86400` per `id`+`jiwa` |
| POST | `/api/v1/rekomendasi` | body `{ profil, stok, jiwa, waktuMaksimalMenit }` → `Rekomendasi[≤5]` | tanpa cache |
| GET | `/api/v1/stok?idKeluarga=KEL-0001` | proksi baca ke Pedaree inventori | `max-age=60` |
| PUT | `/api/v1/profil/[idKeluarga]` | simpan profil keluarga | tanpa cache |
| GET | `/api/v1/kesehatan` | `{ "status": "baik", "kontrakVersi": "v1" }` | tanpa cache |

Validasi memakai Zod: `jiwa` integer 1–8, `waktuMaksimalMenit` 10–45, `gram` ≥ 0, `kodeGrade` salah satu dari `G2-V, G2-U, G2-S, NON-G2`. Gagal validasi → `400 { "kode": "VALIDASI_GAGAL", "pesan": "...", "rinci": [...] }`.

## 3. Skema TypeScript (ringkas)

```ts
export type KodeGrade = "G2-V" | "G2-U" | "G2-S" | "NON-G2";
export interface StokItem { namaBahan: string; gram: number; kedaluwarsaHari: number; kodeGrade: KodeGrade; }
export interface Resep {
  id: string; nama: string;
  kelompok: "kuah" | "tumis" | "sambal" | "lauk" | "kukus";
  waktuMenit: number; kisaranBiayaRp: [number, number];
  bahan: { namaBahan: string; gramDasar4Porsi: number; kodeGrade: KodeGrade; opsional: boolean }[];
  langkah: string[]; alergen: string[]; alatWajib: string[];
}
export interface Rekomendasi {
  resep: Resep; skorTotal: number; alasan: string;
  bahanTerskala: { namaBahan: string; gram: number }[];
  belanjaTambahan: { namaBahan: string; gram: number; perkiraanRp: number }[];
}
```

## 4. Contoh

Permintaan:

```http
POST /api/v1/rekomendasi HTTP/1.1
Authorization: Bearer <token>
Content-Type: application/json

{
  "profil": { "idKeluarga": "KEL-0001", "jumlahJiwa": 4, "alergi": [], "pantangan": ["halal-selalu"], "budgetPerMakan": 35000, "levelPedas": 1, "alatMasak": ["kompor", "wajan", "panci"] },
  "stok": [{ "namaBahan": "kangkung", "gram": 400, "kedaluwarsaHari": 1, "kodeGrade": "G2-S" }],
  "jiwa": 4,
  "waktuMaksimalMenit": 30
}
```

Balikan `200`:

```json
{
  "data": [
    {
      "resep": { "id": "R-06", "nama": "Tumis Kangkung Surplus + Tauge", "kelompok": "tumis", "waktuMenit": 12, "kisaranBiayaRp": [18000, 25000], "bahan": [], "langkah": [], "alergen": [], "alatWajib": ["kompor", "wajan"] },
      "skorTotal": 88.0,
      "alasan": "Memakai 400 g kangkung surplus yang layu besok sehingga serapan 95%. Hanya perlu beli tauge Rp4.000 dan waktu 12 menit.",
      "bahanTerskala": [{ "namaBahan": "kangkung", "gram": 400 }],
      "belanjaTambahan": [{ "namaBahan": "tauge", "gram": 200, "perkiraanRp": 4000 }]
    }
  ]
}
```

Kesalahan baku: `400 VALIDASI_GAGAL`, `404 RESEP_TIDAK_ADA`, `429 BATAS_LAJU` (`{ "kode": "BATAS_LAJU", "retrySetelahDetik": 60 }`), `500 KESALAHAN_SERVER`.

## 5. Kaitan ke modul lain

- Port TypeScript wajib lolos uji paritas dengan KMP: 24 kasus uji skor identik; lihat `platform/10-matriks-KMP-web.md` dan `platform/50-web-bun-svelte.md`.
- Struktur `Resep` dan `Rekomendasi` sama persis dengan kontrak mobile di `produk/20-kontrak-api-KMP-mobile.md`.
