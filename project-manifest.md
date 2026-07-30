# PROJECT MANIFEST
## Platform Web Real Estate Agency — Control Center Dokumentasi Proyek

> **Dokumen ini BUKAN** Project Overview, Changelog, Current Project State, atau Baseline Register — dokumen-dokumen tersebut tetap ada dan tetap otoritatif di areanya masing-masing (lihat Bagian 5). Project Manifest adalah **indeks resmi tunggal** di atas seluruhnya: titik masuk pertama yang wajib dibaca **sebelum** dokumen lain mana pun dibuka, oleh AI Coding Assistant (Claude, ChatGPT, Bolt.new, Cursor, GitHub Copilot) maupun kontributor manusia (Developer, QA, Technical Lead).

**Disusun dalam kapasitas gabungan:** Chief Technology Officer · Enterprise Software Architect · Software Configuration Manager · Technical Documentation Architect · ISO 9001 Documentation Specialist · AI Development Workflow Architect

**Disusun berdasarkan:** seluruh dokumen tersedia di folder project, **versi terbaru** dipakai di setiap kasus duplikasi upload (lihat catatan versi per dokumen di Bagian 7).

**Tidak ada dokumen sumber yang diubah dalam penyusunan Manifest ini.** Pertentangan yang ditemukan dicatat sebagai Governance Notes (Bagian 15), bukan diselesaikan sepihak.

---

# 1. Project Information

| Field | Value |
|---|---|
| **Project Name** | Platform Web Real Estate Agency (SaaS — Mujtahid Aktanto) |
| **Version** | Rilis proyek `0.1.1` (`CHANGELOG.md`) · `PROJECT-CONSTITUTION.md` v1.2 · dokumen sumber bisnis/data/API v1.1 |
| **Current Phase** | **Foundation Phase — selesai secara substantif**, transisi **paralel-bertahap** ke **Architecture Alignment Phase** (Keputusan CTO: **GO WITH CONDITIONS**, `executive-architecture-review.md`, 27 Jul 2026) |
| **Overall Status** | 🟡 GO WITH CONDITIONS — boleh lanjut dengan syarat, lihat Bagian 3 & 6 |
| **Repository** | **Belum ada** — implementasi kode 0%, monorepo belum diinisialisasi, Sprint S0 (Foundation Infrastructure) belum dieksekusi (`CURRENT-PROJECT-STATE.md`) |
| **Last Updated** | 28 Juli 2026 |

---

# 2. Executive Dashboard

| Dimensi | Indikator | Ringkasan |
|---|---|---|
| **Project Health** | 🟡 | 0 temuan Critical di seluruh audit; namun 3 keputusan arsitektur turunan (Search Engine, Job Queue, Caching) masih OPEN dan rekonsiliasi jumlah seed role (7 vs 8) masih belum tertutup di dokumen roadmap/status terbaru. |
| **Current Milestone** | 🟡 | Sprint S0 (Foundation Infrastructure) — **siap dimulai, belum dieksekusi**. Tidak ada blocker untuk S0 murni scaffolding. |
| **Current Phase** | 🟡 | Foundation Phase selesai substantif (9/11 exit criteria) → Architecture Alignment Phase berjalan **paralel**: ERD Alignment & Functional Specification **boleh mulai sekarang**; Technical Specification, Database Schema fisik, dan Module Planning S1+ **ditahan**. |
| **Architecture Status** | 🟡 | `ADR-001` (Backend Architecture) **Approved** 27 Jul 2026 — blocker Critical tunggal sudah tertutup. Namun `ADR-005` (Search Strategy), `ADR-006` (Job Queue Strategy), `ADR-018` (Caching Strategy) masih **OPEN**; `ADR-008` (Maps Provider) masih **condong**, menunggu konfirmasi biaya bisnis. |
| **Documentation Status** | 🟢 | Skor kesiapan fondasi **79/100** (`foundation-validation-report.md`) — **READY WITH MINOR REVISIONS**. 21 dokumen governance/desain tersedia, 0 pasangan dokumen berstatus Major/Critical Conflict. |
| **Baseline Status** | 🟡 | Hanya **7 dari ±21 dokumen** berstatus **Baseline** terkunci (`PROJECT-CONSTITUTION.md`, `PRD`, `TASK-TEMPLATE.md`, `decision-log.md`, `CHANGELOG.md`, `CURRENT-PROJECT-STATE.md`, `foundation-validation-report.md`). Sisanya `Approved`/`Draft`, sebagian besar terhalang oleh belum adanya nama individu Reviewer/Approver (lihat Bagian 6, item OD-06). |
| **Development Readiness** | 🟡 | Sprint S0 (scaffolding murni) **READY**. Sprint S1 ke atas (menyentuh backend/API/database) **NOT READY** — tertahan oleh 6 kondisi CTO di Bagian 6/14 `executive-architecture-review.md`, sebagian sudah terpenuhi (ADR-001), sebagian belum (seed role, Search Engine, Job Queue, nama Owner). |
| **AI Readiness** | 🟢 | Dinilai **Excellent** oleh CTO — `AI-CONTEXT-PACK.md`, `AI-DEVELOPMENT-BLUEPRINT.md` v1.1, `TASK-TEMPLATE.md`, dan `architecture-decision-records.md` memberi gerbang keputusan eksplisit bagi AI Coding Assistant, salah satu aset paling matang di seluruh dokumentasi. |

---

# 3. Current Phase

