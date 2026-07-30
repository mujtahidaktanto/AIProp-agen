# AI DEVELOPMENT BLUEPRINT
## Platform Web Real Estate Agency

**Audiens dokumen:** AI Coding Assistant (Bolt.new, Cursor, Claude Code, GPT, Copilot, dsb.) — dokumen operasional turunan dari `PROJECT-CONSTITUTION.md`.
**Versi:** 1.0 — disusun berdasarkan dokumen sumber v1.1 (26 Juli 2026): `PROJECT-CONSTITUTION.md`, `PRD-Real-Estate-Agency-Platform-v1.1.md`, `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`, `ERD-Diagram-v1.1.mermaid`, `API-Specification-Real-Estate-Agency-Platform-v1.1.md`, `User-Flow-Real-Estate-Agency-Platform-v1.1.md`, `SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md`.
**Sifat dokumen:** Panduan teknis operasional harian bagi AI Coding Assistant saat menulis kode. **Tidak menggantikan** `PROJECT-CONSTITUTION.md` — jika terjadi ketidaksesuaian, Constitution selalu menang (lihat Bagian 3).

---

## 1. PROJECT OVERVIEW

Platform ini mendigitalisasi operasional agensi properti Indonesia secara end-to-end, dengan model **B2B2C**: digunakan internal oleh agensi & agennya, dikonsumsi publik oleh calon pembeli/penyewa.

**Nilai jual utama:** *lead generation* (klik CTA WhatsApp) dan *tooling* (Learning Center, DBR Scoring, katalog developer) — **bukan** payment gateway untuk closing transaksi properti.

**11 Modul Fungsional:**
| # | Modul | Prioritas |
|---|---|---|
| 1 | Registrasi & Autentikasi Agen | Must Have |
| 2 | Profil Agen (+ Review/Rating, aktif Fase 1) | Must Have |
| 3 | Manajemen Listing Properti | Must Have |
| 4 | Learning Center | Must Have |
| 5 | Kalender Event | Should Have |
| 6 | Direktori Kerjasama Developer | Must Have |
| 7 | Sistem Scoring DBR (Kalkulator KPR) | Must Have |
| 8 | Dashboard & Notifikasi | Should Have |
| 9 | Admin Panel / CMS | Must Have |
| 10 | Manajemen Role & Hak Akses (RBAC) | Must Have |
| 11 | SEO, Analytics & Tracking | Must Have |

**7 Role sistem:** `superadmin` → `manager` → `admin` → `instructor` (terbatas Modul 4) → `agent`, ditambah `developer_partner` (eksternal, login opsional) dan `buyer` (eksternal, akun ringan opsional), plus **Guest/Lead** (publik tanpa akun).

**Roadmap Fase (WAJIB diikuti berurutan, lihat Bagian 28–29):**
- **Fase 1 (MVP):** Modul 1, 2, 3-dasar, 9-dasar, 10-dasar (RBAC Superadmin/Admin/Agen), 11-fondasi SEO.
- **Fase 2:** Modul 6 (Katalog Developer), Modul 7 (DBR Scoring).
- **Fase 3:** Modul 4 (Learning Center), Modul 5 (Kalender Event).
- **Fase 4:** Dashboard analitik lanjutan, gamifikasi, integrasi SLIK/BI Checking, payment/komisi otomatis.

**Prinsip arsitektur tertinggi:** keputusan yang mahal diubah kembali (strategi rendering, struktur URL/slug, skema RBAC inti) **wajib diselesaikan di Fase 1**, tidak ditunda.

---

## 2. ARCHITECTURE OVERVIEW

| Layer | Pilihan Final |
|---|---|
| Frontend | **Next.js (App Router, versi stabil terbaru)** — Server Components untuk SSR, ISR untuk halaman jarang berubah |
| Styling | Tailwind CSS + shadcn/ui (headless) |
| Backend/API | Next.js Route Handlers (BFF ringan) **atau** service backend terpisah (NestJS/Express TypeScript) — pilih **satu**, dikunci sebelum Fase 1 selesai, **dilarang dicampur** untuk domain yang sama |
| Database | **PostgreSQL via Supabase** |
| Auth | Supabase Auth (email/password, OTP, Google OAuth2) dibungkus JWT internal platform |
| Search | Typesense (rekomendasi) atau Elasticsearch |
| Storage/CDN | Supabase Storage / Cloudinary / ImageKit (bucket publik vs privat terpisah) |
| Cache/Rate Limit | Redis |
| Job Queue | Redis-based (BullMQ) atau Supabase Edge Functions + cron |
| Maps | Google Maps Platform atau Mapbox (belum final — implementasikan sebagai provider-agnostic) |
| Analytics | GTM + GA4, konfigurasi via `system_configs`, dikelola Superadmin |
| CI/CD | Git-based (GitHub Actions/GitLab CI) — lint, type-check, test, migration check wajib lolos sebelum merge |

**Pola rendering per tipe halaman (keputusan final, tidak boleh diubah tanpa persetujuan eksplisit):**

| Tipe Halaman | Rendering | Route Group |
|---|---|---|
| Homepage, Search/Filter, Detail Listing, Profil Publik Agen, Detail Proyek Developer | SSR/SSG/ISR | `(public)` |
| Login, Register, Forgot Password, Verify OTP | SSR (SEO tidak kritis tapi tetap perlu cepat) | `(auth)` |
| Dashboard Agen, Kalkulator DBR (hasil personal), Learning Center privat | CSR, `noindex, nofollow` | `(dashboard)` |
| Admin Panel (seluruh sub-menu) | CSR, `noindex, nofollow`, role-gated | `(admin)` |
| Halaman statis (Tentang, Privasi, FAQ) | SSG | `(public)` |

**Alur request tipikal (end-to-end):**
```
Client → Next.js (Server Component fetch / Route Handler)
       → Middleware: verifikasi JWT → cek role_permissions → resolve granted_scope
       → Service layer (business logic) → Repository layer (query DB)
       → Supabase Postgres (RLS aktif sebagai lapisan kedua)
       → Response envelope standar → Client
```

**Dua lapisan pertahanan wajib (tidak boleh hanya mengandalkan satu):**
1. **RBAC middleware di backend** — pengecekan permission eksplisit sebelum handler jalan.
2. **RLS di Supabase** — lapisan kedua, tidak menggantikan lapisan pertama.

---

## 3. SOURCE OF TRUTH

AI Coding Assistant **wajib** merujuk hierarki dokumen berikut saat ada pertanyaan/ambiguitas, **dari yang paling mengikat**:

1. **`PROJECT-CONSTITUTION.md`** — mengalahkan semua asumsi default framework/library apa pun; keputusan di "Riwayat Keputusan Arsitektur"-nya bersifat **final**, tidak perlu ditanyakan ulang.
2. **Dokumen ini (`AI-DEVELOPMENT-BLUEPRINT.md`)** — turunan operasional Constitution; jika ada isi yang tampak bertentangan dengan Constitution, **Constitution yang benar** — laporkan sebagai `// TODO: konflik blueprint vs constitution` dan tanyakan ke manusia.
3. **`ERD-Skema-Database-Real-Estate-Agency-v1.1.md`** + **`ERD-Diagram-v1.1.mermaid`** — satu-satunya sumber kebenaran struktur data.
4. **`API-Specification-Real-Estate-Agency-Platform-v1.1.md`** — satu-satunya sumber kebenaran kontrak endpoint.
5. **`PRD-Real-Estate-Agency-Platform-v1.1.md`** — sumber kebenaran business rules & acceptance criteria per modul.
6. **`User-Flow-Real-Estate-Agency-Platform-v1.1.md`** — sumber kebenaran urutan interaksi UI per role.
7. **`SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md`** — sumber kebenaran rendering, slug, structured data, tracking.

