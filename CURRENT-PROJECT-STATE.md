# CURRENT PROJECT STATE
## Platform Web Real Estate Agency

> **CATATAN PENGGUNAAN — WAJIB DIBACA AI CODING ASSISTANT DI SETIAP SESI**
> Dokumen ini adalah **satu-satunya sumber kebenaran tentang apa yang SUDAH ADA secara fisik** di proyek ini (kode, tabel, endpoint, komponen) — bukan apa yang direncanakan/didesain di dokumen governance. Jika sebuah item tercatat **"Belum dibuat"**, AI **dilarang** berasumsi item tersebut sudah ada, sudah sebagian jadi, atau bisa "diisi mengarang" — perlakukan sebagai benar-benar kosong. Dokumen ini wajib **diperbarui di akhir setiap sesi development** yang mengubah kode nyata (bukan hanya dokumen).

---

# Project Information

| Field | Value |
|---|---|
| **Nama Project** | Platform Web Real Estate Agency (nama brand final belum ditentukan — placeholder `{nama_platform}`) |
| **Versi** | 0.1 (Pra-Development — belum ada rilis kode) |
| **Tanggal Update** | 27 Juli 2026 |
| **Status** | 🟡 **Perencanaan & Dokumentasi Selesai — Implementasi Teknis Belum Dimulai** |
| **Development Phase** | **Pre-Phase 0** — seluruh dokumen governance (Constitution, PRD, ERD, API Spec, User Flow, SEO Spec, System Architecture, Technology Decisions, Dependency Manifest, AI Development Blueprint, AI Context Pack, Development Roadmap, Task Template) sudah final/tersedia. **Sprint S0 (Foundation Infrastructure) belum dieksekusi** — belum ada monorepo, belum ada kode, belum ada database fisik. |

---

# Overall Progress

> Kolom **Progress** mengukur *implementasi kode nyata yang berjalan*, bukan kelengkapan desain/dokumentasi. Sebuah modul yang desainnya 100% lengkap di PRD/ERD tetap tercatat **0%** di sini jika belum ada satu baris kode produksi pun.

| Module | Status | Progress |
|---|---|---|
| Governance & Documentation (Constitution, PRD, ERD, API Spec, User Flow, SEO Spec, System Architecture, Technology Decisions, Dependency Manifest, AI Dev Blueprint, AI Context Pack, Development Roadmap, Task Template) | Completed | 100% |
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

**Ringkasan:** 1 dari 14 baris selesai (dokumentasi/governance). Implementasi kode: **0% secara keseluruhan.**

---

# Existing Database

**Belum dibuat.** Belum ada project Supabase/PostgreSQL fisik yang diinisialisasi, belum ada migration yang dijalankan, sehingga belum ada satu tabel pun yang benar-benar berdiri di database.

> Skema **target** (rencana, bukan yang sudah ada) sudah didefinisikan lengkap di `ERD-Skema-Database-Real-Estate-Agency-v1.1.md` (37+ entitas) dan `ERD-Diagram-v1.1.mermaid`, dikelompokkan per modul: Identitas & RBAC (`users`, `roles`, `permissions`, `role_permissions`), Verifikasi Agen (`agent_verification_documents`), Profil Agen (`agent_profiles`, `agent_reviews`), Listing (`listings`, `listing_photos`, `listing_videos`, `listing_leads`, `listing_price_history`, `listing_views`, `listing_amenities`), Wilayah (`ref_provinces`, `ref_cities`, `ref_districts`, `ref_villages`), Developer (`developer_partners`, `developer_projects`, `developer_project_media`, `agent_project_claims`), Learning Center (`courses`, `course_lessons`, `quizzes`, `quiz_questions`, `quiz_options`, `enrollments`, `quiz_attempts`, `certificates`), Event (`events`, `event_registrations`), DBR (`dbr_simulations`, `dbr_config`), Dashboard (`notifications`), Admin/Sistem (`system_configs`, `audit_logs`), SEO (`url_redirects`). Ini adalah **rencana**, bukan status "sudah ada" — jangan diasumsikan sudah tereksekusi.

---

# Existing API

**Belum dibuat.** Belum ada Route Handler/service backend yang di-deploy atau dapat dipanggil. Tidak ada endpoint yang benar-benar hidup.

> Kontrak **target** endpoint sudah didefinisikan lengkap di `API-Specification-Real-Estate-Agency-Platform-v1.1.md` (13 bagian: Auth, Property/Listing, Search, Lead Generation, Communication, Financial/DBR Calculator, Notification, Wilayah, Integrasi Pihak Ketiga, SEO/Analytics, Modul Pendukung). Ini adalah **spesifikasi**, bukan endpoint yang sudah aktif.

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

