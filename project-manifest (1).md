# PROJECT MANIFEST
## Platform Web Real Estate Agency — Control Center Dokumentasi Proyek

> **Dokumen ini BUKAN** Project Overview, Changelog, Current Project State, atau Baseline Register — dokumen-dokumen tersebut tetap ada dan tetap otoritatif di areanya masing-masing (lihat Bagian 5). Project Manifest adalah **indeks resmi tunggal** di atas seluruhnya: titik masuk pertama yang wajib dibaca **sebelum** dokumen lain mana pun dibuka, oleh AI Coding Assistant (Claude, ChatGPT, Bolt.new, Cursor, GitHub Copilot) maupun kontributor manusia (Developer, QA, Technical Lead).

**Disusun dalam kapasitas gabungan:** Chief Technology Officer · Enterprise Software Architect · Software Configuration Manager · Technical Documentation Architect · Enterprise Project Governance Specialist · AI Development Workflow Architect

**Disusun berdasarkan:** seluruh dokumen governance terbaru yang tersedia — `architecture-decision-records.md`, `technology-decisions.md` (v1.2), `SYSTEM-ARCHITECTURE.md` (v1.2), `dependency-manifest.md` (v1.2), `PROJECT-CONSTITUTION.md` (v1.3 — memenuhi peran *Engineering Guidelines*), `development-playbook.md`/`AI-DEVELOPMENT-BLUEPRINT.md` (v1.2), `CURRENT-PROJECT-STATE.md`, `decision-log.md` (39 entry), `CHANGELOG.md` (rilis `0.1.2`), dan `document-governance-baseline-register.md`. Dokumen pendukung yang tidak direvisi pada siklus ini (PRD, ERD, API Spec, User Flow, SEO Spec, `AI-CONTEXT-PACK.md`, `DEVELOPMENT-ROADMAP.md`, `TASK-TEMPLATE.md`, `foundation-validation-report.md`, `executive-architecture-review.md`, `synchronization-report-adr-001.md`) tetap dirujuk pada versi terakhir yang tercatat sebelumnya.

**Jika terjadi konflik antar dokumen:** `architecture-decision-records.md` (ADR berstatus **Approved**) selalu menjadi keputusan tertinggi — lihat Bagian 5 (Source of Truth Index).

**Tidak ada dokumen sumber yang diubah dalam penyusunan Manifest ini.** Pertentangan yang ditemukan dicatat sebagai Governance Notes (Bagian 16), bukan diselesaikan sepihak.

---

# 1. Project Information

| Field | Value |
|---|---|
| **Project Name** | Platform Web Real Estate Agency (SaaS — Mujtahid Aktanto) |
| **Version** | Rilis proyek **`0.1.2`** (`CHANGELOG.md`) · `PROJECT-CONSTITUTION.md` **v1.3** · `technology-decisions.md`/`SYSTEM-ARCHITECTURE.md`/`dependency-manifest.md`/`development-playbook.md` **v1.2** · dokumen sumber bisnis/data/API v1.1 |
| **Current Phase** | **Foundation Phase — selesai secara substantif**, transisi **paralel-bertahap** ke **Architecture Alignment Phase** (Keputusan CTO: **GO WITH CONDITIONS**, `executive-architecture-review.md`, 27 Jul 2026 — snapshot point-in-time, statusnya diperbarui di Bagian 3 berdasarkan ADR terbaru) |
| **Overall Status** | 🟡 GO WITH CONDITIONS — boleh lanjut dengan syarat, lihat Bagian 3 & 7 |
| **Repository** | **Belum ada** — implementasi kode 0%, monorepo belum diinisialisasi, Sprint S0 (Foundation Infrastructure) belum dieksekusi (`CURRENT-PROJECT-STATE.md`) |
| **Last Updated** | 28 Juli 2026 — sinkronisasi resolusi `ADR-005` (Search Strategy, Approved) |

---

# 2. Executive Dashboard

| Dimensi | Indikator | Ringkasan |
|---|---|---|
| **Project Health** | 🟡 | 0 temuan Critical di seluruh audit; **2 keputusan arsitektur turunan** (Job Queue, Caching) masih OPEN — turun dari 3 setelah `ADR-005` (Search Strategy) diselesaikan; rekonsiliasi jumlah seed role (7 vs 8) masih belum tertutup. |
| **Current Milestone** | 🟡 | Sprint S0 (Foundation Infrastructure) — **siap dimulai, belum dieksekusi**. Tidak ada blocker untuk S0 murni scaffolding. |
| **Current Phase** | 🟡 | Foundation Phase selesai substantif (9/11 exit criteria) → Architecture Alignment Phase berjalan **paralel**: ERD Alignment & Functional Specification **boleh mulai sekarang**; Technical Specification & Module Planning S1+ **sebagian masih ditahan** (kini hanya menunggu Job Queue, bukan lagi Search Engine). |
| **Architecture Status** | 🟢 | **22 dari 25 ADR Approved** — `ADR-001` (Backend Architecture) 27 Jul 2026 dan `ADR-005` (Search Strategy) 28 Jul 2026 keduanya sudah terkunci. Hanya **3 ADR OPEN** tersisa: `ADR-006` (Job Queue, High), `ADR-008` (Maps, Medium — condong, menunggu konfirmasi biaya), `ADR-018` (Caching, Low). Naik dari 🟡 ke 🟢 sejak Manifest versi sebelumnya. |
| **Documentation Status** | 🟢 | Skor kesiapan fondasi **79/100** (`foundation-validation-report.md`, snapshot 27 Jul — belum menghitung dampak positif `ADR-005`) — **READY WITH MINOR REVISIONS**. 22 dokumen governance/desain tersedia, 0 pasangan dokumen berstatus Major/Critical Conflict. |
| **Baseline Status** | 🟡 | **7 dari ±22 dokumen** berstatus **Baseline** terkunci (`PROJECT-CONSTITUTION.md` v1.3, `PRD`, `TASK-TEMPLATE.md`, `decision-log.md`, `CHANGELOG.md` 0.1.2, `CURRENT-PROJECT-STATE.md`, `foundation-validation-report.md`). Sisanya `Approved`/`Draft`, sebagian besar terhalang oleh belum adanya nama individu Reviewer/Approver (lihat Bagian 7, item OD-06). |
| **Development Readiness** | 🟡 | Sprint S0 (scaffolding murni) **READY**. Sprint S1 ke atas (menyentuh backend/API/database) **NOT READY** — tertahan oleh 6 kondisi CTO di Bagian 7/12 `executive-architecture-review.md`, **kini 2 dari 6 terpenuhi** (ADR-001, ADR-005), naik dari 1 sebelumnya. |
| **AI Readiness** | 🟢 | Dinilai **Excellent** oleh CTO — `AI-CONTEXT-PACK.md`, `development-playbook.md` v1.2, `TASK-TEMPLATE.md`, dan `architecture-decision-records.md` memberi gerbang keputusan eksplisit bagi AI Coding Assistant. `Golden Rule` baru (poin 35) menegaskan Search Fase 1 tidak lagi memerlukan placeholder — salah satu aset paling matang di seluruh dokumentasi. |
| **Governance Health** | 🟢 | Sinkronisasi berantai `ADR-005` → `technology-decisions.md`/`SYSTEM-ARCHITECTURE.md`/`dependency-manifest.md`/`PROJECT-CONSTITUTION.md`/`development-playbook.md`/`CURRENT-PROJECT-STATE.md`/`decision-log.md`/`CHANGELOG.md`/`document-governance-baseline-register.md` dieksekusi **dalam satu siklus governance**, tidak ada dokumen yang tertinggal (pola yang sama seperti sinkronisasi `ADR-001`). Gap governance lama (rekonsiliasi seed role, nama Owner) tetap tercatat, tidak diselesaikan sepihak oleh Manifest ini. |

---

# 3. Current Phase

## Phase Sekarang
**Foundation Phase (selesai secara substantif) → Architecture Alignment Phase (paralel-bertahap, GO WITH CONDITIONS)** — keputusan resmi `executive-architecture-review.md`, 27 Juli 2026. Seluruh aktivitas proyek sejauh ini adalah produksi dokumentasi governance & desain; implementasi kode tetap 0% (`CURRENT-PROJECT-STATE.md`).