**Aturan sinkronisasi wajib (non-negotiable):**
- Setiap penambahan tabel/field baru → sinkron balik ke `ERD-Skema-Database.md` **dan** `ERD-Diagram.mermaid` di `/docs`.
- Setiap penambahan/perubahan endpoint → sinkron ke `API-Specification.md`, termasuk label Auth-nya.
- Dokumentasi **tidak boleh** menyimpang dari implementasi nyata — kode dan dokumen di `/docs` harus selalu konsisten.
- Item di "Hal Perlu Dikonfirmasi" pada dokumen sumber (threshold DBR final, model monetisasi, provider Maps/payment gateway, kebijakan eksklusivitas developer, dsb.) **bukan** keputusan yang boleh diambil sepihak oleh AI — implementasikan sebagai *configurable placeholder* (lihat Bagian 4 & 32).

---

## 4. DATABASE RULES

**Sumber kebenaran skema:** `ERD-Skema-Database-Real-Estate-Agency-v1.1.md` (37+ entitas, lihat daftar lengkap per modul di dokumen tsb).

### Aturan wajib
1. **Primary key:** UUID di seluruh tabel — bukan auto-increment integer (mencegah enumerasi listing/agen kompetitor).
2. **Migration murni SQL** (bukan ORM auto-sync di production) — setiap perubahan skema lewat file migrasi bernomor urut di `/apps/api/migrations`, dapat di-review, ada rencana rollback. **Tidak pernah** mengedit skema langsung lewat Supabase Studio di environment production.
3. **Soft delete (`deleted_at`) wajib** untuk `listings`, `users`, `developer_projects` — **dilarang** `DELETE` fisik pada tabel ini via aplikasi.
4. **Index wajib sejak migrasi awal** (bukan ditambahkan belakangan):
   - `listings(status, category, transaction_type, city_id, price)`
   - `listing_leads(listing_id, created_at)`
   - `dbr_simulations(agent_id, created_at)`
   - `developer_projects(city_id, status, property_type)`
   - `agent_reviews(agent_id, status)`
   - UNIQUE index: `listings.slug`, `developer_projects.slug`, `agent_profiles.public_slug`.
   - Full-text/trigram index: `listings.area_keyword`.
5. **Enkripsi at-rest wajib** untuk: `agent_verification_documents.file_url`, dan seluruh field finansial di `dbr_simulations` (`net_income`, `existing_installments`).
6. **Counter denormalisasi** (`listings.cta_click_count`, `agent_profiles.total_listings_sold/rented`) — **wajib** diperbarui via trigger DB atau scheduled job, **dilarang** dihitung on-the-fly per request. **Pengecualian:** `agent_reviews.rating` rata-rata **dikecualikan** — dihitung on-the-fly (`AVG(rating) WHERE status='approved'`) karena volume kecil di Fase 1.
7. **`agent_reviews.status` wajib difilter `= 'approved'`** di setiap query publik (profil agen, `aggregateRating` JSON-LD).
8. **`developer_projects.city_id`** (FK → `ref_cities`) — **dilarang** menambahkan kembali kolom freetext untuk lokasi proyek developer.
9. **Trigger wajib `url_redirects`**: setiap `UPDATE` pada `listings.slug`/`developer_projects.slug`, atau soft-delete permanen entitas berhalaman publik, **wajib** menulis baris baru ke `url_redirects` **sebelum** perubahan diterapkan (DB trigger atau hook aplikasi non-optional).
10. **Data wilayah Indonesia** (`ref_provinces/cities/districts/villages`) di-seed sekali dari dataset resmi, di-host internal — **dilarang** memanggil API pihak ketiga untuk data ini di request path pencarian.
11. **Constraint aplikasi non-SQL wajib:** mencegah `UPDATE`/`DELETE` pada `users` yang menghasilkan 0 user aktif dengan `role_id = superadmin`.
12. **Naming:** `snake_case`, tabel jamak (`listings`, bukan `listing`); enum value `snake_case` huruf kecil (`pending_review`).
13. **Konsistensi FE↔BE↔DB wajib**: field sama harus identik penamaannya di ketiga layer (DB `snake_case` → API JSON `snake_case` → hanya dikonversi ke `camelCase` di dalam kode TypeScript via mapper/DTO, **tidak boleh** "bocor" campuran di response API).
14. **Parameter bisnis yang bisa berubah tanpa deploy** (threshold DBR, masa expired listing, GTM Container ID, passing grade) **wajib** di `system_configs`/`dbr_config` — **tidak pernah** hard-code atau di environment variable.

### Larangan Mutlak
- ❌ Mengubah tabel yang sudah ada tanpa alasan yang disetujui.
- ❌ Mengubah nama field yang sudah dipakai FE/BE lain.
- ❌ Membuat tabel baru jika kebutuhan sudah tercakup tabel existing (mis. jangan buat `agent_ratings` baru — sudah ada `agent_reviews`).
- ❌ Menambah kolom lokasi freetext baru — selalu ikuti pola cascading `province_id → city_id → district_id` (kecuali `area_keyword`, maks 20 karakter, dikecualikan by design).
- ❌ Migration langsung ke production tanpa file migrasi & review.

---

## 5. FOLDER RULES

Struktur monorepo Next.js App Router (final, jangan diubah struktur besarnya tanpa persetujuan):

```
/apps
  /web
    /app
      /(public)/          # SSR/SSG — homepage, search, listing detail, agent profile, developer project
        /properti/[slug]/
        /agen/[slug]/
        /developer/[slug]/
        /cari/
      /(auth)/             # login, register, forgot-password, verify-otp
      /(dashboard)/        # CSR privat — agent dashboard, noindex
        /agen/
          /listing/
          /dbr/
          /learning-center/
      /(admin)/            # CSR privat — admin panel, noindex, role-gated
        /users/
        /listings/
        /rbac/
        /system-config/
      /api/                # Route handlers BFF tipis (jika tidak pakai service backend terpisah)
    /components
      /ui/                 # komponen dasar (button, input, card) — reusable, TANPA business logic
      /features/{module}/  # komponen spesifik per modul (listing-form, dbr-calculator, dsb.)
    /lib
      /api-client/         # wrapper fetch/axios ke backend, typed
      /supabase/           # supabase client (browser + server, TERPISAH)
      /seo/                # helper generate meta tag, JSON-LD, sitemap
      /validation/         # skema Zod, satu sumber kebenaran form + API
    /styles
    /public

  /api                     # Backend service terpisah (jika arsitektur split dipilih)
    /src
      /modules
        /auth/  /agents/  /listings/  /learning-center/  /events/
        /developer-projects/  /dbr-calculator/  /notifications/  /rbac/  /seo/
      /middleware/         # auth.middleware, rbac.middleware, rate-limit.middleware
      /shared/
        /errors/  /logger/  /validators/
      /jobs/                # cron & queue consumers
    /migrations             # SQL migration murni, sinkron ERD

/packages
  /shared-types/            # TypeScript types/interfaces — SATU-SATUNYA sumber definisi shape entitas
  /region-data/              # seed data wilayah Indonesia

/docs
  PROJECT-CONSTITUTION.md
  AI-DEVELOPMENT-BLUEPRINT.md
  PRD-Real-Estate-Agency-Platform-v1.1.md
  ERD-Skema-Database-Real-Estate-Agency-v1.1.md
  ERD-Diagram-v1.1.mermaid
  API-Specification-Real-Estate-Agency-Platform-v1.1.md
  User-Flow-Real-Estate-Agency-Platform-v1.1.md
  SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md
```

### Aturan folder wajib
- Halaman di `(public)` **dilarang** memakai data-fetching client-side murni (`useEffect` + `fetch`) sebagai sumber data utama — harus lewat Server Component agar SSR terpenuhi.
- Halaman di `(dashboard)` dan `(admin)` **wajib** menyertakan meta `robots: noindex, nofollow` di layout masing-masing route group — jangan mengandalkan `robots.txt` saja.
- `packages/shared-types` adalah **satu-satunya** tempat definisi tipe entitas — **dilarang** mendefinisikan ulang shape data yang sama di `/apps/web` dan `/apps/api` secara terpisah.
- Backend modules (`/modules/{nama}`) mengikuti struktur konsisten: `*.controller.ts`, `*.service.ts`, `*.repository.ts`, `*.schema.ts` (Zod), `*.types.ts` — **satu tanggung jawab per file**.
- **Sebelum menambah file baru**, AI wajib membaca struktur folder existing untuk memastikan tidak menduplikasi lokasi/pola yang sudah ada.

---

