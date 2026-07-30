# ARCHITECTURE DECISION RECORDS (ADR)
## Platform Web Real Estate Agency

---

# 1. Document Information

| Field | Value |
|---|---|
| **Name** | Architecture Decision Records — Platform Web Real Estate Agency |
| **Version** | 1.0 |
| **Status** | Draft — menunggu review & pengesahan tim (konsisten dengan status dokumen governance turunan lain; lihat Bagian 9) |
| **Owner** | Principal Enterprise Software Architect / Enterprise Solution Architect (nama individu belum ditetapkan — konsisten dengan catatan terbuka yang sama di `technology-decisions.md`, `decision-log.md`, `document-governance-baseline-register.md`) |
| **Last Updated** | 27 Juli 2026 |
| **Purpose** | Menjadi **satu-satunya sumber kebenaran** untuk seluruh keputusan yang memengaruhi **desain arsitektur dan implementasi teknis** proyek — dipakai sebagai rujukan wajib bagi `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `AI-DEVELOPMENT-BLUEPRINT.md` (Development Playbook), `dependency-manifest.md`, Database Schema (saat dibuat), `API-Specification`, dan Technical Specification (saat dibuat). |

---

# 2. ADR Overview

**Tujuan ADR.** Dokumen ini mencatat keputusan arsitektur/teknis dalam format standar industri (Architecture Decision Record) — satu catatan per keputusan, berisi konteks, opsi yang dipertimbangkan, dan konsekuensinya. Tujuannya adalah agar siapa pun (manusia atau AI Coding Assistant) yang bertanya "**mengapa** sistem ini dibangun seperti ini" mendapat jawaban tunggal dan otoritatif, bukan harus merekonstruksi alasan dari tersebarnya isi `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, dan `PROJECT-CONSTITUTION.md` sekaligus.

**Perbedaan ADR dengan Decision Log.** `decision-log.md` adalah **jurnal kronologis** seluruh keputusan proyek — teknis maupun non-teknis (mis. penetapan versi Blueprint aktif, keputusan governance dokumen, keputusan bisnis yang berdampak proses). ADR di dokumen ini **hanya** mencakup subset keputusan yang berdampak langsung pada **desain arsitektur dan implementasi teknis** (stack, pola integrasi, strategi data, keamanan teknis, dsb.) — disusun **per topik arsitektur**, bukan per urutan waktu terjadinya. Satu topik arsitektur = satu ADR yang hidup dan dapat diperbarui statusnya (Superseded, bukan dihapus), berbeda dengan entri Decision Log yang murni menambah baris baru kronologis.

> **Catatan penomoran:** Dokumen ini memakai penomoran `ADR-001`…`ADR-025` sendiri, **terpisah** dari penomoran `ADR-001`…`ADR-037` di `decision-log.md`. Kedua dokumen **tidak berbagi ruang nomor yang sama** meski sama-sama memakai prefiks "ADR-" — ini dicatat sebagai potensi ambiguitas penamaan di Governance Notes (akhir dokumen) dan **belum diputuskan sendiri** solusinya (mis. penambahan prefiks pembeda) karena itu adalah keputusan tata kelola dokumen, bukan keputusan arsitektur.

**Bagaimana AI harus menggunakan ADR.** Lihat Bagian 10 (AI Usage Rules) untuk aturan lengkap. Ringkasnya: ADR dibaca **sebelum** `technology-decisions.md`, karena ADR menjelaskan alasan di balik apa yang tercantum di sana; ADR berstatus **Approved tidak boleh dilanggar**; ADR berstatus **Open tidak boleh diasumsikan/dipilih sendiri oleh AI**.

**Hubungan ADR dengan Technology Decisions.** ADR adalah **lapisan keputusan** (mengapa, dengan alternatif apa, dengan konsekuensi apa); `technology-decisions.md` adalah **lapisan katalog** (stack resmi yang harus dipakai, versi, justifikasi ringkas per teknologi). Setiap baris "Official Technology Stack" di `technology-decisions.md` **seharusnya** dapat ditelusuri balik ke tepat satu ADR di dokumen ini. Jika ditemukan baris di `technology-decisions.md` yang tidak memiliki ADR yang menaunginya (atau sebaliknya), ini dicatat sebagai gap governance, bukan diasumsikan sinkron.

---

# 3. ADR Status Legend

| Status | Arti |
|---|---|
| **Proposed** | Keputusan diusulkan/didraf, alternatif sudah mulai diidentifikasi, namun analisis dampak dan review arsitektur belum selesai. Belum mengikat implementasi. |
| **Open** | Keputusan **belum dapat ditentukan** dari dokumen proyek yang ada — baik karena benar-benar belum dibahas, atau karena dokumen yang membahasnya saling bertentangan/tidak sinkron pada level hierarki yang berbeda. AI/developer **tidak boleh** mengimplementasikan sesuatu berdasarkan entry berstatus ini sebagai final. |
| **Approved** | Keputusan sudah disetujui secara arsitektural berdasarkan dokumen proyek yang tersedia, dan **mengikat** untuk implementasi berikutnya. Tidak berarti sudah ada di kode (proyek saat ini 0% kode — lihat `CURRENT-PROJECT-STATE.md`), hanya berarti keputusan sudah final untuk dieksekusi. |
| **Rejected** | Sebuah opsi/pendekatan secara eksplisit dipertimbangkan dan ditolak (biasanya tercatat di *Architecture Constraints* `technology-decisions.md`), dicatat agar tidak diusulkan ulang tanpa alasan baru yang kuat. |
| **Superseded** | ADR ini pernah `Approved`, namun telah **digantikan sepenuhnya** oleh ADR lain yang lebih baru — dipertahankan sebagai sejarah, tidak dihapus, dengan rujukan ke ADR pengganti. |
| **Deprecated** | ADR ini pernah berlaku dan sebagian implementasinya mungkin masih ada, tetapi **tidak lagi direkomendasikan** untuk kode baru — belum tentu sudah sepenuhnya digantikan (beda dengan *Superseded* yang penggantinya sudah pasti). |

---

# 4. Architecture Decision Records

---

**ADR-001 — Backend Architecture**

**Status:** OPEN
**Date:** —
**Owner:** Technical Lead / Principal Software Architect (nama TBD)

**Context:** Proyek membutuhkan lapisan backend/API untuk 11 modul PRD dengan RBAC granular, kalkulasi finansial (DBR), dan integrasi pihak ketiga (Maps, Search, Email). Dua pola arsitektur mungkin: BFF tipis lewat Next.js Route Handlers (menyatu dengan Supabase), atau service backend terpisah (mis. NestJS/Express).

**Decision:** **Belum ditentukan.** `technology-decisions.md` §9 poin 1 condong ke "Next.js Route Handlers + Supabase, tanpa service Node terpisah", namun `PROJECT-CONSTITUTION.md` §4 dan `SYSTEM-ARCHITECTURE.md` §4/§23 masih menampilkan **dua opsi terbuka** yang secara eksplisit "harus dikunci sebelum Fase 1 selesai". Karena dokumen berhierarki lebih tinggi (Constitution) belum menyatakan final, status ADR ini **OPEN**, bukan Approved — sesuai instruksi untuk tidak memilih sendiri saat dokumen bertentangan pada level hierarki berbeda.

**Alternatives Considered:**
- **Next.js Route Handlers sebagai BFF tipis** (condong dipilih `technology-decisions.md`): kompleksitas operasional rendah, satu deployment unit, cocok tim kecil.
- **Service backend terpisah (NestJS/Express)**: pemisahan concern lebih jelas untuk logic kompleks (RBAC, DBR), namun menambah deployment unit & operational overhead.

**Pros:** *(dari opsi yang condong dipilih)* deployment tunggal via Vercel, tidak ada duplikasi validasi antara BFF dan service; *(dari opsi alternatif)* skalabilitas independen dari frontend, cocok jika tim backend membesar.

**Cons:** *(dari opsi yang condong dipilih)* Route Handlers Next.js kurang ideal untuk proses long-running/heavy compute; *(dari opsi alternatif)* menambah kompleksitas CI/CD & biaya operasional untuk tim kecil di tahap MVP.

**Impact:** Menentukan struktur folder `/apps/api`, pola implementasi seluruh endpoint API-Specification, dan apakah dibutuhkan repository/deployment terpisah.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §4, `SYSTEM-ARCHITECTURE.md` §4/§9/§11/§23, `technology-decisions.md` §9.1, `AI-DEVELOPMENT-BLUEPRINT.md`, Technical Specification (belum ada).

**Dependencies:** Bergantung pada ADR-021 (Frontend Framework — sudah Approved: Next.js App Router). Menjadi prasyarat bagi ADR-005, ADR-006, ADR-012, ADR-018.