## Phase Sebelumnya
**Requirements & Design Documentation** — penyusunan `PROJECT-CONSTITUTION.md`, `PRD`, `ERD`, `API Specification`, `User Flow`, `SEO & Analytics Specification` (resolusi 7 konflik v1.0→v1.1), `SYSTEM-ARCHITECTURE.md`, `technology-decisions.md`, `dependency-manifest.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, `AI-CONTEXT-PACK.md`, `DEVELOPMENT-ROADMAP.md`, `TASK-TEMPLATE.md`, diikuti dua siklus Architecture Review Board (`ADR-001` 27 Jul, `ADR-005` 28 Jul).

## Phase Berikutnya
**Architecture Alignment Phase (penuh)** → ERD Alignment, Database Schema Alignment (kini termasuk kolom `search_vector`/`pg_trgm` dari `ADR-005`), API Alignment, User Flow Alignment, PRD Alignment (closing-the-loop) → **Functional Specification & UI Specification** → **Technical Specification** → **Module Planning** → **Sprint S0 execution** (Bolt.new/AI Coding Assistant diperbolehkan untuk scaffolding murni sejak sekarang).

## Entry Criteria — Architecture Alignment Phase (Penuh)
*(`executive-architecture-review.md` §12 — snapshot 27 Jul 2026, status diperbarui berdasarkan ADR terbaru per 28 Jul 2026; kini 4 dari 6 terpenuhi)*

| # | Syarat | Status |
|---|---|---|
| 1 | ERD stabil sebagai kandidat Baseline | ✅ Terpenuhi |
| 2 | API Specification stabil sebagai kandidat Baseline | ✅ Terpenuhi |
| 3 | Tidak ada konflik struktural pada model data/bisnis inti | ✅ Terpenuhi |
| 4 | Keputusan arsitektur backend terkunci | ✅ Terpenuhi — `ADR-001` Approved 27 Jul 2026 |
| 5 | Search Engine & Job Queue diputuskan | ⚠️ **Sebagian terpenuhi (update)** — `ADR-005` (Search Strategy) **Approved** 28 Jul 2026; `ADR-006` (Job Queue) **masih OPEN** |
| 6 | Reviewer/Approver bernama ditugaskan untuk sign-off Alignment | ❌ Belum terpenuhi |

**Entry parsial** untuk **ERD Alignment**, **Functional Specification**, dan kini **Modul 3 (Listing search/filter/autocomplete)** dinyatakan terpenuhi dan dapat dimulai penuh tanpa placeholder — hanya sisi lokasi peta (Modul 3/6, bergantung `ADR-008`) dan proses asinkron (`ADR-006`) yang masih memerlukan *configurable placeholder*.

## Exit Criteria — Foundation Phase
*(`executive-architecture-review.md` §11 — 9 dari 11 terpenuhi penuh, 2 sebagian, 1 belum; tidak berubah oleh `ADR-005` karena kriteria ini sudah dianggap terpenuhi sejak resolusi `ADR-001`)*

| # | Kriteria | Status |
|---|---|---|
| 1 | Kebutuhan bisnis inti terdokumentasi (PRD) | ✅ Terpenuhi |
| 2 | Model data terancang penuh (ERD, logis) | ✅ Terpenuhi |
| 3 | Kontrak API terdefinisi | ✅ Terpenuhi |
| 4 | Arsitektur terdefinisi tanpa konflik struktural besar | ✅ Terpenuhi — backend terkunci via `ADR-001` |
| 5 | Model keamanan & RBAC terdefinisi | ✅ Terpenuhi — dimensi terkuat proyek |
| 6 | Tech stack diputuskan | ⚠️ **Sebagian (membaik)** — Search Engine kini **Approved** (`ADR-005`); Job Queue & Caching masih OPEN; status dokumen `technology-decisions.md` masih Draft |
| 7 | Tata kelola AI development tersedia | ✅ Terpenuhi |
| 8 | Roadmap & sprint plan tersedia | ✅ Terpenuhi (Draft, substansi matang) |
| 9 | Mekanisme audit-diri (Decision Log, Changelog, Current State) | ✅ Terpenuhi |
| 10 | Tidak ada konflik Critical/blocking | ✅ Terpenuhi — 0 temuan Critical |
| 11 | Kepemilikan dokumen ditugaskan ke individu bernama | ❌ Belum terpenuhi — seluruhnya masih peran |

---

# 4. Current Baseline

| Document | Current Version | Baseline | Status | Owner | Last Review |
|---|---|---|---|---|---|
| PROJECT-CONSTITUTION.md | **1.3** | ✅ 1.3 | Baseline (BERLAKU) | Principal Software Architect (TBD) | 28 Jul 2026 |
| PRD-Real-Estate-Agency-Platform-v1.1.md | 1.1 | ✅ 1.1 | Baseline (Disetujui) | Senior Business Analyst / Product Manager (TBD) | 26 Jul 2026 |
| ERD-Skema-Database-...v1.1.md + Diagram | 1.1 | Kandidat 1.1 | Approved (belum Baseline formal) — target bertambah kolom `search_vector`/`pg_trgm` (`ADR-005`) | Database Architect (TBD) | 26 Jul 2026 |
| API-Specification-...v1.1.md | 1.1 | Kandidat 1.1 | Approved (belum Baseline formal) — mesin `/properties/search` kini terkunci, kontrak tidak berubah | API Architect (TBD) | 26 Jul 2026 |
| User-Flow-...v1.1.md | 1.1 | Kandidat 1.1 | Approved | Senior Business Analyst (TBD) | 26 Jul 2026 |
| SEO-Analytics-Specification-...v1.1.md | 1.1 | Kandidat 1.1 | Approved | Technical Lead (TBD) | 26 Jul 2026 |
| SYSTEM-ARCHITECTURE.md | **1.2** | TBD (menunggu pengesahan nama) | Approved, sinkron penuh dgn ADR — **belum** Baseline formal | Enterprise Solution Architect (TBD) | 28 Jul 2026 |
| architecture-decision-records.md | 1.0 | Belum ada | Draft (dokumen) — `ADR-001` & `ADR-005` entry-level **Approved** | Principal Enterprise Software Architect (TBD) | 28 Jul 2026 |
| technology-decisions.md | **1.2** | Belum ada | Draft — `ADR-006/008/018` masih OPEN, memblokir Baseline | Principal Software Architect (TBD) | 28 Jul 2026 |
| dependency-manifest.md | **1.2** | Belum ada | Draft — mengikuti status technology-decisions.md | Principal Software Architect (TBD) | 28 Jul 2026 |
| development-playbook.md (AI-DEVELOPMENT-BLUEPRINT.md) | **1.2** | Belum ada | Draft (acuan aktif operasional) | Principal Software Architect (TBD) | 28 Jul 2026 |
| AI-CONTEXT-PACK.md | 1.0 | Kandidat 1.0 | Approved | Technical Writer (TBD) | 27 Jul 2026 |
| DEVELOPMENT-ROADMAP.md | 1.0 | Belum ada | Draft (substansi matang) | Engineering Manager (TBD) | 27 Jul 2026 |
| TASK-TEMPLATE.md | 1.0 | ✅ 1.0 | Baseline (BERLAKU) | Staff Software Engineer (TBD) | 27 Jul 2026 |
| decision-log.md | 1.0 | 1.0 (per-ADR, berkembang — 39 entry) | Baseline (Living Document) | Principal Software Architect (TBD) | 28 Jul 2026 (entry `ADR-039`) |
| CHANGELOG.md | **0.1.2** | 0.1.2 | Baseline (Living Document) | Release Manager (TBD) | 28 Jul 2026 |
| CURRENT-PROJECT-STATE.md | 0.1 | 0.1 (per-sesi) | Baseline (Living Document) | Technical Project Manager (TBD) | 28 Jul 2026 |
| document-governance-baseline-register.md | 1.0 | Belum ada | Draft | Software Configuration Manager (TBD) | 28 Jul 2026 |
| foundation-validation-report.md | 1.0 | ✅ 1.0 | Baseline (Final — Quality Gate Deliverable) | AI Audit Panel | 27 Jul 2026 (snapshot, belum menghitung `ADR-005`) |
| executive-architecture-review.md | — (Keputusan CTO) | — | Final — Keputusan Resmi (setara otoritas Constitution untuk kelanjutan fase) | CTO | 27 Jul 2026 (snapshot point-in-time) |
| synchronization-report-adr-001.md | — (Artefak SCM) | — | Final — laporan sinkronisasi (point-in-time, hanya mencakup `ADR-001`) | CTO/SCM | 27 Jul 2026 |
| Database Schema (fisik) | — | — | **Planned** — belum ada (0% kode) | Database Architect (TBD) | — |
| Functional / UI / Technical Specification | — | — | **Planned** — belum ada file | TBD | — |

---

# 5. Source of Truth Index

| Area | Official Document | Priority |
|---|---|---|
| Governance Tertinggi / Engineering Guidelines | `PROJECT-CONSTITUTION.md` (v1.3) | **1 — Mengalahkan seluruh dokumen lain** |
| Business Requirement | `PRD-Real-Estate-Agency-Platform-v1.1.md` | 2 |
| Arsitektur & Keputusan Teknis per Topik (ADR) | `architecture-decision-records.md` | 3 — dibaca **sebelum** Technology Decisions; **selalu menang jika ada konflik** |
| Architecture (High-Level) | `SYSTEM-ARCHITECTURE.md` (v1.2) | 4 |
| Technology Stack & Rasionalisasi | `technology-decisions.md` (v1.2) | 5 |
| Dependency/Package Katalog | `dependency-manifest.md` (v1.2) | 6 |
| Database (Logis/ERD) | `ERD-Skema-Database-...v1.1.md` + Diagram | 7 |
| Database (Fisik/Migration) | **TBD** — migration file aktual setelah Sprint S0 | 7 (menyusul) |
| API Contract | `API-Specification-...v1.1.md` | 8 |
| User Interaction Flow | `User-Flow-...v1.1.md` | 9 |
| SEO & Analytics Strategy | `SEO-Analytics-Specification-...v1.1.md` | 9 |
| Keputusan — Jurnal Kronologis Lintas Proyek | `decision-log.md` (39 entry) | 10 |
| AI Instruction / Development Playbook | `development-playbook.md` (AI-DEVELOPMENT-BLUEPRINT.md, v1.2) | 11 |
| Context Ringkas Proyek | `AI-CONTEXT-PACK.md` §1–2 + `PRD` §1 | 12 |
| Roadmap & Sprint Plan | `DEVELOPMENT-ROADMAP.md` | 13 |
| Format/Unit Kerja Task | `TASK-TEMPLATE.md` | 14 |
| Riwayat Perubahan (Rilis) | `CHANGELOG.md` (rilis `0.1.2`) | 15 (riwayat, bukan keputusan) |
| Status Implementasi Nyata (Living) | `CURRENT-PROJECT-STATE.md` | — **wajib dibaca tiap sesi** |
| Validasi Kesiapan Fondasi | `foundation-validation-report.md` | — Quality Gate |
| Keputusan Eksekutif Kelanjutan Fase | `executive-architecture-review.md` | — Setara Constitution untuk urusan proses/fase |
| Tata Kelola Dokumen Itu Sendiri | `document-governance-baseline-register.md` | — |
| **Indeks & Kontrol Seluruh Dokumentasi** | **`project-manifest.md` (dokumen ini)** | **0 — dibaca sebelum semuanya** |

> **Validasi referensi silang:** Tidak ditemukan dokumen baru maupun perubahan nama dokumen pada siklus ini — seluruh 21 entri di atas (di luar Manifest ini sendiri) tetap merujuk nama file yang sama seperti Manifest versi sebelumnya. Perubahan hanya pada **nomor versi** yang dirujuk (lihat Bagian 14).

---

# 6. Architecture Decision Summary

> Ringkasan seluruh **25 ADR** di `architecture-decision-records.md` per 28 Juli 2026. **Impact Level** dinilai berdasarkan cakupan dampak lintas modul/dokumen (Critical = mengubah fondasi seluruh sistem, High = memengaruhi banyak modul/keputusan turunan, Medium = memengaruhi satu domain, Low = dampak terbatas/administratif).

| ADR ID | Title | Status | Decision Date | Impact Level |
|---|---|---|---|---|
| ADR-001 | Backend Architecture | **Approved** | 2026-07-27 | **Critical** |
| ADR-002 | Authentication Strategy | Approved | — | High |
| ADR-003 | Authorization & RBAC Strategy | Approved | — | High |
| ADR-004 | Database Strategy | Approved | — | High |
| ADR-005 | Search Strategy | **Approved (baru)** | 2026-07-28 | **High** |
| ADR-006 | Job Queue Strategy | 🔴 **OPEN** | — | High |
| ADR-007 | Email Provider | Approved | — | Medium |
| ADR-008 | Maps Provider | 🟡 **OPEN** (condong) | — | Medium |
| ADR-009 | Storage Strategy | Approved | — | Medium |
| ADR-010 | Deployment Strategy | Approved | — | High |
| ADR-011 | State Management Strategy | Approved | — | Medium |
| ADR-012 | API Architecture | Approved | — | High |
| ADR-013 | Error Handling Strategy | Approved | — | Medium |
| ADR-014 | Logging Strategy | Approved | — | Low |
| ADR-015 | Monitoring & Observability | Approved | — | Medium |
| ADR-016 | Testing Strategy | Approved | — | Medium |
| ADR-017 | Security Strategy | Approved | — | High |
| ADR-018 | Caching Strategy (level aplikasi) | 🔴 **OPEN** | — | Low |
| ADR-019 | File Upload Strategy | Approved | — | Low |
| ADR-020 | Notification Strategy | Approved | — | Medium |
| ADR-021 | Frontend Framework & Rendering Strategy | Approved | — | High |
| ADR-022 | Database Schema Conventions | Approved | — | Medium |
| ADR-023 | Multi-Tenancy Strategy | Approved (cakupan saat ini) | — | Low |
| ADR-024 | RBAC Role Model Scope | Approved | — | Medium |
| ADR-025 | Type Safety & Validation Strategy | Approved | — | Medium |

**Ringkasan status:** **22 Approved (88%)**, **3 OPEN (12%)** — ADR-006 (High), ADR-008 (Medium), ADR-018 (Low). Naik dari 21 Approved/4 OPEN pada Manifest versi sebelumnya. Urutan penyelesaian yang direkomendasikan (`architecture-decision-records.md` Bagian 8): **ADR-006 → ADR-008 (paralel, independen) → ADR-018 (setelah ADR-006)**.

### Detail ADR Terbaru — ADR-005 (Search Strategy)
- **Keputusan:** Strategi bertahap — PostgreSQL Full-Text Search + ekstensi `pg_trgm` untuk Fase 1 (MVP), migrasi terjadwal ke Typesense di Fase 2 begitu salah satu dari tiga kriteria ambang tercapai (volume listing aktif >±50.000, latensi p95 `/properties/search` >500ms, atau keluhan relevansi berulang ≥3 laporan/sprint).
- **Cross-reference:** `decision-log.md` `ADR-039`.
- **Catatan kondisional Board (belum ditutup):** (1) proyeksi volume listing 6–12 bulan perlu dikonfirmasi tim bisnis; (2) kapasitas DevOps/anggaran Typesense untuk Fase 2 perlu dikonfirmasi sebelum kriteria ambang tercapai.
- **Dampak langsung:** Menghapus status "OPEN" pada baris Search Engine di `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `PROJECT-CONSTITUTION.md`, dan `dependency-manifest.md` (§4.30); Modul 3 (search/filter/autocomplete) kini dapat dibangun penuh tanpa placeholder.