## 6. FILE NAMING RULES

| Konteks | Konvensi | Contoh |
|---|---|---|
| Tabel & kolom database | `snake_case`, tabel jamak | `listings`, `agent_verification_documents` |
| Enum value DB | `snake_case` huruf kecil | `pending_review`, `fully_furnished` |
| Endpoint REST | `kebab-case`, resource jamak | `/developer-projects`, `/agents/me/dbr-simulations` |
| Query param | `snake_case` | `?property_type=rumah&price_min=...` |
| JSON field API | `snake_case` | `"whatsapp_number"`, `"eligibility_status"` |
| Variabel & fungsi TS | `camelCase` | `getListingBySlug()`, `agentId` |
| Tipe & interface TS | `PascalCase` | `ListingEntity`, `DbrSimulationResult` |
| Komponen React (file) | `PascalCase.tsx`, sama dengan nama komponen | `ListingCard.tsx` |
| Konstanta enum aplikasi | `SCREAMING_SNAKE_CASE` | `LISTING_STATUS.PENDING_REVIEW` |
| Slug URL publik | lowercase, spasi→`-`, diakhiri `{short_id}` | `rumah-minimalis-2-lantai-bsd-city-9f21a` |
| Backend module file | `{nama}.controller.ts` / `.service.ts` / `.repository.ts` / `.schema.ts` / `.types.ts` | `listings.service.ts` |
| Nama branch Git | `{tipe}/{modul}-{ringkasan}` | `feat/m3-listing-crud`, `fix/m7-dbr-rounding` |
| Commit message | Conventional Commits | `feat(listing): add slug auto-generate with short id` |

**Aturan tambahan:** field yang sama harus memakai nama identik di DB → API JSON → hanya dikonversi ke `camelCase` **di dalam** kode TypeScript lewat mapper/DTO. Response API tidak pernah "bocor" campuran `snake_case`/`camelCase`.

---

## 7. COMPONENT RULES

1. **Functional component + hooks saja** — tidak ada class component baru.
2. **Business logic terpisah dari UI**: komponen React **tidak boleh** berisi kalkulasi bisnis (formula DBR, validasi ownership, dsb.) — logic wajib di `/lib` atau service backend, dapat di-unit-test tanpa render komponen.
3. **Dua kategori komponen, jangan dicampur:**
   - `/components/ui/` — komponen dasar (Button, Input, Card, Modal) — reusable lintas modul, **tanpa** pengetahuan tentang domain bisnis (tidak tahu apa itu "listing" atau "agen").
   - `/components/features/{module}/` — komponen spesifik domain (`ListingForm`, `DbrCalculator`, `AgentReviewCard`) — boleh memanggil hooks domain & memakai komponen `ui/`.
4. **Selalu gunakan komponen `ui/` yang sudah ada** sebelum membuat komponen baru serupa — cek dulu apakah `Button`/`Input`/`Modal` yang dibutuhkan sudah tersedia.
5. **Props typed eksplisit** — tidak ada `any`; komponen memakai tipe dari `packages/shared-types` jika merepresentasikan entitas domain.
6. **Satu komponen = satu file**, nama file = nama komponen (`PascalCase.tsx`).
7. **Global error boundary** per route group (React Error Boundary) — kegagalan satu widget dashboard tidak boleh mematikan seluruh halaman.
8. **Progressive disclosure RBAC**: komponen menu/opsi yang tidak relevan bagi role tertentu (mis. Agen tidak boleh lihat menu RBAC) **disembunyikan sepenuhnya**, bukan sekadar `disabled`, agar tidak membocorkan struktur internal.
9. **Mobile-first**: komponen form (listing form, kalkulator DBR) wajib diuji layak pakai di layar kecil — agen bekerja di lapangan.

---

## 8. REACT RULES

1. TypeScript `strict: true` — tidak ada `any` implisit.
2. Hooks kustom domain (`useListingForm`, `useDbrCalculator`) ditempatkan di `/lib` atau folder `hooks/` per modul feature — **tidak** inline besar di dalam komponen.
3. **Tidak ada fetching data langsung di komponen `(public)` route group** via `useEffect` — gunakan Server Component / data-fetching Next.js agar SSR terpenuhi (lihat Bagian 9 & 2).
4. Komponen di `(dashboard)`/`(admin)` boleh CSR penuh, memakai state management client (lihat Bagian 11) dan data-fetching client (`useEffect`/SWR/React Query — pilih satu, konsisten).
5. **Tidak ada duplikasi state**: state form domain divalidasi lewat skema Zod yang sama dengan backend (lihat Bagian 17) — jangan menulis ulang aturan validasi manual di komponen.
6. **Debounce wajib** untuk input filter pencarian (Core Web Vitals INP < 200ms, lihat SEO Spec §5).
7. **Reserve `width`/`height`/`aspect-ratio`** untuk semua gambar/banner sebelum termuat (mencegah CLS).
8. Setiap komponen yang menampilkan data ber-scope kepemilikan (listing, profil, simulasi DBR) **wajib** menerima data yang sudah difilter dari server — komponen **tidak** melakukan filter ownership sendiri di client (itu tanggung jawab backend/RLS).

---

## 9. NEXT.JS RULES

1. **App Router** wajib — tidak menggunakan Pages Router untuk fitur baru.
2. **Server Components sebagai default** untuk halaman `(public)`; `"use client"` hanya untuk komponen yang butuh interaktivitas (form, filter, peta interaktif).
3. **Rendering per tipe halaman mengikuti tabel di Bagian 2** — ini keputusan arsitektur final, tidak dinegosiasikan ulang per fitur.
4. **ISR** dipakai untuk halaman yang jarang berubah (detail listing yang sudah lama publish, profil agen) dengan revalidasi event-driven saat data berubah (lihat sitemap regen di Bagian 21).
5. **Meta tag & JSON-LD** di-generate lewat helper `/lib/seo/` — **satu sumber kebenaran** template meta title/description (lihat SEO Spec §2.1), jangan hard-code template di setiap page file.
6. **Route group `robots: noindex, nofollow`** wajib di layout `(dashboard)` dan `(admin)` — cek Bagian 5.
7. **Lazy-load** skrip peta interaktif dan komponen berat non-kritis (INP target < 200ms).
8. **Image component Next.js** (`next/image`) wajib untuk foto listing — bukan `<img>` biasa — agar resize sesuai viewport dari CDN otomatis terjadi (LCP target < 2.5 detik).
9. **API Route Handlers** (`/app/api/`) hanya dipakai sebagai BFF tipis **jika** arsitektur split backend tidak dipilih (lihat Bagian 2) — jangan taruh business logic kompleks (DBR calculation, RBAC resolution) langsung di route handler; delegasikan ke `/lib` service layer yang sama dipakai kedua arsitektur.

---

## 10. SUPABASE RULES

1. **Row Level Security (RLS) wajib aktif** di seluruh tabel ber-scope kepemilikan (`listings`, `agent_profiles`, `dbr_simulations`, `agent_verification_documents`, `notifications`, dsb.) — **lapisan kedua**, RBAC middleware backend tetap wajib sebagai lapisan pertama.
2. **Service role key** (bypass RLS) **hanya** dipakai server-side untuk operasi admin/job terjadwal (regenerasi sitemap, sinkronisasi counter) — **dilarang keras** ter-expose ke client (browser/mobile). Audit build output berkala untuk memastikan tidak ter-bundle.
3. **Supabase Auth** dipakai untuk mekanisme login saja (email/OTP/Google OAuth) — **role/permission tetap dikelola di tabel `roles`/`role_permissions` milik aplikasi**, bukan `auth.users` metadata sebagai satu-satunya sumber kebenaran role.
4. **Storage buckets dipisah tegas:**
   - Publik (`listing-photos`, `listing-videos`, `developer-project-media`) — boleh CDN publik, dioptimasi WebP/AVIF.
   - Privat (`agent-verification-documents`) — **tidak pernah publik**, akses hanya via signed URL berumur pendek untuk `superadmin`/`manager`/`admin` saat review.