## Phase Sekarang
**Foundation Phase (selesai secara substantif) → Architecture Alignment Phase (paralel-bertahap, GO WITH CONDITIONS)** — keputusan resmi `executive-architecture-review.md`, 27 Juli 2026. Seluruh aktivitas proyek sejauh ini adalah produksi dokumentasi governance & desain; implementasi kode tetap 0% (`CURRENT-PROJECT-STATE.md`).

## Phase Sebelumnya
**Requirements & Design Documentation** — penyusunan `PROJECT-CONSTITUTION.md`, `PRD`, `ERD`, `API Specification`, `User Flow`, `SEO & Analytics Specification` (resolusi 7 konflik v1.0→v1.1), `SYSTEM-ARCHITECTURE.md`, `technology-decisions.md`, `dependency-manifest.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, `AI-CONTEXT-PACK.md`, `DEVELOPMENT-ROADMAP.md`, `TASK-TEMPLATE.md` (Session 1–8, `CHANGELOG.md`).

## Phase Berikutnya
**Architecture Alignment Phase (penuh)** → ERD Alignment, Database Schema Alignment, API Alignment, User Flow Alignment, PRD Alignment (closing-the-loop) → **Functional Specification & UI Specification** → **Technical Specification** → **Module Planning** → **Sprint S0 execution** (Bolt.new/AI Coding Assistant diperbolehkan untuk scaffolding murni sejak sekarang).

## Entry Criteria — Architecture Alignment Phase (Penuh)
*(`executive-architecture-review.md` §12 — 3 dari 6 terpenuhi)*

| # | Syarat | Status |
|---|---|---|
| 1 | ERD stabil sebagai kandidat Baseline | ✅ Terpenuhi |
| 2 | API Specification stabil sebagai kandidat Baseline | ✅ Terpenuhi |
| 3 | Tidak ada konflik struktural pada model data/bisnis inti | ✅ Terpenuhi |
| 4 | Keputusan arsitektur backend terkunci | ✅ **Terpenuhi (update pasca-review)** — `ADR-001` Approved 27 Jul 2026, di luar cakupan `executive-architecture-review.md` yang disusun sebelum resolusi ini (snapshot point-in-time, lihat Bagian 15) |
| 5 | Search Engine & Job Queue diputuskan | ❌ Belum terpenuhi — `ADR-005`/`ADR-006` masih OPEN |
| 6 | Reviewer/Approver bernama ditugaskan untuk sign-off Alignment | ❌ Belum terpenuhi |

**Entry parsial** untuk **ERD Alignment** dan **Functional Specification** dinyatakan terpenuhi dan dapat dimulai segera, paralel dengan resolusi item #5–#6 di atas.

## Exit Criteria — Foundation Phase
*(`executive-architecture-review.md` §11 — 9 dari 11 terpenuhi penuh, 2 sebagian, 1 belum)*

| # | Kriteria | Status |
|---|---|---|
| 1 | Kebutuhan bisnis inti terdokumentasi (PRD) | ✅ Terpenuhi |
| 2 | Model data terancang penuh (ERD, logis) | ✅ Terpenuhi |
| 3 | Kontrak API terdefinisi | ✅ Terpenuhi |
| 4 | Arsitektur terdefinisi tanpa konflik struktural besar | ✅ **Terpenuhi (update)** — backend terkunci via `ADR-001` |
| 5 | Model keamanan & RBAC terdefinisi | ✅ Terpenuhi — dimensi terkuat proyek |
| 6 | Tech stack diputuskan | ⚠️ Sebagian — Search Engine/Job Queue/Caching masih OPEN; status dokumen masih Draft |
| 7 | Tata kelola AI development tersedia | ✅ Terpenuhi |
| 8 | Roadmap & sprint plan tersedia | ✅ Terpenuhi (Draft, substansi matang) |
| 9 | Mekanisme audit-diri (Decision Log, Changelog, Current State) | ✅ Terpenuhi |
| 10 | Tidak ada konflik Critical/blocking | ✅ Terpenuhi — 0 temuan Critical |
| 11 | Kepemilikan dokumen ditugaskan ke individu bernama | ❌ Belum terpenuhi — seluruhnya masih peran |

---

# 4. Current Baseline

| Document | Current Version | Baseline | Status | Owner |
|---|---|---|---|---|
| PROJECT-CONSTITUTION.md | 1.2 | ✅ 1.2 | Baseline (BERLAKU) | Principal Software Architect (nama TBD) |
| PRD-Real-Estate-Agency-Platform-v1.1.md | 1.1 | ✅ 1.1 | Baseline (Disetujui) | Senior Business Analyst / Product Manager (TBD) |
| ERD-Skema-Database-...v1.1.md + Diagram | 1.1 | Kandidat 1.1 | Approved (belum Baseline formal) | Database Architect (TBD) |
| API-Specification-...v1.1.md | 1.1 | Kandidat 1.1 | Approved (belum Baseline formal) | API Architect (TBD) |
| User-Flow-...v1.1.md | 1.1 | Kandidat 1.1 | Approved | Senior Business Analyst (TBD) |
| SEO-Analytics-Specification-...v1.1.md | 1.1 | Kandidat 1.1 | Approved | Technical Lead (TBD) |
| SYSTEM-ARCHITECTURE.md | 1.0 | TBD (menunggu pengesahan nama) | Approved, sinkron penuh dgn ADR — **belum** Baseline formal | Enterprise Solution Architect (TBD) |
| architecture-decision-records.md | 1.0 | Belum ada | Draft (dokumen) — `ADR-001` entry-level **Approved** | Principal Enterprise Software Architect (TBD) |
| technology-decisions.md | 1.1 | Belum ada | Draft — `ADR-005/006/018` masih OPEN, memblokir Baseline | Principal Software Architect (TBD) |
| dependency-manifest.md | 1.1 | Belum ada | Draft — mengikuti status technology-decisions.md | Principal Software Architect (TBD) |
| AI-DEVELOPMENT-BLUEPRINT.md | 1.1 | Belum ada | Draft (acuan aktif operasional) | Principal Software Architect (TBD) |
| AI-CONTEXT-PACK.md | 1.0 | Kandidat 1.0 | Approved | Technical Writer (TBD) |
| DEVELOPMENT-ROADMAP.md | 1.0 | Belum ada | Draft (substansi matang) | Engineering Manager (TBD) |
| TASK-TEMPLATE.md | 1.0 | ✅ 1.0 | Baseline (BERLAKU) | Staff Software Engineer (TBD) |
| decision-log.md | 1.0 | 1.0 (per-ADR, berkembang) | Baseline (Living Document) | Principal Software Architect (TBD) |
| CHANGELOG.md | 0.1.1 | 0.1.1 | Baseline (Living Document) | Release Manager (TBD) |
| CURRENT-PROJECT-STATE.md | 0.1 | 0.1 (per-sesi) | Baseline (Living Document) | Technical Project Manager (TBD) |
| document-governance-baseline-register.md | 1.0 | Belum ada | Draft | Software Configuration Manager (TBD) |
| foundation-validation-report.md | 1.0 | ✅ 1.0 | Baseline (Final — Quality Gate Deliverable) | AI Audit Panel |
| executive-architecture-review.md | — (Keputusan CTO) | — | Final — Keputusan Resmi (setara otoritas Constitution untuk kelanjutan fase) | CTO |
| synchronization-report-adr-001.md | — (Artefak SCM) | — | Final — laporan sinkronisasi (point-in-time) | CTO/SCM |
| Database Schema (fisik) | — | — | **Planned** — belum ada (0% kode) | Database Architect (TBD) |
| Functional / UI / Technical Specification | — | — | **Planned** — belum ada file | TBD |

---

# 5. Source of Truth Index

| Area | Official Document | Priority |
|---|---|---|
| Governance Tertinggi | `PROJECT-CONSTITUTION.md` | **1 — Mengalahkan seluruh dokumen lain** |
| Business Requirement | `PRD-Real-Estate-Agency-Platform-v1.1.md` | 2 |
| Arsitektur & Keputusan Teknis per Topik (ADR) | `architecture-decision-records.md` | 3 — dibaca **sebelum** Technology Decisions |
| Architecture (High-Level) | `SYSTEM-ARCHITECTURE.md` | 4 |
| Technology Stack & Rasionalisasi | `technology-decisions.md` | 5 |
| Dependency/Package Katalog | `dependency-manifest.md` | 6 |
| Database (Logis/ERD) | `ERD-Skema-Database-...v1.1.md` + Diagram | 7 |
| Database (Fisik/Migration) | **TBD** — migration file aktual setelah Sprint S0 | 7 (menyusul) |
| API Contract | `API-Specification-...v1.1.md` | 8 |
| User Interaction Flow | `User-Flow-...v1.1.md` | 9 |
| SEO & Analytics Strategy | `SEO-Analytics-Specification-...v1.1.md` | 9 |
| Keputusan — Jurnal Kronologis Lintas Proyek | `decision-log.md` | 10 |
| AI Instruction / Development Playbook | `AI-DEVELOPMENT-BLUEPRINT.md` | 11 |
| Context Ringkas Proyek | `AI-CONTEXT-PACK.md` §1–2 + `PRD` §1 | 12 |
| Roadmap & Sprint Plan | `DEVELOPMENT-ROADMAP.md` | 13 |
| Format/Unit Kerja Task | `TASK-TEMPLATE.md` | 14 |
| Riwayat Perubahan (Rilis) | `CHANGELOG.md` | 15 (riwayat, bukan keputusan) |
| Status Implementasi Nyata (Living) | `CURRENT-PROJECT-STATE.md` | — **wajib dibaca tiap sesi** |
| Validasi Kesiapan Fondasi | `foundation-validation-report.md` | — Quality Gate |
| Keputusan Eksekutif Kelanjutan Fase | `executive-architecture-review.md` | — Setara Constitution untuk urusan proses/fase |
| Tata Kelola Dokumen Itu Sendiri | `document-governance-baseline-register.md` | — |
| **Indeks & Kontrol Seluruh Dokumentasi** | **`project-manifest.md` (dokumen ini)** | **0 — dibaca sebelum semuanya** |

---

# 6. Active Open Decisions

> Dikonsolidasikan dari `executive-architecture-review.md` §9 (12 item, prioritas dampak×urgensi) dan `decision-log.md` §11 (8 item asli). **Status kolom diperbarui** mencerminkan dokumen ter-sinkron terbaru (mis. `technology-decisions__1_`, `PROJECT-CONSTITUTION__1_`) — lihat Governance Notes (Bagian 15) untuk ketidaksesuaian formatting yang belum tertutup di `decision-log.md` §11 itu sendiri.

| Decision ID | Priority | Status | Impact | Target Resolution |
|---|---|---|---|---|
| OD-01 — Arsitektur Backend/API | Kritis | ✅ **RESOLVED** (`ADR-001`/`ADR-038`, 27 Jul 2026) | Struktur folder, pola implementasi seluruh endpoint | Selesai — disinkronkan ke Constitution v1.2, System Architecture, Technology Decisions, Dependency Manifest |
| OD-02 — Jumlah seed role final (7 vs 8) | Tinggi | 🔴 **OPEN** | Migration seed Sprint S0 | Sebelum migration seed Sprint S0 ditulis |
| OD-03 — Search Engine (Postgres FTS vs Typesense/Elasticsearch) | Tinggi | 🔴 **OPEN** (`ADR-005`) | `/properties/search`, `/properties/autocomplete` | Sebelum Sprint S5 |
| OD-04 — Job Queue (Edge Functions+cron vs BullMQ) | Tinggi | 🔴 **OPEN** (`ADR-006`) | Sitemap event-driven, reminder, sync counter | Sebelum Sprint S6/S13 |
| OD-05 — Provider Maps (Google Maps vs Mapbox) | Sedang | 🟡 **Condong, menunggu konfirmasi biaya** (`ADR-008`) | Form lokasi (M3), peta proyek developer (M6) | Sebelum Sprint S4/S9 |
| OD-06 — Kepemilikan dokumen governance (nama individu) | Sedang | 🔴 **OPEN** | Prosedur Approval formal tidak berjalan tanpa ini | Sebelum Sprint S1 |
| OD-07 — Kebijakan soft-delete seragam | Sedang | 🔴 **OPEN** | Ambiguitas hard/soft-delete di 5 entitas | Sebelum Database Schema Alignment |
| OD-08 — Vercel sebagai hosting resmi di Constitution | Rendah | ✅ **RESOLVED** — Constitution v1.2 §Backend/CI-CD sudah mencantumkan Vercel/ADR-010 | Dokumen tertinggi kini konsisten | Selesai |
| OD-09 — Resend & Sentry belum sinkron ke System Architecture | Rendah | 🟡 **Sebagian** — perlu verifikasi §23 final | Risiko redaksional | Administratif |
| OD-10 — Frasa usang state management ("SWR — pilih satu") | Rendah | ✅ **RESOLVED** — `SYSTEM-ARCHITECTURE__1_` §10 kini eksplisit TanStack Query, SWR dilarang | Kejelasan redaksi | Selesai |
| OD-11 — Model monetisasi platform | Rendah | 🔴 **OPEN** (bisnis) | Tidak memblokir selama `configurable` | Dapat ditunda |
| OD-12 — Threshold DBR final & kebijakan promosi/demosi Manager | Rendah | 🔴 **OPEN** (bisnis/kebijakan) | Tidak memblokir selama `configurable`/hard rule dipertahankan | Dapat ditunda hingga Sprint S14 |

**Ringkasan:** dari 12 item, **3 Resolved** (OD-01, OD-08, OD-10), **1 Sebagian** (OD-09), **8 masih Open** — 3 di antaranya (OD-02, OD-03, OD-04) berprioritas **Tinggi** dan berdampak langsung pada kesiapan Sprint S0–S6.

---

# 7. Documentation Inventory

| # | Nama | Versi | Status | Purpose | Source of Truth | Baseline |
|---|---|---|---|---|---|---|
| 1 | PROJECT-CONSTITUTION.md | 1.2 | Baseline | Governance/engineering guidelines tertinggi | Governance | ✅ |
| 2 | PRD-Real-Estate-Agency-Platform-v1.1.md | 1.1 | Baseline | Kebutuhan bisnis, 11 modul fungsional | Business Requirement | ✅ |
| 3 | ERD-Skema-Database-...v1.1.md + Diagram | 1.1 | Approved | Desain skema database logis (37+ entitas) | Database (Logis) | Kandidat |
| 4 | API-Specification-...v1.1.md | 1.1 | Approved | Kontrak REST API lengkap | API Contract | Kandidat |
| 5 | User-Flow-...v1.1.md | 1.1 | Approved | Alur interaksi UI per role | User Interaction Flow | Kandidat |
| 6 | SEO-Analytics-Specification-...v1.1.md | 1.1 | Approved | Strategi rendering, SEO, analytics | SEO & Analytics | Kandidat |
| 7 | SYSTEM-ARCHITECTURE.md | 1.0 | Approved, sinkron ADR | Arsitektur teknis end-to-end (23 bagian) | Architecture (High-Level) | Belum |
| 8 | architecture-decision-records.md | 1.0 | Draft (dok.) / ADR-001 Approved | 25 ADR per topik arsitektur | Arsitektur & Keputusan Teknis | Belum |
| 9 | technology-decisions.md | 1.1 | Draft | Katalog stack & justifikasi | Technology Stack | Belum |
| 10 | dependency-manifest.md | 1.1 | Draft | Katalog package sah + Bolt.new toolchain | Dependency Katalog | Belum |
| 11 | AI-DEVELOPMENT-BLUEPRINT.md | 1.1 | Draft (acuan aktif) | Panduan operasional harian AI Coding Assistant | AI Instruction | Belum |
| 12 | AI-CONTEXT-PACK.md | 1.0 | Approved | Context ringkas untuk reload tiap sesi AI | Context Ringkas | Kandidat |
| 13 | DEVELOPMENT-ROADMAP.md | 1.0 | Draft | Roadmap 15 sprint (S0–S14) | Roadmap & Sprint Plan | Belum |
| 14 | TASK-TEMPLATE.md | 1.0 | Baseline | Template task reusable | Format Unit Kerja | ✅ |
| 15 | decision-log.md | 1.0 | Baseline (Living) | Jurnal kronologis seluruh keputusan (38 ADR) | Decision (Jurnal) | ✅ |
| 16 | CHANGELOG.md | 0.1.1 | Baseline (Living) | Riwayat perubahan proyek | History | ✅ |
| 17 | CURRENT-PROJECT-STATE.md | 0.1 | Baseline (Living) | Status implementasi nyata per-sesi | Status Implementasi | ✅ |
| 18 | document-governance-baseline-register.md | 1.0 | Draft | Meta-dokumen lifecycle/versi/ownership | Tata Kelola Dokumen | Belum |
| 19 | foundation-validation-report.md | 1.0 | Baseline (Final) | Audit 17 dokumen, skor 79/100 | Validation | ✅ |
| 20 | executive-architecture-review.md | — | Final — Keputusan CTO | GO WITH CONDITIONS, 6 syarat lanjut fase | Keputusan Eksekutif | — |
| 21 | synchronization-report-adr-001.md | — | Final — Artefak SCM | Laporan sinkronisasi resolusi ADR-001 lintas 6 dokumen | — | — |
| 22 | **project-manifest.md** (dokumen ini) | 1.0 | Baru diterbitkan | Indeks & control center seluruh dokumentasi | Indeks Tertinggi | Belum |
| — | Database Schema (fisik) | — | Planned | Migration/DDL nyata | Database (Fisik) | — |
| — | Functional Specification | — | Planned | Belum ada file | — | — |
| — | UI Specification | — | Planned | Belum ada file | — | — |
| — | Technical Specification | — | Ready with Notes (bahan tersebar) | Belum dikonsolidasi | — | — |

---

# 8. Dependency Map

```
PROJECT-CONSTITUTION.md (Engineering Guidelines — tertinggi)
        ↓
