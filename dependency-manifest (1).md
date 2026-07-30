# DEPENDENCY MANIFEST
## Platform Web Real Estate Agency

---

## 1. Document Information

| Field | Value |
|---|---|
| **Name** | Dependency Manifest — Platform Web Real Estate Agency |
| **Version** | 1.1 |
| **Status** | Draft — menunggu review & pengesahan tim (belum eligible untuk status Baseline di `document-governance-baseline-register.md` selama ADR-005 Search Strategy, ADR-006 Job Queue Strategy, dan ADR-018 Caching Strategy masih **OPEN** — lihat Bagian 9) |
| **Last Updated** | 27 Juli 2026 — direvisi (v1.0 → v1.1) untuk mengintegrasikan resolusi **ADR-001 (Backend Architecture, Approved)** dari `architecture-decision-records.md`, termasuk penambahan **Bolt.new** sebagai toolchain resmi proyek (rekomendasi eksplisit Architecture Review Board, 27 Juli 2026) |
| **Owner** | Principal Software Architect / Technical Lead (nama individu belum ditetapkan — konsisten dengan gap yang sama di `technology-decisions.md` & `architecture-decision-records.md`) |

**Tujuan dokumen:** Menjadi katalog resmi seluruh dependency (package) yang **boleh** digunakan dalam pengembangan Platform Web Real Estate Agency. Dokumen ini adalah turunan langsung dari `technology-decisions.md` — setiap package di sini memetakan satu-ke-satu ke keputusan teknologi resmi yang sudah dijelaskan alasannya di dokumen tersebut, yang pada gilirannya bersumber dari `architecture-decision-records.md`.

**Hierarki rujukan (wajib dibaca berurutan):** `architecture-decision-records.md` (mengapa) → `technology-decisions.md` (katalog & justifikasi ringkas) → **dokumen ini** (package/versi/rationale operasional). Jika ditemukan dependency di sini yang tidak dapat ditelusuri balik ke baris "Official Technology Stack" di `technology-decisions.md`, atau ke ADR berstatus Approved, ini adalah gap governance yang wajib dilaporkan, bukan diasumsikan sah.

**Fungsi operasional khusus:** Dokumen ini dipakai oleh **Bolt.new** dan AI Coding Assistant lain (Claude, ChatGPT, Cursor) sebagai daftar rujukan package yang sah untuk di-install — mencegah penambahan dependency liar/duplikatif yang membuat bundle besar, sulit dipelihara, atau menyimpang dari `technology-decisions.md` Bagian 6 (Architecture Constraints).

**Prinsip dasar dokumen ini:**
- Satu kapabilitas → satu package resmi (tidak ada dua library dengan fungsi tumpang tindih tanpa Architecture Decision).
- Versi yang dicantumkan adalah **rekomendasi rentang mayor** (bukan pin patch/minor persis), karena versi patch berubah sangat sering — tim tetap wajib mengunci versi presisi di lockfile (`package-lock.json`/`pnpm-lock.yaml`) saat instalasi nyata.
- Dokumen ini **tidak** menghasilkan `package.json` — hanya katalog & aturan, sesuai instruksi pembuatan dokumen.