---

# 7. Open Decision Summary

> Dikonsolidasikan dari `executive-architecture-review.md` §9 (12 item, prioritas dampak×urgensi) dan `decision-log.md` §11 (8 baris asli). **Status kolom diperbarui** mencerminkan dokumen ter-sinkron terbaru (`technology-decisions__2_`, `PROJECT-CONSTITUTION__2_`, `architecture-decision-records__2_`) — lihat Governance Notes (Bagian 16) untuk ketidaksesuaian formatting yang belum tertutup di `decision-log.md` §11 itu sendiri.

## Decision yang Telah Selesai
| Decision ID | Topik | Resolusi | Tanggal |
|---|---|---|---|
| OD-01 | Arsitektur Backend/API | ✅ **RESOLVED** — `ADR-001`/`ADR-038`: Route Handlers + Supabase | 27 Jul 2026 |
| OD-03 | Search Engine (Postgres FTS vs Typesense/Elasticsearch) | ✅ **RESOLVED (baru)** — `ADR-005`/`ADR-039`: PostgreSQL FTS + `pg_trgm` Fase 1, migrasi terjadwal Typesense Fase 2 | 28 Jul 2026 |
| OD-08 | Vercel sebagai hosting resmi di Constitution | ✅ **RESOLVED** — Constitution v1.3 §4 sudah mencantumkan Vercel/ADR-010 | 27 Jul 2026 |
| OD-10 | Frasa usang state management ("SWR — pilih satu") | ✅ **RESOLVED** — `SYSTEM-ARCHITECTURE.md` §10 kini eksplisit TanStack Query, SWR dilarang | 27 Jul 2026 |

## Decision yang Masih Terbuka
| Decision ID | Priority | Status | Impact | Target Resolution |
|---|---|---|---|---|
| OD-02 — Jumlah seed role final (7 vs 8) | Tinggi | 🔴 **OPEN** | Migration seed Sprint S0 | Sebelum migration seed Sprint S0 ditulis |
| OD-04 — Job Queue (Edge Functions+cron vs BullMQ) | Tinggi | 🔴 **OPEN** (`ADR-006`) | Sitemap event-driven, reminder, sync counter, **mekanisme sinkronisasi index Fase 2 Search (`ADR-005`)** | Sebelum Sprint S6/S13 |
| OD-05 — Provider Maps (Google Maps vs Mapbox) | Sedang | 🟡 **Condong, menunggu konfirmasi biaya** (`ADR-008`) | Form lokasi (M3), peta proyek developer (M6) | Sebelum Sprint S4/S9 |
| OD-06 — Kepemilikan dokumen governance (nama individu) | Sedang | 🔴 **OPEN** | Prosedur Approval formal tidak berjalan tanpa ini | Sebelum Sprint S1 |
| OD-07 — Kebijakan soft-delete seragam | Sedang | 🔴 **OPEN** | Ambiguitas hard/soft-delete di 5 entitas | Sebelum Database Schema Alignment |
| OD-09 — Resend & Sentry belum sinkron ke System Architecture | Rendah | 🟡 **Sebagian** — perlu verifikasi §23 final | Risiko redaksional | Administratif |
| OD-11 — Model monetisasi platform | Rendah | 🔴 **OPEN** (bisnis) | Tidak memblokir selama `configurable` | Dapat ditunda |
| OD-12 — Threshold DBR final & kebijakan promosi/demosi Manager | Rendah | 🔴 **OPEN** (bisnis/kebijakan) | Tidak memblokir selama `configurable`/hard rule dipertahankan | Dapat ditunda hingga Sprint S14 |
| OD-13 — Caching Strategy (level aplikasi/Redis) | Rendah | 🔴 **OPEN** (`ADR-018`) | Rate limiting endpoint sensitif; tidak memblokir Sprint S0–S1 | Dapat ditunda melewati MVP, idealnya bersamaan OD-04 |

