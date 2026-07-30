# CHANGELOG
## Platform Web Real Estate Agency

Semua perubahan penting pada proyek ini dicatat di file ini.

Format mengikuti prinsip [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), dan proyek ini mengikuti [Semantic Versioning](https://semver.org/) (`MAJOR.MINOR.PATCH`).

---

## Aturan Wajib Pengelolaan Dokumen Ini

1. **History tidak boleh dihapus.** Entri lama tidak pernah dihapus atau ditulis ulang isinya — koreksi atas entri lama ditambahkan sebagai entri baru yang merujuk balik ke entri yang dikoreksi, bukan mengedit entri asal.
2. **Selalu append perubahan baru** di bagian paling atas tiap seksi kronologis (entri terbaru di atas) — tidak pernah disisipkan di tengah riwayat.
3. **Gunakan Semantic Versioning** secara ketat:
   - `MAJOR` — breaking change pada kontrak API/skema data yang sudah live.
   - `MINOR` — fitur/modul baru yang backward-compatible.
   - `PATCH` — bug fix/perbaikan kecil yang backward-compatible.
   - Selama fase **Initial Development** (`0.y.z`), API publik dianggap belum stabil — kenaikan `y` (minor) dapat menyertakan perubahan yang bersifat lebih besar dari biasanya, sesuai ketentuan SemVer poin 4, namun tetap wajib dicatat sebagai **Breaking Changes** bila relevan.
4. **Catat seluruh perubahan database** — setiap migration baru (tabel, kolom, index, constraint, RLS policy) wajib punya entri di **Database Changes**, disinkronkan dengan `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`.
5. **Catat seluruh perubahan API** — setiap endpoint baru/diubah/deprecated wajib punya entri di **API Changes**, disinkronkan dengan `API-Specification-Real-Estate-Agency-Platform-v1.1.md`.
6. **Catat seluruh perubahan UI** — setiap halaman/komponen baru/diubah yang berdampak ke pengguna wajib punya entri di **UI Changes**.
7. **Catat bug fix** — setiap perbaikan bug, sekecil apa pun, wajib punya entri di **Bug Fixes** dengan referensi Task ID (`TASK-...`) dari `TASK-TEMPLATE.md` yang menanganinya.
8. Setiap entri versi baru **wajib** disertai tanggal (format `YYYY-MM-DD`) dan label fase pengembangan bila relevan (mis. "Initial Development", "Phase 1 MVP").
9. Jika sebuah kategori tidak punya perubahan pada suatu rilis, tulis eksplisit `Tidak ada perubahan pada kategori ini di rilis ini` — jangan menghilangkan sub-bagian kategori tsb.

---

# CURRENT VERSION

**`0.1.2`** — *Initial Development*
Dirilis: 2026-07-28
Fase: Pra-Development — governance sync: `architecture-decision-records.md` `ADR-005` (Search Strategy) diselesaikan Approved, `decision-log.md` disinkronkan (`ADR-039`), dan seluruh dokumen turunan (`technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `dependency-manifest.md`, `PROJECT-CONSTITUTION.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, `CURRENT-PROJECT-STATE.md`) disinkronkan mengikuti keputusan tsb. Implementasi kode masih belum dimulai (lihat `CURRENT-PROJECT-STATE.md`).

---

# RELEASE HISTORY

## [Unreleased]
Belum ada perubahan yang menunggu rilis berikutnya. Perubahan berikutnya yang direncanakan: Sprint S0 — Foundation Infrastructure (lihat **Next Planned Release**).

## [0.1.2] - 2026-07-28 - Initial Development (Governance Sync)

### Added
- `decision-log.md` `ADR-039` — entry baru "Search Strategy: PostgreSQL Full-Text Search + pg_trgm (Fase 1), Migrasi Terjadwal ke Typesense (Fase 2)", sinkronisasi dari `architecture-decision-records.md` `ADR-005` (Status: **Approved**, tanggal 2026-07-28, hasil sesi Architecture Review Board).
- `technology-decisions.md` §4.30 — Decision Detail baru untuk PostgreSQL Full-Text Search + `pg_trgm` sebagai Search Engine resmi Fase 1, termasuk kriteria ambang migrasi eksplisit ke Typesense Fase 2.
- `SYSTEM-ARCHITECTURE.md` — node **Search Engine** baru pada Component Diagram (Bagian 3), subseksi **Catatan Search Service** pada Service Layer (Bagian 11), dan alur khusus pencarian pada Data Flow (Bagian 7), seluruhnya mendokumentasikan implementasi PostgreSQL FTS + `pg_trgm`.
- `dependency-manifest.md` — baris **Search Engine (`pg_trgm`)** dan **`typesense` (Fase 2, belum diinstal)** pada Production Dependencies (Bagian 3), mendokumentasikan bahwa Fase 1 tidak menambah dependency npm.
- `PROJECT-CONSTITUTION.md` — baris #9 baru pada tabel "Riwayat Keputusan Arsitektur", prinsip arsitektur baru (Bagian 22 poin 8), dan technical constraint baru (Bagian 24 poin 6) terkait Search Strategy.
- `AI-DEVELOPMENT-BLUEPRINT.md` — Golden Rule baru (poin 35) dan aturan prompting baru (Bagian 21 poin 6) yang mewajibkan AI Coding Assistant mengasumsikan PostgreSQL FTS + `pg_trgm` sebagai konteks default untuk task pencarian listing.

### Changed
- **Keputusan Search Strategy dikunci final**: setelah sebelumnya dicatat sebagai bagian dari pertentangan terbuka antar dokumen governance (lihat **Known Issues #5**), `architecture-decision-records.md` `ADR-005` menetapkan strategi bertahap — **PostgreSQL Full-Text Search + `pg_trgm` untuk Fase 1** (MVP, tanpa komponen infrastruktur tambahan), dengan **migrasi terjadwal ke Typesense di Fase 2** begitu salah satu dari tiga kriteria ambang tercapai (volume listing aktif >±50.000, latensi p95 `/properties/search` >500ms, atau keluhan relevansi berulang ≥3 laporan/sprint) — sebagai keputusan **Approved**, dengan dua catatan kondisional Board: (1) proyeksi volume listing 6–12 bulan perlu dikonfirmasi tim bisnis; (2) kapasitas DevOps/anggaran Typesense perlu dikonfirmasi sebelum kriteria ambang tercapai.
- `technology-decisions.md` naik dari v1.1 → **v1.2**: baris **Search Engine** pada Official Technology Stack (Bagian 3) berubah dari "OPEN, wajib diselesaikan sebelum Sprint S5" menjadi **Approved**; baris Search Engine dihapus dari Open Questions (Bagian 9); Future Evaluation (Bagian 8) diperbarui agar Typesense tercatat sebagai target migrasi Fase 2 terjadwal, bukan Open Question.
- `SYSTEM-ARCHITECTURE.md` naik dari v1.1 → **v1.2**: seluruh referensi "ADR-005 — OPEN" diganti "ADR-005 — Approved" di Component Diagram, Technology Stack, Module Architecture (5.3), API Architecture, Scalability Strategy, Risks, AI Development Notes, Open Questions, dan ADR Cross-Reference Matrix; ringkasan status ADR berubah dari "21 dari 25 Approved, 4 OPEN" menjadi **"22 dari 25 Approved, 3 OPEN"**.
- `dependency-manifest.md` naik dari v1.1 → **v1.2**: Open Questions poin Search Engine dihapus (RESOLVED); AI Development Guidelines poin 5 dan Maintenance Plan "ADR status watch" diperbarui agar tidak lagi mencantumkan ADR-005 sebagai area placeholder.
- `PROJECT-CONSTITUTION.md` naik dari v1.2 → **v1.3**: baris **Search Engine** pada Bagian 4 (Tech Stack) berubah dari "Belum final (ADR-005 — OPEN)" menjadi keputusan final; Technical Constraints poin 4 dikoreksi dari "Tiga area OPEN" yang sebelumnya salah mencantumkan 4 ADR menjadi benar-benar tiga (ADR-006, ADR-008, ADR-018); Governance poin 3 diperbarui.
- `AI-DEVELOPMENT-BLUEPRINT.md` naik dari v1.1 → **v1.2**: AI Workflow (Bagian 4), Module Development (Bagian 22.3 — baris ADR-005 dihapus dari tabel modul terdampak ADR OPEN, dikoreksi dari "Empat ADR" menjadi "Tiga ADR"), dan Development Order (Bagian 23.2 — baris ADR-005 dihapus dari tabel prioritas resolusi) diperbarui.
- `CURRENT-PROJECT-STATE.md` — *ADR & Governance Snapshot* diperbarui: **22 ADR Approved** (dari 21), **3 ADR OPEN** (dari 4); *Readiness Snapshot* kondisi #3 ("Strategi Search Engine Fase 1 diputuskan") berubah dari ❌ menjadi **✅ TERPENUHI**; kesimpulan readiness berubah dari "1 dari 6 kondisi terpenuhi" menjadi **"2 dari 6 kondisi terpenuhi"**.
- `decision-log.md` — field **Last Updated** diperbarui untuk mencerminkan penambahan entry `ADR-039`. Open Decisions Bagian 11 poin 5 dipisah: bagian Search Engine ditandai `RESOLVED`, bagian Job Queue tetap Open. Tidak ada entry lama yang diubah isinya.

### Removed
Tidak ada perubahan pada kategori ini di rilis ini.

### Deprecated
Tidak ada perubahan pada kategori ini di rilis ini.

### Fixed
Tidak ada perubahan pada kategori ini di rilis ini (belum ada kode untuk diperbaiki; ini adalah rilis governance/dokumentasi). Catatan: koreksi hitungan "Tiga area teknis OPEN" di `PROJECT-CONSTITUTION.md` Bagian 24 poin 4 (sebelumnya salah mencantumkan 4 ADR dengan label "Tiga") kini akurat menyusul resolusi ADR-005 — dicatat di sini sebagai efek samping governance, bukan bug kode.

### Security
Tidak ada perubahan pada kategori ini di rilis ini (belum ada kode yang berpotensi memiliki celah keamanan). Kebijakan keamanan yang **akan** berlaku tetap sesuai `PROJECT-CONSTITUTION.md` Bagian 20 dan `architecture-decision-records.md` `ADR-017` (Security Strategy). Catatan desain: query pencarian (`/properties/search`, `/properties/autocomplete`) tetap tunduk RLS Supabase yang sudah berjalan (ADR-003/ADR-004) — tidak ada lapisan otorisasi baru yang perlu dirancang untuk Fase 1 (native Postgres, tanpa sistem index eksternal).

### Database Changes
Tidak ada perubahan database fisik — belum ada project database yang diinisialisasi. **Skema target bertambah**: `listings` direncanakan memiliki kolom generated `search_vector` (tipe `tsvector`, digabung dari `title`/`description`/`area_keyword`) dengan index GIN, serta ekstensi `pg_trgm` diaktifkan — lihat `technology-decisions.md` §4.30 dan `SYSTEM-ARCHITECTURE.md` Bagian 7. Belum tereksekusi; direncanakan sebagai bagian migration Sprint S0/S4 (Modul 3).

### API Changes
Tidak ada perubahan kontrak — belum ada endpoint yang diimplementasikan. Mesin di balik `GET /properties/search` dan `GET /properties/autocomplete` kini terkunci sebagai PostgreSQL FTS + `pg_trgm` via `architecture-decision-records.md` `ADR-005` (lihat **RELEASE HISTORY [0.1.2]**), kontrak `API-Specification-Real-Estate-Agency-Platform-v1.1.md` §3 tidak berubah — bentuk request/response tetap sama terlepas dari mesin pencari, termasuk saat migrasi Fase 2 ke Typesense kelak terjadi.

### UI Changes
Tidak ada perubahan pada kategori ini di rilis ini — belum ada halaman/komponen yang diimplementasikan.

---

## [0.1.1] - 2026-07-27 - Initial Development (Governance Sync)

### Added
- `architecture-decision-records.md` — dokumen Architecture Decision Records (ADR) resmi proyek, berisi **25 ADR** (`ADR-001`–`ADR-025`) yang mencakup seluruh keputusan arsitektur & teknis inti (Backend Architecture, Authentication, Authorization/RBAC, Database, Search, Job Queue, Email, Maps, Storage, Deployment, State Management, API Architecture, Error Handling, Logging, Monitoring, Testing, Security, Caching, File Upload, Notification, Frontend Framework, Database Schema Conventions, Multi-Tenancy, RBAC Role Model Scope, Type Safety & Validation). Ditetapkan sebagai **satu-satunya sumber kebenaran** untuk desain arsitektur & implementasi teknis, dirujuk wajib oleh `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, dan `dependency-manifest.md`. Status dokumen: **Draft — menunggu review & pengesahan tim**.
- `decision-log.md` `ADR-038` — entry baru "Backend Architecture: Next.js Route Handlers sebagai BFF Tipis (Tanpa Service Node Terpisah)", sinkronisasi dari `architecture-decision-records.md` `ADR-001` (Status: **Approved**, tanggal 2026-07-27, hasil sesi Architecture Review Board).

### Changed
- **Keputusan arsitektur backend dikunci final**: setelah sebelumnya dicatat sebagai pertentangan terbuka antar dokumen governance (lihat **Known Issues #1**), `architecture-decision-records.md` `ADR-001` menetapkan **Next.js Route Handlers sebagai BFF tipis** (terintegrasi langsung ke Supabase, tanpa service backend Node terpisah seperti NestJS/Express) sebagai keputusan **Approved**, dengan dua catatan kondisional Board: (1) batas eksekusi serverless Vercel wajib didokumentasikan eksplisit di `SYSTEM-ARCHITECTURE.md`; (2) **Bolt.new** sebagai toolchain resmi proyek perlu ditambahkan eksplisit ke `technology-decisions.md`/`dependency-manifest.md`.
- `decision-log.md` — field **Last Updated** diperbarui untuk mencerminkan penambahan entry `ADR-038`. Tidak ada entry lama yang diubah isinya.

### Removed
Tidak ada perubahan pada kategori ini di rilis ini.

### Deprecated
Tidak ada perubahan pada kategori ini di rilis ini.

### Fixed
Tidak ada perubahan pada kategori ini di rilis ini (belum ada kode untuk diperbaiki; ini adalah rilis governance/dokumentasi).

### Security
Tidak ada perubahan pada kategori ini di rilis ini (belum ada kode yang berpotensi memiliki celah keamanan). Kebijakan keamanan yang **akan** berlaku tetap sesuai `PROJECT-CONSTITUTION.md` Bagian 20 dan `architecture-decision-records.md` `ADR-017` (Security Strategy).

### Database Changes
Tidak ada perubahan pada kategori ini di rilis ini — belum ada database fisik yang diinisialisasi.

### API Changes
Tidak ada perubahan pada kategori ini di rilis ini — belum ada endpoint yang diimplementasikan. Konvensi lokasi eksekusi API (Route Handlers, bukan service terpisah) kini terkunci via `ADR-001`, namun kontrak API itu sendiri tidak berubah dari `API-Specification-Real-Estate-Agency-Platform-v1.1.md`.

### UI Changes
Tidak ada perubahan pada kategori ini di rilis ini — belum ada halaman/komponen yang diimplementasikan.

---

## [0.1.0] - 2026-07-27 - Initial Development

### Added
- `PROJECT-CONSTITUTION.md` — aturan tetap proyek (tujuan sistem, role, tech stack, konvensi, security rules, dsb.), v1.1.
- `PRD-Real-Estate-Agency-Platform-v1.1.md` — Product Requirements Document, 11 modul fungsional.
- `ERD-Skema-Database-Real-Estate-Agency-v1.1.md` + `ERD-Diagram-v1.1.mermaid` — desain skema database (37+ entitas).
- `API-Specification-Real-Estate-Agency-Platform-v1.1.md` — kontrak REST API lengkap.
- `User-Flow-Real-Estate-Agency-Platform-v1.1.md` — alur interaksi UI per role.
- `SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md` — strategi rendering, SEO, analytics.
- `SYSTEM-ARCHITECTURE.md` — arsitektur teknis end-to-end (23 bagian).
- `technology-decisions.md` — keputusan teknologi resmi & justifikasinya.
- `dependency-manifest.md` — katalog dependency resmi yang boleh digunakan.
- `AI-DEVELOPMENT-BLUEPRINT.md` — panduan operasional eksekusi harian AI Coding Assistant (ditetapkan sebagai versi acuan aktif).
- `AI-CONTEXT-PACK.md` — ringkasan context tetap untuk di-reload setiap sesi AI.
- `DEVELOPMENT-ROADMAP.md` — roadmap 15 sprint (S0–S14) dengan urutan modul berbasis dependency.
- `TASK-TEMPLATE.md` — template task reusable untuk seluruh jenis pekerjaan development.
- `CURRENT-PROJECT-STATE.md` — dokumen status proyek berjalan (living document).
- `CHANGELOG.md` — dokumen ini.

### Changed
- Resolusi 7 konflik lintas dokumen sumber v1.0 → v1.1 (role `buyer` & `instructor` diformalkan, cakupan Manager ditegaskan selalu global, satuan tenor DBR ditegaskan selalu bulan, `developer_projects.city` dimigrasi ke `city_id`, framework Next.js ditetapkan, fitur review agen diaktifkan Fase 1) — lihat "Riwayat Keputusan Arsitektur" di `PROJECT-CONSTITUTION.md`.

### Database Changes
Tidak ada perubahan database fisik — belum ada project database yang diinisialisasi. Skema **target** didefinisikan penuh sebagai desain di `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`.

### API Changes
Tidak ada endpoint yang diimplementasikan — kontrak **target** didefinisikan penuh sebagai desain di `API-Specification-Real-Estate-Agency-Platform-v1.1.md`.

### UI Changes
Tidak ada UI yang diimplementasikan — belum ada monorepo/komponen fisik (lihat `CURRENT-PROJECT-STATE.md`).

### Fixed
Tidak ada perubahan pada kategori ini di rilis ini (belum ada kode untuk diperbaiki).

### Security
Tidak ada perubahan pada kategori ini di rilis ini (belum ada kode untuk diamankan). Kebijakan keamanan yang **akan** berlaku sudah didefinisikan di `PROJECT-CONSTITUTION.md` Bagian 20.

---

# MODULE HISTORY

Riwayat status tiap modul dari waktu ke waktu. Baris baru ditambahkan setiap kali status sebuah modul berubah — baris lama tidak dihapus.

| Version | Tanggal | Modul | Status Baru | Catatan |
|---|---|---|---|---|
| 0.1.0 | 2026-07-27 | Governance & Documentation | Completed | Seluruh dokumen desain/governance v1.1 selesai |
| 0.1.0 | 2026-07-27 | Phase 0 — Foundation Infrastructure | Not Started | Menunggu Sprint S0 |
| 0.1.0 | 2026-07-27 | Modul 1 — Authentication | Not Started | Menunggu Sprint S1 |
| 0.1.0 | 2026-07-27 | Modul 2 — Agent Profile | Not Started | Menunggu Sprint S2 |
| 0.1.0 | 2026-07-27 | Modul 9+10 — Admin Panel & RBAC | Not Started | Menunggu Sprint S3 |
| 0.1.0 | 2026-07-27 | Modul 3 — Listing Management | Not Started | Menunggu Sprint S4–S5 |
| 0.1.0 | 2026-07-27 | Modul 11 — SEO & Analytics | Not Started | Menunggu Sprint S6 |
| 0.1.0 | 2026-07-27 | Modul 2 ext. — Buyer & Reviews | Not Started | Menunggu Sprint S7 |
| 0.1.0 | 2026-07-27 | Modul 8 — Dashboard & Notifikasi | Not Started | Menunggu Sprint S8 |
| 0.1.0 | 2026-07-27 | Modul 6 — Developer Directory | Not Started | Menunggu Sprint S9 |
| 0.1.0 | 2026-07-27 | Modul 7 — DBR Scoring | Not Started | Menunggu Sprint S10 |
| 0.1.0 | 2026-07-27 | Modul 4 — Learning Center | Not Started | Menunggu Sprint S12 |
| 0.1.0 | 2026-07-27 | Modul 5 — Kalender Event | Not Started | Menunggu Sprint S13 |

---

# DATABASE CHANGES

Log kumulatif seluruh perubahan skema database lintas versi (agregasi dari **Release History** di atas, disusun agar mudah ditelusuri per kategori).

## [0.1.2] - 2026-07-28
- Tidak ada perubahan fisik — belum ada database. **Skema target bertambah**: kolom generated `search_vector` (tsvector) + index GIN pada `listings`, ekstensi `pg_trgm` diaktifkan — via `architecture-decision-records.md` `ADR-005` (Approved). Lihat `technology-decisions.md` §4.30.

## [0.1.1] - 2026-07-27
- Tidak ada perubahan — belum ada database fisik. Skema target tidak berubah dari `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`.

## [0.1.0] - 2026-07-27
- Tidak ada perubahan — belum ada database fisik. Skema target: lihat `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`.

---

# API CHANGES

Log kumulatif seluruh perubahan kontrak API lintas versi.

## [0.1.2] - 2026-07-28
- Tidak ada perubahan kontrak — belum ada endpoint yang diimplementasikan. Mesin pencari di balik `GET /properties/search`/`GET /properties/autocomplete` terkunci sebagai PostgreSQL FTS + `pg_trgm` via `architecture-decision-records.md` `ADR-005` (lihat **RELEASE HISTORY [0.1.2]**), kontrak `API-Specification-Real-Estate-Agency-Platform-v1.1.md` §3 tidak berubah.

## [0.1.1] - 2026-07-27
- Tidak ada perubahan kontrak — belum ada endpoint yang diimplementasikan. Lokasi eksekusi API terkunci sebagai Next.js Route Handlers via `architecture-decision-records.md` `ADR-001` (lihat **RELEASE HISTORY [0.1.1]**), kontrak `API-Specification-Real-Estate-Agency-Platform-v1.1.md` tidak berubah.

## [0.1.0] - 2026-07-27
- Tidak ada perubahan — belum ada endpoint yang diimplementasikan. Kontrak target: lihat `API-Specification-Real-Estate-Agency-Platform-v1.1.md`.

---

# UI CHANGES

Log kumulatif seluruh perubahan antarmuka pengguna lintas versi.

## [0.1.2] - 2026-07-28
- Tidak ada perubahan — belum ada halaman/komponen yang diimplementasikan.

## [0.1.1] - 2026-07-27
- Tidak ada perubahan — belum ada halaman/komponen yang diimplementasikan.

## [0.1.0] - 2026-07-27
- Tidak ada perubahan — belum ada halaman/komponen yang diimplementasikan.

---

# SECURITY FIXES

Log kumulatif seluruh perbaikan keamanan lintas versi.

## [0.1.2] - 2026-07-28
- Tidak ada — belum ada kode yang berpotensi memiliki celah keamanan.

## [0.1.1] - 2026-07-27
- Tidak ada — belum ada kode yang berpotensi memiliki celah keamanan.

## [0.1.0] - 2026-07-27
- Tidak ada — belum ada kode yang berpotensi memiliki celah keamanan.

---

# PERFORMANCE IMPROVEMENTS

Log kumulatif seluruh peningkatan performa lintas versi.

## [0.1.2] - 2026-07-28
- Tidak ada — belum ada kode yang dapat diukur performanya.

## [0.1.1] - 2026-07-27
- Tidak ada — belum ada kode yang dapat diukur performanya.

## [0.1.0] - 2026-07-27
- Tidak ada — belum ada kode yang dapat diukur performanya.

---

# BUG FIXES

Log kumulatif seluruh perbaikan bug lintas versi, dengan referensi Task ID.

## [0.1.2] - 2026-07-28
- Tidak ada — belum ada kode yang dapat memiliki bug.

## [0.1.1] - 2026-07-27
- Tidak ada — belum ada kode yang dapat memiliki bug.

## [0.1.0] - 2026-07-27
- Tidak ada — belum ada kode yang dapat memiliki bug.

---

# BREAKING CHANGES

Log kumulatif seluruh breaking change lintas versi — setiap entri wajib menyertakan panduan migrasi atau rujukan ke **Migration Notes**.

## [0.1.2] - 2026-07-28
- Tidak ada. Penguncian keputusan Search Strategy (`ADR-005`) bersifat penegasan governance, bukan breaking change terhadap kontrak API yang sudah live (belum ada kontrak live — proyek 0% kode).

## [0.1.1] - 2026-07-27
- Tidak ada. Penguncian keputusan Backend Architecture (`ADR-001`) bersifat penegasan governance, bukan breaking change terhadap kontrak API yang sudah live (belum ada kontrak live — proyek 0% kode).

## [0.1.0] - 2026-07-27
- Tidak ada.

---

# MIGRATION NOTES

Panduan migrasi (data, skema, atau kode konsumen API) untuk setiap rilis yang membutuhkannya.

## [0.1.2] - 2026-07-28
- Tidak ada migration yang perlu dijalankan — belum ada migration file yang dibuat. Kolom `search_vector` + index GIN + ekstensi `pg_trgm` direncanakan masuk migration Sprint S0/S4, bukan migration terpisah untuk rilis ini.

## [0.1.1] - 2026-07-27
- Tidak ada migration yang perlu dijalankan — belum ada migration file yang dibuat.

## [0.1.0] - 2026-07-27
- Tidak ada migration yang perlu dijalankan — belum ada migration file yang dibuat.

---

# KNOWN ISSUES

Isu yang diketahui namun belum diperbaiki, bersifat kumulatif — ditandai `RESOLVED` (bukan dihapus) begitu selesai ditangani, dengan versi resolusinya.

| # | Isu | Sejak Versi | Status | Dampak |
|---|---|---|---|---|
| 1 | Keputusan arsitektur backend (Route Handlers vs service terpisah) sudah condong ke Route Handlers+Supabase di `technology-decisions.md`, namun belum disinkronkan balik ke `PROJECT-CONSTITUTION.md`/`SYSTEM-ARCHITECTURE.md` yang masih mencatatnya terbuka. | 0.1.0 | **RESOLVED (0.1.1)** — keputusan dikunci final via `architecture-decision-records.md` `ADR-001` & `decision-log.md` `ADR-038` (2026-07-27). **Catatan sisa:** sinkronisasi redaksional ke `PROJECT-CONSTITUTION.md`/`SYSTEM-ARCHITECTURE.md` itu sendiri belum dieksekusi — lihat catatan kondisional Board di entry `0.1.1`. | Governance — tidak lagi memblokir Sprint S0/S1; sinkronisasi redaksional dapat menyusul tanpa mengubah keputusan |
| 2 | State management server-state (TanStack Query vs SWR) sudah diputuskan tegas di `technology-decisions.md`, namun `SYSTEM-ARCHITECTURE.md` Bagian 10 masih menulis "pilih salah satu". | 0.1.0 | Open | Governance — non-blocking |
| 3 | Vercel sebagai hosting resmi belum tercatat formal di `PROJECT-CONSTITUTION.md`. | 0.1.0 | Open | Governance — non-blocking |
| 4 | Provider Maps (Google Maps Platform) sudah final di `technology-decisions.md`, namun `PROJECT-CONSTITUTION.md`/`API-Specification` masih mencatat "belum final". | 0.1.0 | Open | Governance — perlu diselesaikan sebelum Modul 3 (form lokasi) |
| 5 | Search Engine (Typesense/Elasticsearch) & Job Queue (BullMQ vs Supabase Edge Functions) belum masuk *Official Technology Stack*. | 0.1.0 | **Search Engine: RESOLVED (0.1.2)** — keputusan dikunci final via `architecture-decision-records.md` `ADR-005` & `decision-log.md` `ADR-039` (2026-07-28): PostgreSQL FTS + `pg_trgm` Fase 1, migrasi terjadwal ke Typesense Fase 2. **Job Queue: tetap Open.** | Search Engine — tidak lagi memblokir apa pun; Job Queue tetap perlu diputuskan sebelum Sprint S6/S13 |
| 6 | Provider Email (Resend) & Monitoring (Sentry) sudah diputuskan di `technology-decisions.md`, belum disinkronkan ke `SYSTEM-ARCHITECTURE.md` yang masih mencatatnya kosong. | 0.1.0 | Open | Governance — non-blocking |

---

# TECHNICAL DEBT

Belum ada technical debt kode (belum ada kode yang ditulis). Debt yang tercatat saat ini seluruhnya bersifat **debt keputusan governance** — lihat tabel **Known Issues** di atas dan `CURRENT-PROJECT-STATE.md` bagian "Known Technical Debt" untuk detail dan rekomendasi penyelesaian.

---

# NEXT PLANNED RELEASE

## [0.2.0] (Planned) - Sprint S0 — Foundation Infrastructure
Cakupan yang direncanakan (lihat `DEVELOPMENT-ROADMAP.md` & `CURRENT-PROJECT-STATE.md` bagian "Next Recommended Module"):
- Inisialisasi monorepo (`apps/web`, `packages/shared-types`), Next.js + TypeScript `strict: true`.
- Setup Tailwind v4 + shadcn/ui, ESLint/Prettier/Husky.
- CI pipeline (lint + type-check + test) di GitHub Actions.
- Migration awal: `users`, `roles`, `permissions`, `role_permissions` (seed 8 role & permission dasar).
- Seed data referensi wilayah Indonesia (`ref_provinces/cities/districts/villages`).
- Skeleton route group `(public)/(auth)/(dashboard)/(admin)` + middleware skeleton.

Target versi berikutnya mengikuti Sprint Plan: `0.3.0` (Modul 1 — Authentication, Sprint S1), dan seterusnya hingga `1.0.0` ditetapkan sebagai **Phase 1 MVP selesai** (setelah Sprint S8 lolos penuh, per milestone `DEVELOPMENT-ROADMAP.md`).

---

# AI SESSION SUMMARY

Ringkasan tiap sesi kerja AI Coding Assistant yang menghasilkan perubahan nyata pada proyek (dokumen maupun kode) — bersifat kumulatif, entri baru selalu ditambahkan, tidak menggantikan entri lama.

## Session 1 — 2026-07-26
**Peran:** Principal Software Architect
**Output:** Review menyeluruh dokumen sumber v1.0 (PRD, ERD, API Spec, User Flow, SEO Spec) → `PROJECT-CONSTITUTION.md` v1.0 dibuat, mendokumentasikan 7 konflik lintas dokumen beserta rekomendasi resolusi.

## Session 2 — 2026-07-26
**Peran:** Principal Software Architect
**Output:** 7 konflik yang ditemukan di Session 1 diperbaiki langsung di seluruh dokumen sumber (naik ke v1.1: PRD, ERD, ERD Diagram, API Spec, User Flow, SEO Spec). `PROJECT-CONSTITUTION.md` direvisi mengikuti dokumen v1.1 (bagian "Daftar Konflik" diganti menjadi "Riwayat Keputusan Arsitektur").

## Session 3 — 2026-07-26/27
**Peran:** Principal Software Architect
**Output:** `AI-DEVELOPMENT-BLUEPRINT.md` v1.0 dibuat (32 bagian) sebagai panduan operasional AI Coding Assistant.

## Session 4 — 2026-07-27
**Peran:** Principal Software Architect / Senior Product Manager / AI Coding Workflow Designer
**Output:** Mempelajari 6 dokumen tambahan yang diupload user: `AI-CONTEXT-PACK.md`, `ai-development-blueprint` (versi upload, 24 bagian), `technology-decisions.md`, `dependency-manifest.md`, `SYSTEM-ARCHITECTURE.md`, `DEVELOPMENT-ROADMAP.md`. Ditemukan bahwa versi Blueprint upload berbeda dari yang dibuat di Session 3, serta beberapa ketidaksinkronan status "final" antar dokumen (lihat **Known Issues**). Tidak ada file baru dibuat pada sesi ini (tugas murni riset/analisis).

## Session 5 — 2026-07-27
**Keputusan:** User menetapkan `AI-DEVELOPMENT-BLUEPRINT.md` (versi upload, 24 bagian) sebagai **acuan Blueprint aktif**, menggantikan versi Session 3. Dokumen governance lain akan diperbarui satu per satu di sesi-sesi berikutnya.

## Session 6 — 2026-07-27
**Peran:** Staff Software Engineer
**Output:** `TASK-TEMPLATE.md` dibuat — template reusable untuk 9 jenis task development (New Feature, New Module, Bug Fix, Enhancement, Refactoring, Performance, Security, Testing, Deployment), lengkap dengan panduan pengisian per task type.

## Session 7 — 2026-07-27
**Peran:** Technical Project Manager
**Output:** `CURRENT-PROJECT-STATE.md` dibuat — living document status proyek, mencatat bahwa implementasi kode 0% (seluruh bagian "Existing ..." berstatus "Belum dibuat"), dengan rekomendasi modul berikutnya (Sprint S0).

## Session 8 — 2026-07-27
**Peran:** Release Manager
**Output:** `CHANGELOG.md` (dokumen ini) dibuat — versi awal `0.1.0` "Initial Development", mencatat seluruh dokumen yang dihasilkan Session 1–7 sebagai rilis pertama proyek (rilis dokumentasi, bukan rilis kode).

## Session 9 — 2026-07-27
**Peran:** Principal Software Architect / Release Manager
**Output:** `architecture-decision-records.md` dibuat (25 ADR, `ADR-001`–`ADR-025`), termasuk penguncian `ADR-001` (Backend Architecture: Next.js Route Handlers, tanpa service Node terpisah) berstatus **Approved** hasil sesi Architecture Review Board. `decision-log.md` disinkronkan dengan entry baru `ADR-038` merujuk balik ke `ADR-001` tsb. `CHANGELOG.md` dirilis sebagai `0.1.1` mencatat kedua perubahan ini dan menandai **Known Issue #1** sebagai `RESOLVED (0.1.1)`.

## Session 10 — 2026-07-28
**Peran:** Architecture Review Board (CTO, Principal Software Architect, Enterprise Solution Architect, Senior Backend Architect, Senior Frontend Architect, Cloud Architect, DevOps Architect, Database Architect, Security Architect, AI Development Architect, Technical Lead)
**Output:** Sesi Architecture Review Board 10-tahap menyelesaikan `ADR-005` (Search Strategy) berstatus **Approved** — PostgreSQL Full-Text Search + `pg_trgm` sebagai mesin pencari Fase 1, migrasi terjadwal ke Typesense Fase 2 berdasarkan kriteria ambang eksplisit. `decision-log.md` disinkronkan dengan entry baru `ADR-039` merujuk balik ke `ADR-005` tsb. Sinkronisasi berantai dieksekusi ke seluruh dokumen turunan: `technology-decisions.md` (v1.1→v1.2), `SYSTEM-ARCHITECTURE.md` (v1.1→v1.2), `dependency-manifest.md` (v1.1→v1.2), `PROJECT-CONSTITUTION.md` (v1.2→v1.3), `AI-DEVELOPMENT-BLUEPRINT.md`/`development-playbook.md` (v1.1→v1.2), dan `CURRENT-PROJECT-STATE.md` (snapshot governance). `CHANGELOG.md` dirilis sebagai `0.1.2` mencatat seluruh perubahan ini dan menandai bagian Search Engine pada **Known Issue #5** sebagai `RESOLVED (0.1.2)`.

---

*Dokumen ini adalah log perubahan resmi proyek, wajib dipelihara sepanjang siklus hidup proyek. Setiap sesi development — baik menghasilkan dokumen governance, kode, maupun perbaikan — wajib menambahkan entri baru di sini sebelum sesi ditutup. Tidak ada entri yang boleh dihapus atau ditulis ulang; koreksi selalu berupa entri baru.*