**Dokumen terkait:** `architecture-decision-records.md` (sumber kebenaran keputusan arsitektur), `technology-decisions.md` (alasan pemilihan setiap teknologi), `PROJECT-CONSTITUTION.md` Bagian 4 & 17, `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 2 & 32.

> **Catatan sinkronisasi (ADR-001, Approved 27 Juli 2026):** Backend/API dikunci final sebagai **Next.js Route Handlers + Supabase**, tanpa service Node.js terpisah (NestJS/Express dsb.). Dokumen ini **tidak mencantumkan** dependency backend terpisah (mis. `express`, `@nestjs/core`) secara sengaja — konsisten dengan `technology-decisions.md` Bagian 6 poin 10 (Architecture Constraint). Seluruh kebutuhan backend dipenuhi oleh `next` (Route Handlers) + `@supabase/*` yang sudah tercantum di Bagian 2.

---

## 2. Development Toolchain (Non-Package)

> Bagian baru — mencatat toolchain resmi proyek yang **bukan** dependency npm (tidak masuk `package.json`), namun tetap bagian sah dari alur pengembangan dan wajib didokumentasikan sesuai catatan kondisional ADR-001 (Architecture Review Board, 27 Juli 2026): *"Bolt.new sebagai bagian toolchain resmi proyek belum tercatat di `technology-decisions.md`/`dependency-manifest.md` dan direkomendasikan ditambahkan secara eksplisit, bukan hanya menjadi konteks satu sesi percakapan."*

| Toolchain | Purpose | Category | Reason | Required |
|---|---|---|---|---|
| **Bolt.new** | Lingkungan pengembangan AI-assisted (WebContainer) yang menjalankan aplikasi Next.js/Node secara langsung di browser untuk iterasi cepat | AI Coding Assistant / Dev Environment | Dicatat eksplisit sebagai bagian alur pengembangan proyek per `architecture-decision-records.md` ADR-001 (Context & Notes) — kompatibilitas strukturalnya (satu aplikasi full-stack Node/Next.js dalam satu WebContainer, bukan orkestrasi dua service terpisah) adalah salah satu rationale langsung di balik keputusan **tidak** mengadakan backend service terpisah (lihat ADR-001 Rationale poin 2) | Wajib dicatat (bukan dependency `package.json`, tidak memengaruhi lockfile) |

**Catatan AI Development:** Karena Bolt.new menjalankan seluruh proyek dalam satu WebContainer Node/Next.js, AI Coding Assistant **tidak boleh** mengusulkan penambahan service backend terpisah (`apps/api` Node.js mandiri) sebagai "solusi cepat" untuk kebutuhan apa pun — hal ini akan bertentangan langsung dengan ADR-001 (Approved) dan `technology-decisions.md` Bagian 6 poin 10.

---

## 3. Production Dependencies

| Package Name | Purpose | Module Used | Reason | Category | Required | Version Recommendation |
|---|---|---|---|---|---|---|
| `next` | Framework React full-stack, SSR/SSG/ISR, **Route Handlers sebagai BFF tipis** (backend resmi proyek) | Seluruh modul (rendering & BFF) | Keputusan final `technology-decisions.md` 4.1; lokasi eksekusi backend dikunci final oleh `architecture-decision-records.md` ADR-001 (Approved) — seluruh endpoint `API-Specification-v1.1.md` diimplementasikan di `app/api/v1/**/route.ts`, tanpa `apps/api` terpisah | Framework | Wajib | ^16 (Active LTS per Juli 2026 — App Router, Turbopack default) |
| `react` | UI library inti | Seluruh modul | Dependency inti Next.js | Framework | Wajib | ^19 (selaras Next.js 16) |
| `react-dom` | Rendering DOM React | Seluruh modul | Dependency inti Next.js | Framework | Wajib | ^19 |
| `typescript` | Bahasa & compiler statis | Seluruh modul | Keputusan final 4.2 | Framework/Type Checking | Wajib | ^5.x terbaru stabil |
| `tailwindcss` | Utility-first CSS engine | Seluruh modul UI | Keputusan final 4.4 | UI | Wajib | ^4 (CSS-first config, kompatibel shadcn/ui terbaru) |
| `@tailwindcss/postcss` | Plugin PostCSS untuk Tailwind v4 | Build pipeline styling | Wajib untuk setup Tailwind v4 di Next.js | UI | Wajib | Selaras versi `tailwindcss` |
| `class-variance-authority` | Utilitas varian styling komponen (dipakai shadcn/ui) | `components/ui/*` | Dependency generate shadcn/ui | UI | Wajib | ^0.7 |
| `clsx` | Utilitas gabung className kondisional | `components/ui/*`, seluruh komponen | Dependency generate shadcn/ui | UI | Wajib | ^2 |
| `tailwind-merge` | Resolusi konflik className Tailwind | `components/ui/*` | Dependency generate shadcn/ui | UI | Wajib | ^2 |
| `tw-animate-css` | Utilitas animasi Tailwind v4 (pengganti `tailwindcss-animate`) | `components/ui/*` | Direkomendasikan resmi shadcn/ui untuk Tailwind v4 | UI | Wajib | Versi stabil terbaru |
| `@radix-ui/*` (per komponen, mis. `@radix-ui/react-dialog`) | Primitive headless UI aksesibel | `components/ui/*` | Basis shadcn/ui | UI | Wajib (sesuai komponen dipakai) | Ditambahkan otomatis oleh CLI `shadcn add` per komponen |
| `lucide-react` | Ikon SVG | Seluruh modul UI | Keputusan final 4.5 | Icons | Wajib | ^0.4xx terbaru stabil |
| `@supabase/supabase-js` | Client SDK Supabase (DB/Auth/Storage) — dipanggil langsung dari Route Handlers (`app/api/v1/**`) sesuai ADR-001 | M1 Auth, M2 Profil, M3 Listing, M6 Developer, M7 DBR, M8 Notifikasi, M9 Admin | Keputusan final 4.6–4.10; lokasi pemanggilan server-side dikunci final oleh ADR-001 (Approved) | Database/Authentication/Storage | Wajib | ^2.x terbaru stabil |
| `@supabase/ssr` | Helper session Supabase untuk SSR Next.js (cookie-based) | Middleware auth, Server Component data-fetch | Diperlukan agar sesi Supabase Auth konsisten di Server & Client Component | Authentication | Wajib | ^0.5 terbaru stabil |
| `@tanstack/react-query` | Server state management (fetch/cache/mutation) | M2, M3, M4, M5, M6, M7, M8, M9 (route group `(dashboard)`/`(admin)`) | Keputusan final 4.16 | State | Wajib | ^5.x |
| `zustand` | UI state management | Wizard form multi-step, filter UI, modal global | Keputusan final 4.17 | State | Wajib | ^5.x |
| `react-hook-form` | Manajemen state form | M1 Registrasi, M3 Form Listing, M7 Kalkulator DBR, seluruh form Admin | Keputusan final 4.18 | Forms | Wajib | ^7.x |
| `@hookform/resolvers` | Jembatan React Hook Form ↔ Zod | Seluruh form | Diperlukan untuk resolver Zod di RHF | Forms | Wajib | ^3.x |
| `zod` | Skema validasi & tipe | Seluruh modul (client & server) | Keputusan final 4.19 | Validation | Wajib | ^3.x |
| `recharts` | Visualisasi chart | M8 Dashboard, M9 Admin Laporan | Keputusan final 4.23 | Charts | Wajib | ^2.x terbaru stabil |
| `@tanstack/react-table` | Tabel data headless | M9 Admin Panel (daftar user, listing, laporan) | Keputusan final 4.24 | Table | Wajib | ^8.x |
| `@dnd-kit/core` | Engine drag-and-drop | M3 Reorder foto listing, M4 Reorder lesson/soal | Keputusan final 4.25 | Utilities | Wajib | ^6.x |
| `@dnd-kit/sortable` | Preset sortable list untuk dnd-kit | M3, M4 | Pelengkap `@dnd-kit/core` untuk kasus reorder list | Utilities | Wajib | ^8.x (selaras `@dnd-kit/core`) |
| `date-fns` | Utilitas tanggal | M3 (expiry listing), M5 (jadwal event), M7 (tenor DBR) | Keputusan final 4.26 | Date | Wajib | ^3.x/^4.x terbaru stabil |
| `pdf-lib` | Generate PDF terprogram | M7 Export hasil simulasi DBR | Keputusan final 4.27 | PDF | Wajib | ^1.x terbaru stabil |
| `browser-image-compression` | Kompresi gambar sisi client | M3 Upload foto listing | Keputusan final 4.28 | Image | Wajib | ^2.x terbaru stabil |
| `resend` | SDK pengiriman email transaksional | M1 OTP/verifikasi, M8 Notifikasi email | Keputusan final 4.14 | Email | Wajib | ^4.x terbaru stabil |
| `react-email` / `@react-email/components` | Membuat template email berbasis komponen React untuk Resend | M1, M8 | Rekomendasi resmi ekosistem Resend untuk template email maintainable | Email | Direkomendasikan | Versi stabil terbaru |
| `@sentry/nextjs` | Error tracking & performance monitoring — menginstrumentasi Server Component, Edge runtime, **dan Route Handlers** (lokasi backend final ADR-001) | Seluruh modul (frontend + Route Handlers) | Keputusan final 4.15 | Notification/Monitoring | Wajib | ^9.x terbaru stabil |
| **Maps SDK — belum final (lihat Bagian 10 Open Questions)** | Integrasi Google Maps Platform (Autocomplete/Geocoding/Distance Matrix) | M3 Form lokasi listing, M6 Peta proyek developer | Kandidat: `@vis.gl/react-google-maps` (wrapper resmi Google) **atau** `@react-google-maps/api` (populer, matang secara komunitas) — menunggu ADR-008 (masih **OPEN**) naik status ke Approved | Maps | Wajib (paket spesifik menunggu keputusan) | — |

---

## 4. Development Dependencies

| Package Name | Purpose | Category | Required | Version Recommendation |
|---|---|---|---|---|
| `vitest` | Unit testing framework | Testing | Wajib | ^2.x terbaru stabil |
| `@vitejs/plugin-react` | Plugin React untuk Vitest (transform JSX/TSX) | Testing | Wajib | Selaras versi `vitest` |
| `@testing-library/react` | Pengujian komponen React berbasis interaksi pengguna | Testing | Wajib | ^16.x (kompatibel React 19) |
| `@testing-library/jest-dom` | Matcher tambahan untuk assertion DOM | Testing | Wajib | ^6.x |
| `@testing-library/user-event` | Simulasi interaksi pengguna realistis (klik, ketik) | Testing | Direkomendasikan | ^14.x |
| `@playwright/test` | E2E testing lintas browser — dijalankan terhadap `next build && next start` (bukan `next dev`) | Testing | Wajib | ^1.5x terbaru stabil |
| `eslint` | Linting kode | Linting | Wajib | ^9.x (flat config) |
| `eslint-config-next` | Preset ESLint resmi Next.js | Linting | Wajib | Selaras versi `next` |
| `@typescript-eslint/parser` / `@typescript-eslint/eslint-plugin` | Linting khusus TypeScript | Linting | Wajib | Selaras versi `eslint` |
| `prettier` | Formatting kode otomatis | Formatting | Wajib | ^3.x |
| `prettier-plugin-tailwindcss` | Auto-sort class Tailwind saat format | Formatting | Direkomendasikan | Versi stabil terbaru, selaras `tailwindcss` v4 |
| `typescript` *(juga dev dependency)* | Type checking (`tsc --noEmit` sebagai CI gate) | Type Checking | Wajib | Sama dengan Bagian 3 |
| `supabase` (CLI) | Migration, seed lokal, generate tipe DB (`supabase gen types typescript`) | Build Tools | Wajib | Versi stabil terbaru |
| `husky` | Git hooks (pre-commit lint/format) | Build Tools | Direkomendasikan | ^9.x |
| `lint-staged` | Menjalankan lint/format hanya pada file staged | Build Tools | Direkomendasikan | ^15.x |
| `@commitlint/cli` / `@commitlint/config-conventional` | Validasi format Conventional Commits | CI/CD | Direkomendasikan | Versi stabil terbaru |
| GitHub Actions (workflow YAML, bukan package npm) | Pipeline CI: lint, type-check, test, migration check | CI/CD | Wajib | — (dikelola sebagai file `.github/workflows/*.yml`, bukan dependency) |

---

## 5. Dependency Rules

1. **Hindari package dengan fungsi yang sama** — setiap kapabilitas (state server, form, chart, date, dsb.) hanya memiliki **satu** package resmi di Bagian 3/4; penambahan package kedua dengan fungsi tumpang tindih wajib melalui Architecture Decision Record (ADR) tertulis.
2. **Prioritaskan library yang sudah dipilih** — sebelum menambah package baru, AI Coding Assistant maupun developer wajib mengecek apakah kapabilitas yang dibutuhkan sudah terpenuhi oleh salah satu package di Bagian 3/4 (mis. jangan tambah `axios` karena `fetch` + TanStack Query sudah cukup).
3. **Jangan menambah dependency tanpa review** — setiap penambahan package baru wajib melalui code review eksplisit yang menyebutkan alasan, kategori, dan dampaknya terhadap bundle size.
4. **Selalu gunakan versi stabil** — tidak menginstal versi `alpha`/`beta`/`canary`/`rc` untuk dependency production, kecuali ada kebutuhan eksplisit yang didokumentasikan (mis. menunggu fitur stabil dirilis) dan disetujui sebagai pengecualian sementara.
5. **Hindari package yang sudah deprecated** — mis. `react-beautiful-dnd` (lihat `technology-decisions.md` 4.25), `moment` (lihat 4.26); jika sebuah package resmi di dokumen ini kelak dinyatakan deprecated oleh maintainer-nya, wajib diajukan sebagai revisi dokumen, bukan diam-diam diganti oleh AI Coding Assistant.
6. **Hindari package dengan komunitas kecil/tidak aktif** — indikator minimal: rilis dalam 12 bulan terakhir, jumlah *open issues* dikelola wajar, dependency count sehat (lihat prinsip Community Support & Long Term Support di `technology-decisions.md` Bagian 2).
7. **Dependency untuk kapabilitas yang statusnya "belum final"** (mis. Maps SDK spesifik di Bagian 3, atau seluruh kandidat di Bagian 8 `technology-decisions.md` Future Evaluation) **tidak boleh diinstal sebagai keputusan sepihak** — tandai `// TODO: menunggu resolusi ADR-XXX` di kode yang membutuhkannya, mengikuti pola AI Usage Rules `architecture-decision-records.md` Bagian 10.
8. **Package generate shadcn/ui (`@radix-ui/*` dkk.) hanya ditambahkan sesuai komponen yang benar-benar dipakai** — jangan menjalankan `shadcn add` untuk seluruh katalog komponen "untuk jaga-jaga".
9. **Jangan menambahkan dependency backend service Node.js terpisah** (`express`, `@nestjs/core`, dsb.) — dilarang eksplisit oleh `architecture-decision-records.md` ADR-001 (Approved) & `technology-decisions.md` Bagian 6 poin 10. Perubahan atas larangan ini hanya sah melalui ADR baru yang secara eksplisit men-supersede ADR-001.

---

## 6. Package Compatibility

| Pasangan Dependency | Keterkaitan |
|---|---|
| `next` ↔ `react` / `react-dom` | Versi Next.js menentukan versi React minimum yang didukung (Next.js 16 → React 19) — selalu upgrade keduanya bersamaan. |
| `next` ↔ `vercel` (platform) | Pasangan native — fitur ISR/Edge Middleware/Image Optimization bekerja penuh tanpa konfigurasi tambahan hanya di Vercel. |
| `react-hook-form` ↔ `zod` (via `@hookform/resolvers`) | Skema Zod yang sama dipakai sebagai `resolver` form (client) sekaligus validasi ulang di server — satu sumber kebenaran. |
| `@tanstack/react-query` ↔ `@supabase/supabase-js` | Fungsi Supabase client dibungkus sebagai *query function*/`mutationFn` TanStack Query — cache & invalidation dikelola Query, bukan manual `useState`. |
| `@supabase/supabase-js` ↔ `@supabase/ssr` | `@supabase/ssr` menyediakan helper client browser vs server yang benar untuk sesi Auth di lingkungan Next.js App Router (Server Component tidak bisa memakai client browser biasa). |
| `resend` ↔ `react-email` | Template email ditulis sebagai komponen React (`react-email`), di-render ke HTML, lalu dikirim lewat `resend`. |
| `@sentry/nextjs` ↔ `next` | SDK ini secara spesifik menginstrumentasi App Router (server component, edge runtime, **route handler** — lokasi backend final ADR-001) — versi SDK perlu disesuaikan dengan versi mayor Next.js yang dipakai. |
| `@playwright/test` ↔ `next` | Playwright dijalankan terhadap `next build && next start` (bukan `next dev`) di CI agar representatif kondisi production. |
| `vitest` ↔ `@testing-library/react` | Vitest sebagai test runner, React Testing Library sebagai utilitas query/render komponen di dalamnya. |
| `tailwindcss` v4 ↔ shadcn/ui (Radix + CVA + clsx + tailwind-merge) | shadcn/ui versi terbaru sudah mendukung penuh Tailwind v4 (`@theme` CSS-first) & React 19 — pastikan diinisialisasi langsung di kombinasi versi terbaru untuk menghindari migrasi v3→v4 di kemudian hari. |
| `@dnd-kit/core` ↔ `@dnd-kit/sortable` | `sortable` adalah preset di atas primitives `core` — keduanya wajib versi yang saling kompatibel (rilis dari monorepo yang sama). |
| `supabase` (CLI) ↔ migration di `app/api` migrations directory | CLI ini yang menghasilkan & menjalankan file migrasi bernomor urut yang menjadi acuan `ERD-Skema-Database.md`. |
| `eslint` ↔ `eslint-config-next` ↔ `@typescript-eslint/*` | Preset Next.js diperluas dengan aturan TypeScript — versi ketiganya perlu disinkronkan saat upgrade ESLint mayor (mis. migrasi ke flat config). |
| **Bolt.new (toolchain)** ↔ `next` (satu aplikasi `apps/web`) | Bolt.new mengasumsikan satu aplikasi full-stack Node/Next.js dalam satu WebContainer — kompatibel penuh dengan struktur `apps/web` tunggal (ADR-001, Approved), **tidak kompatibel** dengan skenario dua service terpisah (backend Node.js mandiri + frontend). |