**Ringkasan:** dari 13 item (naik dari 12 — `ADR-018`/OD-13 kini eksplisit dicatat terpisah, sebelumnya implisit tergabung), **4 Resolved** (OD-01, OD-03, OD-08, OD-10), **1 Sebagian** (OD-09), **8 masih Open** — **2 di antaranya berprioritas Tinggi** (OD-02, OD-04), turun dari 3 pada Manifest versi sebelumnya (OD-03 kini Resolved).

## Prioritas Berikutnya
1. **OD-04 (Job Queue Strategy, `ADR-006`)** — prioritas tertinggi berikutnya untuk diselesaikan; arah Supabase Edge Functions + cron menguat setelah `ADR-001` dan `ADR-005` sama-sama mengasumsikan integrasi rapat Supabase. Menyelesaikan ini juga membuka jalan bagi mekanisme sinkronisasi index Search Fase 2.
2. **OD-02 (rekonsiliasi seed role 7 vs 8)** — item termurah untuk ditutup, direkomendasikan diselesaikan sebelum sesi AI berikutnya menyentuh migration seed apa pun (Sprint S0).
3. **OD-05 (Maps Provider, `ADR-008`)** — independen, dapat diputuskan paralel kapan pun, murni menunggu konfirmasi biaya bisnis.
4. **OD-13 (Caching Strategy, `ADR-018`)** — prioritas terendah, direkomendasikan diputuskan bersamaan OD-04 jika Job Queue mengarah ke BullMQ (butuh Redis).
5. **OD-06 (nama individu Owner/Reviewer/Approver)** — administratif, tapi menjadi prasyarat agar prosedur Approval formal (Bagian 15) benar-benar berjalan, bukan hanya berlaku di atas kertas.

---

# 8. Documentation Inventory

| # | Nama | Versi | Status | Purpose | Source of Truth | Baseline | Last Updated |
|---|---|---|---|---|---|---|---|
| 1 | PROJECT-CONSTITUTION.md | **1.3** | Baseline | Governance/engineering guidelines tertinggi | Governance | ✅ | 28 Jul 2026 |
| 2 | PRD-Real-Estate-Agency-Platform-v1.1.md | 1.1 | Baseline | Kebutuhan bisnis, 11 modul fungsional | Business Requirement | ✅ | 26 Jul 2026 |
| 3 | ERD-Skema-Database-...v1.1.md + Diagram | 1.1 | Approved | Desain skema database logis (37+ entitas, target bertambah `search_vector`) | Database (Logis) | Kandidat | 26 Jul 2026 |
| 4 | API-Specification-...v1.1.md | 1.1 | Approved | Kontrak REST API lengkap | API Contract | Kandidat | 26 Jul 2026 |
| 5 | User-Flow-...v1.1.md | 1.1 | Approved | Alur interaksi UI per role | User Interaction Flow | Kandidat | 26 Jul 2026 |
| 6 | SEO-Analytics-Specification-...v1.1.md | 1.1 | Approved | Strategi rendering, SEO, analytics | SEO & Analytics | Kandidat | 26 Jul 2026 |
| 7 | SYSTEM-ARCHITECTURE.md | **1.2** | Approved, sinkron ADR | Arsitektur teknis end-to-end (24 bagian) | Architecture (High-Level) | Belum | 28 Jul 2026 |
| 8 | architecture-decision-records.md | 1.0 | Draft (dok.) / 22 ADR Approved | 25 ADR per topik arsitektur | Arsitektur & Keputusan Teknis | Belum | 28 Jul 2026 |
| 9 | technology-decisions.md | **1.2** | Draft | Katalog stack & justifikasi, kini termasuk §4.30 Search Engine | Technology Stack | Belum | 28 Jul 2026 |
| 10 | dependency-manifest.md | **1.2** | Draft | Katalog package sah + Bolt.new toolchain + klarifikasi `pg_trgm` | Dependency Katalog | Belum | 28 Jul 2026 |
| 11 | development-playbook.md (AI-DEVELOPMENT-BLUEPRINT.md) | **1.2** | Draft (acuan aktif) | Panduan operasional harian AI Coding Assistant, 26 bagian | AI Instruction | Belum | 28 Jul 2026 |
| 12 | AI-CONTEXT-PACK.md | 1.0 | Approved | Context ringkas untuk reload tiap sesi AI | Context Ringkas | Kandidat | 27 Jul 2026 |
| 13 | DEVELOPMENT-ROADMAP.md | 1.0 | Draft | Roadmap 15 sprint (S0–S14) | Roadmap & Sprint Plan | Belum | 27 Jul 2026 |
| 14 | TASK-TEMPLATE.md | 1.0 | Baseline | Template task reusable | Format Unit Kerja | ✅ | 27 Jul 2026 |
| 15 | decision-log.md | 1.0 | Baseline (Living) | Jurnal kronologis seluruh keputusan (**39 ADR**, naik dari 38) | Decision (Jurnal) | ✅ | 28 Jul 2026 |
| 16 | CHANGELOG.md | **0.1.2** | Baseline (Living) | Riwayat perubahan proyek | History | ✅ | 28 Jul 2026 |
| 17 | CURRENT-PROJECT-STATE.md | 0.1 | Baseline (Living) | Status implementasi nyata per-sesi | Status Implementasi | ✅ | 28 Jul 2026 |
| 18 | document-governance-baseline-register.md | 1.0 | Draft | Meta-dokumen lifecycle/versi/ownership | Tata Kelola Dokumen | Belum | 28 Jul 2026 |
| 19 | foundation-validation-report.md | 1.0 | Baseline (Final) | Audit 17 dokumen, skor 79/100 (snapshot, belum menghitung `ADR-005`) | Validation | ✅ | 27 Jul 2026 |
| 20 | executive-architecture-review.md | — | Final — Keputusan CTO | GO WITH CONDITIONS, 6 syarat lanjut fase | Keputusan Eksekutif | — | 27 Jul 2026 |
| 21 | synchronization-report-adr-001.md | — | Final — Artefak SCM | Laporan sinkronisasi resolusi ADR-001 lintas 6 dokumen (belum mencakup ADR-005) | — | — | 27 Jul 2026 |
| 22 | **project-manifest.md** (dokumen ini) | **1.1** | Diperbarui | Indeks & control center seluruh dokumentasi | Indeks Tertinggi | Belum | 28 Jul 2026 |
| — | Database Schema (fisik) | — | Planned | Migration/DDL nyata (termasuk `search_vector`/`pg_trgm`) | Database (Fisik) | — | — |
| — | Functional Specification | — | Planned | Belum ada file | — | — | — |
| — | UI Specification | — | Planned | Belum ada file | — | — | — |
| — | Technical Specification | — | Ready with Notes (bahan tersebar) | Belum dikonsolidasi | — | — | — |

---

# 9. Dependency Map