Architecture Decision Records (architecture-decision-records.md)
        ↓
Technology Decisions
        ↓
Dependency Manifest
        ↓
System Architecture
        ↓
ERD (Skema Database Logis) + ERD Diagram
        ↓
Database Schema (fisik — TBD, menyusul Sprint S0)
        ↓
API Specification
        ↓
User Flow
        ↓
PRD Alignment (verifikasi silang berkelanjutan)
        ↓
Functional Specification (Planned)
        ↓
UI Specification (Planned)
        ↓
Technical Specification (Ready with Notes — perlu konsolidasi)
        ↓
Module Planning (Development Roadmap sudah memenuhi fungsi ini — Ready)
        ↓
Sprint S0 Execution (Bolt.new / AI Coding Assistant)
```

**Dokumen operasional paralel** (tidak linear terhadap rantai di atas): `decision-log.md` (jurnal kronologis lintas dokumen), `CHANGELOG.md` (bergantung `CURRENT-PROJECT-STATE.md`), `AI-CONTEXT-PACK.md` & `AI-DEVELOPMENT-BLUEPRINT.md` (bergantung seluruh dokumen sumber v1.1 + System Architecture + Technology Decisions), `TASK-TEMPLATE.md` (bergantung seluruh dokumen governance), `foundation-validation-report.md` (snapshot audit atas 17 dokumen), `executive-architecture-review.md` (keputusan dibangun di atas audit + Decision Log + Baseline Register).

---

# 9. AI Reading Order

| # | Dokumen | Alasan Urutan |
|---|---|---|
| 1 | **project-manifest.md** (dokumen ini) | Indeks & status keseluruhan — wajib pertama |
| 2 | `CURRENT-PROJECT-STATE.md` | Kondisi implementasi *nyata* saat ini (living, per-sesi) |
| 3 | `PROJECT-CONSTITUTION.md` | Engineering Guidelines tertinggi — mengalahkan seluruhnya jika konflik |
| 4 | `architecture-decision-records.md` | Alasan di balik seluruh keputusan arsitektur/teknis (dibaca sebelum Technology Decisions) |
| 5 | `decision-log.md` | Jurnal kronologis keputusan lintas proyek, termasuk non-teknis |
| 6 | `technology-decisions.md` | Katalog stack resmi & justifikasi |
| 7 | `SYSTEM-ARCHITECTURE.md` | Arsitektur teknis end-to-end |
| 8 | `dependency-manifest.md` | Package/toolchain sah untuk di-install |
| 9 | `AI-CONTEXT-PACK.md` (§1–2) + `PRD` (§1) | Project Overview / konteks ringkas |
| 10 | `PRD-Real-Estate-Agency-Platform-v1.1.md` | Kebutuhan bisnis lengkap, 11 modul |
| 11 | `ERD-Skema-Database-...v1.1.md` + Diagram | Skema database logis |
| 12 | Database Schema (fisik, setelah tersedia) | Skema database nyata |
| 13 | `API-Specification-...v1.1.md` | Kontrak REST API |
| 14 | `User-Flow-...v1.1.md` | Alur interaksi UI per role |
| 15 | `SEO-Analytics-Specification-...v1.1.md` | Strategi rendering/SEO |
| 16 | `AI-DEVELOPMENT-BLUEPRINT.md` | Prosedur kerja harian AI Coding Assistant |
| 17 | `DEVELOPMENT-ROADMAP.md` | Roadmap & urutan sprint |
| 18 | `TASK-TEMPLATE.md` | Format unit kerja sebelum eksekusi task |
| 19 | `document-governance-baseline-register.md` | Rujukan status/versi/ownership jika ragu dokumen mana yang menang |
| 20 | `foundation-validation-report.md` + `executive-architecture-review.md` | Konteks audit & keputusan kelanjutan fase (dibaca saat butuh alasan strategis) |
| 21 | `CHANGELOG.md` | Riwayat perubahan (referensi, bukan keputusan) |

---

# 10. Development Readiness

| Area | Status | Catatan |
|---|---|---|
| **Architecture** | 🟡 IN PROGRESS | Backend terkunci (`ADR-001`); Search/Job Queue/Caching (`ADR-005/006/018`) masih OPEN |
| **Database** | 🟡 IN PROGRESS | ERD logis Ready with Notes; soft-delete belum seragam (OD-07); skema fisik belum ada |
| **API** | 🟡 IN PROGRESS | Konvensi inti matang (Ready with Notes); kedalaman endpoint modul pendukung & endpoint bergantung Maps belum tuntas |
| **Documentation** | 🟢 READY | Skor 79/100, 0 konflik Major/Critical; item minor tercatat di Bagian 15 |
| **Security** | 🟢 READY | Dinilai *Excellent* — enkripsi at-rest, RLS+middleware berlapis, rate limiting, audit trail |
| **Technology** | 🟡 IN PROGRESS | Stack inti final; status dokumen masih Draft, 3 sub-keputusan OPEN |
| **Testing** | 🔴 NOT READY | Testing Strategy (`ADR-016`) tercatat, belum ada kerangka konkret (Vitest/RTL/Playwright terpilih, belum diimplementasi) |
| **Deployment** | 🔴 NOT READY | Vercel terpilih & terformalkan; 0% kode berarti belum ada deployment nyata (skor Deployment Readiness 35/100, wajar untuk tahap ini) |
| **UI** | 🔴 NOT READY | UI Specification/Wireframe belum ada sebagai dokumen |
| **Functional Spec** | 🔴 NOT READY | Belum ada file — dapat **dimulai sekarang**, tidak ada dependency ke Open Decision teknis |
| **Technical Spec** | 🟡 IN PROGRESS | Bahan baku tersebar (System Architecture + Technology Decisions + Dependency Manifest), perlu konsolidasi; ditahan sampai OD-03/OD-04 selesai |
| **Module Planning** | 🟡 IN PROGRESS | `DEVELOPMENT-ROADMAP.md` sudah memenuhi fungsi untuk S0; Module Planning penuh S1+ ditahan sampai 6 kondisi CTO terpenuhi |

---

# 11. Pending Activities

Urutan kerja yang direkomendasikan (`executive-architecture-review.md` §10, §13), dua jalur **paralel**:

```
Jalur A — Governance/Keputusan                Jalur B — Alignment/Spesifikasi
─────────────────────────────                 ────────────────────────────
Rekonsiliasi seed role (7 vs 8)                ERD Alignment (mulai sekarang)
        ↓                                              ↓