---

## 7. Installation Priority

Urutan instalasi yang direkomendasikan saat inisiasi proyek (fondasi terlebih dahulu, baru lapisan di atasnya):

| Fase | Cakupan | Package/Toolchain Representatif |
|---|---|---|
| **Phase 0 — Dev Environment** | Inisialisasi lingkungan pengembangan AI-assisted | **Bolt.new** (lihat Bagian 2) — satu aplikasi `apps/web`, tanpa service backend terpisah |
| **Phase 1 — Core Framework** | Inisialisasi proyek Next.js + TypeScript | `next`, `react`, `react-dom`, `typescript` |
| **Phase 2 — Database & Backend** | Koneksi Supabase, migration awal, Route Handlers sebagai BFF (ADR-001) | `@supabase/supabase-js`, `@supabase/ssr`, `supabase` (CLI, dev dependency) |
| **Phase 3 — Authentication** | Setup Supabase Auth + middleware sesi | (memakai `@supabase/supabase-js`/`@supabase/ssr` yang sudah terpasang di Phase 2) |
| **Phase 4 — UI** | Setup Tailwind v4 + shadcn/ui | `tailwindcss`, `@tailwindcss/postcss`, `class-variance-authority`, `clsx`, `tailwind-merge`, `tw-animate-css`, `lucide-react`, komponen `@radix-ui/*` sesuai kebutuhan |
| **Phase 5 — State** | Server state & UI state | `@tanstack/react-query`, `zustand` |
| **Phase 6 — Forms & Validation** | Form + skema validasi | `react-hook-form`, `@hookform/resolvers`, `zod` |
| **Phase 7 — Utilities** | Chart, table, drag-drop, date, PDF, image, maps, email, monitoring | `recharts`, `@tanstack/react-table`, `@dnd-kit/core`, `@dnd-kit/sortable`, `date-fns`, `pdf-lib`, `browser-image-compression`, Maps SDK (menunggu resolusi ADR-008), `resend`, `react-email`, `@sentry/nextjs` |
| **Phase 8 — Testing & Tooling** | Test & quality gate | `vitest`, `@vitejs/plugin-react`, `@testing-library/react`, `@testing-library/jest-dom`, `@testing-library/user-event`, `@playwright/test`, `eslint`, `eslint-config-next`, `@typescript-eslint/*`, `prettier`, `prettier-plugin-tailwindcss`, `husky`, `lint-staged`, `@commitlint/*` |