```
PROJECT-CONSTITUTION.md (Engineering Guidelines — tertinggi, v1.3)
        ↓
Architecture Decision Records (architecture-decision-records.md — 22/25 Approved)
        ↓
Technology Decisions (v1.2)
        ↓
Dependency Manifest (v1.2)
        ↓
System Architecture (v1.2)
        ↓
ERD (Skema Database Logis) + ERD Diagram  — target bertambah search_vector/pg_trgm
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

**Dokumen operasional paralel** (tidak linear terhadap rantai di atas): `decision-log.md` (jurnal kronologis lintas dokumen, kini 39 entry), `CHANGELOG.md` (bergantung `CURRENT-PROJECT-STATE.md`, rilis `0.1.2`), `AI-CONTEXT-PACK.md` & `development-playbook.md` (bergantung seluruh dokumen sumber v1.1 + System Architecture + Technology Decisions), `TASK-TEMPLATE.md` (bergantung seluruh dokumen governance), `foundation-validation-report.md` (snapshot audit atas 17 dokumen), `executive-architecture-review.md` (keputusan dibangun di atas audit + Decision Log + Baseline Register).

**Perubahan hubungan pada siklus ini:** Tidak ada perubahan **arah** dependency (rantai tetap identik dengan Manifest versi sebelumnya). Perubahan yang terjadi murni pada **status simpul** — simpul "Architecture Decision Records" kini mengalirkan keputusan Search Engine yang sudah final ke simpul "Technology Decisions" dan "Dependency Manifest" di bawahnya, menghilangkan status "OPEN" yang sebelumnya mengalir ke rantai ERD/API/Module Planning untuk topik Search.

---

# 10. AI Reading Order

| # | Dokumen | Alasan Urutan |
|---|---|---|
| 1 | **project-manifest.md** (dokumen ini) | Indeks & status keseluruhan — wajib pertama |
| 2 | `CURRENT-PROJECT-STATE.md` | Kondisi implementasi *nyata* saat ini (living, per-sesi) |
| 3 | `PROJECT-CONSTITUTION.md` (v1.3) | Engineering Guidelines tertinggi — mengalahkan seluruhnya jika konflik |
| 4 | `architecture-decision-records.md` | Alasan di balik seluruh keputusan arsitektur/teknis (dibaca sebelum Technology Decisions) |
| 5 | `decision-log.md` | Jurnal kronologis keputusan lintas proyek, termasuk non-teknis (39 entry) |
| 6 | `technology-decisions.md` (v1.2) | Katalog stack resmi & justifikasi |
| 7 | `SYSTEM-ARCHITECTURE.md` (v1.2) | Arsitektur teknis end-to-end |
| 8 | `dependency-manifest.md` (v1.2) | Package/toolchain sah untuk di-install |
| 9 | `AI-CONTEXT-PACK.md` (§1–2) + `PRD` (§1) | Project Overview / konteks ringkas |
| 10 | `PRD-Real-Estate-Agency-Platform-v1.1.md` | Kebutuhan bisnis lengkap, 11 modul |
| 11 | `ERD-Skema-Database-...v1.1.md` + Diagram | Skema database logis |
| 12 | Database Schema (fisik, setelah tersedia) | Skema database nyata |
| 13 | `API-Specification-...v1.1.md` | Kontrak REST API |
| 14 | `User-Flow-...v1.1.md` | Alur interaksi UI per role |
| 15 | `SEO-Analytics-Specification-...v1.1.md` | Strategi rendering/SEO |
| 16 | `development-playbook.md` (AI-DEVELOPMENT-BLUEPRINT.md, v1.2) | Prosedur kerja harian AI Coding Assistant |
| 17 | `DEVELOPMENT-ROADMAP.md` | Roadmap & urutan sprint |
| 18 | `TASK-TEMPLATE.md` | Format unit kerja sebelum eksekusi task |
| 19 | `document-governance-baseline-register.md` | Rujukan status/versi/ownership jika ragu dokumen mana yang menang |
| 20 | `foundation-validation-report.md` + `executive-architecture-review.md` | Konteks audit & keputusan kelanjutan fase (dibaca saat butuh alasan strategis) |
| 21 | `CHANGELOG.md` (rilis `0.1.2`) | Riwayat perubahan (referensi, bukan keputusan) |

> **Tidak ada perubahan urutan** pada siklus ini — seluruh 21 posisi tetap identik dengan Manifest versi sebelumnya. Tidak ada dokumen baru yang perlu disisipkan; `ADR-005` adalah entri **di dalam** dokumen #4 (`architecture-decision-records.md`), bukan dokumen terpisah.

---

# 11. Development Readiness

| Area | Status | Catatan |
|---|---|---|
| **Architecture** | 🟡 IN PROGRESS | Backend (`ADR-001`) dan Search (`ADR-005`) terkunci; Job Queue/Caching (`ADR-006`/`018`) masih OPEN — membaik dari sebelumnya (3 area OPEN → 2 area OPEN) |
| **Database** | 🟡 IN PROGRESS | ERD logis Ready with Notes; soft-delete belum seragam (OD-07); skema fisik belum ada; target skema kini termasuk `search_vector`/`pg_trgm` |
| **API** | 🟡 IN PROGRESS | Konvensi inti matang (Ready with Notes); mesin `/properties/search` kini final; kedalaman endpoint modul pendukung & endpoint bergantung Maps belum tuntas |
| **Security** | 🟢 READY | Dinilai *Excellent* — enkripsi at-rest, RLS+middleware berlapis, rate limiting, audit trail; RLS untuk query search tidak memerlukan lapisan baru (native Postgres) |
| **Documentation** | 🟢 READY | Skor 79/100, 0 konflik Major/Critical; item minor tercatat di Bagian 16 |
| **Testing** | 🔴 NOT READY | Testing Strategy (`ADR-016`) tercatat, belum ada kerangka konkret (Vitest/RTL/Playwright terpilih, belum diimplementasi) |
| **Deployment** | 🔴 NOT READY | Vercel terpilih & terformalkan; 0% kode berarti belum ada deployment nyata |
| **Functional Specification** | 🔴 NOT READY | Belum ada file — dapat **dimulai sekarang**, tidak ada dependency ke Open Decision teknis |
| **UI Specification** | 🔴 NOT READY | Belum ada file — wireframe/desain visual belum tersedia |
| **Technical Specification** | 🟡 IN PROGRESS | Bahan baku tersebar (System Architecture + Technology Decisions + Dependency Manifest), perlu konsolidasi; kini hanya ditahan oleh OD-04 (sebelumnya juga OD-03) |
| **Module Planning** | 🟡 IN PROGRESS | `DEVELOPMENT-ROADMAP.md` sudah memenuhi fungsi untuk S0; Module Planning penuh S1+ ditahan sampai 6 kondisi CTO terpenuhi (2/6 terpenuhi) |

---

# 12. Pending Activities

Urutan kerja yang direkomendasikan (`executive-architecture-review.md` §10, §13, disesuaikan dengan resolusi `ADR-005`), dua jalur **paralel**:

```
Jalur A — Governance/Keputusan                Jalur B — Alignment/Spesifikasi
─────────────────────────────                 ────────────────────────────
Rekonsiliasi seed role (7 vs 8)                ERD Alignment (mulai sekarang,
        ↓                                       termasuk kolom search_vector)
ADR Job Queue (rekomendasi: Edge Functions)           ↓
        ↓                                       Functional Specification (mulai sekarang)
Konfirmasi biaya Maps Provider (OD-05)                ↓
        ↓                                       UI Specification / Screen Inventory
Tugaskan nama individu Owner/Reviewer                 ↓
        ↓                                       Technical Specification (konsolidasi)
Deklarasi kebijakan soft-delete seragam               ↓
        ↓                                       Database Schema Alignment (fisik)
ADR Caching (opsional, bersamaan Job Queue)           ↓
        ↓                                       API Alignment (kedalaman endpoint)
        └──────────────── keduanya bertemu di ─────────┘
                                ↓
                        Module Planning (S1+)
                                ↓
                        Sprint S0 Execution (Bolt.new — scaffolding, sudah boleh mulai)
                                ↓
                        Sprint S1 Execution (backend/API — menunggu Jalur A selesai)
                                ↓
                        Sprint S4/S5 Execution (Listing — search sudah final via ADR-005,
                                                  hanya lokasi peta menunggu ADR-008)