5. **Realtime subscriptions** (notifikasi in-app) **wajib** difilter RLS per `user_id` — tidak boleh subscribe ke tabel penuh tanpa filter.
6. **Edge Functions** untuk logic dekat-DB & idempotent (trigger `url_redirects`, panggilan Google Indexing API saat listing publish) — **bukan** untuk business logic kompleks yang lebih cocok di backend service utama.
7. **Migration lewat Supabase CLI migration files** disimpan di repo (`/apps/api/migrations`) — **tidak** mengedit skema langsung lewat Supabase Studio di production.

---

## 11. STATE MANAGEMENT RULES

1. **Server state vs client state dipisah tegas:**
   - Data dari API (listing, profil, simulasi DBR) = *server state* — dikelola lewat data-fetching layer (`lib/api-client`) + caching library (React Query/SWR — pilih satu secara konsisten project-wide, jangan campur keduanya).
   - UI state lokal (toggle modal, step wizard form, filter sementara sebelum submit) = *client state* — cukup `useState`/`useReducer`, tidak perlu global store.
2. **Global client state** (jika dibutuhkan, mis. current user session di client) memakai React Context terbatas — **bukan** Redux/Zustand kecuali kompleksitas benar-benar membutuhkan (harus dijustifikasi, bukan default).
3. **Form state** dikelola oleh library form (React Hook Form direkomendasikan) terintegrasi dengan skema Zod yang sama dipakai backend (lihat Bagian 17) — **jangan** menyimpan validasi form di state management terpisah.
4. **Tidak ada duplikasi source of truth**: data yang sudah ada di server state (mis. daftar `ref_cities` untuk dropdown) **tidak** disalin manual ke state lokal yang bisa stale — selalu re-fetch/cache via layer server state.
5. **State ownership scope**: state yang merepresentasikan data ber-scope kepemilikan (listing milik agen X) **tidak boleh** dicampur di store global lintas user — setiap fetch selalu terikat sesi user yang sedang login (token JWT saat ini).

---

## 12. AUTHENTICATION RULES

1. **JWT Bearer Token**: access token umur pendek (15–60 menit), refresh token umur panjang (30 hari) — refresh token disimpan sebagai **httpOnly secure cookie**, **bukan** localStorage (mitigasi XSS).
2. **Metode login didukung:** email/password (dengan OTP verification saat registrasi), Google OAuth2 (verifikasi `id_token` **server-side** via Google Auth Library resmi — **tidak boleh** trust token client tanpa verifikasi server).
3. **Login via Google untuk role `agent` tetap melalui alur `pending_review`** (wajib upload dokumen legalitas) — Google OAuth **tidak** melewati approval manual. Untuk role `buyer`, akun langsung `active` setelah OAuth berhasil.
4. **Endpoint sensitif** (`/auth/login`, `/auth/register`, `/auth/verify-otp`, `/auth/resend-otp`, `/auth/forgot-password`) **wajib** rate-limited 5 req/menit/IP+identifier.
5. **Manajemen sesi:** `POST /auth/logout` (invalidasi 1 device) dan `POST /auth/logout-all` (invalidasi seluruh sesi) wajib benar-benar menghapus refresh token dari storage (blocklist Redis/DB), bukan hanya menghapus cookie client.
6. **Password hashing** wajib algoritma adaptif (bcrypt/argon2) — **tidak boleh** SHA/MD5 telanjang.
7. Endpoint registrasi (`POST /auth/register`) menerima pilihan `role`: `buyer` atau `agent`. Role `agent` → status awal `pending_review`; role `buyer` → langsung `active` setelah verifikasi OTP.
8. Password reset flow: `forgot-password` → `reset-password` (butuh reset token) — token bersifat sekali pakai & kadaluarsa.

---

## 13. AUTHORIZATION RULES

Mengacu penuh PRD Modul 10 & ERD §2.28–2.30.

1. **Struktur data:** `roles` → `permissions` (`module_code` + `action_code`, unik) → `role_permissions` (pivot dengan `granted_scope` override & `editable_by_role_code`).
2. **Middleware wajib di backend untuk setiap endpoint** — menyembunyikan tombol di UI **tidak cukup**. Response untuk role tanpa akses: halaman/response "Akses Ditolak" informatif (bukan error generik 500), `error.code = "FORBIDDEN_ROLE_ACCESS"`.
3. **Urutan pengecekan baku (API Spec §0.6):**
   1. Validasi token → identifikasi `role_id`.
   2. Cek permission `module_code + action_code`.
   3. Jika `granted_scope = 'own'` → filter otomatis `WHERE agent_id = current_user.id`.
   4. Jika `granted_scope = 'all'` → tanpa filter kepemilikan — tetap wajib lolos pengecekan permission.
   5. Jika `granted_scope = 'none'` → 403.
   6. `superadmin` selalu bypass (short-circuit `true`) sebelum langkah 2.
4. **Tidak ada level `scoped`/tim/wilayah** — keputusan final, jangan diimplementasikan tanpa perubahan skema eksplisit yang disetujui ulang.
5. **Data privat milik user lain → 404** (bukan 403) untuk mencegah enumerasi keberadaan resource.
6. **Setiap perubahan role/permission wajib tercatat di `audit_logs`** (siapa, kapan, nilai lama/baru), berlaku real-time pada request berikutnya — token yang sudah terbit tetap dicek ulang scope-nya **per-request** di backend (tidak perlu re-login).

---

## 14. RBAC RULES

**Hierarki:** `superadmin` → `manager` → `admin` → `instructor` (terbatas Modul 4) → `agent`; plus `developer_partner`, `buyer`, Guest.

### Hard Rules Lintas Role (tidak boleh dilonggarkan lewat Permission Matrix Editor manapun)
1. **Agen tidak pernah** dapat mengedit/menghapus listing atau profil agen lain — enforcement di level `agent_id` ownership **di kode**, bukan sekadar permission konfigurasi. Bahkan jika Manager keliru memberi `granted_scope=all` ke role Agen, backend **tetap** menolak (403) `UPDATE`/`DELETE` Agen atas baris bukan miliknya.
2. **`superadmin` selalu bypass** pengecekan `role_permissions` (short-circuit `true` di kode).
3. Sistem **wajib mencegah** penghapusan/downgrade akun `superadmin` terakhir yang aktif.
4. **Manager hanya boleh `UPDATE` baris `role_permissions`** di mana `editable_by_role_code` memuat kode role si pengubah — divalidasi di **aplikasi**, bukan hanya UI. Manager hanya berwenang mengubah permission role `agent`.
5. **Manager cakupan akses selalu global** (`granted_scope='all'`) — tidak ada mode "scoped tim/wilayah" dalam bentuk apa pun.
6. **Assign Role:** Superadmin dapat mengubah role user apa pun. Manager dapat promote/demote **hanya** antara Agen ↔ Admin — **tidak dapat** membuat/mengubah/menghapus akun ber-role Manager atau Superadmin.
7. Setiap perubahan role/permission **wajib** tercatat di `audit_logs`.

### Matriks Ringkas per Role
| Role | Scope Listing/Profil/DBR | Admin Panel | Konfigurasi Sistem | Ubah Permission |
|---|---|---|---|---|
| Superadmin | Global, tanpa batas | Penuh | Ya | Semua role |
| Manager | Global (`all`) | Operasional Admin + kelola permission Agen | Tidak | Hanya role `agent` |
| Admin | Global (`all`) | Operasional standar | Tidak | Tidak |
| Instructor | Modul 4 saja | Learning Center saja | Tidak | Tidak |
| Agen | `own` saja | Tidak ada akses | Tidak | Tidak |
| Developer Partner | Portal pengajuan miliknya sendiri | Tidak ada akses | Tidak | Tidak |
| Buyer | Simpan listing, submit review (status `pending`) | Tidak ada akses | Tidak | Tidak |

**Implementasi wajib:** setiap query/endpoint yang menyentuh data ber-scope (`listings`, `dbr_simulations`, `agent_profiles`) menambahkan filter `WHERE agent_id = :current_user_id` ketika `granted_scope='own'`. Untuk `granted_scope='all'`, query tanpa filter kepemilikan namun **tetap** melalui pengecekan permission.

---

## 15. CRUD PATTERN