---

## 8. AI Development Guidelines

1. **Jangan menginstal package di luar daftar ini** (Bagian 3 & 4) tanpa justifikasi tertulis yang merujuk pada `technology-decisions.md` Bagian 2 (10 prinsip) dan Bagian 6 (Architecture Constraints).
2. **Jika membutuhkan package baru**, berikan analisis singkat: (a) manfaat konkret yang tidak bisa dipenuhi package existing, (b) risiko (ukuran bundle, maintenance, keamanan), (c) alternatif yang sudah tersedia di Bagian 3/4 beserta alasan kenapa dianggap tidak cukup.
3. **Gunakan package resmi proyek secara konsisten** — jangan mencampur dua pendekatan untuk kapabilitas yang sama dalam codebase yang sama (mis. sebagian halaman pakai TanStack Query, sebagian lain pakai `useEffect` + `fetch` manual untuk data server yang setara).
4. **Jangan membuat fungsi yang sudah disediakan library** — mis. jangan menulis ulang date formatting manual jika `date-fns` sudah menyediakan fungsi tersebut, jangan menulis validator manual jika `zod` sudah cukup.
5. **Package berstatus "belum final"** (Maps SDK di Bagian 3, seluruh kandidat Future Evaluation di `technology-decisions.md` Bagian 8) tidak boleh diinstal sebagai keputusan final sepihak — implementasikan sebagai placeholder/interface yang provider-agnostic jika memungkinkan, tandai `// TODO: menunggu resolusi ADR-XXX` (ADR-005/006/008/018 — lihat Bagian 10).
6. **Setiap penambahan/pergantian package wajib disinkronkan** ke dokumen ini — dependency yang benar-benar dipakai di codebase tapi tidak tercatat di sini dianggap menyimpang dari governance proyek.
7. **Jangan mengusulkan/menginisialisasi backend service Node.js terpisah** (`apps/api` mandiri) — bertentangan dengan ADR-001 (Approved) dan struktur toolchain Bolt.new (Bagian 2) yang mengasumsikan satu aplikasi `apps/web`.