> Struktur **target** sudah didefinisikan lengkap di `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 7 dan `SYSTEM-ARCHITECTURE.md` Bagian 6 (route groups `(public)/(auth)/(dashboard)/(admin)`, `components/ui/` vs `components/features/{module}/`, `lib/`, `hooks/`, dsb.). Struktur ini **wajib** diikuti persis saat Sprint S0 dieksekusi — bukan didesain ulang saat itu.

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

Namun tercatat beberapa **"debt keputusan governance"** (ketidaksinkronan antar dokumen desain) yang sebaiknya diselesaikan sebelum atau selama Sprint S0, agar tidak menghambat implementasi nanti:

| # | Ketidaksinkronan | Dokumen Terdampak |
|---|---|---|
| 1 | Arsitektur backend (Next.js Route Handlers vs service Node terpisah) sudah condong diputuskan ke Route Handlers + Supabase di `technology-decisions.md`, namun `PROJECT-CONSTITUTION.md` & `SYSTEM-ARCHITECTURE.md` masih mencatatnya sebagai opsi terbuka. | `PROJECT-CONSTITUTION.md` Bagian 4, `SYSTEM-ARCHITECTURE.md` Bagian 4 & 23 |
| 2 | State management server-state (React Query vs SWR) sudah diputuskan tegas ke TanStack Query di `technology-decisions.md`, namun `SYSTEM-ARCHITECTURE.md` Bagian 10 masih menulis "pilih salah satu, konsisten". | `SYSTEM-ARCHITECTURE.md` Bagian 10 |
| 3 | Vercel sebagai hosting resmi dipakai di `SYSTEM-ARCHITECTURE.md` & `technology-decisions.md`, tapi belum tercatat sebagai keputusan formal di `PROJECT-CONSTITUTION.md`. | `PROJECT-CONSTITUTION.md` Bagian 4 |
| 4 | Google Maps Platform ditetapkan final di `technology-decisions.md`, namun `PROJECT-CONSTITUTION.md`/`API-Specification` masih mencatat provider Maps sebagai "belum final" (vs Mapbox). | `PROJECT-CONSTITUTION.md`, `API-Specification-v1.1.md` §13/§9.1 |
| 5 | Search Engine (Typesense/Elasticsearch) dan Job Queue (BullMQ vs Supabase Edge Functions) belum masuk *Official Technology Stack* di `technology-decisions.md`, padahal beberapa fitur (search kombinasi filter, regenerasi sitemap event-driven) secara teknis membutuhkannya. | `technology-decisions.md` Bagian 9 poin 2 & 4 |
| 6 | Provider Email transaksional (Resend) dan Monitoring (Sentry) sudah diputuskan di `technology-decisions.md`, namun belum disinkronkan balik sebagai keputusan formal ke `PROJECT-CONSTITUTION.md`/`SYSTEM-ARCHITECTURE.md` yang masih mencatatnya sebagai kekosongan. | `SYSTEM-ARCHITECTURE.md` Bagian 23 poin 10–11 |

> Item-item ini **tidak memblokir** dimulainya Sprint S0, tetapi direkomendasikan dikonfirmasi oleh tim sebelum Sprint S1 (Authentication) agar tidak ada dua dokumen "final" yang saling bertentangan saat implementasi berjalan.

---

# Next Recommended Module

## Sprint S0 — Foundation Infrastructure

**Alasan ini yang paling logis dikerjakan berikutnya:**

1. **Prasyarat murni teknis untuk seluruh modul lain.** Sesuai Module Order di `DEVELOPMENT-ROADMAP.md`, tidak ada satu pun modul bisnis (Auth, Profil, Listing, dst.) yang dapat dibangun tanpa: (a) skema `roles`/`permissions`/`role_permissions` — karena `users.role_id` bergantung padanya, dan (b) data referensi wilayah (`ref_provinces/cities/districts/villages`) — karena field lokasi cascading di Listing & Developer Projects bergantung padanya.
2. **Keputusan mahal harus dikunci di awal.** Struktur monorepo, strategi rendering (SSR/SSG per route group), dan pipeline CI/CD adalah keputusan yang sangat mahal diubah setelah kode lain dibangun di atasnya — sejalan dengan prinsip *"keputusan arsitektur mahal diambil di Fase 1, bukan ditambal belakangan"* di `PROJECT-CONSTITUTION.md`.
3. **Tidak ada modul bisnis yang bisa diuji end-to-end tanpanya** — bahkan Sprint S1 (Authentication) butuh tabel `roles` untuk bisa menerbitkan `role_id` saat registrasi.

**Cakupan konkret Sprint S0** (per `DEVELOPMENT-ROADMAP.md`):
- Inisialisasi monorepo (`apps/web`, `packages/shared-types`), Next.js + TypeScript `strict: true`.
- Setup Tailwind v4 + shadcn/ui, ESLint/Prettier/Husky.
- CI pipeline (lint + type-check + test) di GitHub Actions.
- Project Supabase + migration awal: `users`, `roles`, `permissions`, `role_permissions` (seed 8 role & permission dasar).
- Seed `ref_provinces/cities/districts/villages` dari data resmi.
- Skeleton route group `(public)/(auth)/(dashboard)/(admin)` + middleware skeleton (`auth`/`rbac`/`rate-limit`).
- Scaffold environment variables.

**Acceptance Criteria S0** (dari `DEVELOPMENT-ROADMAP.md`): CI pipeline lolos pada commit kosong; migration dapat dijalankan ulang dari nol tanpa error; seed data wilayah terverifikasi jumlah barisnya sesuai sumber resmi.

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

---

*Dokumen ini adalah catatan status proyek yang hidup (living document) — wajib diupload ulang ke AI Coding Assistant di setiap sesi development baru, dan wajib diperbarui setiap kali ada perubahan nyata pada kode/skema/struktur proyek. Tidak ada informasi di dokumen ini yang dikarang — setiap bagian yang belum ada dicatat eksplisit sebagai "Belum dibuat".*