```

**Catatan:** Sprint S0 murni scaffolding (monorepo, CI/CD, styling dasar, aktivasi ekstensi `pg_trgm` di migration awal) **tidak menunggu** jalur mana pun dan boleh dieksekusi Bolt.new/AI Coding Assistant sekarang. **Perubahan pada siklus ini:** langkah "ADR Search Engine" pada Jalur A **dihapus** dari daftar pending (sudah selesai via `ADR-005`) — Jalur A kini lebih pendek satu langkah dibanding Manifest versi sebelumnya.

---

# 13. Risk Summary

## 🔴 High
1. **Job Queue belum diputuskan** (OD-04) — komponen yang secara fungsional disyaratkan SEO/API Specification (regenerasi sitemap event-driven, sinkronisasi counter) namun absen dari Official Technology Stack; risiko rework pada Modul 5/8/11 jika ditunda hingga tengah sprint. **Turun dari risiko gabungan "Search Engine & Job Queue"** — porsi Search Engine sudah tertutup via `ADR-005`.
2. **Rekonsiliasi jumlah seed role (7 vs 8) belum tertutup** — masih ditemukan di `DEVELOPMENT-ROADMAP.md` dan `CURRENT-PROJECT-STATE.md` versi terbaru; risiko dua sesi AI menghasilkan migration seed berbeda di Sprint S0.

## 🟡 Medium
3. **Provider Maps belum final** (OD-05) — memblokir implementasi penuh Modul 3 & 6 sebelum Sprint S4/S9 jika konfirmasi biaya tidak turun tepat waktu.
4. **Kepemilikan dokumen governance masih peran, bukan nama** (OD-06) — tanpa ini, prosedur Approval formal (`decision-log.md` §8, `document-governance-baseline-register.md` §11) hanya berlaku di atas kertas; Baseline formal pertama tidak dapat benar-benar disahkan.
5. **Kebijakan soft-delete belum seragam** (OD-07) — ambiguitas untuk 5 entitas berisiko migration awal ditulis dengan asumsi salah.
6. **Functional/UI Specification belum ada** — Module Planning tidak dapat menyentuh implementasi UI presisi tanpa ini, meski tidak memblokir Sprint S0.
7. **Kriteria ambang migrasi Search Fase 2 (`ADR-005`) berisiko tidak terpantau** — jika tim tidak menetapkan mekanisme monitoring volume listing/latensi p95 sejak Sprint S0, migrasi ke Typesense berisiko terlambat dieksekusi saat kriteria sudah terlampaui (**baru**, konsekuensi langsung `ADR-005`).

## 🟢 Low
8. Resend/Sentry — verifikasi akhir sinkronisasi ke `SYSTEM-ARCHITECTURE.md` §23 (OD-09).
9. Model monetisasi & threshold DBR final (OD-11, OD-12) — keputusan bisnis, aman ditunda selama tetap `configurable`.
10. Duplikasi `CHANGELOG.md` Known Issues vs `decision-log.md` Open Decisions tanpa cross-reference kanonik tunggal (lihat Governance Notes).
11. Ambiguitas penomoran "ADR-" antara `architecture-decision-records.md` (ADR-001–025) dan `decision-log.md` (ADR-001–**039**) — kosmetik, tidak memengaruhi isi keputusan.
12. Caching Strategy (`ADR-018`) belum diputuskan (OD-13) — prioritas terendah, tidak memblokir Sprint S0–S1.

---

# 14. Document Version Matrix

> Perbandingan versi **sebelum** dan **sesudah** siklus sinkronisasi `ADR-005` (28 Juli 2026), konsisten dengan `document-governance-baseline-register.md` Bagian 10.

| Document | Versi Sebelumnya | Versi Saat Ini | Jenis Perubahan | Pemicu |
|---|---|---|---|---|
| architecture-decision-records.md | 1.0 (Draft, `ADR-005` OPEN) | 1.0 (Draft, `ADR-005` **Approved**) | Perubahan status entri, bukan versi dokumen | Sesi Architecture Review Board 28 Jul 2026 |
| technology-decisions.md | 1.1 | **1.2** | MINOR — baris Search Engine berubah dari OPEN ke Approved, §4.30 ditambahkan | `ADR-005` |
| SYSTEM-ARCHITECTURE.md | 1.1 | **1.2** | MINOR — Component Diagram, Data Flow, Service Layer, Scalability, Risks, Open Questions diperbarui | `ADR-005` |
| dependency-manifest.md | 1.1 | **1.2** | MINOR — klarifikasi `pg_trgm` sebagai ekstensi native (bukan package npm), `typesense` dicatat kondisional | `ADR-005` |
| PROJECT-CONSTITUTION.md | 1.2 | **1.3** | MINOR — Riwayat Keputusan Arsitektur baris #9, Architecture Principles poin 8, Technical Constraints poin 6 | `ADR-005` |
| development-playbook.md (AI-DEVELOPMENT-BLUEPRINT.md) | 1.1 | **1.2** | MINOR — Golden Rule poin 35, AI Workflow, Module Development 22.3, Development Order 23.2 | `ADR-005` |
| CURRENT-PROJECT-STATE.md | 0.1 (snapshot 27 Jul) | 0.1 (snapshot **28 Jul**) | Update snapshot living document, bukan kenaikan versi | `ADR-005` |
| decision-log.md | 1.0 (38 entry) | 1.0 (**39 entry**) | Penambahan entry `ADR-039`, versi dokumen tidak naik (living document) | `ADR-005` |
| CHANGELOG.md | rilis 0.1.1 | rilis **0.1.2** | PATCH — rilis Governance Sync baru | `ADR-005` |
| document-governance-baseline-register.md | 1.0 (Draft) | 1.0 (Draft, isi disinkronkan) | Update isi (Baseline Register, Source of Truth, Dependency Matrix), versi dokumen tidak naik (masih Draft) | `ADR-005` |
| project-manifest.md | 1.0 | **1.1** | MINOR — sinkronisasi menyeluruh siklus `ADR-005` | `ADR-005` |
| PRD, ERD, API Spec, User Flow, SEO Spec | 1.1 | 1.1 (tidak berubah) | Tidak ada perubahan pada siklus ini | — |
| AI-CONTEXT-PACK.md, DEVELOPMENT-ROADMAP.md, TASK-TEMPLATE.md | 1.0 | 1.0 (tidak berubah) | Tidak ada perubahan pada siklus ini | — |
| foundation-validation-report.md, executive-architecture-review.md, synchronization-report-adr-001.md | 1.0 / — / — | 1.0 / — / — (tidak berubah, snapshot historis) | Tidak diedit — snapshot point-in-time dipertahankan apa adanya | — |

**Validasi konsistensi:** Seluruh nomor versi di atas **konsisten** dengan `document-governance-baseline-register.md` Bagian 10 (dikonfirmasi silang saat penyusunan Manifest ini). Tidak ditemukan referensi ke versi dokumen lama (v1.1 pada `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `dependency-manifest.md`, `development-playbook.md`, atau v1.2 pada `PROJECT-CONSTITUTION.md`) di dalam isi Manifest ini di luar tabel perbandingan eksplisit ini.

---

# 15. Baseline Status

## Current Active Baseline
Dokumen berstatus **Baseline** yang berlaku aktif saat ini (7 dokumen, tidak bertambah pada siklus ini — perubahan versi `PROJECT-CONSTITUTION.md`/`CHANGELOG.md` adalah kenaikan **versi Baseline**, bukan perubahan status):
- `PROJECT-CONSTITUTION.md` **v1.3** (BERLAKU)
- `PRD-Real-Estate-Agency-Platform-v1.1.md` v1.1
- `TASK-TEMPLATE.md` v1.0
- `decision-log.md` v1.0 (39 entry)
- `CHANGELOG.md` rilis **0.1.2**
- `CURRENT-PROJECT-STATE.md` v0.1 (snapshot 28 Jul 2026)
- `foundation-validation-report.md` v1.0 (Final)

## Previous Baseline (Superseded pada Siklus Ini)
| Dokumen | Baseline Lama | Baseline Baru | Tanggal Transisi |
|---|---|---|---|
| PROJECT-CONSTITUTION.md | v1.2 | **v1.3** | 28 Jul 2026 |
| CHANGELOG.md | rilis 0.1.1 | rilis **0.1.2** | 28 Jul 2026 |