Pola standar untuk seluruh entitas ber-CRUD (listings, agent_profiles, courses, developer_projects, events, dsb.):

1. **Create:**
   - Validasi Zod di client (real-time) **dan** server (wajib, tidak percaya client) — lihat Bagian 17.
   - Set `agent_id`/`owner_id` dari `current_user.id` di server — **tidak pernah** dari payload client (mencegah spoofing ownership).
   - Status awal entitas mengikuti lifecycle masing-masing modul (mis. listing → `draft`).
   - Audit log untuk aksi sensitif (approval, moderasi).
2. **Read (list):**
   - Selalu paginated (`?page=1&per_page=20&sort=created_at&order=desc`) — **tidak ada** endpoint mengembalikan seluruh baris tanpa limit (lihat Bagian 22).
   - Filter `granted_scope` diterapkan otomatis di repository/service layer, bukan di controller.
3. **Read (detail):**
   - Data privat milik user lain → 404, bukan 403.
   - Untuk halaman publik (`(public)` route group): via Server Component, bukan client fetch.
4. **Update:**
   - Backend **selalu** validasi ulang ownership (`agent_id = current_user.id`) sebelum `UPDATE`, terlepas dari hasil pengecekan permission.
   - Perubahan field sensitif (slug, harga listing Primary yang tertaut developer) mengikuti aturan bisnis modul terkait (lihat catatan di Bagian 4 & PRD Modul 3).
   - Perubahan `slug` **wajib** memicu penulisan baris ke `url_redirects` sebelum diterapkan.
5. **Delete:**
   - **Soft delete** (`deleted_at`) untuk `listings`, `users`, `developer_projects` — **dilarang** `DELETE` fisik.
   - Penghapusan entitas berhalaman publik **wajib** menulis `url_redirects` (redirect 301 ke halaman kategori/pencarian terdekat).
6. **Konsistensi lintas modul**: pola CRUD yang sama dipakai ulang untuk modul baru — **jangan** menciptakan pola CRUD baru per modul tanpa alasan kuat; ikuti struktur `*.controller.ts` / `*.service.ts` / `*.repository.ts` yang sudah ada.

---

## 16. FORM PATTERN

1. **Satu skema Zod per entitas form**, didefinisikan sekali di `packages/shared-types` atau `lib/validation` — dipakai **baik** di client (real-time feedback) **maupun** server (validasi ulang wajib) — **tidak diduplikasi manual** di dua tempat.
2. **Form multi-step** (mis. form listing dengan banyak grup field: Informasi Dasar, Lokasi, Harga, Spesifikasi, Legalitas, Media) — state per-step dikelola client, **submit final** tetap tervalidasi utuh terhadap skema Zod lengkap sebelum status berubah ke `pending_review`.
3. **Field lokasi cascading** (province_id → city_id → district_id): dropdown ke-2 dan ke-3 **wajib** disabled/kosong sampai dropdown sebelumnya dipilih, opsi difetch sesuai parent terpilih — **tidak** memakai field freetext, kecuali `area_keyword` (maks 20 karakter).
4. **Konversi satuan di client sebelum submit**: form DBR menampilkan tenor dalam **tahun** untuk kenyamanan pengguna, tapi **wajib dikonversi ke bulan (tahun × 12)** sebelum payload dikirim ke API — `tenor_months` adalah satu-satunya kontrak data.
5. **Pesan error validasi dalam Bahasa Indonesia** yang jelas untuk pengguna akhir (agen) — terpisah dari `error.code` teknis (SCREAMING_SNAKE_CASE) untuk debugging.
6. **Auto-fill dari sumber lain** (mis. nomor WA CTA listing auto-terisi dari `agent_profiles.whatsapp_number`, dapat di-override per listing) — hindari duplikasi input manual jika data sudah tersedia dari profil.
7. **Draft/autosave** direkomendasikan untuk form panjang (listing) agar agen di lapangan dengan koneksi tidak stabil tidak kehilangan input.

---

## 17. VALIDATION PATTERN

1. **Zod sebagai satu sumber skema validasi** — dipakai frontend **dan** backend. **Backend tidak boleh mempercayai validasi frontend** — validasi ulang di server wajib untuk semua endpoint mutating (`POST`/`PUT`/`PATCH`).
2. **Field wajib listing** (judul, lokasi lengkap cascading, harga, minimal 3 foto, status legalitas, nomor WA) divalidasi **sebelum** status listing bisa berubah ke `pending_review`.
3. **Field lokasi administratif** (`province_id`/`city_id`/`district_id`) divalidasi terhadap **keberadaan baris** di tabel referensi — bukan hanya format UUID; cascading (city harus benar berada di province terpilih).
4. **`area_keyword`**: validasi panjang maksimal 20 karakter, freetext, **tidak** divalidasi terhadap data wilayah.
5. **Data finansial DBR** (`net_income`, `existing_installments`): validasi tipe numerik positif, batas wajar (tidak boleh negatif/nol untuk `net_income`), **konversi tenor tahun→bulan dilakukan di layer validasi frontend** sebelum payload dikirim.
6. **Validasi bisnis gagal** (bukan validasi format) → HTTP 422 (mis. DBR melebihi threshold saat submit final, kuota event penuh) — beda dengan 400 (format input salah).
7. **Validasi tipe file di server** (magic bytes/MIME type sesungguhnya, bukan ekstensi nama file client) untuk upload foto/dokumen — mencegah file executable menyamar sebagai gambar.
8. **Skema validasi tidak diduplikasi** antara `/apps/web` dan `/apps/api` — letakkan di `packages/shared-types`/`lib/validation` sebagai satu sumber import.

---

## 18. API PATTERN

Mengikuti `API-Specification-Real-Estate-Agency-Platform-v1.1.md` secara ketat.

1. **Base URL & versioning:** `https://api.<domain>.id/api/v1` — perubahan breaking wajib naik versi (`/v2`), **tidak boleh** mengubah kontrak `/v1` yang sudah live.
2. **Auth header:** `Authorization: Bearer {access_token}`. Setiap endpoint punya label eksplisit di dokumentasi: `Public`, `Authenticated`, atau role spesifik (termasuk `Buyer`).
3. **Response envelope wajib** (lihat Bagian 8 Constitution):
   ```json
   // Sukses
   { "success": true, "data": { ... }, "meta": { "page": 1, "per_page": 20, "total": 134 } }
   // Gagal
   { "success": false, "error": { "code": "LISTING_NOT_FOUND", "message": "...", "details": null } }
   ```
4. **Kode status HTTP** mengikuti tabel standar (200/201, 400, 401, 403, 404, 409, 422, 429) — data privat milik user lain **disamarkan 404**, bukan 403.
5. **Pagination standar** di semua endpoint list: `?page=1&per_page=20&sort=created_at&order=desc`.
6. **Rate limiting:** publik 60 req/menit/IP; authenticated 300 req/menit/user; endpoint sensitif 5 req/menit/IP+identifier.
7. **Idempotency:** endpoint `POST` yang bisa dipicu ganda double-click (`POST /listings/{id}/cta-click`, `POST /courses/{id}/enroll`) wajib idempotent atau punya guard duplikat (`UNIQUE` constraint DB).
8. **RBAC middleware wajib di setiap endpoint** — urutan pengecekan mengikuti Bagian 13.
9. **Filter geografis:** endpoint publik yang menerima lokasi wajib menerima `province_id`/`city_id`/`district_id` (UUID), **bukan** nama teks bebas. Pemetaan slug URL human-readable (`?kota=tangerang-selatan`) ke ID adalah tanggung jawab **frontend**, bukan mengubah kontrak API.
10. **Tenor DBR:** field `tenor_months` pada `POST /calculator/dbr` **selalu dalam bulan** — tidak ada varian endpoint/parameter yang menerima tahun.
11. **Review agen:** `POST /agents/{id}/reviews` (Auth: Buyer) selalu membuat baris `pending`; hanya `PUT /admin/agent-reviews/{id}/approve` (Superadmin/Manager/Admin) yang menampilkannya publik & masuk `aggregateRating`.
12. **Kode error** memakai format `SCREAMING_SNAKE_CASE` deskriptif, didaftarkan di satu file konstanta pusat (`packages/shared-types/error-codes.ts`).

