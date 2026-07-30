# CURRENT PROJECT STATE
## Platform Web Real Estate Agency

> **CATATAN PENGGUNAAN — WAJIB DIBACA AI CODING ASSISTANT DI SETIAP SESI**
> Dokumen ini adalah **satu-satunya sumber kebenaran tentang apa yang SUDAH ADA secara fisik** di proyek ini (kode, tabel, endpoint, komponen) — bukan apa yang direncanakan/didesain di dokumen governance. Jika sebuah item tercatat **"Belum dibuat"**, AI **dilarang** berasumsi item tersebut sudah ada, sudah sebagian jadi, atau bisa "diisi mengarang" — perlakukan sebagai benar-benar kosong. Dokumen ini wajib **diperbarui di akhir setiap sesi development** yang mengubah kode nyata (bukan hanya dokumen).

---

# Project Information

| Field | Value |
|---|---|
| **Nama Project** | Platform Web Real Estate Agency (nama brand final belum ditentukan — placeholder `{nama_platform}`) |
| **Versi** | 0.1 (Pra-Development — belum ada rilis kode; lihat catatan versi di bawah) |
| **Tanggal Update** | 28 Juli 2026 |
| **Status** | 🟡 **Perencanaan & Dokumentasi Selesai — Implementasi Teknis Belum Dimulai.** Blocker arsitektur tertinggi (ADR-001 — Backend Architecture) telah **diselesaikan (Approved, 27 Juli 2026)**; **tidak ada lagi Open Decision yang memblokir dimulainya Sprint S0.** |
| **Development Phase** | **Pre-Phase 0** — seluruh dokumen governance (Constitution, PRD, ERD, API Spec, User Flow, SEO Spec, System Architecture, Technology Decisions, Dependency Manifest, AI Development Blueprint, AI Context Pack, Development Roadmap, Task Template, **Architecture Decision Records**) sudah final/tersedia. **Sprint S0 (Foundation Infrastructure) belum dieksekusi** — belum ada monorepo, belum ada kode, belum ada database fisik. Tidak ada perubahan fase pada sesi ini (implementasi kode tetap 0%) — pembaruan bersifat governance/keputusan arsitektur, bukan transisi fase. |
| **Milestone Berikutnya** | **Sprint S0 — Foundation Infrastructure.** Tidak berubah dari sebelumnya sebagai modul berikutnya yang direkomendasikan, namun kini secara resmi **tidak lagi memiliki dependency terhadap Open Decision arsitektur backend** (ADR-001 sudah Approved) — lihat bagian *ADR & Governance Snapshot* dan *Next Recommended Module*. |

---

# ADR & Governance Snapshot

> Bagian baru — merangkum status **Architecture Decision Records** (`architecture-decision-records.md`, v1.0) per 27–28 Juli 2026. Rincian penuh setiap ADR (Context/Decision/Rationale/Consequences) tetap berada di dokumen sumber; bagian ini hanya mencatat **status**, bukan menggandakan isinya.

## ADR Terbaru

**ADR-001 — Backend Architecture** — **Status naik dari OPEN menjadi APPROVED (dengan catatan)** pada **27 Juli 2026**, melalui sesi Architecture Review Board.

- **Keputusan:** Next.js Route Handlers sebagai BFF tipis, terintegrasi langsung dengan Supabase (Auth, Postgres, Storage) — **tidak ada** service backend Node.js terpisah (NestJS/Express) untuk cakupan proyek saat ini.
- **Cross-reference:** Dicatat ulang sebagai **ADR-038** di `decision-log.md` (dua rangkaian penomoran ADR yang berbeda, mengacu topik yang sama — bukan hubungan Supersedes/Superseded By).
- **Catatan kondisional dari Board (belum ditutup):**
  1. Batas eksekusi fungsi serverless Vercel (~10–60 detik) wajib didokumentasikan eksplisit di `SYSTEM-ARCHITECTURE.md`.
  2. **Bolt.new** sebagai bagian toolchain resmi proyek belum tercatat di `technology-decisions.md`/`dependency-manifest.md` — direkomendasikan ditambahkan secara eksplisit.