ADR Search Engine (rekomendasi: Postgres FTS)  Functional Specification (mulai sekarang)
        ↓                                              ↓
ADR Job Queue (rekomendasi: Edge Functions)    UI Specification / Screen Inventory
        ↓                                              ↓
Konfirmasi biaya Maps Provider (OD-05)         Technical Specification (konsolidasi)
        ↓                                              ↓
Tugaskan nama individu Owner/Reviewer          Database Schema Alignment (fisik)
        ↓                                              ↓
Deklarasi kebijakan soft-delete seragam        API Alignment (kedalaman endpoint)
        ↓                                              ↓
        └──────────────── keduanya bertemu di ─────────┘
                                ↓
                        Module Planning (S1+)
                                ↓
                        Sprint S0 Execution (Bolt.new — scaffolding, sudah boleh mulai)
                                ↓
                        Sprint S1 Execution (backend/API — menunggu Jalur A selesai)
```

**Catatan:** Sprint S0 murni scaffolding (monorepo, CI/CD, styling dasar) **tidak menunggu** jalur mana pun dan boleh dieksekusi Bolt.new/AI Coding Assistant sekarang.

---

# 12. Risk Summary

## 🔴 High
1. **Search Engine & Job Queue belum diputuskan** (OD-03, OD-04) — dua komponen yang secara fungsional disyaratkan API/SEO Specification namun absen dari Official Technology Stack; risiko rework besar pada Modul 3/5/8/11 jika ditunda hingga tengah sprint.
2. **Rekonsiliasi jumlah seed role (7 vs 8) belum tertutup** — masih ditemukan di `DEVELOPMENT-ROADMAP.md` dan `CURRENT-PROJECT-STATE.md` versi terbaru; risiko dua sesi AI menghasilkan migration seed berbeda di Sprint S0.

## 🟡 Medium
3. **Provider Maps belum final** (OD-05) — memblokir implementasi penuh Modul 3 & 6 sebelum Sprint S4/S9 jika konfirmasi biaya tidak turun tepat waktu.
4. **Kepemilikan dokumen governance masih peran, bukan nama** (OD-06) — tanpa ini, prosedur Approval formal (`decision-log.md` §8, `document-governance-baseline-register.md` §11) hanya berlaku di atas kertas; Baseline formal pertama tidak dapat benar-benar disahkan.
5. **Kebijakan soft-delete belum seragam** (OD-07) — ambiguitas untuk 5 entitas berisiko migration awal ditulis dengan asumsi salah.
6. **Functional/UI Specification belum ada** — Module Planning tidak dapat menyentuh implementasi UI presisi tanpa ini, meski tidak memblokir Sprint S0.

## 🟢 Low
7. Resend/Sentry — verifikasi akhir sinkronisasi ke `SYSTEM-ARCHITECTURE.md` §23 (OD-09).
8. Model monetisasi & threshold DBR final (OD-11, OD-12) — keputusan bisnis, aman ditunda selama tetap `configurable`.
9. Duplikasi `CHANGELOG.md` Known Issues vs `decision-log.md` Open Decisions tanpa cross-reference kanonik tunggal (lihat Governance Notes).
10. Ambiguitas penomoran "ADR-" antar `architecture-decision-records.md` (ADR-001–025) dan `decision-log.md` (ADR-001–038) — kosmetik, tidak memengaruhi isi keputusan.

---

# 13. AI Usage Instructions

AI Coding Assistant apa pun (Claude, ChatGPT, Bolt.new, Cursor, GitHub Copilot, dsb.) **wajib**:

1. **Membaca `project-manifest.md` ini terlebih dahulu**, sebelum dokumen proyek lain mana pun — gunakan Bagian 9 (AI Reading Order) sebagai urutan lanjutan.
2. **Menggunakan Source of Truth Index (Bagian 5)** untuk menentukan dokumen mana yang berwenang menjawab suatu pertanyaan — jangan mencampur informasi dari dua dokumen yang membahas topik sama tanpa mengecek prioritas.
3. **Mengikuti ADR** (`architecture-decision-records.md`) — ADR berstatus `Approved` **mengikat** dan tidak boleh dilanggar; ADR berstatus `Open`/`Proposed` **tidak boleh** diasumsikan/dipilih sendiri oleh AI.
4. **Tidak menggunakan dokumen berstatus `Deprecated`/`Archived`** (mis. `AI-DEVELOPMENT-BLUEPRINT.md` versi Session 3, 32 bagian, sudah digantikan versi 24 bagian) sebagai rujukan keputusan.
5. **Selalu menggunakan versi Baseline/versi terbaru** yang tercatat di Bagian 4 & 7 — jika ditemukan duplikasi upload dokumen yang sama, versi dengan `Last Updated` paling akhir (atau bernomor versi lebih tinggi) yang berlaku, bukan diasumsikan identik.
6. **Tidak mengambil keputusan arsitektur/bisnis baru secara sepihak** untuk item yang tercatat `OPEN` di Bagian 6 — laporkan sebagai temuan, tunggu ADR resmi dengan keterlibatan manusia berwenang.
7. **Tidak menulis kode backend/API produksi** sebelum memverifikasi status Open Decision terkait modul yang disentuh (lihat Bagian 6 & 10) — Sprint S0 scaffolding murni terkecuali.
8. **Melaporkan, bukan memperbaiki sendiri**, setiap pertentangan baru antar dokumen yang ditemukan — catat sebagai Governance Note (pola Bagian 15), konsisten dengan `document-governance-baseline-register.md` §13 poin 6.

---

# 14. Quick Navigation

| Saya butuh... | Buka dokumen |
|---|---|
| Memahami proyek secara umum / konteks singkat | `AI-CONTEXT-PACK.md` §1–2 + `PRD` §1 |
| Aturan tetap tertinggi (role, security, tech stack) | `PROJECT-CONSTITUTION.md` |
| Alasan **mengapa** sebuah keputusan teknis diambil (per topik) | `architecture-decision-records.md` |
| Riwayat **seluruh** keputusan proyek (kronologis) | `decision-log.md` |
| Requirement bisnis / acceptance criteria per modul | `PRD-Real-Estate-Agency-Platform-v1.1.md` |
| Struktur tabel database & relasi | `ERD-Skema-Database-...v1.1.md` + Diagram |
| Kontrak endpoint API | `API-Specification-...v1.1.md` |
| Alur layar/interaksi per role | `User-Flow-...v1.1.md` |
| Strategi SEO/rendering/analytics | `SEO-Analytics-Specification-...v1.1.md` |
| Stack teknologi resmi & alasan pemilihan | `technology-decisions.md` |
| Daftar package yang boleh di-install | `dependency-manifest.md` |
| Arsitektur teknis end-to-end | `SYSTEM-ARCHITECTURE.md` |
| Cara AI Coding Assistant bekerja hari-hari | `AI-DEVELOPMENT-BLUEPRINT.md` |
| Urutan & isi sprint | `DEVELOPMENT-ROADMAP.md` |
| Format standar sebuah task | `TASK-TEMPLATE.md` |
| Status implementasi kode **saat ini** | `CURRENT-PROJECT-STATE.md` |
| Riwayat rilis/perubahan versi | `CHANGELOG.md` |
| Status/versi/ownership dokumen mana pun | `document-governance-baseline-register.md` |
| Hasil audit kesiapan fondasi & skor | `foundation-validation-report.md` |
| Keputusan resmi CTO soal kelanjutan fase | `executive-architecture-review.md` |
| Apa yang berubah saat ADR-001 disinkronkan | `synchronization-report-adr-001.md` |
| **Indeks semua dokumen & status proyek** | **`project-manifest.md` (dokumen ini)** |

---

# 15. Governance Notes

> Konsisten dengan mandat Manifest ini: pertentangan yang ditemukan **dicatat**, **tidak diperbaiki sepihak** di sini.

1. **`decision-log.md` §11 (Open Decisions) belum diformat-ulang meski `ADR-038` sudah ada.** `synchronization-report-adr-001.md` mengklaim baris #1 "ditandai resolved dan dirujuk-silang ke ADR-038", namun isi aktual `decision-log__1_.md` §11 baris #1 **masih identik** dengan redaksi lama (belum ada penanda Resolved). **Rekomendasi:** update baris #1 §11 secara eksplisit (append status, bukan menghapus), agar klaim di laporan sinkronisasi konsisten dengan isi dokumen sebenarnya.
2. **Duplikasi versi dokumen di repositori proyek.** Beberapa dokumen ditemukan dalam dua salinan (versi dasar vs versi bersufiks upload berikutnya) dengan isi berbeda signifikan (mis. `technology-decisions.md` vs versi revisi 1.1, `PROJECT-CONSTITUTION.md` v1.1 vs v1.2, `decision-log.md` 37 ADR vs 38 ADR). Manifest ini secara konsisten memakai **versi dengan `Last Updated`/nomor versi paling akhir**. **Rekomendasi:** konsolidasikan menjadi satu salinan kanonik per dokumen di repositori, hapus/arsipkan salinan usang secara eksplisit (jangan dibiarkan ambigu mana yang "sungguh" berlaku).
3. **`executive-architecture-review.md` adalah snapshot point-in-time** yang disusun **sebelum** `ADR-001` disinkronkan penuh — dokumen tsb masih mencantumkan Open Decision #1 (Backend) sebagai "Kritis, belum terkunci". Manifest ini memperbarui status tersebut menjadi Resolved berdasarkan bukti dari dokumen yang lebih baru (`PROJECT-CONSTITUTION.md` v1.2, `technology-decisions.md` v1.1, `dependency-manifest.md` v1.1), **tanpa mengedit isi asli** `executive-architecture-review.md` itu sendiri (konsisten dengan sifatnya sebagai laporan historis/keputusan resmi bertanggal). **Rekomendasi:** terbitkan addendum singkat pada `executive-architecture-review.md` yang mencatat resolusi Kondisi #1 (Bagian 14), tanpa mengubah verdict aslinya.
4. **Rekonsiliasi jumlah seed role (7 vs 8) belum tertutup**, meski tercatat sebagai Kondisi #2 wajib sebelum status "GO" penuh (`executive-architecture-review.md` §14) dan sudah lebih dari satu sesi berlalu (`CURRENT-PROJECT-STATE__1_.md` masih menandainya `❌ belum terpenuhi`, `DEVELOPMENT-ROADMAP.md` masih memakai "7 role"). **Rekomendasi:** ini adalah item Prioritas Tinggi tunggal yang paling murah untuk ditutup — sebaiknya diselesaikan sebelum sesi AI berikutnya menyentuh migration seed apa pun.
5. **`document-governance-baseline-register.md` sempat dilaporkan "tidak tersedia"** oleh `synchronization-report-adr-001.md` (poin 3, item Belum Selesai) pada sesi penyusunan laporan tsb, namun **kini tersedia** sebagai file (dua versi ditemukan: dasar dan revisi yang sudah menyertakan `architecture-decision-records.md`). **Rekomendasi:** tandai item "Belum Selesai" di `synchronization-report-adr-001.md` sebagai closed pada sesi berikutnya, tanpa mengedit laporan historisnya.
6. **Ambiguitas penomoran "ADR-"** antara `architecture-decision-records.md` (`ADR-001`–`ADR-025`, per topik) dan `decision-log.md` (`ADR-001`–`ADR-038`, kronologis) — sudah dicatat oleh kedua dokumen sumber sendiri sebagai potensi ambiguitas penamaan, **belum diputuskan** solusinya (mis. prefiks pembeda). Diteruskan di sini sebagai rekomendasi terbuka.
7. **Duplikasi Known Issues (`CHANGELOG.md`) dan Open Decisions (`decision-log.md`)** mencatat sebagian besar item yang sama dengan penomoran/redaksi berbeda, tanpa cross-reference kanonik. `foundation-validation-report.md` sudah merekomendasikan konsolidasi (prioritas Low) — Manifest ini meneruskan rekomendasi tsb, bukan mengambil keputusan konsolidasi.
8. **Ketidaksesuaian versi rilis proyek antar salinan `CHANGELOG.md`** — salinan dasar mencatat versi rilis `0.1.0`, salinan revisi mencatat `0.1.1` (rilis Governance Sync: penambahan `architecture-decision-records.md`, resolusi Known Issue #1). Manifest ini memakai `0.1.1` sebagai versi rilis proyek terkini secara konsisten di Bagian 1, 2, dan 4.

---

*Project Manifest ini adalah dokumen kontrol tertinggi seluruh dokumentasi proyek — bukan pengganti isi teknis dokumen mana pun (lihat Bagian 5, Source of Truth Index), melainkan indeks dan status agregat di atasnya. Wajib ditinjau ulang setiap kali Baseline, Open Decision, atau Phase proyek berubah material — konsisten dengan prinsip Software Configuration Management, Enterprise Architecture, dan Project Governance yang mendasari penyusunannya. Tidak ada isi dokumen sumber proyek lain yang diubah dalam penyusunan Manifest ini.*
