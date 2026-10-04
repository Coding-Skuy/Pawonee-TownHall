> Versi: v1.0.0 | Status: disetujui | Menggantikan: -

# SRS Glossary

Pawonee mengelola AI cooking assistant yang serap hortikultura grade-2 menjadi resep, dengan 12 resep bank.

## Terminology

- **TDD (Test-Driven Development):** Requirement → Red Test → Green Code → Refactor.
- **Acceptance Criteria:** Kondisi konkret yang harus dipenuhi feature untuk diterima QA.
- **QA Gate:** Barrier otomatis (test, coverage, review) sebelum merge/release.
- **DB per Service:** Database logis terpisah per divisi; satu cluster Postgres untuk pilot.
- **JWT Audiens:** Token JWT memiliki audiens ('aud') per service untuk validasi cross-service.

## Batasan

Glossary hanya referensi; definisi formal ada di spec teknis masing-masing.