---

## 19. ERROR HANDLING PATTERN

1. Seluruh error API **wajib** memakai envelope standar (Bagian 18).
2. **Tidak boleh** membocorkan detail internal (stack trace, query SQL, nama tabel) ke response API — detail teknis hanya masuk ke log server.
3. **Data privat milik user lain → 404** (bukan 403) — mencegah enumerasi keberadaan resource.
4. **Percobaan akses tanpa izin RBAC → 403** dengan `error.code = "FORBIDDEN_ROLE_ACCESS"` dan pesan informatif (bukan generik).
5. **Validasi bisnis gagal (bukan format) → 422** (mis. DBR melebihi threshold, kuota event penuh).
6. **Global error boundary** di frontend (React Error Boundary per route group) — kegagalan satu widget dashboard tidak mematikan seluruh halaman.
7. Setiap error tak tertangani backend **wajib** ter-log dengan `request_id`/`correlation_id` yang sama dengan yang dikembalikan ke client (`error.details.request_id`) untuk mempermudah tracing.
8. **Data sensitif dilarang masuk log** dalam bentuk plain text: `net_income`, `existing_installments`, KTP/NPWP, password (bahkan hash), token JWT penuh (hanya prefix/hash untuk korelasi).

---

## 20. LOADING PATTERN

1. **Server Component (SSR/SSG/ISR)**: gunakan Next.js `loading.tsx` per route segment untuk skeleton/fallback saat data-fetching server berlangsung — hindari layout shift (CLS).
2. **Client-side fetch (dashboard/admin)**: tampilkan skeleton loader / spinner konsisten dari komponen `ui/` (`Skeleton`, `Spinner`) — jangan buat loading indicator baru per fitur.
3. **Reserve dimensi gambar** (`width`/`height`/`aspect-ratio`) sebelum konten termuat — wajib untuk CLS < 0.1.
4. **Optimistic UI** boleh dipakai untuk aksi ringan non-kritis (mis. toggle simpan listing favorit oleh Buyer) — **tidak** untuk aksi yang mengubah data finansial/status approval (harus tunggu konfirmasi server).
5. **Progressive/lazy load** untuk komponen berat (peta interaktif, chart dashboard) — jangan blok render halaman utama.
6. **Error state & empty state** wajib disediakan berdampingan dengan loading state pada setiap komponen data-fetch (tiga state minimum: loading, empty, error, success).

---

## 21. SEARCH PATTERN

1. **Search engine:** Typesense (rekomendasi) atau Elasticsearch — dipakai untuk `/properties/search`, `/properties/autocomplete`, mendukung typo-tolerance & filter kombinasi (AND logic).
2. **Filter kombinasi** (kategori, tujuan transaksi, tipe properti, lokasi/radius, range harga, range luas, kamar tidur/mandi, status legalitas) — hasil filter **disimpan sebagai URL query** agar dapat dibagikan (`?kota=tangerang-selatan&tipe=rumah`).
3. **URL publik memakai query string human-readable** (`?kota=tangerang-selatan`) — pemetaan ke `city_id` internal adalah tanggung jawab **frontend**, backend tetap menerima ID.
4. **Debounce wajib** pada input filter/keyword bebas (target INP < 200ms).
5. **Data wilayah Indonesia dilayani dari DB internal**, bukan API pihak ketiga per-request pencarian.
6. **Sort options:** Terbaru, Harga Terendah–Tertinggi, Harga Tertinggi–Terendah, Terpopuler (paling banyak klik CTA).
7. **Mode tampilan:** List dan Peta (Map View) — keduanya memakai hasil query yang sama, hanya beda presentasi.
8. **Full-text/trigram index** untuk `listings.area_keyword` (freetext) agar pencarian keyword kawasan tetap cepat.

---

## 22. PAGINATION PATTERN

1. **Wajib di semua endpoint list** — tidak ada endpoint yang mengembalikan seluruh baris tanpa limit.
2. **Query param standar:** `?page=1&per_page=20&sort=created_at&order=desc`.
3. **Response meta wajib:** `{ "page": 1, "per_page": 20, "total": 134 }` di envelope sukses.
4. **Default `per_page`:** 20, dengan batas maksimum yang wajar (mis. 100) untuk mencegah query berat.
5. **Cursor-based pagination** boleh dipertimbangkan untuk feed dengan volume sangat tinggi (fase lanjutan) — **tidak** menggantikan pola offset-based di atas tanpa keputusan arsitektur eksplisit.
6. **Counter agregat** (`total_listings_sold`, `cta_click_count`) dibaca dari kolom denormalisasi, **bukan** `COUNT()` on-the-fly saat load dashboard (lihat Bagian 4 poin 6).

---

## 23. IMAGE UPLOAD PATTERN

1. **Foto listing:** minimal 3 foto wajib sebelum submit review, format JPEG/PNG/WebP, upload lewat `POST /listings/{id}/media`, disimpan ke CDN publik dengan transformasi otomatis (resize sesuai viewport, kompresi WebP/AVIF) — **tidak** menyimpan file resolusi penuh mentah sebagai satu-satunya salinan.
2. **`alt_text` wajib terisi** untuk setiap foto (auto-generate dari template `"{title} - foto {n}"` jika agen tidak mengisi manual) — untuk SEO gambar.
3. **Video/virtual tour:** opsional; validasi ukuran file & durasi maksimal ditentukan saat implementasi sebagai **konfigurasi**, bukan angka arbitrer hard-code.
4. **Dokumen legalitas agen** (KTP/NPWP/sertifikasi): upload ke **bucket privat terpisah**, **wajib** dienkripsi at-rest, **tidak pernah** melalui CDN publik, akses hanya via signed URL berumur pendek untuk role review (`superadmin`/`manager`/`admin`).
5. **Validasi tipe file di server** (magic bytes/MIME type, bukan ekstensi nama file client saja).
6. **Foto cover:** dipilih dari salah satu foto yang sudah diupload (`is_cover=true`), hanya satu foto cover aktif per listing — enforce di **service layer** (bukan constraint DB tunggal, karena butuh "hanya satu true").
7. **`next/image`** wajib dipakai di frontend untuk rendering foto listing (lihat Bagian 9).

---

## 24. STORAGE PATTERN

1. **Bucket dipisah tegas** (lihat Bagian 10):
   - Publik: `listing-photos`, `listing-videos`, `developer-project-media`.
   - Privat: `agent-verification-documents` — tidak pernah publik.
2. **Signed URL berumur pendek** untuk seluruh akses dokumen privat — **tidak ada** URL publik permanen untuk KTP/NPWP.
3. **CDN** (Cloudinary/ImageKit/Supabase Storage) untuk konten publik, dengan transformasi otomatis format modern (WebP/AVIF) & lazy-loading.
4. **API key pihak ketiga dipisah**: client-key (dibatasi domain/referrer, quota rendah) vs server-key (rahasia, quota penuh) — khususnya Google Maps & Google Indexing API.
5. **Storage untuk file yang mendukung fitur** (materi kursus Learning Center, brosur developer) mengikuti pola bucket yang sama — publik jika materi marketing, privat jika terbatas internal.

---

## 25. NOTIFICATION PATTERN

1. **Kanal:** in-app, email, dan push (opsional WA Business API — fase lanjutan).
2. **Jenis notifikasi** (`notifications.type`): `approval_status`, `event_reminder`, `listing_expiring`, `certificate_issued`, `lead_new`, `lainnya`.
3. **Selalu personal per user** — notifikasi approval hanya dikirim ke agen ybs dan ke role yang berwenang approve sesuai RBAC. **Tidak ada** notifikasi lintas-scope yang bocor ke role tanpa akses terkait.
4. **Realtime subscription (jika dipakai)** wajib difilter RLS per `user_id` — tidak subscribe ke tabel penuh tanpa filter (lihat Bagian 10).
5. **Job queue** (BullMQ/Edge Functions + cron) menangani: pengiriman notifikasi terjadwal (listing akan expired, event reminder), bukan dikirim sinkron di request path utama.
6. **Data ke GA4/GTM dari notifikasi tidak boleh menyertakan PII** (nama lengkap, no. HP, email calon pembeli) — hanya event & parameter agregat.