- **Dampak langsung ke dokumen ini:** Menghapus status "blocking tertinggi" ADR-001 yang sebelumnya tercatat di *Known Technical Debt* dan *Next Recommended Module* (lihat kedua bagian tsb di bawah, sudah diperbarui).

## Keputusan yang Telah Selesai (Approved)

Dari total **25 ADR** di `architecture-decision-records.md`, **21 ADR berstatus Approved** (termasuk ADR-001 yang baru diselesaikan pada sesi ini):

| ADR | Topik | Status | Catatan |
|---|---|---|---|
| ADR-001 | Backend Architecture | **Approved (baru)** | Next.js Route Handlers + Supabase, 27 Juli 2026 |
| ADR-002 | Authentication Strategy | Approved | Supabase Auth + JWT internal |
| ADR-003 | Authorization & RBAC Strategy | Approved | RBAC aplikasi + RLS dua lapis |
| ADR-004 | Database Strategy | Approved | PostgreSQL/Supabase, UUID PK, soft delete (3 tabel eksplisit), migration SQL murni |
| ADR-007 | Email Provider | Approved | Resend + React Email |
| ADR-009 | Storage Strategy | Approved | — |
| ADR-010 | Deployment Strategy | Approved | Vercel |
| ADR-011 | State Management Strategy | Approved | — |
| ADR-012 | API Architecture | Approved | — |
| ADR-013 | Error Handling Strategy | Approved | — |
| ADR-014 | Logging Strategy | Approved | — |
| ADR-015 | Monitoring & Observability | Approved | — |
| ADR-016 | Testing Strategy | Approved | — |
| ADR-017 | Security Strategy | Approved | — |
| ADR-019 | File Upload Strategy | Approved | — |
| ADR-020 | Notification Strategy | Approved | — |
| ADR-021 | Frontend Framework & Rendering Strategy | Approved | Next.js App Router |
| ADR-022 | Database Schema Conventions | Approved | — |
| ADR-023 | Multi-Tenancy Strategy | Approved (cakupan saat ini) | Evaluasi masa depan berstatus Proposed |
| ADR-024 | RBAC Role Model Scope | Approved | Formalisasi role & cakupan Manager |
| ADR-025 | Type Safety & Validation Strategy | Approved | — |

## Open Decision yang Tersisa

**4 ADR masih berstatus OPEN** — lihat rincian di bagian *Open Decision (ADR) yang Tersisa* di bawah: **ADR-005 (Search Strategy)**, **ADR-006 (Job Queue Strategy)**, **ADR-008 (Maps Provider)**, **ADR-018 (Caching Strategy)**. Tidak satu pun dari keempatnya memblokir Sprint S0.

---

# Overall Progress

> Kolom **Progress** mengukur *implementasi kode nyata yang berjalan*, bukan kelengkapan desain/dokumentasi. Sebuah modul yang desainnya 100% lengkap di PRD/ERD tetap tercatat **0%** di sini jika belum ada satu baris kode produksi pun.

| Module | Status | Progress |
|---|---|---|
| Governance & Documentation (Constitution, PRD, ERD, API Spec, User Flow, SEO Spec, System Architecture, Technology Decisions, Dependency Manifest, AI Dev Blueprint, AI Context Pack, Development Roadmap, Task Template, Architecture Decision Records) | Completed | 100% |
| Phase 0 — Foundation Infrastructure (monorepo, CI/CD, RBAC core schema, region data seed) | Not Started | 0% |
| Modul 1 — Authentication | Not Started | 0% |
| Modul 2 (dasar) — Agent Profile Core | Not Started | 0% |
| Modul 9 + 10 (dasar) — Admin Panel & RBAC Enforcement | Not Started | 0% |
| Modul 3 — Listing Management | Not Started | 0% |
| Modul 11 — SEO Foundation Hardening | Not Started | 0% |
| Modul 2 (ext.) — Buyer Account & Agent Reviews | Not Started | 0% |
| Modul 8 — Dashboard & Notifikasi | Not Started | 0% |
| Modul 6 — Developer Directory | Not Started | 0% |
| Modul 7 — DBR Scoring Calculator | Not Started | 0% |
| Modul 4 — Learning Center | Not Started | 0% |
| Modul 5 — Kalender Event | Not Started | 0% |
| Phase 4 — Production Readiness & Launch | Not Started | 0% |