---

## 9. Maintenance Plan

| Aktivitas | Strategi |
|---|---|
| **Update dependency (rutin)** | Review bulanan terhadap update minor/patch package Bagian 3 & 4; jalankan lint + type-check + test otomatis + E2E sebelum merge update apa pun (selaras CI gate `PROJECT-CONSTITUTION.md` Bagian 21). |
| **Security patch** | Prioritas tinggi — patch keamanan (terutama pada `next`, `react`, `@supabase/supabase-js`) diterapkan segera setelah rilis resmi tersedia, di luar siklus review bulanan biasa; pantau advisory resmi vendor (mis. Vercel security release notes untuk Next.js/React). |
| **Major version upgrade** | Direncanakan sebagai pekerjaan terpisah (bukan disisipkan di PR fitur) — wajib melalui staging, pengujian regresi penuh (unit + component + E2E), dan review dampak terhadap `technology-decisions.md` (apakah alasan pemilihan masih berlaku). |
| **Deprecation handling** | Jika sebuah package resmi dinyatakan deprecated oleh maintainer-nya (seperti kasus `react-beautiful-dnd` yang sudah dihindari sejak awal), migrasi ke pengganti direncanakan sebagai revisi `technology-decisions.md` & `dependency-manifest.md` secara bersamaan, bukan penggantian diam-diam oleh satu kontributor/AI Coding Assistant. |
| **Vulnerability review** | Audit dependency (`npm audit`/setara) dijalankan sebagai bagian CI, minimal setiap PR yang mengubah lockfile; kerentanan tingkat tinggi/kritis memblokir merge sampai diperbaiki atau di-mitigasi dengan alasan terdokumentasi. |
| **ADR status watch** | Setiap kali status ADR di `architecture-decision-records.md` berubah (terutama ADR-005/006/008/018, saat ini OPEN), dokumen ini wajib direvisi bersamaan — package baru (mis. `typesense`, `bullmq`/`ioredis`, Maps SDK final) hanya ditambahkan setelah ADR terkait naik status ke **Approved**. |

