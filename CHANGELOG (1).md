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

**`0.1.1`** — *Initial Development*
Dirilis: 2026-07-27
Fase: Pra-Development — governance sync: `architecture-decision-records.md` ditambahkan sebagai sumber kebenaran arsitektur, dan `decision-log.md` disinkronkan dengan keputusan Backend Architecture (ADR-001 arsitektur / ADR-038 decision log). Implementasi kode masih belum dimulai (lihat `CURRENT-PROJECT-STATE.md`).

---

# RELEASE HISTORY

## [Unreleased]
Belum ada perubahan yang menunggu rilis berikutnya. Perubahan berikutnya yang direncanakan: Sprint S0 — Foundation Infrastructure (lihat **Next Planned Release**).

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

## [0.1.1] - 2026-07-27
- Tidak ada perubahan — belum ada database fisik. Skema target tidak berubah dari `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`.

## [0.1.0] - 2026-07-27
- Tidak ada perubahan — belum ada database fisik. Skema target: lihat `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`.

---

# API CHANGES

Log kumulatif seluruh perubahan kontrak API lintas versi.

## [0.1.1] - 2026-07-27
- Tidak ada perubahan kontrak — belum ada endpoint yang diimplementasikan. Lokasi eksekusi API terkunci sebagai Next.js Route Handlers via `architecture-decision-records.md` `ADR-001` (lihat **RELEASE HISTORY [0.1.1]**), kontrak `API-Specification-Real-Estate-Agency-Platform-v1.1.md` tidak berubah.

## [0.1.0] - 2026-07-27
- Tidak ada perubahan — belum ada endpoint yang diimplementasikan. Kontrak target: lihat `API-Specification-Real-Estate-Agency-Platform-v1.1.md`.

---

# UI CHANGES

Log kumulatif seluruh perubahan antarmuka pengguna lintas versi.

## [0.1.1] - 2026-07-27
- Tidak ada perubahan — belum ada halaman/komponen yang diimplementasikan.

## [0.1.0] - 2026-07-27
- Tidak ada perubahan — belum ada halaman/komponen yang diimplementasikan.

---

# SECURITY FIXES

Log kumulatif seluruh perbaikan keamanan lintas versi.

## [0.1.1] - 2026-07-27
- Tidak ada — belum ada kode yang berpotensi memiliki celah keamanan.

## [0.1.0] - 2026-07-27
- Tidak ada — belum ada kode yang berpotensi memiliki celah keamanan.

---

# PERFORMANCE IMPROVEMENTS

Log kumulatif seluruh peningkatan performa lintas versi.

## [0.1.1] - 2026-07-27
- Tidak ada — belum ada kode yang dapat diukur performanya.

## [0.1.0] - 2026-07-27
- Tidak ada — belum ada kode yang dapat diukur performanya.

---

# BUG FIXES

Log kumulatif seluruh perbaikan bug lintas versi, dengan referensi Task ID.

## [0.1.1] - 2026-07-27
- Tidak ada — belum ada kode yang dapat memiliki bug.

## [0.1.0] - 2026-07-27
- Tidak ada — belum ada kode yang dapat memiliki bug.

---

# BREAKING CHANGES

Log kumulatif seluruh breaking change lintas versi — setiap entri wajib menyertakan panduan migrasi atau rujukan ke **Migration Notes**.

## [0.1.1] - 2026-07-27
- Tidak ada. Penguncian keputusan Backend Architecture (`ADR-001`) bersifat penegasan governance, bukan breaking change terhadap kontrak API yang sudah live (belum ada kontrak live — proyek 0% kode).

## [0.1.0] - 2026-07-27
- Tidak ada.

---

# MIGRATION NOTES

Panduan migrasi (data, skema, atau kode konsumen API) untuk setiap rilis yang membutuhkannya.

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
| 5 | Search Engine (Typesense/Elasticsearch) & Job Queue (BullMQ vs Supabase Edge Functions) belum masuk *Official Technology Stack*. | 0.1.0 | Open | Teknis — perlu diputuskan sebelum Sprint S5/S6/S13 |
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

---

*Dokumen ini adalah log perubahan resmi proyek, wajib dipelihara sepanjang siklus hidup proyek. Setiap sesi development — baik menghasilkan dokumen governance, kode, maupun perbaikan — wajib menambahkan entri baru di sini sebelum sesi ditutup. Tidak ada entri yang boleh dihapus atau ditulis ulang; koreksi selalu berupa entri baru.*