**Ringkasan:** 1 dari 14 baris selesai (dokumentasi/governance). Implementasi kode: **0% secara keseluruhan.** Tidak ada perubahan fase implementasi pada sesi ini — pembaruan sesi ini murni governance (resolusi ADR-001).

---

# Existing Database

**Belum dibuat.** Belum ada project Supabase/PostgreSQL fisik yang diinisialisasi, belum ada migration yang dijalankan, sehingga belum ada satu tabel pun yang benar-benar berdiri di database.

> Skema **target** (rencana, bukan yang sudah ada) sudah didefinisikan lengkap di `ERD-Skema-Database-Real-Estate-Agency-v1.1.md` (37+ entitas) dan `ERD-Diagram-v1.1.mermaid`, dikelompokkan per modul: Identitas & RBAC (`users`, `roles`, `permissions`, `role_permissions`), Verifikasi Agen (`agent_verification_documents`), Profil Agen (`agent_profiles`, `agent_reviews`), Listing (`listings`, `listing_photos`, `listing_videos`, `listing_leads`, `listing_price_history`, `listing_views`, `listing_amenities`), Wilayah (`ref_provinces`, `ref_cities`, `ref_districts`, `ref_villages`), Developer (`developer_partners`, `developer_projects`, `developer_project_media`, `agent_project_claims`), Learning Center (`courses`, `course_lessons`, `quizzes`, `quiz_questions`, `quiz_options`, `enrollments`, `quiz_attempts`, `certificates`), Event (`events`, `event_registrations`), DBR (`dbr_simulations`, `dbr_config`), Dashboard (`notifications`), Admin/Sistem (`system_configs`, `audit_logs`), SEO (`url_redirects`). Ini adalah **rencana**, bukan status "sudah ada" — jangan diasumsikan sudah tereksekusi.

---

# Existing API

**Belum dibuat.** Belum ada Route Handler/service backend yang di-deploy atau dapat dipanggil. Tidak ada endpoint yang benar-benar hidup.

> Kontrak **target** endpoint sudah didefinisikan lengkap di `API-Specification-Real-Estate-Agency-Platform-v1.1.md` (13 bagian: Auth, Property/Listing, Search, Lead Generation, Communication, Financial/DBR Calculator, Notification, Wilayah, Integrasi Pihak Ketiga, SEO/Analytics, Modul Pendukung). Ini adalah **spesifikasi**, bukan endpoint yang sudah aktif. Pola implementasinya kini **terkunci** sebagai Route Handlers (`app/api/v1/**/route.ts`) menyusul ADR-001 (Approved, 27 Juli 2026) — tidak ada lagi opsi bercabang "Route Handlers vs service terpisah".

---

# Existing Components

**Belum dibuat.** Belum ada folder `components/` yang diinisialisasi — shadcn/ui belum di-generate, belum ada satu pun komponen dasar (Button, Input, Card, Modal, Table, Pagination, Badge, Upload, Toast, Sidebar, Navbar, dsb.) yang benar-benar ada di repo.

---

# Existing Layouts

**Belum dibuat.** Belum ada `app/layout.tsx` root maupun layout per route group (`(public)`, `(auth)`, `(dashboard)`, `(admin)`) yang diimplementasikan.

---

# Existing Hooks

**Belum dibuat.** Belum ada folder `hooks/` maupun custom hook domain (`useListingForm`, `useDbrCalculator`, dsb.) yang diimplementasikan.

---

# Existing Services