---

## 26. DASHBOARD PATTERN

1. **Cakupan data per role:**
   - Superadmin, Manager, Admin: data **global** (seluruh agen, listing, transaksi/simulasi) — ketiganya setara dalam cakupan visibilitas.
   - Agen: hanya data **miliknya sendiri** (listing sendiri, lead sendiri, progress kursus sendiri, prospek DBR sendiri).
2. **Ringkasan dashboard Agen:** jumlah listing aktif & status, jumlah lead (klik CTA WA) 7/30 hari terakhir, progress kursus Learning Center, event mendatang yang di-RSVP, riwayat simulasi DBR terbaru.
3. **Ringkasan dashboard Admin/Manager/Superadmin:** agen baru pending approval, listing pending review, engagement Learning Center, laporan proyek per agen.
4. **Counter agregat dashboard dibaca dari kolom denormalisasi** — bukan `COUNT()`/agregasi on-the-fly setiap load (lihat Bagian 4 & 22).
5. **CSR + `noindex, nofollow`** — dashboard tidak butuh SEO, harus dicegah dari indeks mesin pencari.
6. **Setiap widget dashboard dibungkus error boundary sendiri** — kegagalan satu widget (mis. chart lead) tidak mematikan widget lain.

---

## 27. ADMIN PANEL PATTERN

1. **Sub-menu utama:** Manajemen User, Moderasi Listing, Kelola Learning Center, Kelola Developer & Proyek, Konfigurasi Sistem, RBAC/Permission, Laporan & Analitik.
2. **Akses bertingkat sesuai role** (lihat Bagian 14 matriks) — menu yang tidak relevan bagi role tertentu **disembunyikan penuh** dari UI (bukan disabled).
3. **Moderasi mengikuti pola baku `status: pending → approved/rejected`** — dipakai konsisten untuk: registrasi agen, listing, review agen (`agent_reviews`), event dari developer partner. **Pola `agent_reviews` adalah referensi baku** untuk fitur user-generated content baru di masa depan.
4. **Setiap perubahan lewat Admin Panel tercatat di `audit_logs`** — tidak dapat dihapus oleh siapa pun kecuali proses retensi resmi terjadwal.
5. **Konfigurasi sistem** (threshold DBR, suku bunga default, masa expired listing, passing grade kursus) **hanya** dapat diubah lewat sub-menu Konfigurasi Sistem — tersimpan di `system_configs`/`dbr_config`, bukan hard-code.
6. **Laporan & analitik** mendukung export ke Excel/PDF.
7. **Setiap penambahan tabel moderasi baru di luar `agent_reviews`** yang menyentuh data sensitif/UGC publik wajib melalui review keamanan eksplisit sebelum dikembangkan — bukan diasumsikan aman karena "fitur tambahan kecil".

---

## 28. MODULE DEPENDENCY

Peta ketergantungan antar-modul (referensi saat AI menentukan urutan implementasi dalam satu fase):

```
Modul 10 (RBAC)          ← fondasi wajib SEMUA modul lain (middleware permission)
Modul 1 (Auth)           ← fondasi Modul 2, 3, 4, 5, 6, 7, 8, 9
Modul 2 (Profil Agen)    ← depends on Modul 1
                         → dipakai Modul 3 (WA auto-fill), Modul 4 (badge), Modul 8 (ringkasan)
Modul 3 (Listing)        ← depends on Modul 1, 2, referensi wilayah (ref_provinces/cities/districts)
                         → terhubung Modul 6 (Primary listing ← developer_projects)
                         → sumber data Modul 8 (dashboard lead/listing), Modul 11 (SEO per listing)
Modul 4 (Learning Center)← depends on Modul 1 (Agen/Instructor)
                         → badge di Modul 2, terhubung Modul 5 (kelas live)
Modul 5 (Kalender Event) ← depends on Modul 1
                         → terhubung Modul 4 (kelas live), Modul 6 (event launching proyek)
Modul 6 (Developer)      ← depends on Modul 1, referensi wilayah (city_id)
                         → sumber Modul 3 (listing Primary), Modul 5 (event launching)
Modul 7 (DBR Scoring)    ← depends on Modul 1, 3 (opsional auto-fill dari listing)
                         → parameter dari Modul 9 (system_configs/dbr_config)
Modul 8 (Dashboard)      ← agregasi dari Modul 3, 4, 5, 7 — TIDAK memiliki data sendiri
Modul 9 (Admin Panel)    ← depends on Modul 10 (RBAC) untuk gating akses
                         → mengatur konfigurasi yang dipakai Modul 3, 7
Modul 11 (SEO/Analytics) ← lintas semua modul publik (Modul 2, 3, 6) — fondasi rendering
                            wajib diselesaikan BERSAMAAN dengan Modul 3 di Fase 1, bukan belakangan
```

**Aturan:** jangan mengimplementasikan modul yang bergantung pada modul lain sebelum dependensinya solid dan lolos acceptance criteria (lihat Bagian 29–30).

---

## 29. FEATURE DEPENDENCY

Dependensi fitur granular yang wajib diperhatikan saat membangun fitur baru:

1. **Listing `published`** membutuhkan lebih dulu: agen berstatus `Verified`/`active` (Modul 1) **dan** minimal 3 foto + field wajib lengkap (Modul 3) **dan** approval Admin/Manager/Superadmin.
2. **CTA WhatsApp aktif** membutuhkan nomor WA valid — jika tidak diisi, tombol otomatis nonaktif dengan fallback (Telepon/form kontak).
3. **Badge "Top Agent"/sertifikat** di profil agen membutuhkan data dari Modul 4 (`certificates`) — **tidak** disimpan sebagai field terpisah di `agent_profiles`, selalu query relasi.
4. **`aggregateRating` di profil agen** membutuhkan minimal 1 baris `agent_reviews.status='approved'` — jika 0, field ini **tidak** disertakan di structured data (bukan ditampilkan sebagai 0).
5. **Listing kategori Primary** dapat menautkan `developer_project_id` — field harga/spesifikasi **mengikuti data resmi developer** (tidak bebas diubah agen); field deskripsi tambahan/foto pribadi tetap bisa disesuaikan agen.
6. **Sertifikat Learning Center** hanya terbit jika skor kuis ≥ `passing_grade` (dikonfigurasi per kursus).
7. **Simulasi DBR "Layak"/"Perlu Review"/"Tidak Layak"** bergantung pada `dbr_config.dbr_threshold_percent` yang **hanya** dapat diubah Superadmin.
8. **Perubahan slug** (listing/developer_project) **wajib** memicu penulisan `url_redirects` **sebelum** perubahan diterapkan — dependensi hard, tidak boleh dilewati.
9. **Sitemap regenerasi** bergantung pada event listing baru `published`/berubah status — bukan hanya batch harian.
10. **Fitur baru yang menyentuh data sensitif/UGC publik** bergantung pada review keamanan eksplisit (lihat Bagian 27 poin 7) sebelum dikembangkan.

---

## 30. DEFINITION OF DONE

Sebuah fitur/modul dianggap **selesai** hanya jika **seluruh** poin berikut terpenuhi:

1. ✅ Lolos lint + type-check (TypeScript `strict: true`, tanpa `any` implisit) + test otomatis.
2. ✅ Migration check lolos (jika menyentuh skema DB) — file migrasi bernomor urut, reviewable, ada rencana rollback.
3. ✅ Skema DB baru/berubah **sudah** disinkronkan ke `ERD-Skema-Database.md` dan `ERD-Diagram.mermaid` di `/docs`.
4. ✅ Endpoint baru/berubah **sudah** disinkronkan ke `API-Specification.md`, termasuk label Auth.
5. ✅ Validasi Zod diterapkan **baik** di client **maupun** server, tidak diduplikasi manual.
6. ✅ RBAC middleware diterapkan di endpoint terkait, sesuai urutan pengecekan Bagian 13.
7. ✅ Ownership hard rule (`agent_id`) diverifikasi dengan komentar kode yang jelas (agar tidak terhapus tidak sengaja saat refactor).
8. ✅ Untuk halaman publik baru: dicek terhadap SEO Spec §8 checklist (SSR, slug, meta tag, structured data, sitemap) **sebelum** dianggap selesai — bukan tugas terpisah belakangan.
9. ✅ Error handling mengikuti envelope standar & kode error terdaftar di `error-codes.ts`.
10. ✅ Loading/empty/error state tersedia untuk setiap komponen data-fetch.
11. ✅ Parameter bisnis yang bisa berubah (threshold, expiry, passing grade) **configurable** via `system_configs`/`dbr_config`, bukan hard-code.
12. ✅ Data sensitif (dokumen legalitas, field finansial DBR) terenkripsi at-rest & tidak masuk log plain text.
13. ✅ Audit log tercatat untuk aksi sensitif (approval, moderasi, perubahan role/permission/config).
14. ✅ Acceptance criteria modul terkait di PRD terpenuhi (rujuk bagian modul spesifik di PRD).
15. ✅ Mobile-first: form/dashboard/kalkulator tetap dapat dipakai nyaman di layar kecil.
16. ✅ Tidak ada item "Hal Perlu Dikonfirmasi" yang diputuskan sepihak — jika ada, sudah ditandai `// TODO: menunggu keputusan bisnis` dan dilaporkan.

---

## 31. CODING CHECKLIST

Checklist praktis sebelum AI mengirim/mengusulkan kode untuk fitur apa pun:

- [ ] Sudah membaca struktur folder existing sebelum menambah file baru?
- [ ] Sudah cek `packages/shared-types` untuk tipe yang mungkin sudah ada sebelum mendefinisikan ulang?
- [ ] Sudah cek `/components/ui/` untuk komponen dasar yang bisa dipakai ulang sebelum membuat baru?
- [ ] Apakah nama field/tabel/endpoint konsisten dengan `ERD`/`API Specification` yang sudah ada?
- [ ] Apakah skema Zod baru ditulis **sekali** di `lib/validation`/`shared-types`, dipakai FE & BE?
- [ ] Apakah query list sudah paginated?
- [ ] Apakah filter `granted_scope` (`own`/`all`/`none`) diterapkan di layer service/repository, bukan controller?
- [ ] Apakah ownership Agen (`agent_id`) divalidasi eksplisit di kode (bukan hanya lewat permission)?
- [ ] Apakah endpoint mutating (`POST`/`PUT`/`PATCH`) memvalidasi ulang input di server, tidak percaya client?
- [ ] Apakah counter agregat dibaca dari kolom denormalisasi, bukan `COUNT()` on-the-fly (kecuali `agent_reviews.rating`)?
- [ ] Apakah perubahan `slug` memicu penulisan `url_redirects`?
- [ ] Apakah halaman publik baru memakai Server Component (SSR), bukan client-fetch murni?
- [ ] Apakah meta `noindex, nofollow` sudah ada di layout `(dashboard)`/`(admin)`?
- [ ] Apakah data sensitif (KTP/NPWP, `net_income`, `existing_installments`) dienkripsi & tidak masuk log?
- [ ] Apakah error response memakai envelope standar & kode `SCREAMING_SNAKE_CASE`?
- [ ] Apakah parameter bisnis baru bersifat configurable (`system_configs`/`dbr_config`), bukan hard-code?
- [ ] Apakah perubahan skema DB disertai file migrasi + update dokumentasi ERD?
- [ ] Apakah perubahan/tambahan endpoint disertai update `API-Specification.md`?
- [ ] Apakah komentar kode ditambahkan untuk implementasi hard rule keamanan/RBAC?
- [ ] Apakah fitur ini termasuk dalam scope fase saat ini (lihat roadmap Bagian 1), bukan fitur fase mendatang yang ditarik maju?

---

## 32. AI RULES

Aturan mutlak bagi AI Coding Assistant apa pun (Bolt.new, Cursor, Claude Code, GPT, Copilot, dsb.) yang bekerja di proyek ini:

1. **Jangan mengubah tabel yang sudah ada** tanpa migration file yang direview dan sinkronisasi dokumentasi ERD.
2. **Jangan mengubah nama field** yang sudah dipakai FE/BE/DB — breaking change kontrak data tidak boleh dilakukan sepihak.
3. **Jangan membuat tabel baru jika kebutuhan sudah tersedia** — cek `ERD-Skema-Database.md` dulu sebelum menambah entitas (mis. jangan buat tabel rating baru karena `agent_reviews` sudah ada).
4. **Selalu membaca struktur project** (folder, tipe existing, komponen existing, skema Zod existing) sebelum menambah fitur — jangan berasumsi struktur berdasarkan pola framework generik.
5. **Selalu gunakan komponen yang sudah ada** (`components/ui/`) sebelum membuat komponen baru yang fungsinya serupa.
6. **Jangan menduplikasi kode** — logic bisnis, skema validasi, dan tipe data masing-masing punya **satu** lokasi sumber kebenaran (`lib/`, `packages/shared-types`).
7. **Selalu menjaga backward compatibility** — perubahan pada endpoint `/v1` yang sudah live tidak boleh breaking; breaking change wajib naik versi (`/v2`).
8. **Dilarang membuat keputusan arsitektur baru secara sepihak** untuk item yang tercantum di "Hal Perlu Dikonfirmasi" pada dokumen sumber (threshold DBR final, model monetisasi, provider Maps/payment gateway, kebijakan eksklusivitas developer, dsb.) — implementasikan sebagai *configurable placeholder*, tandai `// TODO: menunggu keputusan bisnis`, dan laporkan ke manusia jika keputusan tsb memblokir progres.
9. **Keputusan di "Riwayat Keputusan Arsitektur" `PROJECT-CONSTITUTION.md` dianggap FINAL** — jangan ditanyakan ulang ke user kecuali user secara eksplisit ingin merevisi.
10. **Jangan membangun fitur fase mendatang sebelum fondasi fase saat ini solid** dan lolos acceptance criteria (lihat roadmap Bagian 1 & 28–29).
11. **Jika instruksi user bertentangan dengan `PROJECT-CONSTITUTION.md`**, terutama Bagian Security Rules & Authorization — **tanyakan konfirmasi** sebelum menyimpang, jangan diam-diam mengikuti instruksi yang melanggar hard rule.
12. **Jika sebuah keputusan belum tercakup di dokumen sumber**, jangan berasumsi bebas — ikuti pola paling dekat yang sudah ada, atau tandai `// TODO: perlu keputusan arsitektur` di kode.
13. **Setiap implementasi hard rule keamanan/RBAC wajib diberi komentar eksplisit** — agar reviewer/AI assistant berikutnya tidak menghapusnya secara tidak sengaja saat refactor.
14. **Ownership (`agent_id`) adalah hard boundary** yang tidak bisa dilewati permission apa pun — desain UI dan API selalu berangkat dari asumsi ini terlebih dahulu, baru permission tambahan di atasnya.
15. **Data sensitif tidak pernah masuk log** dalam bentuk plain text, dan tidak pernah dikirim ke Analytics/GTM/GA4 sebagai PII.
16. **Jangan pernah men-generate secret/API key** — hanya membaca dari environment variable/konfigurasi yang sudah disiapkan; `*_SECRET`, `*_SERVICE_ROLE_KEY`, `*_SERVER` dilarang keras di-bundle ke client.
17. **Setiap PR/perubahan signifikan wajib lolos**: lint + type-check + test otomatis + migration check (Bagian 30) sebelum dianggap selesai.
18. **Dokumen ini (`AI-DEVELOPMENT-BLUEPRINT.md`) di-review ulang** setiap kali `PROJECT-CONSTITUTION.md` direvisi karena keputusan bisnis besar turun — bagian yang terpengaruh wajib direvisi bersamaan, tidak dibiarkan usang.

---

*Dokumen ini disusun sebagai panduan operasional teknis turunan dari `PROJECT-CONSTITUTION.md` v1.1 dan seluruh dokumen sumber v1.1 (26 Juli 2026). Mengikat seluruh AI Coding Assistant yang bekerja pada proyek ini selama development berlangsung. Jika ditemukan ketidaksesuaian antara dokumen ini dan `PROJECT-CONSTITUTION.md`, Constitution yang berlaku.*