---

## 10. Open Questions

> **Item yang telah diselesaikan dan dihapus dari daftar ini:** *Backend/API service terpisah* — **RESOLVED** via `architecture-decision-records.md` ADR-001 (Approved, 27 Juli 2026): Next.js Route Handlers + Supabase, tanpa dependency backend Node.js terpisah. **Bolt.new** juga telah ditambahkan secara eksplisit sebagai toolchain resmi (lihat Bagian 2), menyelesaikan catatan kondisional Architecture Review Board terkait dokumen ini.

1. **Package/wrapper spesifik untuk Google Maps Platform di React** belum ditentukan — kandidat: `@vis.gl/react-google-maps` (wrapper resmi Google, lebih baru) vs `@react-google-maps/api` (lebih matang secara komunitas/riwayat penggunaan). Bergantung pada resolusi **ADR-008 (Maps Provider, OPEN — target sebelum Sprint S4/S9)**; perlu keputusan tim sebelum implementasi Modul 3 (form lokasi listing) dimulai.
2. **Package Search Engine belum ditentukan** karena Search Engine sendiri belum menjadi bagian Official Technology Stack (lihat **ADR-005, OPEN — target sebelum Sprint S5**, `technology-decisions.md` Bagian 9 poin 1) — jika keputusan mengarah ke Typesense, package klien terkait (`typesense` Node/JS client) perlu ditambahkan ke dokumen ini melalui revisi. Jika mengarah ke Postgres full-text/trigram (opsi MVP direkomendasikan `foundation-validation-report.md`), tidak ada package npm tambahan yang diperlukan.
3. **Package Cache/Rate Limiting belum ditentukan** (lihat **ADR-018, OPEN — prioritas terendah, dapat ditunda melewati MVP**, `technology-decisions.md` Bagian 9 poin 2) — jika keputusan mengarah ke Redis, package terkait (mis. `ioredis`, atau SDK rate-limiting edge seperti `@upstash/ratelimit` + `@upstash/redis` jika memakai Redis terkelola serverless) perlu ditambahkan melalui revisi.
4. **Package Job Queue belum ditentukan** (lihat **ADR-006, OPEN — target sebelum Sprint S6/S13**, `technology-decisions.md` Bagian 9 poin 3) — dengan ADR-001 kini Approved ke arah Route Handlers + Supabase, opsi **Supabase Edge Functions + cron** menguat secara arsitektural dan **tidak memerlukan package npm tambahan** di sisi aplikasi Next.js. Jika keputusan akhir tetap mengarah ke BullMQ, package `bullmq` + `ioredis` perlu ditambahkan (sekaligus menyelesaikan poin 3 di atas jika Redis dipakai bersama).
5. **Versi presisi (minor/patch) tiap package** sengaja tidak dipin di dokumen ini — hanya rentang mayor yang dicantumkan agar dokumen tidak cepat usang; versi presisi ditentukan saat instalasi nyata dan dikunci di lockfile proyek.

---

*Dokumen ini adalah katalog dependency resmi turunan dari `architecture-decision-records.md` dan `technology-decisions.md`. Dipakai sebagai rujukan package yang sah oleh seluruh AI Coding Assistant (termasuk Bolt.new) dan developer manusia. Versi 1.1 (27 Juli 2026) mengintegrasikan resolusi ADR-001 (Backend Architecture, Approved) dan menambahkan Bolt.new sebagai toolchain resmi. Wajib direvisi bersamaan setiap kali `architecture-decision-records.md` atau `technology-decisions.md` direvisi, terutama saat ADR-005/006/008/018 (saat ini OPEN) diselesaikan.*