**Belum dibuat.** Belum ada layer service (`*.service.ts`) untuk modul manapun (Auth, Listing, DBR, RBAC, dsb.) yang diimplementasikan.

---

# Existing Utilities

**Belum dibuat.** Belum ada folder `lib/utils/` atau fungsi helper (format tanggal, konversi tenor tahun→bulan, slug generator, dsb.) yang diimplementasikan.

---

# Existing Middleware

**Belum dibuat.** Belum ada `auth.middleware`, `rbac.middleware`, atau `rate-limit.middleware` yang diimplementasikan — termasuk belum ada Next.js middleware (`middleware.ts`) untuk proteksi route.

---

# Existing Authentication

**Belum dibuat.** Belum ada integrasi Supabase Auth, belum ada JWT internal platform yang diterbitkan, belum ada alur registrasi/login/OTP/OAuth yang berjalan.

> Desain lengkap sudah tersedia (Supabase Auth + JWT internal, email/password + OTP, Google OAuth2 dengan verifikasi server-side, access token 15–60 menit + refresh token httpOnly 30 hari) — lihat `PROJECT-CONSTITUTION.md` Bagian 10, `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 13, `technology-decisions.md` 4.8. Ini adalah **desain**, belum ada implementasi.

---

# Existing Authorization

**Belum dibuat.** Belum ada tabel `roles`/`permissions`/`role_permissions` fisik, belum ada middleware RBAC yang berjalan, belum ada RLS policy yang diterapkan di Supabase manapun (karena project Supabase-nya sendiri belum ada).

> Desain lengkap sudah tersedia: 8 role (Superadmin, Manager, Admin, Instructor, Agen, Developer Partner, Buyer, Guest), model `granted_scope` (`own`/`all`/`none`), hard rule ownership `agent_id`, RBAC middleware + RLS berlapis — lihat `PROJECT-CONSTITUTION.md` Bagian 3 & 11, `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 14. Ini adalah **desain**, belum ada implementasi.

---

# Existing Folder Structure

**Belum dibuat.** Belum ada monorepo fisik — belum ada folder `/apps/web`, `/apps/api`, `/packages/shared-types`, `/packages/region-data` yang diinisialisasi di repository manapun.