> Sesuai `document-governance-baseline-register.md` Bagian 4.2, Baseline lama **tidak dihapus** — ditandai *Deprecated* pada hari transisi yang sama, lalu *Archived* setelah masa transisi wajar. Isi v1.2 `PROJECT-CONSTITUTION.md` dan rilis 0.1.1 `CHANGELOG.md` tetap tersedia sebagai riwayat di dalam dokumen masing-masing (`CHANGELOG.md` Aturan Wajib #1: history tidak boleh dihapus).

## Superseded Documents (Non-Baseline, Versi Naik Tanpa Status Baseline)
`technology-decisions.md` (1.1→1.2), `SYSTEM-ARCHITECTURE.md` (1.1→1.2), `dependency-manifest.md` (1.1→1.2), `development-playbook.md` (1.1→1.2) — keempatnya **belum pernah** berstatus Baseline formal (tetap Draft di kedua versi), sehingga tidak ada transisi Baseline→Deprecated yang perlu dicatat; hanya isi kontennya yang berubah mengikuti pola *Change Management Rules* (`document-governance-baseline-register.md` Bagian 11) secara informal (belum melalui Review & Approval formal bernama).

## Review Schedule
| Dokumen | Next Review | Trigger |
|---|---|---|
| architecture-decision-records.md | Setelah pengesahan tim formal + `ADR-006`/`008`/`018` diselesaikan | ADR OPEN berikutnya naik status |
| technology-decisions.md | Setelah Open Questions §9 diselesaikan (Job Queue, Maps, Caching) + Bolt.new eksplisit ditambahkan | Resolusi ADR OPEN tersisa |
| SYSTEM-ARCHITECTURE.md | Setelah Open Decision `ADR-006`/`008`/`018` diselesaikan | Resolusi ADR OPEN tersisa |
| dependency-manifest.md | Bersamaan technology-decisions.md | Sinkron |
| PROJECT-CONSTITUTION.md | Setiap keputusan bisnis besar turun | Keputusan bisnis/arsitektur baru |
| development-playbook.md | Saat AI Workflow/Rules berubah | Resolusi ADR OPEN berikutnya |
| CURRENT-PROJECT-STATE.md | Akhir setiap sesi development | Setiap sesi yang mengubah kode/keputusan nyata |
| decision-log.md | Berkelanjutan (tiap ADR baru) | ADR baru Approved |
| CHANGELOG.md | Setiap rilis versi baru | Rilis baru |
| document-governance-baseline-register.md | Setiap kali status/versi dokumen lain berubah material | Perubahan versi/status dokumen manapun |
| **project-manifest.md** (dokumen ini) | **Setelah resolusi `ADR-006` (Job Queue)** — direkomendasikan sebagai siklus sinkronisasi berikutnya | Open Decision berikutnya diselesaikan |

---

# 16. Governance Notes

> Konsisten dengan mandat Manifest ini: pertentangan yang ditemukan **dicatat**, **tidak diperbaiki sepihak** di sini. Poin 1–8 dipertahankan penuh dari Manifest versi sebelumnya (histori tidak dihapus); poin 9 baru ditambahkan pada siklus ini.

1. **`decision-log.md` §11 (Open Decisions) belum diformat-ulang meski `ADR-038` sudah ada.** `synchronization-report-adr-001.md` mengklaim baris #1 "ditandai resolved dan dirujuk-silang ke ADR-038", namun isi aktual `decision-log.md` §11 baris #1 **masih identik** dengan redaksi lama (belum ada penanda Resolved). **Rekomendasi:** update baris #1 §11 secara eksplisit (append status, bukan menghapus), agar klaim di laporan sinkronisasi konsisten dengan isi dokumen sebenarnya.
2. **Duplikasi versi dokumen di repositori proyek.** Beberapa dokumen ditemukan dalam dua salinan (versi dasar vs versi bersufiks upload berikutnya) dengan isi berbeda signifikan. Manifest ini secara konsisten memakai **versi dengan `Last Updated`/nomor versi paling akhir**. **Rekomendasi:** konsolidasikan menjadi satu salinan kanonik per dokumen di repositori, hapus/arsipkan salinan usang secara eksplisit.
3. **`executive-architecture-review.md` adalah snapshot point-in-time** yang disusun **sebelum** `ADR-001` maupun `ADR-005` disinkronkan penuh — dokumen tsb masih mencantumkan Open Decision #1 (Backend) dan #3 (Search Engine) sebagai belum terkunci. Manifest ini memperbarui status tersebut menjadi Resolved berdasarkan bukti dari dokumen yang lebih baru, **tanpa mengedit isi asli** `executive-architecture-review.md` itu sendiri. **Rekomendasi:** terbitkan addendum singkat pada `executive-architecture-review.md` yang mencatat resolusi Kondisi #1 dan #3 (Bagian 14 dokumen tsb), tanpa mengubah verdict aslinya.
4. **Rekonsiliasi jumlah seed role (7 vs 8) belum tertutup**, meski tercatat sebagai Kondisi #2 wajib sebelum status "GO" penuh dan sudah lebih dari dua sesi berlalu. **Rekomendasi:** ini tetap menjadi item Prioritas Tinggi tunggal yang paling murah untuk ditutup — sebaiknya diselesaikan sebelum sesi AI berikutnya menyentuh migration seed apa pun.
5. **`document-governance-baseline-register.md` sempat dilaporkan "tidak tersedia"** oleh `synchronization-report-adr-001.md` pada sesi penyusunan laporan tsb, namun **kini tersedia** dan sudah disinkronkan dua kali (siklus `ADR-001` dan `ADR-005`). **Rekomendasi:** tandai item "Belum Selesai" di `synchronization-report-adr-001.md` sebagai closed pada sesi berikutnya, tanpa mengedit laporan historisnya.
6. **Ambiguitas penomoran "ADR-"** antara `architecture-decision-records.md` (`ADR-001`–`ADR-025`, per topik) dan `decision-log.md` (`ADR-001`–`ADR-039`, kronologis) — sudah dicatat oleh kedua dokumen sumber sendiri sebagai potensi ambiguitas penamaan, **belum diputuskan** solusinya. Diteruskan di sini sebagai rekomendasi terbuka.
7. **Duplikasi Known Issues (`CHANGELOG.md`) dan Open Decisions (`decision-log.md`)** mencatat sebagian besar item yang sama dengan penomoran/redaksi berbeda, tanpa cross-reference kanonik. `foundation-validation-report.md` sudah merekomendasikan konsolidasi (prioritas Low) — Manifest ini meneruskan rekomendasi tsb.
8. **Ketidaksesuaian versi rilis proyek antar salinan `CHANGELOG.md`** (historis) — sudah tidak relevan pada siklus ini karena seluruh salinan yang diupload kini konsisten pada rilis `0.1.2`. Dipertahankan sebagai catatan historis, bukan temuan aktif.
9. **(Baru) Sinkronisasi `ADR-005` mengikuti pola identik dengan `ADR-001` — bukti governance chain berfungsi konsisten.** Sembilan dokumen (`architecture-decision-records.md`, `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `dependency-manifest.md`, `PROJECT-CONSTITUTION.md`, `development-playbook.md`, `CURRENT-PROJECT-STATE.md`, `decision-log.md`, `CHANGELOG.md`) plus `document-governance-baseline-register.md` diperbarui **dalam satu siklus governance berurutan**, masing-masing mengutip ADR yang sama (`ADR-005`/`ADR-039`) dan tanggal yang sama (28 Jul 2026) — tidak ditemukan drift versi/tanggal antar dokumen pada pemeriksaan silang untuk Manifest ini. **Tidak ada perubahan pada hierarki governance atau Source of Truth Index** (Bagian 5) — urutan kemenangan dokumen (`ADR` > `PROJECT-CONSTITUTION.md` > dokumen sumber v1.1 > `SYSTEM-ARCHITECTURE.md` > `technology-decisions.md` > `dependency-manifest.md` > `development-playbook.md`) tetap identik dengan siklus `ADR-001`. **Rekomendasi:** pertahankan pola sinkronisasi sembilan-dokumen ini sebagai *runbook* standar setiap kali sebuah ADR OPEN naik status menjadi Approved — cocok dijadikan checklist eksplisit di `document-governance-baseline-register.md` Bagian 11 (Change Management Rules) pada revisi berikutnya.

---

# 17. Executive Summary

**Sejak Project Manifest versi sebelumnya (v1.0, 28 Juli 2026, disusun tepat setelah resolusi `ADR-001`), satu siklus Open Decision tambahan telah diselesaikan: `ADR-005` (Search Strategy), disahkan Approved pada 28 Juli 2026 melalui sesi Architecture Review Board kedua.** Keputusannya: PostgreSQL Full-Text Search + ekstensi `pg_trgm` sebagai mesin pencari resmi Fase 1 (MVP) untuk `/properties/search` dan `/properties/autocomplete`, tanpa menambah komponen infrastruktur di luar Supabase — dengan migrasi terjadwal ke Typesense di Fase 2 begitu salah satu dari tiga kriteria ambang produksi tercapai (volume listing, latensi p95, atau keluhan relevansi berulang).

Resolusi ini disinkronkan secara berantai ke sembilan dokumen turunan dalam satu siklus governance yang sama: `technology-decisions.md` (v1.1→v1.2), `SYSTEM-ARCHITECTURE.md` (v1.1→v1.2), `dependency-manifest.md` (v1.1→v1.2), `PROJECT-CONSTITUTION.md` (v1.2→v1.3), `development-playbook.md` (v1.1→v1.2), `CURRENT-PROJECT-STATE.md` (snapshot diperbarui), `decision-log.md` (entry `ADR-039` ditambahkan, total 39 entry), `CHANGELOG.md` (rilis baru `0.1.2`), dan `document-governance-baseline-register.md` (Baseline Register & Source of Truth Matrix disinkronkan). Tidak satu pun dokumen tertinggal — pola yang identik dengan sinkronisasi `ADR-001` pada siklus sebelumnya (lihat Governance Notes poin 9).

**Dampak langsung:** jumlah ADR berstatus Approved naik dari 21 menjadi **22 dari 25** (88%), menyisakan **3 ADR OPEN** (`ADR-006` Job Queue — High, `ADR-008` Maps — Medium, `ADR-018` Caching — Low), turun dari 4 sebelumnya. Modul 3 (Listing search/filter/autocomplete) kini dapat dibangun **penuh tanpa placeholder** — satu-satunya bagian Modul 3 yang masih memerlukan *configurable placeholder* adalah form lokasi peta, yang bergantung pada `ADR-008`. Entry Criteria Architecture Alignment Phase naik dari 3/6 menjadi **4/6 terpenuhi**; Development Readiness kondisi CTO naik dari 1/6 menjadi **2/6 terpenuhi**.

**Tidak ada perubahan fase proyek maupun status Overall (tetap 🟡 GO WITH CONDITIONS)** — implementasi kode tetap 0%, Sprint S0 tetap satu-satunya milestone yang siap dieksekusi tanpa blocker. Risiko "Search Engine & Job Queue belum diputuskan" pada Manifest sebelumnya kini terpecah: porsi Search Engine **resolved**, porsi Job Queue **tetap High-risk** dan menjadi prioritas Open Decision berikutnya yang direkomendasikan. Dua item governance historis (rekonsiliasi seed role 7 vs 8, nama individu Owner/Reviewer) tetap belum tertutup dan tidak terpengaruh oleh siklus ini — keduanya tetap tercatat sebagai risiko aktif di Bagian 13.

**Rekomendasi langkah berikutnya:** selesaikan `ADR-006` (Job Queue Strategy) sebagai prioritas Open Decision tertinggi berikutnya, idealnya bersamaan dengan rekonsiliasi seed role (item termurah untuk ditutup) sebelum Sprint S0 menyentuh migration seed apa pun.

---

# 18. AI Usage Instructions

AI Coding Assistant apa pun (Claude, ChatGPT, Bolt.new, Cursor, GitHub Copilot, dsb.) **wajib**:

1. **Membaca `project-manifest.md` ini terlebih dahulu**, sebelum dokumen proyek lain mana pun — gunakan Bagian 10 (AI Reading Order) sebagai urutan lanjutan.
2. **Menggunakan Source of Truth Index (Bagian 5)** untuk menentukan dokumen mana yang berwenang menjawab suatu pertanyaan — jangan mencampur informasi dari dua dokumen yang membahas topik sama tanpa mengecek prioritas.
3. **Mengikuti ADR** (`architecture-decision-records.md`) — ADR berstatus `Approved` (termasuk `ADR-001` dan `ADR-005`) **mengikat** dan tidak boleh dilanggar; ADR berstatus `Open`/`Proposed` (`ADR-006`, `ADR-008`, `ADR-018`) **tidak boleh** diasumsikan/dipilih sendiri oleh AI.
4. **Tidak menggunakan dokumen berstatus `Deprecated`/`Archived`** (mis. `AI-DEVELOPMENT-BLUEPRINT.md` versi Session 3, 32 bagian, sudah digantikan versi 24/26 bagian) sebagai rujukan keputusan.
5. **Selalu menggunakan versi Baseline/versi terbaru** yang tercatat di Bagian 4 & 8 — jika ditemukan duplikasi upload dokumen yang sama, versi dengan `Last Updated` paling akhir (atau bernomor versi lebih tinggi) yang berlaku, bukan diasumsikan identik.
6. **Untuk implementasi `/properties/search`/`/properties/autocomplete`, gunakan langsung PostgreSQL FTS + `pg_trgm`** (`ADR-005`, Approved) — tidak perlu lagi menandai `// TODO: menunggu resolusi ADR-005`. Jangan menginstal Typesense/Elasticsearch/Algolia secara preventif; migrasi Fase 2 hanya sah setelah kriteria ambang tercapai **dan** disetujui manusia.
7. **Tidak mengambil keputusan arsitektur/bisnis baru secara sepihak** untuk item yang tercatat `OPEN` di Bagian 6 & 7 — laporkan sebagai temuan, tunggu ADR resmi dengan keterlibatan manusia berwenang.
8. **Tidak menulis kode backend/API produksi** sebelum memverifikasi status Open Decision terkait modul yang disentuh (lihat Bagian 6, 7 & 11) — Sprint S0 scaffolding murni terkecuali.
9. **Melaporkan, bukan memperbaiki sendiri**, setiap pertentangan baru antar dokumen yang ditemukan — catat sebagai Governance Note (pola Bagian 16), konsisten dengan `document-governance-baseline-register.md` §13 poin 6.

---

# 19. Quick Navigation

| Saya butuh... | Buka dokumen |
|---|---|
| Memahami proyek secara umum / konteks singkat | `AI-CONTEXT-PACK.md` §1–2 + `PRD` §1 |
| Aturan tetap tertinggi (role, security, tech stack) | `PROJECT-CONSTITUTION.md` (v1.3) |
| Alasan **mengapa** sebuah keputusan teknis diambil (per topik) | `architecture-decision-records.md` |
| Riwayat **seluruh** keputusan proyek (kronologis) | `decision-log.md` |
| Requirement bisnis / acceptance criteria per modul | `PRD-Real-Estate-Agency-Platform-v1.1.md` |
| Struktur tabel database & relasi | `ERD-Skema-Database-...v1.1.md` + Diagram |
| Kontrak endpoint API | `API-Specification-...v1.1.md` |
| Alur layar/interaksi per role | `User-Flow-...v1.1.md` |
| Strategi SEO/rendering/analytics | `SEO-Analytics-Specification-...v1.1.md` |
| Stack teknologi resmi & alasan pemilihan | `technology-decisions.md` (v1.2) |
| Daftar package yang boleh di-install | `dependency-manifest.md` (v1.2) |
| Arsitektur teknis end-to-end | `SYSTEM-ARCHITECTURE.md` (v1.2) |
| Cara AI Coding Assistant bekerja hari-hari | `development-playbook.md` (v1.2) |
| Urutan & isi sprint | `DEVELOPMENT-ROADMAP.md` |
| Format standar sebuah task | `TASK-TEMPLATE.md` |
| Status implementasi kode **saat ini** | `CURRENT-PROJECT-STATE.md` |
| Riwayat rilis/perubahan versi | `CHANGELOG.md` (rilis `0.1.2`) |
| Status/versi/ownership dokumen mana pun | `document-governance-baseline-register.md` |
| Hasil audit kesiapan fondasi & skor | `foundation-validation-report.md` |
| Keputusan resmi CTO soal kelanjutan fase | `executive-architecture-review.md` |
| Apa yang berubah saat ADR-001 disinkronkan | `synchronization-report-adr-001.md` |
| **Indeks semua dokumen & status proyek** | **`project-manifest.md` (dokumen ini)** |

---

*Project Manifest ini adalah dokumen kontrol tertinggi seluruh dokumentasi proyek — bukan pengganti isi teknis dokumen mana pun (lihat Bagian 5, Source of Truth Index), melainkan indeks dan status agregat di atasnya. Wajib ditinjau ulang setiap kali Baseline, Open Decision, atau Phase proyek berubah material — konsisten dengan prinsip Software Configuration Management, Enterprise Architecture, dan Project Governance yang mendasari penyusunannya. Tidak ada isi dokumen sumber proyek lain yang diubah dalam penyusunan Manifest ini. Versi 1.1 (28 Juli 2026) menyinkronkan seluruh isi dengan resolusi `ADR-005` (Search Strategy, Approved) — lihat Bagian 17 (Executive Summary) untuk ringkasan lengkap perubahan.*

---

# PROJECT MANIFEST UPDATE SUMMARY

| Item | Detail |
|---|---|
| **Dokumen yang berubah sejak versi sebelumnya** | `architecture-decision-records.md` (status entri `ADR-005`), `technology-decisions.md` (1.1→1.2), `SYSTEM-ARCHITECTURE.md` (1.1→1.2), `dependency-manifest.md` (1.1→1.2), `PROJECT-CONSTITUTION.md` (1.2→1.3), `development-playbook.md` (1.1→1.2), `CURRENT-PROJECT-STATE.md` (snapshot), `decision-log.md` (+entry `ADR-039`), `CHANGELOG.md` (0.1.1→0.1.2), `document-governance-baseline-register.md` (isi disinkronkan), `project-manifest.md` (1.0→1.1) — **11 dokumen** |
| **Open Decision yang telah diselesaikan** | OD-03 — Search Engine (`ADR-005`/`ADR-039`, Approved 28 Jul 2026): PostgreSQL FTS + `pg_trgm` Fase 1, migrasi terjadwal Typesense Fase 2 |
| **Baseline terbaru yang aktif** | `PROJECT-CONSTITUTION.md` v1.3, `CHANGELOG.md` rilis `0.1.2`, plus 5 Baseline living document lain (`PRD` v1.1, `TASK-TEMPLATE.md` v1.0, `decision-log.md` v1.0, `CURRENT-PROJECT-STATE.md` v0.1, `foundation-validation-report.md` v1.0) — **tidak berubah jumlahnya (7 dokumen Baseline)** |
| **Jumlah dokumen governance aktif** | **22 dokumen** tercatat di Documentation Inventory (Bagian 8), tidak bertambah/berkurang pada siklus ini — hanya 4 dalam status *Planned* (Database Schema fisik, Functional/UI/Technical Specification) |
| **Jumlah ADR aktif** | **22 dari 25 Approved** (naik dari 21), **3 OPEN** (`ADR-006` Job Queue, `ADR-008` Maps, `ADR-018` Caching) |
| **Jumlah Open Decision yang masih tersisa** | **8 dari 13 item** (naik dari 12 total karena `ADR-018`/OD-13 kini eksplisit dipisah) — **2 berprioritas Tinggi** (OD-02 seed role, OD-04 Job Queue), 2 Sedang (OD-05 Maps, OD-06 nama Owner) + OD-07 (soft-delete), 1 Sebagian (OD-09), 3 Rendah (OD-11, OD-12, OD-13) |
| **Rekomendasi langkah berikutnya** | Selesaikan **`ADR-006` (Job Queue Strategy)** sebagai prioritas Open Decision tertinggi berikutnya — arah Supabase Edge Functions + cron sudah menguat pasca `ADR-001`/`ADR-005`. Jalankan **paralel** dengan rekonsiliasi seed role (OD-02, item termurah) sebelum Sprint S0 menulis migration seed. Setelah `ADR-006` selesai, evaluasi `ADR-018` (Caching) bersamaan karena keduanya berpotensi sama-sama membutuhkan Redis. |
