> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# Platform ~ Aplikasi (Pawonee)

Folder ini adalah **indeks aplikasi** divisi Pawonee: tujuh repo kode yang hidup di
organisasi Coding-Skuy ([tim Pawonee](https://github.com/orgs/Coding-Skuy/teams/pawonee/repositories)),
ditautkan ke repo ini sebagai **git submodule**.

- `platform/versions/` — dokumen formal beku (BRD/PRD/FRD/FSD/SRS).
- `platform/apps/` — kode aplikasi (submodule, repo terpisah).

## Repo aplikasi

| Submodule | Repo | Peran | Stack |
|---|---|---|---|
| `app-kmp/` | [pawonee-app-kmp](https://github.com/Coding-Skuy/pawonee-app-kmp) | Mobile beta (rekomendasi resep luring) | Kotlin MPP / CMP |
| `web/` | [pawonee-web](https://github.com/Coding-Skuy/pawonee-web) | Web pendamping (paritas skor) | Bun + SvelteKit + TS |
| `backend-service/` | [pawonee-backend-service](https://github.com/Coding-Skuy/pawonee-backend-service) | Serving rekomendasi di atas DB pawonee | Rust |
| `ai-models/` | [pawonee-ai-models](https://github.com/Coding-Skuy/pawonee-ai-models) | Bank resep `:shared:pantry-resep` (evaluasi 12/12) | Python (training) + Rust (serving) |
| `data-pipeline/` | [pawonee-data-pipeline](https://github.com/Coding-Skuy/pawonee-data-pipeline) | Penerbit `sinyal.pedaree.v1` tiap 15 menit | — |
| `design/` | [pawonee-design](https://github.com/Coding-Skuy/pawonee-design) | Desain Pawonee | Figma |
| `infra-devops/` | [pawonee-infra-devops](https://github.com/Coding-Skuy/pawonee-infra-devops) | Infra & deploy Pawonee | CI/CD |

## Cara pakai

```bash
# clone beserta submodule
git clone --recurse-submodules https://github.com/Coding-Skuy/Pawonee-TownHall.git
# bila repo sudah ter-clone tanpa submodule:
git submodule update --init --recursive
# perbarui ke commit main terbaru tiap repo app:
git submodule update --remote
```

## Batasan

Indeks ini hanya menautkan repo. Kode dan riwayatnya tetap milik repo app masing-masing;
keputusan versi di tulis di `platform/versions/vX.Y.Z/`, bukan di sini.