> Struktur **target** sudah didefinisikan lengkap di `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 7 dan `SYSTEM-ARCHITECTURE.md` Bagian 6 (route groups `(public)/(auth)/(dashboard)/(admin)`, `components/ui/` vs `components/features/{module}/`, `lib/`, `hooks/`, dsb.). Struktur ini **wajib** diikuti persis saat Sprint S0 dieksekusi — bukan didesain ulang saat itu. Menyusul ADR-001 (Approved), opsi `/apps/api` terpisah **dihapus** dari struktur target — seluruh implementasi API berada di dalam `apps/web`.

---

# Active Dependencies

**Belum ada dependency yang terinstal** — belum ada `package.json`/lockfile di repository manapun, sehingga tidak ada dependency yang "aktif" secara teknis saat ini.

> Daftar dependency **resmi yang akan digunakan** begitu Sprint S0 dimulai sudah dikatalogkan lengkap di `dependency-manifest.md` (status: Draft, menunggu pengesahan tim), mengikuti keputusan `technology-decisions.md`. Ringkasan stack inti yang akan diinstal pertama kali (Phase 1 — Core Framework, per Installation Priority `dependency-manifest.md` Bagian 6): `next`, `react`, `react-dom`, `typescript`, kemudian `@supabase/supabase-js` + `@supabase/ssr` (Phase 2), lalu `tailwindcss` + shadcn/ui (Phase 4), `@tanstack/react-query` + `zustand` (Phase 5), `react-hook-form` + `zod` (Phase 6). Ini adalah **rencana instalasi**, bukan status terinstal.

---

# Pending Modules

Seluruh modul berikut **belum dibuat sama sekali** (0% implementasi), diurutkan sesuai Module Order resmi di `DEVELOPMENT-ROADMAP.md`:

1. Phase 0 — Foundation Infrastructure (RBAC core schema, region data seed, monorepo, CI/CD)
2. Modul 1 — Authentication
3. Modul 2 (dasar) — Agent Profile Core
4. Modul 9 + 10 (dasar) — Admin Panel & RBAC Enforcement
5. Modul 3 — Listing Management (CRUD, search, filter, CTA WhatsApp)
6. Modul 11 — SEO Foundation Hardening (sitemap, GTM/GA4, Search Console)
7. Modul 2 (ext.) — Buyer Account & Agent Reviews
8. Modul 8 — Dashboard & Notifikasi
9. Modul 6 — Developer Directory
10. Modul 7 — DBR Scoring Calculator
11. Modul 4 — Learning Center
12. Modul 5 — Kalender Event
13. Phase 4 — Production Readiness & Launch

---

# Known Technical Debt

**Belum ada technical debt kode** — wajar, karena belum ada kode yang ditulis.

Namun tercatat beberapa **"debt keputusan governance"** (ketidaksinkronan antar dokumen desain). Status per 28 Juli 2026:

| # | Ketidaksinkronan | Dokumen Terdampak | Status |
|---|---|---|---|
| 1 | ~~Arsitektur backend (Next.js Route Handlers vs service Node terpisah) sudah condong diputuskan ke Route Handlers + Supabase di `technology-decisions.md`, namun `PROJECT-CONSTITUTION.md` & `SYSTEM-ARCHITECTURE.md` masih mencatatnya sebagai opsi terbuka.~~ | `PROJECT-CONSTITUTION.md` Bagian 4, `SYSTEM-ARCHITECTURE.md` Bagian 4 & 23 | ✅ **RESOLVED** — diputuskan **Approved** via **ADR-001** / `decision-log.md` **ADR-038** (Architecture Review Board, 27 Juli 2026); disinkronkan ke `technology-decisions.md` dan `SYSTEM-ARCHITECTURE.md`. `PROJECT-CONSTITUTION.md` §4 **belum** disinkronkan (di luar cakupan sesi sinkronisasi 27 Juli 2026 — direkomendasikan sebagai tindak lanjut terpisah). |
| 2 | State management server-state (React Query vs SWR) sudah diputuskan tegas ke TanStack Query di `technology-decisions.md`, namun `SYSTEM-ARCHITECTURE.md` Bagian 10 masih menulis "pilih salah satu, konsisten". | `SYSTEM-ARCHITECTURE.md` Bagian 10 | Open — non-blocking |
| 3 | Vercel sebagai hosting resmi dipakai di `SYSTEM-ARCHITECTURE.md` & `technology-decisions.md`, tapi belum tercatat sebagai keputusan formal di `PROJECT-CONSTITUTION.md`. | `PROJECT-CONSTITUTION.md` Bagian 4 | Open — non-blocking |
| 4 | Google Maps Platform ditetapkan final di `technology-decisions.md`, namun `PROJECT-CONSTITUTION.md`/`API-Specification` masih mencatat provider Maps sebagai "belum final" (vs Mapbox). | `PROJECT-CONSTITUTION.md`, `API-Specification-v1.1.md` §13/§9.1 | Open — terkait **ADR-008** (lihat *Open Decision* di bawah) |
| 5 | Search Engine (Typesense/Elasticsearch) & Job Queue (BullMQ vs Supabase Edge Functions) belum masuk *Official Technology Stack*. | `technology-decisions.md` Bagian 9 poin 2 & 4 | Open — terkait **ADR-005** & **ADR-006** |
| 6 | Provider Email (Resend) & Monitoring (Sentry) sudah diputuskan di `technology-decisions.md`, belum disinkronkan ke `SYSTEM-ARCHITECTURE.md` yang masih mencatatnya kosong. | `SYSTEM-ARCHITECTURE.md` Bagian 23 poin 10–11 | Open — non-blocking |

---

# Open Decision (ADR) yang Tersisa

> Sumber: `architecture-decision-records.md` Bagian 4, 7 (Impact Analysis), dan 8 (Implementation Order). Keempat ADR berikut masih **OPEN** — AI Coding Assistant **tidak boleh berasumsi** salah satu opsi sudah dipilih untuk area ini.

| Prioritas Penyelesaian | ADR | Topik | Blocking Sprint | Rekomendasi (belum final) |
|---|---|---|---|---|
| 1 (tertinggi berikutnya) | **ADR-006** | Job Queue Strategy | S6, S13 | Supabase Edge Functions + cron — arah ini menguat setelah ADR-001 dikunci ke Route Handlers + Supabase |
| 2 (paralel dengan #1) | **ADR-005** | Search Strategy | **S5** | PostgreSQL full-text/trigram sebagai MVP Fase 1, dengan kriteria ambang migrasi eksplisit ke Typesense |
| 3 (independen, kapan pun) | **ADR-008** | Maps Provider | S4, S9 | Menunggu konfirmasi biaya dari tim bisnis — sepenuhnya independen dari ADR lain |
| 4 (terendah) | **ADR-018** | Caching Strategy (Redis) | Tidak ada sprint spesifik | Direkomendasikan diputuskan **setelah** ADR-006, agar keputusan Redis (jika ada) diambil satu kali untuk kedua kebutuhan |

**Tidak ada satu pun dari keempat ADR ini yang memblokir Sprint S0.** ADR-005 wajib diselesaikan sebelum Sprint S5; ADR-006 sebelum Sprint S6/S13; ADR-008 sebelum Sprint S4/S9 (khususnya form lokasi & Developer Directory). Sesuai `architecture-decision-records.md` Bagian 10 (AI Usage Rules), AI **wajib berhenti dan meminta keputusan eksplisit** dari Technical Lead/manusia berwenang jika sebuah task menyentuh area yang dinaungi ADR berstatus OPEN — bukan memilih sendiri salah satu opsi.

---

# Readiness Snapshot (Governance)

> Ringkasan status kesiapan per `executive-architecture-review.md` (Keputusan CTO, 27 Juli 2026) — **Final Verdict: GO WITH CONDITIONS**. Snapshot ini bersifat point-in-time dan tidak diedit ulang di dokumen sumbernya; bagian ini mencatat kondisi mana yang **sudah** dan **belum** terpenuhi per 28 Juli 2026.

**Baseline Readiness per dokumen:**

| Dokumen | Status |
|---|---|
| PRD | **Ready** (sudah Baseline) |
| ERD | **Ready with Notes** — perlu kebijakan soft-delete seragam & verifikasi audit-column |
| API Specification | **Ready with Notes** — kedalaman endpoint modul pendukung perlu diperluas; sebagian bergantung pada ADR-008 |
| Technology Decisions | **Not Ready** — status dokumen masih "Draft"; 3 sub-keputusan (Maps/ADR-008, Search/ADR-005, Job Queue/ADR-006) belum tuntas |
| System Architecture | **Ready with Notes** — sebagian frasa usang (state management, Resend/Sentry) masih perlu disinkronkan; keputusan backend (§4) **kini terkunci** menyusul ADR-001 |

**Kondisi GO WITH CONDITIONS (Bagian 14 `executive-architecture-review.md`) — status per 28 Juli 2026:**

1. ✅ **ADR resmi arsitektur Backend/API dibuat & disahkan** — **TERPENUHI** (ADR-001/ADR-038, Approved 27 Juli 2026).
2. ❌ Jumlah seed role (7 vs 8) direkonsiliasi — **belum terpenuhi**.
3. ❌ Strategi Search Engine Fase 1 diputuskan (ADR-005) — **belum terpenuhi**.
4. ❌ Mekanisme Job Queue diputuskan (ADR-006) — **belum terpenuhi**.
5. ❌ Kebijakan soft-delete seragam dideklarasikan untuk seluruh entitas ERD — **belum terpenuhi**.
6. ❌ Minimal 4 nama individu ditugaskan (Technical Lead, Product Owner, Database Architect, API Architect) — **belum terpenuhi**.

**Kesimpulan readiness terbaru:** 1 dari 6 kondisi GO WITH CONDITIONS kini terpenuhi (kondisi #1, sebelumnya dinilai sebagai satu-satunya blocker arsitektur berkategori Critical). Status proyek secara keseluruhan **tetap GO WITH CONDITIONS** — Sprint S0 (scaffolding murni) tidak pernah terblokir oleh kondisi manapun dan tetap dapat dimulai kapan saja; Sprint S1 ke atas (menyentuh backend/API/database) tetap menunggu penyelesaian kondisi #2–#6, khususnya #2 dan #5 yang berdampak langsung ke migration Sprint S0.

---

# Next Recommended Module

## Sprint S0 — Foundation Infrastructure

**Alasan ini yang paling logis dikerjakan berikutnya:**

1. **Prasyarat murni teknis untuk seluruh modul lain.** Sesuai Module Order di `DEVELOPMENT-ROADMAP.md`, tidak ada satu pun modul bisnis (Auth, Profil, Listing, dst.) yang dapat dibangun tanpa: (a) skema `roles`/`permissions`/`role_permissions` — karena `users.role_id` bergantung padanya, dan (b) data referensi wilayah (`ref_provinces/cities/districts/villages`) — karena field lokasi cascading di Listing & Developer Projects bergantung padanya.
2. **Keputusan mahal harus dikunci di awal.** Struktur monorepo, strategi rendering (SSR/SSG per route group), dan pipeline CI/CD adalah keputusan yang sangat mahal diubah setelah kode lain dibangun di atasnya — sejalan dengan prinsip *"keputusan arsitektur mahal diambil di Fase 1, bukan ditambal belakangan"* di `PROJECT-CONSTITUTION.md`.
3. **Tidak ada modul bisnis yang bisa diuji end-to-end tanpanya** — bahkan Sprint S1 (Authentication) butuh tabel `roles` untuk bisa menerbitkan `role_id` saat registrasi.
4. **Kini tidak lagi ada risiko drift arsitektur backend.** Sebelumnya, memulai Sprint S0 dengan ADR-001 masih OPEN membawa risiko sesi AI berbeda mengasumsikan pola backend yang berbeda-beda; risiko ini sudah hilang menyusul ADR-001 Approved (27 Juli 2026) — Sprint S0 dapat dieksekusi dengan konteks backend yang sudah pasti (Route Handlers + Supabase, tanpa `/apps/api` terpisah).

**Cakupan konkret Sprint S0** (per `DEVELOPMENT-ROADMAP.md`):
- Inisialisasi monorepo (`apps/web`, `packages/shared-types`), Next.js + TypeScript `strict: true`.
- Setup Tailwind v4 + shadcn/ui, ESLint/Prettier/Husky.
- CI pipeline (lint + type-check + test) di GitHub Actions.
- Project Supabase + migration awal: `users`, `roles`, `permissions`, `role_permissions` (seed role & permission dasar — **jumlah baris seed role wajib direkonsiliasi lebih dulu, lihat *Readiness Snapshot* kondisi #2**).
- Seed `ref_provinces/cities/districts/villages` dari data resmi.
- Skeleton route group `(public)/(auth)/(dashboard)/(admin)` + middleware skeleton (`auth`/`rbac`/`rate-limit`).
- Scaffold environment variables.

**Acceptance Criteria S0** (dari `DEVELOPMENT-ROADMAP.md`): CI pipeline lolos pada commit kosong; migration dapat dijalankan ulang dari nol tanpa error; seed data wilayah terverifikasi jumlah barisnya sesuai sumber resmi.

**Catatan tambahan sesi ini:** Meskipun ADR-001 sudah Approved, migration seed `roles` di Sprint S0 sebaiknya **menunggu rekonsiliasi jumlah role (7 vs 8)** — lihat *Readiness Snapshot* kondisi #2 dan Known Technical Debt — agar tidak ditulis dua kali.

---

# AI Session Rules

Aturan berikut **wajib dibaca dan dipatuhi** AI Coding Assistant sebelum mengerjakan modul berikutnya — berlaku mulai saat kode pertama kali ditulis (Sprint S0) dan seterusnya:

1. **Jangan mengubah modul yang sudah selesai** (status *Completed* di tabel Overall Progress) kecuali task eksplisit memang menargetkan modul tsb (Bug Fix/Enhancement dengan Task Template terisi).
2. **Jangan menghapus file** yang sudah ada di repository tanpa instruksi eksplisit dan alasan yang tercatat di Task Template terkait.
3. **Jangan rename folder/file** yang sudah menjadi bagian struktur final (`AI-DEVELOPMENT-BLUEPRINT.md` Bagian 7) tanpa persetujuan eksplisit — ini termasuk kategori refactor besar yang butuh task terpisah.
4. **Gunakan komponen reusable yang sudah ada** (`components/ui/`, `components/features/{module}/`) sebelum menulis komponen baru yang fungsinya serupa — cek dokumen ini (bagian *Existing Components*) dulu sebelum membuat apa pun.
5. **Tambahkan/perbarui dokumentasi** (ERD, API Specification, dan dokumen ini — *CURRENT PROJECT STATE*) setiap kali membuat fitur baru, tabel baru, atau endpoint baru — jangan menunda ke sesi berikutnya.
6. **Jangan membuat duplicate logic** — satu business logic, satu skema validasi, satu definisi tipe hanya boleh ada di satu lokasi sumber kebenaran (`lib/`, `packages/shared-types`).
7. **Pastikan backward compatibility** — perubahan pada endpoint/skema/komponen yang sudah dipakai modul lain tidak boleh breaking tanpa keputusan eksplisit dan kenaikan versi.
8. **Baca dokumen ini (*CURRENT PROJECT STATE*) di awal SETIAP sesi**, sebelum membaca dokumen governance lain — dokumen ini menjawab "apa yang benar-benar sudah ada", sedangkan dokumen governance lain menjawab "bagaimana seharusnya sesuatu dibangun".
9. **Jangan mengasumsikan progres yang tidak tercatat di sini** — jika sebuah modul tertulis "Not Started"/"Belum dibuat", perlakukan sebagai benar-benar kosong meski tampak "mungkin sudah ada sebagian" dari konteks percakapan lain.
10. **Jangan membangun modul di luar urutan *Pending Modules*** tanpa alasan eksplisit yang disetujui — ikuti Sprint Plan `DEVELOPMENT-ROADMAP.md`, jangan melompat ke Fase 2/3 sebelum Fase 1 solid.
11. **Perbarui dokumen ini di akhir sesi** — setiap kali sesi development mengubah status modul, menambah tabel/endpoint/komponen/hook/service/dependency nyata, bagian terkait di dokumen ini wajib diperbarui sebelum sesi ditutup, bukan "nanti".
12. Jika instruksi task tampak bertentangan dengan status yang tercatat di sini (mis. diminta mengedit modul yang menurut dokumen ini belum ada) — **berhenti dan konfirmasi ke manusia**, jangan melanjutkan dengan asumsi.
13. **Jika sebuah task menyentuh area yang dinaungi ADR berstatus OPEN** (ADR-005 Search, ADR-006 Job Queue, ADR-008 Maps, ADR-018 Caching) — **berhenti dan minta keputusan eksplisit**, jangan memilih sendiri salah satu opsi (lihat `architecture-decision-records.md` Bagian 10, AI Usage Rules poin 4).

---

*Dokumen ini adalah catatan status proyek yang hidup (living document) — wajib diupload ulang ke AI Coding Assistant di setiap sesi development baru, dan wajib diperbarui setiap kali ada perubahan nyata pada kode/skema/struktur proyek. Tidak ada informasi di dokumen ini yang dikarang — setiap bagian yang belum ada dicatat eksplisit sebagai "Belum dibuat".*