**Review Date:** Wajib diselesaikan **sebelum Sprint S1** (Executive Architecture Review Bagian 14, kondisi #1).

**Notes:** Ini adalah satu-satunya keputusan arsitektur berkategori blocking tertinggi di seluruh ADR ini per `executive-architecture-review.md`.

---

**ADR-002 — Authentication Strategy**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan mekanisme login (email/password + OTP, Google OAuth2) yang terintegrasi rapat dengan RLS Postgres, tanpa membangun ulang dari nol.

**Decision:** Menggunakan **Supabase Auth**, dibungkus **JWT internal platform** — seluruh layer di luar Auth hanya mengenal JWT ini, bukan metode login aslinya. Verifikasi `id_token` Google OAuth wajib server-side.

**Alternatives Considered:** Auth0/Clerk (vendor Auth terpisah dari database — biaya & kompleksitas tambahan); NextAuth.js/Auth.js custom (tidak seintegrasi Supabase Auth dengan RLS Postgres).

**Pros:** OTP & OAuth2 siap pakai; terintegrasi rapat dengan `auth.uid()` untuk RLS; tidak menambah vendor Auth terpisah.

**Cons:** Role/permission RBAC kustom (8 role) tidak native di Supabase Auth — tetap harus dikelola manual di tabel `roles`/`role_permissions` (lihat ADR-003).

**Impact:** Seluruh alur registrasi/login Modul 1, sesi 15–60 menit (access token) + 30 hari (refresh token httpOnly cookie).

**Affected Documents:** `PROJECT-CONSTITUTION.md` §10, `technology-decisions.md` §4.8, `API-Specification-v1.1.md` §0.1 & §1.1, `decision-log.md` ADR-005.

**Dependencies:** Prasyarat bagi ADR-003 (Authorization/RBAC).

**Review Date:** Tidak ada pemicu spesifik diantisipasi; tinjau ulang jika kebutuhan SSO enterprise muncul.

**Notes:** Login via Google untuk role Agen tetap wajib melalui alur `pending_review` (upload dokumen legalitas) — OAuth tidak melewati approval manual.

---

**ADR-003 — Authorization & RBAC Strategy**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Sistem membutuhkan kontrol akses granular untuk 8 role dengan hard rule ownership (`agent_id`) yang tidak boleh bocor lintas agen, bahkan jika satu lapisan pertahanan gagal.

**Decision:** Dua lapis pertahanan: **RBAC kustom aplikasi** (`roles`/`permissions`/`role_permissions`, model `granted_scope`: `own`/`all`/`none`) sebagai lapisan pertama, dan **Row Level Security (RLS)** Supabase sebagai lapisan kedua. Manager **selalu** `granted_scope = 'all'` (global) tanpa mode "scoped tim/wilayah". Superadmin selalu bypass (short-circuit). Role formal: Superadmin, Manager, Admin, Instructor, Agen, Developer Partner, Buyer, Guest.

**Alternatives Considered:** RBAC aplikasi saja tanpa RLS (ditolak — bertentangan dengan hard rule keamanan dua-lapis); level `granted_scope` tambahan "scoped tim/wilayah" untuk Manager (ditolak untuk rilis ini — lihat `decision-log.md` ADR-033).

**Pros:** Satu lapisan gagal tidak langsung membocorkan seluruh data; hard rule ownership dicek di kode, bukan hanya konfigurasi yang bisa salah diatur.

**Cons:** Kompleksitas ganda — perubahan skema permission harus disinkronkan hati-hati di kedua lapisan.

**Impact:** Berlaku di **setiap** endpoint/tabel ber-scope kepemilikan di seluruh 11 modul.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §11 & §20, `ERD-Skema-Database-v1.1.md` §2.28–2.30, `technology-decisions.md` §4.9, `decision-log.md` ADR-006/032/033.

**Dependencies:** Bergantung ADR-002 (Authentication) dan ADR-004 (Database Strategy — untuk RLS). Prasyarat bagi ADR-012 (API Architecture).

**Review Date:** Jika kebutuhan bisnis "Manager per wilayah" muncul eksplisit — akan menjadi ADR baru yang men-supersede sebagian ADR ini.

**Notes:** Jumlah role bernama saat ini **7 role dengan akun** (superadmin, manager, admin, instructor, agent, developer_partner, buyer) + Guest tanpa akun — lihat catatan inkonsistensi angka "7 vs 8" di Bagian 5 & Governance Notes.

---

**ADR-004 — Database Strategy**

**Status:** Approved
**Date:** 2026-07-25/26
**Owner:** Database Architect (nama TBD)

**Context:** Skema ERD proyek (37+ entitas) memakai relasi ketat (FK, ENUM, UNIQUE composite, cascading wilayah) yang menjadi tulang punggung RBAC dan ownership hard rule.

**Decision:** **PostgreSQL** (di-host via Supabase) sebagai database relasional utama. **UUID sebagai primary key** di seluruh tabel (bukan auto-increment). **Soft delete** (`deleted_at`) wajib untuk `listings`, `users`, `developer_projects`. Migration **murni SQL** bernomor urut via Supabase CLI, tanpa ORM auto-sync ke production.

**Alternatives Considered:** MongoDB/NoSQL (ditolak — tidak cocok relasi ketat); auto-increment integer + hard delete (ditolak — risiko enumerasi kompetitor & kehilangan data tanpa jejak); ORM auto-sync `db push` ke production (ditolak — schema drift tak terkontrol tanpa jejak review).

**Pros:** ACID compliance, indexing kaya (composite, trigram/full-text), migration dapat direview & di-rollback.

**Cons:** Scaling horizontal butuh strategi eksplisit (partitioning/sharding) jika volume tumbuh sangat besar di masa depan.

**Impact:** Seluruh 37+ entitas ERD, seluruh endpoint API yang membaca/menulis data.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §4 & §9, `ERD-Skema-Database-v1.1.md`, `technology-decisions.md` §4.7, `decision-log.md` ADR-004/029/030, Database Schema (fisik — belum ada, TBD).

**Dependencies:** Prasyarat bagi ADR-003 (RLS), ADR-022 (Schema Conventions — dilebur ke sini), Database Schema fisik.

**Review Date:** Jika kebutuhan multi-tenant/multi-region (lihat ADR-023) memerlukan penyesuaian skema besar.

**Notes:** **Gap terbuka (bukan mengubah status Approved):** kebijakan soft-delete **belum dideklarasikan seragam** untuk seluruh entitas — hanya eksplisit untuk 3 tabel. Ini adalah gap implementasi-lanjutan, bukan keputusan yang masih diperdebatkan (lihat `foundation-validation-report.md` Gap M-3). Direkomendasikan diselesaikan sebelum Database Schema Alignment, dicatat di Bagian 5 sebagai item yang perlu tindak lanjut meski status ADR ini tetap Approved untuk keputusan intinya (Postgres/UUID/soft-delete-untuk-3-tabel/migration-SQL).

---

**ADR-005 — Search Strategy**

**Status:** OPEN
**Date:** —
**Owner:** —

**Context:** `API-Specification-v1.1.md` §3 mensyaratkan `/properties/search` & `/properties/autocomplete` dengan filter kombinasi & typo-tolerance. Belum ada pilihan resmi mesin pencari di `technology-decisions.md` "Official Technology Stack".

**Decision:** **Belum ditentukan.** Tercatat sebagai Open Question di `technology-decisions.md` §9.2 dan Open Decision di `decision-log.md`/`foundation-validation-report.md`.

**Alternatives Considered (belum diputuskan, sekadar opsi yang tercatat):**
- **PostgreSQL full-text/trigram index** — biaya operasional rendah, tetap dalam ekosistem Supabase, direkomendasikan `foundation-validation-report.md` sebagai MVP Fase 1.
- **Typesense** — direkomendasikan API Spec sebagai kandidat untuk typo-tolerance & performa filter kombinasi skala besar.
- **Elasticsearch** — alternatif enterprise-grade, operasional lebih berat.

**Pros/Cons:** *(Postgres FTS)* Pro: tanpa vendor tambahan, cocok volume awal; Con: typo-tolerance & performa filter kompleks lebih terbatas dibanding mesin pencari khusus. *(Typesense/Elasticsearch)* Pro: performa & fitur pencarian superior di skala besar; Con: menambah komponen infrastruktur & biaya operasional.

**Impact:** Modul 3 (Listing search/filter/autocomplete) — implementasi salah asumsi berisiko rework besar.

**Affected Documents:** `technology-decisions.md` §9.2, `API-Specification-v1.1.md` §3, `dependency-manifest.md`, `SEO-Analytics-Specification-v1.1.md` (indexing sinkron).

**Dependencies:** Terkait ADR-001 (Backend Architecture) dan ADR-006 (Job Queue, untuk sinkronisasi index).

**Review Date:** Wajib diselesaikan **sebelum Sprint S5**.

**Notes:** `foundation-validation-report.md` Gap H2 merekomendasikan mulai dengan Postgres FTS sebagai MVP dengan kriteria ambang migrasi eksplisit ke Typesense — namun ini tetap rekomendasi, bukan keputusan final, sampai ada ADR yang disahkan manusia berwenang.

---

**ADR-006 — Job Queue Strategy**

**Status:** OPEN
**Date:** —
**Owner:** —

**Context:** Tiga fitur lintas modul membutuhkan proses asinkron: regenerasi sitemap event-driven (M11), reminder event H-1 (M5), sinkronisasi counter denormalisasi (M3/M8).

**Decision:** **Belum ditentukan.** Tercatat sebagai Open Question di `technology-decisions.md` §9.4.

**Alternatives Considered:** **Supabase Edge Functions + cron** (tetap dalam ekosistem Supabase, tanpa infrastruktur tambahan); **BullMQ** (butuh Redis — lebih matang untuk queue kompleks, namun menambah komponen operasional).

**Pros/Cons:** *(Edge Functions+cron)* Pro: tidak menambah vendor/infrastruktur; Con: kurang matang untuk queue dengan retry/backoff kompleks. *(BullMQ)* Pro: fitur queue matang (retry, prioritas, dead-letter); Con: menambah Redis sebagai dependency infrastruktur baru.

**Impact:** Modul 3, 5, 8, 11 — keterlambatan indexing SEO & reminder jika salah asumsi.

**Affected Documents:** `technology-decisions.md` §9.4, `SEO-Analytics-Specification-v1.1.md` §1.5 & §4.3, `decision-log.md` Open Decision #4/#3 (Bagian 10 Future Decisions — Redis).

**Dependencies:** Sangat terkait **ADR-001** (Backend Architecture) — jika backend tetap Route Handlers+Supabase, Edge Functions+cron lebih konsisten secara arsitektural.

**Review Date:** Wajib diselesaikan **sebelum Sprint S6/S13**.

**Notes:** Keputusan ini sebaiknya diambil **bersamaan** dengan ADR-001, bukan terpisah, karena pilihan backend memengaruhi pilihan queue yang paling konsisten.

---

**ADR-007 — Email Provider**

**Status:** Approved
**Date:** 2026-07-27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan pengiriman email transaksional (OTP registrasi, notifikasi status approval, reminder).

**Decision:** Menggunakan **Resend**, dipasangkan dengan **React Email** untuk template berbasis komponen React.

**Alternatives Considered:** SendGrid, Postmark (valid, DX TypeScript/React dinilai kurang seselaras); Amazon SES (butuh konfigurasi infrastruktur tambahan — domain warm-up, DKIM manual).

**Pros:** DX TypeScript/React paling selaras stack Next.js; deliverability baik; dashboard log pengiriman untuk debugging.

**Cons:** Harga per volume email perlu dipantau seiring pertumbuhan basis pengguna.

**Impact:** Modul 1 (OTP) dan Modul 8 (notifikasi status) — bukan untuk marketing/bulk email.

**Affected Documents:** `technology-decisions.md` §4.14, `dependency-manifest.md`, `decision-log.md` ADR-010.

**Dependencies:** Terkait ADR-020 (Notification Strategy).

**Review Date:** Jika kebutuhan volume email melonjak signifikan (mis. campaign marketing skala besar).

**Notes:** **Gap administratif (non-blocking):** keputusan ini belum disinkronkan ke `SYSTEM-ARCHITECTURE.md` §23 yang masih mencatatnya kosong — lihat Bagian 5.

---

**ADR-008 — Maps Provider**

**Status:** OPEN
**Date:** —
**Owner:** —

**Context:** Fitur lokasi listing (Modul 3) dan peta proyek developer (Modul 6) membutuhkan autocomplete alamat (client-side) serta reverse geocoding & distance matrix (server-side).

**Decision:** **Belum final secara otoritatif.** `technology-decisions.md` §4.29 condong memilih **Google Maps Platform**, namun dokumen tsb sendiri mensyaratkan **konfirmasi biaya bisnis** sebelum disinkronkan sebagai final, dan `PROJECT-CONSTITUTION.md` serta `API-Specification-v1.1.md` §13/§9.1 — dokumen berhierarki lebih tinggi — **masih mencatat provider Maps sebagai "belum final (Google Maps Platform atau Mapbox)"**. Karena dokumen tertinggi belum menyatakan final, status ADR ini **OPEN**.

**Alternatives Considered:** **Google Maps Platform** (akurasi data alamat/POI Indonesia matang, satu vendor untuk seluruh kebutuhan); **Mapbox** (tetap alternatif valid secara teknis, belum dipilih pada iterasi ini).

**Pros/Cons:** *(Google Maps)* Pro: akurasi & dokumentasi matang; Con: model harga per-request setelah kuota gratis, perlu pemisahan client-key/server-key ketat. *(Mapbox)* Pro/Con belum dieksplorasi mendalam di dokumen sumber.

**Impact:** Form lokasi listing (M3), peta proyek developer (M6) — tidak dapat diimplementasikan penuh tanpa keputusan final tersinkron.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §4, `API-Specification-v1.1.md` §13 & §9.1, `technology-decisions.md` §4.29, `decision-log.md` ADR-028 & Open Decision #4.

**Dependencies:** Tidak bergantung pada ADR lain — dapat diselesaikan paralel dengan ADR-001/005/006.

**Review Date:** Wajib diselesaikan **sebelum Sprint S4/S9**.

**Notes:** Perlu input tim bisnis atas estimasi biaya per-request pada volume listing yang diproyeksikan sebelum status dapat naik menjadi Approved.

---

**ADR-009 — Storage Strategy**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan penyimpanan foto/video listing (publik) dan dokumen legalitas agen (privat, sensitif) dengan kontrol akses terintegrasi Auth/RLS.

**Decision:** Menggunakan **Supabase Storage** dengan bucket terpisah tegas: publik (`listing-photos`, `listing-videos`, `developer-project-media`) vs privat (`agent-verification-documents`). Dokumen legalitas tidak pernah lewat CDN publik — akses hanya via signed URL berumur pendek.

**Alternatives Considered:** Cloudinary, ImageKit, AWS S3+CloudFront (dicatat sebagai evaluasi fase lanjutan jika kebutuhan transformasi gambar melampaui kapasitas Supabase Storage).

**Pros:** Terintegrasi langsung dengan Supabase Auth/RLS; mengurangi vendor tambahan di MVP.

**Cons:** Transformasi gambar (resize/WebP/AVIF) tidak sekaya CDN gambar khusus — dikompensasi kompresi client-side (ADR-019).

**Impact:** Modul 1 (dokumen legalitas), Modul 3 (foto/video listing), Modul 6 (media proyek developer).

**Affected Documents:** `PROJECT-CONSTITUTION.md` §12 & §16, `technology-decisions.md` §4.10, `decision-log.md` ADR-007.

**Dependencies:** Bergantung ADR-004 (Database Strategy, untuk RLS). Prasyarat bagi ADR-019 (File Upload Strategy).

**Review Date:** Jika kebutuhan transformasi gambar/video bertumbuh kompleks.

**Notes:** —

---

**ADR-010 — Deployment Strategy**

**Status:** Approved
**Date:** 2026-07-27
**Owner:** Enterprise Solution Architect (nama TBD)

**Context:** Dibutuhkan platform hosting yang mendukung penuh fitur Next.js App Router (ISR, Edge Middleware, Image Optimization) dengan alur deploy cepat untuk tim kecil, serta repository/CI yang terintegrasi.

**Decision:** **Vercel** sebagai hosting/deployment aplikasi Next.js; **GitHub** sebagai repository dengan **GitHub Actions** untuk CI (lint, type-check, test, migration check).

**Alternatives Considered:** Self-hosted Docker+VPS/Kubernetes (beban DevOps tidak sepadan untuk tim kecil MVP); Netlify/AWS Amplify (dukungan App Router kurang seketat Vercel); GitLab/Bitbucket (tidak memberi keuntungan tambahan dibanding integrasi GitHub↔Vercel).

**Pros:** Kombinasi Next.js+Vercel native (pembuat framework sama); zero-config deploy; preview deployment per PR; edge caching bawaan mendukung target TTFB < 600ms.

**Cons:** Model harga serverless/edge dapat signifikan pada traffic sangat tinggi.

**Impact:** Seluruh pipeline deploy & environment variable management.

**Affected Documents:** `SYSTEM-ARCHITECTURE.md` §4 & §18, `technology-decisions.md` §4.11–4.13, `decision-log.md` ADR-008/009.

**Dependencies:** Bergantung ADR-021 (Frontend Framework). Prasyarat bagi ADR-015 (Monitoring).

**Review Date:** Jika biaya serverless/edge menjadi tidak proporsional terhadap traffic aktual.

**Notes:** **Gap administratif (non-blocking):** belum tercatat formal sebagai keputusan di `PROJECT-CONSTITUTION.md` §4 — lihat Bagian 5.

---

**ADR-011 — State Management Strategy**

**Status:** Approved
**Date:** 2026-07-27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dashboard/admin panel (CSR) membutuhkan pengelolaan cache server-state; form/filter/wizard membutuhkan state UI lokal-lintas-komponen.

**Decision:** **TanStack Query** untuk server-state (khusus route group CSR); **Zustand** untuk UI state (satu store per domain UI, bukan satu store raksasa).

**Alternatives Considered:** SWR (**ditolak eksplisit** — Architecture Constraint, untuk menghindari dua library server-state berdampingan); Redux Toolkit (**ditolak eksplisit** — boilerplate berlebihan); Context API murni (tidak dioptimasi update frekuensi tinggi); Jotai/Recoil (tidak dipilih agar tidak menambah library tanpa kebutuhan jelas).

**Pros:** Automatic refetching & cache invalidation granular (TanStack Query); API minimal tanpa boilerplate (Zustand).

**Cons:** Konsep cache key/invalidation butuh pemahaman tim; tanpa disiplin, Zustand store bisa jadi "keranjang sampah" state acak.

**Impact:** Seluruh halaman `(dashboard)`/`(admin)`; halaman `(public)` tetap mengandalkan Server Component fetch (tidak memakai TanStack Query).

**Affected Documents:** `technology-decisions.md` §4.16–4.17, `dependency-manifest.md`, `decision-log.md` ADR-015/016.

**Dependencies:** Bergantung ADR-021 (Frontend Framework).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** **Gap administratif (non-blocking):** `SYSTEM-ARCHITECTURE.md` §10 masih memakai frasa usang "React Query/SWR — pilih satu, konsisten" — lihat Bagian 5.

---

**ADR-012 — API Architecture**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** API Architect (nama TBD)

**Context:** Dibutuhkan konvensi REST yang konsisten (versioning, envelope, error, pagination, RBAC middleware) di seluruh endpoint 11 modul.

**Decision:** REST murni, base URL `/api/v1` (breaking change wajib naik versi), response envelope standar (`success`/`data`/`meta` atau `success`/`error`), RBAC middleware 5-langkah baku (validasi token → cek permission → resolusi scope → superadmin bypass), filter geografis berbasis ID referensi (bukan freetext), rate limiting bertingkat (60/300/5 per menit).

**Alternatives Considered:** GraphQL (tidak dipilih — tidak ada kebutuhan eksplisit query fleksibel yang mengungguli kompleksitas tambahan GraphQL untuk tim kecil); gRPC (tidak relevan untuk API publik berbasis web/mobile).

**Pros:** Konvensi matang, mudah diaudit, konsisten dengan skema ERD.

**Cons:** Lokasi fisik implementasi API (Route Handlers vs service terpisah) **masih bergantung pada ADR-001 yang Open** — konvensi API sudah matang, tetapi *di mana* ia dijalankan belum terkunci.

**Impact:** Seluruh 11 modul PRD, seluruh endpoint `API-Specification-v1.1.md`.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §8, `API-Specification-v1.1.md`, `SYSTEM-ARCHITECTURE.md` §9.

**Dependencies:** Bergantung **ADR-001 (Open)** untuk lokasi fisik implementasi; bergantung ADR-002/003 untuk auth/RBAC middleware.

**Review Date:** Ditinjau ulang bersamaan dengan resolusi ADR-001.

**Notes:** Konvensi API (bentuk kontrak) berstatus Approved secara independen dari *tempat* kontrak ini dijalankan (ADR-001).

---

**ADR-013 — Error Handling Strategy**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan penanganan error yang konsisten agar tidak membocorkan detail internal, sekaligus memudahkan tracing lintas frontend-backend.

**Decision:** Envelope error standar dengan kode `SCREAMING_SNAKE_CASE` terpusat; data privat milik user lain → 404 (bukan 403, mencegah enumerasi); percobaan akses tanpa izin RBAC → 403 informatif; validasi bisnis gagal → 422; `request_id`/`correlation_id` dikembalikan ke client dan dicatat di log server; Error Boundary React per route group.

**Alternatives Considered:** Membocorkan stack trace ke client untuk debugging cepat (ditolak — risiko keamanan); error generik tanpa kode terstruktur (ditolak — menyulitkan FE menampilkan pesan lokal yang tepat).

**Pros:** Tidak ada kebocoran detail internal; tracing presisi via `request_id`.

**Cons:** Membutuhkan disiplin tim menjaga daftar kode error terpusat tetap sinkron FE↔BE.

**Impact:** Seluruh endpoint API dan seluruh komponen frontend yang menangani response error.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §13, `API-Specification-v1.1.md` §0.3.

**Dependencies:** Bergantung ADR-012 (API Architecture).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** —

---

**ADR-014 — Logging Strategy**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan logging yang cukup untuk investigasi insiden, namun tidak boleh membocorkan data sensitif (finansial DBR, dokumen legalitas, PII).

**Decision:** Structured logging (JSON) dengan level `error`/`warn`/`info`/`debug`; audit log bisnis (`audit_logs`) **terpisah** dari log teknis, retensi permanen, tidak dapat dihapus/di-rotate; data sensitif (`net_income`, KTP/NPWP, password, token JWT penuh) dilarang masuk log; PII dilarang masuk data yang dikirim ke GA4/GTM.

**Alternatives Considered:** Menggabungkan audit log bisnis dengan log teknis biasa (ditolak — audit log butuh retensi permanen & tidak boleh ikut ter-rotate seperti log teknis).

**Pros:** Investigasi insiden presisi (`request_id`); audit trail bisnis (approval, perubahan role/permission) tidak dapat dihapus.

**Cons:** Membutuhkan disiplin eksplisit di kode untuk memisahkan mana yang masuk log teknis vs audit log.

**Impact:** Seluruh aksi bisnis sensitif (approval agen, moderasi listing, perubahan role/permission, perubahan konfigurasi sistem).

**Affected Documents:** `PROJECT-CONSTITUTION.md` §15, `SEO-Analytics-Specification-v1.1.md` §4.4.

**Dependencies:** Terkait ADR-015 (Monitoring & Observability).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** Retensi log teknis minimal 30 hari; audit log bisnis retensi permanen atau sesuai kebijakan kepatuhan yang ditentukan kemudian.

---

**ADR-015 — Monitoring & Observability**

**Status:** Approved
**Date:** 2026-07-27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan error tracking & performance monitoring untuk frontend (Next.js) dan lapisan API.

**Decision:** **Sentry** (`@sentry/nextjs`) sebagai tool monitoring resmi — source map otomatis, performance tracing untuk mendeteksi regresi Core Web Vitals/TTFB.

**Alternatives Considered:** Datadog, self-hosted Grafana+Loki (jauh lebih kompleks & mahal untuk kebutuhan MVP); LogRocket (lebih fokus session replay dibanding error/performance tracing).

**Pros:** SDK resmi terintegrasi App Router (server & client components, edge runtime).

**Cons:** Volume error/tracing tinggi dapat memakan kuota paket berbayar — perlu sampling rate wajar.

**Impact:** Seluruh error runtime frontend & backend.

**Affected Documents:** `technology-decisions.md` §4.15, `dependency-manifest.md`.

**Dependencies:** Bergantung ADR-010 (Deployment), ADR-014 (Logging).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** **Gap administratif (non-blocking):** belum disinkronkan ke `SYSTEM-ARCHITECTURE.md` §23 yang masih mencatatnya kosong — lihat Bagian 5. Data sensitif (`net_income`, KTP/NPWP, token JWT penuh) dilarang masuk breadcrumb/context Sentry (konsisten ADR-014).

---

**ADR-016 — Testing Strategy**

**Status:** Approved
**Date:** 2026-07-27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan unit/component/E2E testing untuk business logic sensitif, interaksi form/dashboard, dan alur kritis lintas halaman.

**Decision:** **Vitest** (unit testing), **React Testing Library** (component testing), **Playwright** (E2E testing, dijalankan terhadap `next build && next start`, bukan `next dev`).

**Alternatives Considered:** Jest (valid, Vitest dipilih untuk performa & kompatibilitas ESM/TypeScript); Enzyme (tidak kompatibel React Server Components); Cypress (secara historis lebih terbatas pada multi-tab/multi-origin testing yang relevan untuk alur OAuth Google).

**Pros:** Kombinasi tiga tool saling melengkapi tanpa tumpang tindih fungsi; Playwright cocok untuk alur OAuth lintas origin.

**Cons:** E2E lebih lambat dibanding unit test — butuh strategi seleksi alur kritis.

**Impact:** Business logic sensitif (kalkulasi DBR, filter `granted_scope`, ownership check) wajib unit test; alur kritis (registrasi, publish listing, submit DBR, moderasi admin) wajib E2E.

**Affected Documents:** `technology-decisions.md` §4.20–4.22, `dependency-manifest.md`.

**Dependencies:** Bergantung ADR-012 (API Architecture), ADR-021 (Frontend Framework).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** **Gap dokumentasi (bukan gap keputusan tool):** `foundation-validation-report.md` Bagian 15 mencatat **belum ada dokumen Testing Strategy/Test Plan konsolidasi** (target coverage, strategi data uji, lingkungan test, kriteria rilis) — tool sudah diputuskan (status Approved di atas tetap berlaku), namun dokumen strategi menyeluruh masih menjadi Missing Document terpisah, direkomendasikan disusun sebagai tindak lanjut, tidak memblokir status Approved keputusan tool ini.

---

**ADR-017 — Security Strategy**

**Status:** Approved
**Date:** 2026-07-25/26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Platform menangani data sensitif (dokumen legalitas agen, data finansial DBR calon pembeli) dan membutuhkan proteksi berlapis terhadap akses tidak sah.

**Decision:** Enkripsi at-rest wajib untuk dokumen legalitas & field finansial DBR; RLS+middleware RBAC berlapis (ADR-003); tidak ada trust terhadap input client (validasi ulang server); signed URL berumur pendek untuk dokumen privat; API key pihak ketiga dipisah client-key/server-key; rate limiting bertingkat; audit trail tidak dapat dihapus; minimal 1 akun Superadmin aktif dijamin di level aplikasi; PII tidak masuk log/Analytics; cookie consent + Google Consent Mode.

**Alternatives Considered:** Tidak ada trade-off keamanan yang dilonggarkan demi kemudahan development — seluruh hard rule dinyatakan non-negotiable di dokumen sumber.

**Pros:** Salah satu dimensi paling matang di seluruh dokumentasi proyek (skor 88/100 di `foundation-validation-report.md`).

**Cons:** Tidak ada catatan trade-off signifikan.

**Impact:** Lintas seluruh modul, khususnya Modul 1 (dokumen legalitas), Modul 7 (data finansial DBR), Modul 10 (RBAC).

**Affected Documents:** `PROJECT-CONSTITUTION.md` §20, `SYSTEM-ARCHITECTURE.md` §14, `API-Specification-v1.1.md` §0.

**Dependencies:** Menaungi ADR-002, ADR-003, ADR-009, ADR-019.

**Review Date:** Tidak ada pemicu spesifik diantisipasi — ditinjau ulang jika insiden keamanan nyata terjadi pasca-rilis.

**Notes:** RLS policy SQL konkret belum dapat diverifikasi karena skema fisik belum ada — wajar, bagian dari Database Schema Alignment mendatang.

---

**ADR-018 — Caching Strategy**

**Status:** OPEN
**Date:** —
**Owner:** —

**Context:** Halaman publik SSR/ISR membutuhkan caching edge (CDN-level) untuk mencapai target TTFB < 600ms; belum ada keputusan eksplisit untuk caching **level aplikasi** (mis. Redis) di luar caching bawaan Next.js/Vercel.

**Decision:** **Belum ditentukan untuk lapisan aplikasi.** Caching edge/CDN untuk halaman publik **inheren** dari keputusan ADR-021 (Next.js ISR) & ADR-010 (Vercel edge caching) — bagian ini sudah tercakup dan tidak perlu ADR terpisah. Namun **Redis sebagai application-level cache** hanya tercatat sebagai item **Proposed** di `decision-log.md` Bagian 10 (Future Decisions), belum menjadi keputusan aktif.

**Alternatives Considered:** Redis (untuk cache & rate limiting endpoint sensitif jika fitur bawaan Vercel/Supabase edge dinilai belum cukup); tanpa cache aplikasi tambahan (mengandalkan sepenuhnya ISR/edge cache + index database).

**Pros/Cons:** *(Redis)* Pro: cache granular & rate-limit terpusat yang presisi; Con: menambah komponen infrastruktur baru (kembali terkait ADR-006 Job Queue jika dipasangkan dengan BullMQ). *(Tanpa cache tambahan)* Pro: kesederhanaan operasional; Con: rate limiting endpoint sensitif saat ini belum punya mekanisme penyimpanan status lintas-instance yang eksplisit selain "blocklist Redis/DB" yang disebut di `PROJECT-CONSTITUTION.md` §10 tanpa kepastian implementasi konkret.

**Impact:** Rate limiting endpoint sensitif (Auth), performa dashboard/laporan admin bervolume besar.

**Affected Documents:** `technology-decisions.md`, `dependency-manifest.md`, `decision-log.md` Bagian 10 (Future Decisions — Redis).

**Dependencies:** Terkait ADR-006 (Job Queue) — jika BullMQ dipilih, Redis otomatis dibutuhkan, menyelesaikan dua keputusan sekaligus.

**Review Date:** Dapat ditunda melewati MVP awal — direkomendasikan diputuskan bersamaan dengan ADR-006 jika arah Job Queue mengarah ke BullMQ.

**Notes:** Prioritas paling rendah di antara seluruh ADR berstatus Open di dokumen ini — tidak memblokir Sprint S0–S1.

---

**ADR-019 — File Upload Strategy**

**Status:** Approved
**Date:** 2026-07-26/27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Foto listing (publik, wajib minimal 3) dan dokumen legalitas agen (privat, sensitif) membutuhkan alur upload yang aman dan efisien bandwidth.

**Decision:** Validasi tipe file di server via magic bytes/MIME type (bukan hanya ekstensi); kompresi gambar sisi client via **browser-image-compression** sebelum upload; `alt_text` wajib per foto (auto-generate fallback); satu foto cover aktif per listing; dokumen legalitas tidak pernah lewat CDN publik.

**Alternatives Considered:** Kompresi server-side penuh (Sharp)/CDN transformation sebagai satu-satunya lapisan (ditolak — menambah beban proses di serverless function; pendekatan hybrid dipilih).

**Pros:** Kompresi client mengurangi bandwidth & beban server; validasi server tetap jadi lapisan keamanan utama.

**Cons:** Kompresi client bergantung kemampuan device pengguna di lapangan — tetap perlu validasi ulang di server.

**Impact:** Modul 3 (foto/video listing), Modul 1 (dokumen legalitas).

**Affected Documents:** `PROJECT-CONSTITUTION.md` §16, `technology-decisions.md` §4.28.

**Dependencies:** Bergantung ADR-009 (Storage Strategy).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** Batas ukuran/durasi video/virtual tour belum ada angka final di dokumen sumber — wajib configurable, bukan hard-code.

---

**ADR-020 — Notification Strategy**

**Status:** Approved
**Date:** 2026-07-26/27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Sistem membutuhkan mekanisme pemberitahuan lintas modul (approval agen, moderasi listing, review agen baru, kursus baru, event registrasi) ke pengguna yang relevan.

**Decision:** Tabel `notifications` (Modul 8) sebagai sumber kebenaran, ditulis lewat satu service terpusat (bukan ditulis langsung dari banyak tempat). Channel: **in-app** (selalu) + **email** (via Resend, ADR-007) untuk event penting (approval/reject, OTP, reminder). WhatsApp/push notification dicatat sebagai kemungkinan fase lanjutan.

**Alternatives Considered:** Menulis langsung ke tabel `notifications` dari tiap modul tanpa service terpusat (ditolak — risiko format/state tidak konsisten).

**Pros:** Satu titik kontrol format & konsistensi notifikasi lintas modul.

**Cons:** Channel WhatsApp/push belum tersedia di MVP — perlu ekspektasi yang jelas ke stakeholder bisnis.

**Impact:** Modul 8 (Dashboard & Notifikasi), dipicu dari Modul 1, 2, 3, 4, 5, 6, 9.

**Affected Documents:** `ERD-Skema-Database-v1.1.md` (`notifications`), `PRD-v1.1.md` Modul 8, `decision-log.md` ADR-010 (Resend).

**Dependencies:** Bergantung ADR-007 (Email Provider).

**Review Date:** Jika kebutuhan channel WhatsApp Business API/push notification dikonfirmasi bisnis.

**Notes:** Kedalaman endpoint API untuk Modul 8 dicatat oleh `foundation-validation-report.md` sebagai perlu diperluas saat API Alignment — tidak mengubah status Approved keputusan strategi ini.

---

**ADR-021 — Frontend Framework & Rendering Strategy**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Seluruh halaman publik wajib SSR/SSG/ISR agar terindeks mesin pencari secepat & seakurat mungkin sejak hari pertama rilis; halaman privat sebaliknya harus dicegah dari indeks.

**Decision:** **Next.js (App Router)** sebagai satu-satunya framework frontend. Homepage/Search/Detail Listing/Profil Agen/Detail Proyek Developer → SSR/SSG/ISR; Dashboard/Admin/hasil DBR personal/Chat → CSR dengan `noindex, nofollow`; halaman statis → SSG.

**Alternatives Considered:** Remix (ekosistem lebih kecil, dukungan Vercel-native lebih lemah); Astro (kurang cocok aplikasi interaktif kompleks); SPA React murni + backend terpisah (gagal memenuhi syarat SSR wajib).

**Pros:** Satu-satunya pilihan arus utama yang memenuhi seluruh syarat SSR/SSG/ISR wajib sekaligus ekosistem React penuh & integrasi native Vercel.

**Cons:** Kurva belajar App Router (Server Components, model caching).

**Impact:** Seluruh struktur route group, strategi SEO, dan pilihan hosting (ADR-010).

**Affected Documents:** `PROJECT-CONSTITUTION.md` §4, `SEO-Analytics-Specification-v1.1.md` §1.1, `technology-decisions.md` §4.1, `decision-log.md` ADR-001/031.

**Dependencies:** Prasyarat bagi ADR-001, ADR-010, ADR-011, ADR-016.

**Review Date:** Tidak ada pemicu spesifik diantisipasi — keputusan fondasi jangka panjang.

**Notes:** —

---

**ADR-022 — Database Schema Conventions**

**Status:** Approved
**Date:** 2026-07-25/26
**Owner:** Database Architect (nama TBD)

**Context:** Dibutuhkan konvensi seragam (penamaan, index, migration) agar 37+ entitas ERD konsisten dan mudah diaudit.

**Decision:** `snake_case` untuk tabel (jamak)/kolom/enum; `{referenced_table_singular}_id` untuk FK; index wajib sejak migrasi awal (bukan ditambah belakangan) sesuai daftar prioritas ERD; UNIQUE index untuk seluruh kolom `slug`; migration murni SQL bernomor urut.

**Alternatives Considered:** Penamaan campuran/tidak konsisten antar tim (ditolak — menyulitkan AI Coding Assistant lintas sesi memprediksi nama field).

**Pros:** Prediktabilitas tinggi untuk AI Coding Assistant & developer baru.

**Cons:** Tidak ada trade-off signifikan dicatat.

**Impact:** Seluruh migration & query di seluruh modul.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §7 & §9, `ERD-Skema-Database-v1.1.md`.

**Dependencies:** Bagian dari ADR-004 (Database Strategy).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** Sebagian tumpang tindih dengan ADR-004 secara sengaja — dipisah agar konvensi penamaan dapat dirujuk independen dari keputusan pemilihan PostgreSQL itu sendiri.

---

**ADR-023 — Multi-Tenancy Strategy**

**Status:** Approved (implisit, untuk cakupan saat ini) — evaluasi masa depan berstatus **Proposed**
**Date:** —
**Owner:** —

**Context:** Skema ERD saat ini tidak memiliki kolom `tenant_id` di manapun — seluruh entitas diasumsikan satu instans aplikasi melayani satu basis data bersama (single-tenant).

**Decision:** Arsitektur **single-tenant** berlaku secara implisit dari struktur skema yang sudah ada (tidak ada dokumen yang secara eksplisit mendiskusikan/menolak multi-tenancy — ini adalah keadaan default berdasarkan tidak-adanya kebutuhan yang dinyatakan). Kebutuhan **multi-tenant** dicatat sebagai **Future Decision berstatus Proposed** di `decision-log.md` Bagian 10 — bukan kebutuhan aktif.

**Alternatives Considered:** Multi-tenant dengan `tenant_id` di setiap tabel (belum dievaluasi — tidak ada kebutuhan bisnis yang mendorongnya saat ini).

**Pros:** Kesederhanaan skema & RLS policy untuk kebutuhan saat ini (satu agensi, banyak agen).

**Cons:** Jika kebutuhan white-label/multi-agensi muncul di masa depan, akan memerlukan migrasi skema besar-besaran (`tenant_id` + penyesuaian RLS menyeluruh).

**Impact:** Seluruh skema ERD saat ini disusun dengan asumsi single-tenant.

**Affected Documents:** `ERD-Skema-Database-v1.1.md`, `decision-log.md` Bagian 10 (Future Decisions — Multi Tenant).

**Dependencies:** Terkait ADR-004 (Database Strategy).

**Review Date:** Jika kebutuhan bisnis multi-agensi/white-label dikonfirmasi eksplisit di masa depan.

**Notes:** Dicatat di sini untuk mendokumentasikan keputusan implisit yang sebelumnya tidak pernah dinyatakan eksplisit di ADR manapun — bukan keputusan baru, hanya formalisasi keadaan yang sudah berlaku.

---

**ADR-024 — RBAC Role Model Scope (Formalisasi Role & Cakupan Manager)**

**Status:** Approved
**Date:** 2026-07-26
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dokumen v1.0 tidak konsisten mengenai keberadaan role `buyer`/`instructor` dan cakupan akses Manager (lihat resolusi konflik v1.0→v1.1).

**Decision:** `buyer` (akun ringan opsional) dan `instructor` (role internal terbatas Modul 4) diformalkan sebagai baris resmi tabel `roles`. Manager **selalu** `granted_scope = 'all'` — tidak ada mode "scoped tim/wilayah".

**Alternatives Considered:** Buyer = Agent dengan flag tambahan (ditolak — mencampur domain kepemilikan listing dengan domain pencarian); Instructor = alias Admin (ditolak — Instructor tidak boleh punya akses moderasi listing/RBAC); level `granted_scope` tambahan untuk Manager regional (ditolak untuk rilis ini).

**Pros:** Menghilangkan ambiguitas lintas dokumen; permission matrix eksplisit per role.

**Cons:** Menambah 2 baris seed `roles`; permission matrix perlu didefinisikan eksplisit sebelum fitur terkait aktif.

**Impact:** Seluruh RBAC middleware, seed data Sprint S0.

**Affected Documents:** `PROJECT-CONSTITUTION.md` Riwayat Keputusan Arsitektur #1–#3, `ERD-Skema-Database-v1.1.md` §2.28, `decision-log.md` ADR-032/033.

**Dependencies:** Bagian dari ADR-003 (Authorization & RBAC Strategy).

**Review Date:** Jika kebutuhan bisnis "Manager per wilayah" muncul eksplisit.

**Notes:** **Lihat inkonsistensi jumlah role (7 vs 8) di Bagian 5** — ini adalah gap penghitungan lintas dokumen turunan, bukan ambiguitas pada keputusan role model itu sendiri (daftar role bernama sudah jelas: superadmin, manager, admin, instructor, agent, developer_partner, buyer = 7 role dengan akun, ditambah Guest tanpa baris `roles`).

---

**ADR-025 — Type Safety & Validation Strategy**

**Status:** Approved
**Date:** 2026-07-26/27
**Owner:** Principal Software Architect (nama TBD)

**Context:** Dibutuhkan satu sumber kebenaran tipe data & aturan validasi yang konsisten di frontend dan backend, mengingat kompleksitas RBAC/ownership yang rawan bug jika tidak bertipe statis.

**Decision:** **TypeScript** (`strict: true`, tanpa `any` implisit) di seluruh codebase; **Zod** sebagai satu-satunya skema validasi (dengan `z.infer` untuk tipe otomatis), dipakai baik client (React Hook Form resolver) maupun server (validasi ulang wajib sebelum tulis DB).

**Alternatives Considered:** JavaScript murni (ditolak — tidak mendukung Single Source of Truth tipe data); Yup/Joi (tidak memiliki inferensi tipe TypeScript native sekuat Zod).

**Pros:** Deteksi error compile-time; refactor besar lebih aman; validasi client & server tidak pernah drift karena berasal dari skema yang sama.

**Cons:** Build time lebih lambat dibanding JS murni; skema kondisional kompleks (mis. konversi tenor tahun→bulan) butuh `.refine()`/`.transform()` yang perlu didokumentasikan.

**Impact:** Seluruh codebase frontend & backend, seluruh form dan endpoint mutating.

**Affected Documents:** `PROJECT-CONSTITUTION.md` §6 & §14, `technology-decisions.md` §4.2 & §4.19, `decision-log.md` ADR-002/017/018/034.

**Dependencies:** Prasyarat bagi ADR-012 (API Architecture), ADR-016 (Testing).

**Review Date:** Tidak ada pemicu spesifik diantisipasi.

**Notes:** Satuan tenor DBR selalu bulan (`tenor_months`) — konversi tahun→bulan hanya boleh terjadi di satu titik (layer validasi client), tidak diduplikasi.

---

# 5. Open Decisions Summary

| ADR | Current Status | Priority | Reason Still Open | Affected Documents | Recommended Resolution Time |
|---|---|---|---|---|---|
| **ADR-001** Backend Architecture | OPEN | **Critical** | `technology-decisions.md` condong ke Route Handlers+Supabase, namun `PROJECT-CONSTITUTION.md`/`SYSTEM-ARCHITECTURE.md` (hierarki lebih tinggi) masih menampilkan 2 opsi terbuka — belum disinkronkan | Constitution §4, System Architecture §4/§23, Technical Specification | Sebelum Sprint S1 |
| **ADR-006** Job Queue Strategy | OPEN | **High** | Tidak masuk *Official Technology Stack*; terkait langsung ADR-001 yang juga Open | technology-decisions.md §9.4, SEO Spec §1.5/§4.3 | Sebelum Sprint S6/S13 |
| **ADR-005** Search Strategy | OPEN | **High** | Tidak masuk *Official Technology Stack* meski disyaratkan fungsional oleh API Spec §3 | technology-decisions.md §9.2, API Spec §3 | Sebelum Sprint S5 |
| **ADR-008** Maps Provider | OPEN | **Medium** | Dipilih tentatif (Google Maps) di `technology-decisions.md` namun dokumen itu sendiri mensyaratkan konfirmasi biaya bisnis; Constitution/API Spec (hierarki lebih tinggi) masih "belum final" | Constitution §4, API Spec §13/§9.1 | Sebelum Sprint S4/S9 |
| **ADR-018** Caching Strategy (level aplikasi/Redis) | OPEN | **Low** | Hanya tercatat sebagai *Future Decision* Proposed, belum ada kebutuhan aktif mendesak; terkait ADR-006 | technology-decisions.md, decision-log.md Bagian 10 | Dapat ditunda melewati MVP, diputuskan bersamaan ADR-006 jika relevan |

**Catatan tambahan (bukan status Open pada ADR itu sendiri, melainkan gap implementasi-lanjutan yang tercatat di Notes masing-masing ADR):** kebijakan soft-delete belum seragam (ADR-004), sinkronisasi administratif Vercel/Resend/Sentry/state-management ke dokumen tertinggi (ADR-007/010/011/015), dan dokumen Testing Strategy konsolidasi belum ada (ADR-016). Item-item ini **tidak** mengubah status Approved ADR terkait — dicatat agar tidak hilang dari perhatian tim.

---

# 6. Dependency Matrix

```
ADR-021 (Frontend Framework/Rendering)
   ↓
ADR-001 (Backend Architecture) [OPEN]
   ↓
ADR-012 (API Architecture)
   ↓
ADR-006 (Job Queue Strategy) [OPEN]  ←→  ADR-018 (Caching/Redis) [OPEN]
   ↓
ADR-020 (Notification Strategy)
```

```
ADR-004 (Database Strategy)
   ↓
ADR-022 (Database Schema Conventions)
   ↓
ADR-003 (Authorization & RBAC Strategy)
   ↓
ADR-024 (RBAC Role Model Scope)
```

```
ADR-002 (Authentication Strategy)
   ↓
ADR-003 (Authorization & RBAC Strategy)
   ↓
ADR-012 (API Architecture)
   ↓
ADR-013 (Error Handling) → ADR-014 (Logging) → ADR-015 (Monitoring)
```

```
ADR-009 (Storage Strategy)
   ↓
ADR-019 (File Upload Strategy)
```

```
ADR-007 (Email Provider)
   ↓
ADR-020 (Notification Strategy)
```

```
ADR-021 (Frontend Framework)
   ↓
ADR-010 (Deployment Strategy) → ADR-015 (Monitoring)
   ↓
ADR-011 (State Management Strategy)
   ↓
ADR-016 (Testing Strategy)
```

```
ADR-025 (Type Safety & Validation) ── mendasari ──▶ ADR-012, ADR-016, seluruh ADR yang menyentuh form/endpoint
```

```
ADR-005 (Search Strategy) [OPEN]  ←──terkait──▶  ADR-001 (Backend Architecture) [OPEN]
```

```
ADR-008 (Maps Provider) [OPEN] ── tidak bergantung ADR lain — dapat diselesaikan independen/paralel
```

**Simpul kritis:** **ADR-001** adalah simpul dengan dependency turunan terbanyak (ADR-005, ADR-006, ADR-012, ADR-018, dan transitif ADR-013/014/015/016/020) — ini mengonfirmasi penilaian `executive-architecture-review.md` bahwa ADR-001 adalah prioritas Critical tunggal.

---

# 7. Impact Analysis

> Hanya mencakup ADR berstatus **OPEN** — ADR berstatus Approved tidak memerlukan impact analysis penundaan karena sudah final untuk dieksekusi.

## ADR-001 — Backend Architecture
- **Risiko bila ditunda:** Sesi AI Coding Assistant yang berbeda dapat membangun struktur `/apps/api` dengan asumsi berbeda (Route Handlers vs service terpisah), menghasilkan kode yang harus di-rewrite total. Technical Specification tidak dapat disusun final tanpa ini.
- **Dokumen terdampak:** `PROJECT-CONSTITUTION.md` §4, `SYSTEM-ARCHITECTURE.md` §4/§9/§11/§23, `technology-decisions.md` §9.1, Technical Specification (belum ada).
- **Modul terdampak:** Seluruh 11 modul (setiap modul memiliki lapisan API).
- **Memblokir development?** **Ya — memblokir Sprint S1 dan seterusnya** untuk pekerjaan apa pun yang menyentuh backend/API. Sprint S0 (scaffolding murni) **tidak** terblokir.

## ADR-005 — Search Strategy
- **Risiko bila ditunda:** `/properties/search` & `/properties/autocomplete` berisiko diimplementasikan di atas asumsi backend pencarian yang salah — rework besar bila diputuskan terlambat, terutama jika sudah ada volume listing signifikan saat migrasi mesin pencari terjadi.
- **Dokumen terdampak:** `technology-decisions.md` §9.2, `API-Specification-v1.1.md` §3, `dependency-manifest.md`.
- **Modul terdampak:** Modul 3 (Listing search/filter/autocomplete).
- **Memblokir development?** **Ya, untuk Sprint S5** (fitur search & filter lanjutan). Tidak memblokir Sprint S0–S4.

## ADR-006 — Job Queue Strategy
- **Risiko bila ditunda:** Regenerasi sitemap event-driven, reminder event H-1, dan sinkronisasi counter denormalisasi berisiko diimplementasikan dengan mekanisme ad-hoc yang tidak konsisten, memerlukan refactor saat keputusan akhirnya diambil.
- **Dokumen terdampak:** `technology-decisions.md` §9.4, `SEO-Analytics-Specification-v1.1.md` §1.5 & §4.3.
- **Modul terdampak:** Modul 3, 5, 8, 11.
- **Memblokir development?** **Ya, untuk Sprint S6 (SEO Hardening) dan S13 (Event)**. Tidak memblokir Sprint S0–S5.

## ADR-008 — Maps Provider
- **Risiko bila ditunda:** Form lokasi listing (M3) dan peta proyek developer (M6) tidak dapat diselesaikan penuh; integrasi client-key/server-key tidak dapat dikonfigurasi final.
- **Dokumen terdampak:** `PROJECT-CONSTITUTION.md` §4, `API-Specification-v1.1.md` §13/§9.1, `technology-decisions.md` §4.29.
- **Modul terdampak:** Modul 3, Modul 6.
- **Memblokir development?** **Ya, untuk Sprint S4 (Listing lokasi) dan S9 (Developer Directory)**. Dapat diselesaikan paralel — tidak bergantung pada ADR lain.

## ADR-018 — Caching Strategy (level aplikasi/Redis)
- **Risiko bila ditunda:** Rate limiting endpoint sensitif mungkin memerlukan mekanisme penyimpanan status lintas-instance yang belum eksplisit; risiko rendah pada volume traffic awal MVP.
- **Dokumen terdampak:** `technology-decisions.md`, `dependency-manifest.md`.
- **Modul terdampak:** Lintas modul (rate limiting Auth), tidak spesifik satu modul.
- **Memblokir development?** **Tidak** — dapat ditunda melewati MVP awal tanpa dampak langsung ke sprint manapun di roadmap saat ini.

---

# 8. Implementation Order

Urutan penyelesaian Open Decision yang direkomendasikan (bukan urutan eksekusi kode, melainkan urutan **pengambilan keputusan** oleh manusia berwenang):

1. **ADR-001 — Backend Architecture** — diselesaikan **pertama**, karena menjadi prasyarat langsung/tidak langsung bagi ADR-005, ADR-006, dan seluruh turunannya (lihat Bagian 6, simpul kritis).
2. **ADR-006 — Job Queue Strategy** — diselesaikan **bersamaan/segera setelah** ADR-001, karena pilihan yang konsisten sangat bergantung pada arah ADR-001.
3. **ADR-005 — Search Strategy** — dapat diselesaikan paralel dengan #1–#2; tidak memerlukan hasil ADR-001 secara ketat, namun secara arsitektural lebih baik dipertimbangkan bersamaan.
4. **ADR-008 — Maps Provider** — sepenuhnya independen, dapat diselesaikan **kapan pun secara paralel** dengan #1–#3, murni menunggu konfirmasi biaya dari tim bisnis.
5. **ADR-018 — Caching Strategy** — prioritas terendah, direkomendasikan diputuskan **setelah** ADR-006 (agar keputusan Redis, jika ada, diambil satu kali untuk dua kebutuhan sekaligus).

---

# 9. Governance Rules

**Kapan ADR boleh dibuat:** Setiap kali sebuah keputusan memengaruhi desain arsitektur atau implementasi teknis dengan **lebih dari satu alternatif yang secara wajar dipertimbangkan** — bukan untuk keputusan trivial yang tidak memiliki alternatif nyata. ADR baru wajib mengikuti format Bagian 4 secara lengkap (tidak boleh mengosongkan field).

**Kapan ADR boleh diubah:** ADR berstatus **Approved tidak boleh diedit langsung** isinya. Perubahan substantif menghasilkan **ADR baru** yang secara eksplisit menyebut ADR mana yang digantikannya — ADR lama kemudian diberi status **Superseded** (bukan dihapus). Perubahan redaksional kecil (typo, perbaikan tautan) yang tidak mengubah keputusan boleh diedit langsung tanpa ADR baru, namun tetap dicatat di `CHANGELOG.md`.

**Siapa yang berwenang menyetujui:** Mengikuti Review & Approval Matrix di `document-governance-baseline-register.md` Bagian 9 — untuk ADR arsitektur/teknis, Approver adalah **Technical Lead / Enterprise Solution Architect / CTO** (nama individu belum ditetapkan — lihat Bagian 1). **AI Coding Assistant tidak berwenang mengubah status ADR menjadi Approved** — AI dapat mengusulkan (Proposed) dan membantu Impact Analysis, tetapi Approval selalu memerlukan konfirmasi manusia.

**Hubungan ADR dengan Decision Log:** Setiap ADR di dokumen ini **berkorespondensi** dengan satu atau lebih entri di `decision-log.md` (dicatat di field *Affected Documents*/*Notes* masing-masing ADR). Decision Log tetap menjadi jurnal kronologis lengkap (termasuk keputusan non-arsitektur); dokumen ADR ini adalah **pandangan tersaring** (filtered view) khusus arsitektur/teknis dari subset entri yang sama. Jika sebuah ADR di sini disahkan/diubah, `decision-log.md` **wajib** menerima entri baru yang merujuk balik ke ADR terkait — bukan dua sumber yang berjalan sendiri-sendiri.

**Hubungan ADR dengan Changelog:** `CHANGELOG.md` mencatat **kapan** dan **apa** yang berubah di kode/rilis sebagai akibat dari sebuah ADR — ADR menjelaskan **mengapa**, Changelog mencatat **dampaknya di rilis**. Setiap kali status ADR berubah menjadi Approved dan diimplementasikan, entri terkait wajib muncul di `CHANGELOG.md` Release History pada versi rilis yang relevan.

**Hubungan ADR dengan Baseline Register:** Status **Baseline** sebuah dokumen teknis (`technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, dsb.) di `document-governance-baseline-register.md` **tidak dapat dicapai** selama ADR yang menaunginya masih berstatus Open — ini menegaskan kembali Baseline Rule 4.1 poin 3 di dokumen tsb ("tidak ada Open Decision yang secara langsung memengaruhi isi dokumen"). Dokumen ADR ini menjadi **input wajib** bagi proses penilaian kelayakan Baseline suatu dokumen turunan.

---

# 10. AI Usage Rules

1. **AI wajib membaca ADR ini sebelum membaca `technology-decisions.md`** — ADR menjelaskan alasan & alternatif di balik setiap baris "Official Technology Stack"; membaca katalog tanpa konteks keputusan berisiko AI salah memahami tingkat kepastian sebuah pilihan teknologi.
2. **AI tidak boleh mengabaikan ADR berstatus Approved** — termasuk larangan eksplisit yang tercatat di *Alternatives Considered* (mis. SWR, Redux, Formik, Moment.js, MUI, Ant Design, react-beautiful-dnd, Auth0/Clerk, service backend terpisah tanpa ADR-001 disahkan ulang).
3. **Jika terjadi konflik antara ADR berstatus Approved dan dokumen lain (`SYSTEM-ARCHITECTURE.md`, `technology-decisions.md`, dsb.), ADR ini menjadi referensi utama** sampai dokumen lain diperbarui secara resmi untuk mencerminkan ADR tsb — bukan sebaliknya. Ini menegaskan hierarki: keputusan arsitektur (ADR) lebih otoritatif daripada katalog/deskripsi yang menaunginya.
4. **Jika sebuah ADR masih berstatus OPEN, AI tidak boleh membuat asumsi apa pun** untuk melanjutkan implementasi pada area yang dipengaruhinya — AI **wajib berhenti dan meminta keputusan eksplisit dari pengguna/Technical Lead**, mengikuti pola *configurable placeholder* dengan penanda `// TODO: menunggu resolusi ADR-XXX` jika implementasi sementara benar-benar tidak dapat dihindari untuk pekerjaan yang tidak terdampak langsung.
5. **AI dapat mengusulkan ADR baru berstatus Proposed** ketika menemukan keputusan arsitektur yang belum tercatat di dokumen ini, namun **tidak berwenang** menaikkan statusnya menjadi Approved (lihat Bagian 9).

---

## Governance Notes

> Bagian tambahan ini mencatat temuan tata kelola yang muncul selama penyusunan ADR ini sendiri — bersifat advisory, tidak mengubah isi dokumen proyek manapun.

1. **Kolisi penomoran "ADR-XXX"**: Dokumen ini memakai `ADR-001`…`ADR-025` untuk topik arsitektur/teknis, sementara `decision-log.md` sudah lebih dulu memakai `ADR-001`…`ADR-037` untuk seluruh keputusan proyek (termasuk yang non-arsitektur). **Kedua rangkaian penomoran ini tidak sinkron** — mis. `ADR-001` di dokumen ini berarti "Backend Architecture (OPEN)", sedangkan `ADR-001` di `decision-log.md` berarti "Next.js App Router (Approved)". Ini berpotensi membingungkan siapa pun yang menyebut "ADR-001" tanpa menyebut dokumen sumbernya. **Tidak diputuskan sendiri di sini** — direkomendasikan sebagai keputusan governance terpisah (mis. memberi prefiks pembeda seperti `TADR-` untuk dokumen ini atau `DLG-` untuk Decision Log) yang perlu disahkan pemilik dokumentasi.
2. **Inkonsistensi jumlah seed role (7 vs 8)** — dikonfirmasi ulang di sini (lihat ADR-003 & ADR-024 Notes) sebagai gap penghitungan lintas dokumen turunan (`DEVELOPMENT-ROADMAP.md` menyebut 7, `CHANGELOG.md`/`CURRENT-PROJECT-STATE.md`/`decision-log.md` menyebut 8) — bukan ambiguitas pada keputusan role model itu sendiri. Perlu rekonsiliasi sebelum Sprint S0 migration seed ditulis.
3. **Gap administratif berulang** (Vercel, Resend, Sentry, frasa state management usang) yang tercatat di berbagai *Notes* ADR Bagian 4 seluruhnya bersifat sinkronisasi dokumen, bukan keputusan yang masih diperdebatkan — dikelompokkan di sini agar tidak dianggap sebanding tingkat urgensinya dengan ADR-001/005/006/008 yang benar-benar OPEN.

---

*Dokumen ini adalah Architecture Decision Records resmi proyek — sumber kebenaran tunggal untuk keputusan arsitektur/teknis, terpisah dari `decision-log.md` (jurnal kronologis seluruh keputusan). Tidak ada isi dokumen proyek lain yang diubah dalam penyusunannya. Wajib dirujuk oleh `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, `dependency-manifest.md`, Database Schema, `API-Specification`, dan Technical Specification setiap kali dokumen-dokumen tersebut direvisi.*
