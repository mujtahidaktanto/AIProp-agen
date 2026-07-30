# DEPENDENCY MANIFEST
## Platform Web Real Estate Agency

---

## 1. Overview

**Tujuan dokumen:** Menjadi katalog resmi seluruh dependency (package) yang **boleh** digunakan dalam pengembangan Platform Web Real Estate Agency. Dokumen ini adalah turunan langsung dari `technology-decisions.md` — setiap package di sini memetakan satu-ke-satu ke keputusan teknologi resmi yang sudah dijelaskan alasannya di dokumen tersebut.

**Fungsi operasional khusus:** Dokumen ini dipakai oleh **Bolt.new** dan AI Coding Assistant lain (Claude, ChatGPT, Cursor) sebagai daftar rujukan package yang sah untuk di-install — mencegah penambahan dependency liar/duplikatif yang membuat bundle besar, sulit dipelihara, atau menyimpang dari `technology-decisions.md` Bagian 6 (Architecture Constraints).

**Prinsip dasar dokumen ini:**
- Satu kapabilitas → satu package resmi (tidak ada dua library dengan fungsi tumpang tindih tanpa Architecture Decision).
- Versi yang dicantumkan adalah **rekomendasi rentang mayor** (bukan pin patch/minor persis), karena versi patch berubah sangat sering — tim tetap wajib mengunci versi presisi di lockfile (`package-lock.json`/`pnpm-lock.yaml`) saat instalasi nyata.
- Dokumen ini **tidak** menghasilkan `package.json` — hanya katalog & aturan, sesuai instruksi pembuatan dokumen.

**Dokumen terkait:** `technology-decisions.md` (alasan pemilihan setiap teknologi), `PROJECT-CONSTITUTION.md` Bagian 4 & 17, `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 2 & 32.

---

## 2. Production Dependencies

| Package Name | Purpose | Module Used | Reason | Category | Required | Version Recommendation |
|---|---|---|---|---|---|---|
| `next` | Framework React full-stack, SSR/SSG/ISR, Route Handlers | Seluruh modul (rendering & BFF) | Keputusan final `technology-decisions.md` 4.1 | Framework | Wajib | ^16 (Active LTS per Juli 2026 — App Router, Turbopack default) |
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
| `@supabase/supabase-js` | Client SDK Supabase (DB/Auth/Storage) | M1 Auth, M2 Profil, M3 Listing, M6 Developer, M7 DBR, M8 Notifikasi, M9 Admin | Keputusan final 4.6–4.10 | Database/Authentication/Storage | Wajib | ^2.x terbaru stabil |
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
| `@sentry/nextjs` | Error tracking & performance monitoring | Seluruh modul (frontend + Route Handlers) | Keputusan final 4.15 | Notification/Monitoring | Wajib | ^9.x terbaru stabil |
| **Maps SDK — belum final (lihat Bagian 9 Open Questions)** | Integrasi Google Maps Platform (Autocomplete/Geocoding/Distance Matrix) | M3 Form lokasi listing, M6 Peta proyek developer | Kandidat: `@vis.gl/react-google-maps` (wrapper resmi Google) **atau** `@react-google-maps/api` (populer, matang secara komunitas) | Maps | Wajib (paket spesifik menunggu keputusan) | — |

---

## 3. Development Dependencies

| Package Name | Purpose | Category | Required | Version Recommendation |
|---|---|---|---|---|
| `vitest` | Unit testing framework | Testing | Wajib | ^2.x terbaru stabil |
| `@vitejs/plugin-react` | Plugin React untuk Vitest (transform JSX/TSX) | Testing | Wajib | Selaras versi `vitest` |
| `@testing-library/react` | Pengujian komponen React berbasis interaksi pengguna | Testing | Wajib | ^16.x (kompatibel React 19) |
| `@testing-library/jest-dom` | Matcher tambahan untuk assertion DOM | Testing | Wajib | ^6.x |
| `@testing-library/user-event` | Simulasi interaksi pengguna realistis (klik, ketik) | Testing | Direkomendasikan | ^14.x |
| `@playwright/test` | E2E testing lintas browser | Testing | Wajib | ^1.5x terbaru stabil |
| `eslint` | Linting kode | Linting | Wajib | ^9.x (flat config) |
| `eslint-config-next` | Preset ESLint resmi Next.js | Linting | Wajib | Selaras versi `next` |
| `@typescript-eslint/parser` / `@typescript-eslint/eslint-plugin` | Linting khusus TypeScript | Linting | Wajib | Selaras versi `eslint` |
| `prettier` | Formatting kode otomatis | Formatting | Wajib | ^3.x |
| `prettier-plugin-tailwindcss` | Auto-sort class Tailwind saat format | Formatting | Direkomendasikan | Versi stabil terbaru, selaras `tailwindcss` v4 |
| `typescript` *(juga dev dependency)* | Type checking (`tsc --noEmit` sebagai CI gate) | Type Checking | Wajib | Sama dengan Bagian 2 |
| `supabase` (CLI) | Migration, seed lokal, generate tipe DB (`supabase gen types typescript`) | Build Tools | Wajib | Versi stabil terbaru |
| `husky` | Git hooks (pre-commit lint/format) | Build Tools | Direkomendasikan | ^9.x |
| `lint-staged` | Menjalankan lint/format hanya pada file staged | Build Tools | Direkomendasikan | ^15.x |
| `@commitlint/cli` / `@commitlint/config-conventional` | Validasi format Conventional Commits | CI/CD | Direkomendasikan | Versi stabil terbaru |
| GitHub Actions (workflow YAML, bukan package npm) | Pipeline CI: lint, type-check, test, migration check | CI/CD | Wajib | — (dikelola sebagai file `.github/workflows/*.yml`, bukan dependency) |

---

## 4. Dependency Rules

1. **Hindari package dengan fungsi yang sama** — setiap kapabilitas (state server, form, chart, date, dsb.) hanya memiliki **satu** package resmi di Bagian 2/3; penambahan package kedua dengan fungsi tumpang tindih wajib melalui Architecture Decision Record (ADR) tertulis.
2. **Prioritaskan library yang sudah dipilih** — sebelum menambah package baru, AI Coding Assistant maupun developer wajib mengecek apakah kapabilitas yang dibutuhkan sudah terpenuhi oleh salah satu package di Bagian 2/3 (mis. jangan tambah `axios` karena `fetch` + TanStack Query sudah cukup).
3. **Jangan menambah dependency tanpa review** — setiap penambahan package baru wajib melalui code review eksplisit yang menyebutkan alasan, kategori, dan dampaknya terhadap bundle size.
4. **Selalu gunakan versi stabil** — tidak menginstal versi `alpha`/`beta`/`canary`/`rc` untuk dependency production, kecuali ada kebutuhan eksplisit yang didokumentasikan (mis. menunggu fitur stabil dirilis) dan disetujui sebagai pengecualian sementara.
5. **Hindari package yang sudah deprecated** — mis. `react-beautiful-dnd` (lihat `technology-decisions.md` 4.25), `moment` (lihat 4.26); jika sebuah package resmi di dokumen ini kelak dinyatakan deprecated oleh maintainer-nya, wajib diajukan sebagai revisi dokumen, bukan diam-diam diganti oleh AI Coding Assistant.
6. **Hindari package dengan komunitas kecil/tidak aktif** — indikator minimal: rilis dalam 12 bulan terakhir, jumlah *open issues* dikelola wajar, dependency count sehat (lihat prinsip Community Support & Long Term Support di `technology-decisions.md` Bagian 2).
7. **Dependency untuk kapabilitas yang statusnya "belum final"** (mis. Maps SDK spesifik di Bagian 2, atau seluruh kandidat di Bagian 8 Future Evaluation) **tidak boleh diinstal sebagai keputusan sepihak** — tandai `// TODO: menunggu keputusan arsitektur` di kode yang membutuhkannya.
8. **Package generate shadcn/ui (`@radix-ui/*` dkk.) hanya ditambahkan sesuai komponen yang benar-benar dipakai** — jangan menjalankan `shadcn add` untuk seluruh katalog komponen "untuk jaga-jaga".

---

## 5. Package Compatibility

| Pasangan Dependency | Keterkaitan |
|---|---|
| `next` ↔ `react` / `react-dom` | Versi Next.js menentukan versi React minimum yang didukung (Next.js 16 → React 19) — selalu upgrade keduanya bersamaan. |
| `next` ↔ `vercel` (platform) | Pasangan native — fitur ISR/Edge Middleware/Image Optimization bekerja penuh tanpa konfigurasi tambahan hanya di Vercel. |
| `react-hook-form` ↔ `zod` (via `@hookform/resolvers`) | Skema Zod yang sama dipakai sebagai `resolver` form (client) sekaligus validasi ulang di server — satu sumber kebenaran. |
| `@tanstack/react-query` ↔ `@supabase/supabase-js` | Fungsi Supabase client dibungkus sebagai *query function*/`mutationFn` TanStack Query — cache & invalidation dikelola Query, bukan manual `useState`. |
| `@supabase/supabase-js` ↔ `@supabase/ssr` | `@supabase/ssr` menyediakan helper client browser vs server yang benar untuk sesi Auth di lingkungan Next.js App Router (Server Component tidak bisa memakai client browser biasa). |
| `resend` ↔ `react-email` | Template email ditulis sebagai komponen React (`react-email`), di-render ke HTML, lalu dikirim lewat `resend`. |
| `@sentry/nextjs` ↔ `next` | SDK ini secara spesifik menginstrumentasi App Router (server component, edge runtime, route handler) — versi SDK perlu disesuaikan dengan versi mayor Next.js yang dipakai. |
| `@playwright/test` ↔ `next` | Playwright dijalankan terhadap `next build && next start` (bukan `next dev`) di CI agar representatif kondisi production. |
| `vitest` ↔ `@testing-library/react` | Vitest sebagai test runner, React Testing Library sebagai utilitas query/render komponen di dalamnya. |
| `tailwindcss` v4 ↔ shadcn/ui (Radix + CVA + clsx + tailwind-merge) | shadcn/ui versi terbaru sudah mendukung penuh Tailwind v4 (`@theme` CSS-first) & React 19 — pastikan diinisialisasi langsung di kombinasi versi terbaru untuk menghindari migrasi v3→v4 di kemudian hari. |
| `@dnd-kit/core` ↔ `@dnd-kit/sortable` | `sortable` adalah preset di atas primitives `core` — keduanya wajib versi yang saling kompatibel (rilis dari monorepo yang sama). |
| `supabase` (CLI) ↔ migration di `/apps/api/migrations` | CLI ini yang menghasilkan & menjalankan file migrasi bernomor urut yang menjadi acuan `ERD-Skema-Database.md`. |
| `eslint` ↔ `eslint-config-next` ↔ `@typescript-eslint/*` | Preset Next.js diperluas dengan aturan TypeScript — versi ketiganya perlu disinkronkan saat upgrade ESLint mayor (mis. migrasi ke flat config). |

---

## 6. Installation Priority

Urutan instalasi yang direkomendasikan saat inisiasi proyek (fondasi terlebih dahulu, baru lapisan di atasnya):

| Fase | Cakupan | Package Representatif |
|---|---|---|
| **Phase 1 — Core Framework** | Inisialisasi proyek Next.js + TypeScript | `next`, `react`, `react-dom`, `typescript` |
| **Phase 2 — Database & Backend** | Koneksi Supabase, migration awal | `@supabase/supabase-js`, `@supabase/ssr`, `supabase` (CLI, dev dependency) |
| **Phase 3 — Authentication** | Setup Supabase Auth + middleware sesi | (memakai `@supabase/supabase-js`/`@supabase/ssr` yang sudah terpasang di Phase 2) |
| **Phase 4 — UI** | Setup Tailwind v4 + shadcn/ui | `tailwindcss`, `@tailwindcss/postcss`, `class-variance-authority`, `clsx`, `tailwind-merge`, `tw-animate-css`, `lucide-react`, komponen `@radix-ui/*` sesuai kebutuhan |
| **Phase 5 — State** | Server state & UI state | `@tanstack/react-query`, `zustand` |
| **Phase 6 — Forms & Validation** | Form + skema validasi | `react-hook-form`, `@hookform/resolvers`, `zod` |
| **Phase 7 — Utilities** | Chart, table, drag-drop, date, PDF, image, maps, email, monitoring | `recharts`, `@tanstack/react-table`, `@dnd-kit/core`, `@dnd-kit/sortable`, `date-fns`, `pdf-lib`, `browser-image-compression`, Maps SDK (menunggu keputusan Bagian 9), `resend`, `react-email`, `@sentry/nextjs` |
| **Phase 8 — Testing & Tooling** | Test & quality gate | `vitest`, `@vitejs/plugin-react`, `@testing-library/react`, `@testing-library/jest-dom`, `@testing-library/user-event`, `@playwright/test`, `eslint`, `eslint-config-next`, `@typescript-eslint/*`, `prettier`, `prettier-plugin-tailwindcss`, `husky`, `lint-staged`, `@commitlint/*` |

---

## 7. AI Development Guidelines

1. **Jangan menginstal package di luar daftar ini** (Bagian 2 & 3) tanpa justifikasi tertulis yang merujuk pada `technology-decisions.md` Bagian 2 (10 prinsip) dan Bagian 6 (Architecture Constraints).
2. **Jika membutuhkan package baru**, berikan analisis singkat: (a) manfaat konkret yang tidak bisa dipenuhi package existing, (b) risiko (ukuran bundle, maintenance, keamanan), (c) alternatif yang sudah tersedia di Bagian 2/3 beserta alasan kenapa dianggap tidak cukup.
3. **Gunakan package resmi proyek secara konsisten** — jangan mencampur dua pendekatan untuk kapabilitas yang sama dalam codebase yang sama (mis. sebagian halaman pakai TanStack Query, sebagian lain pakai `useEffect` + `fetch` manual untuk data server yang setara).
4. **Jangan membuat fungsi yang sudah disediakan library** — mis. jangan menulis ulang date formatting manual jika `date-fns` sudah menyediakan fungsi tersebut, jangan menulis validator manual jika `zod` sudah cukup.
5. **Package berstatus "belum final"** (Maps SDK di Bagian 2, seluruh kandidat Future Evaluation di `technology-decisions.md` Bagian 8) tidak boleh diinstal sebagai keputusan final sepihak — implementasikan sebagai placeholder/interface yang provider-agnostic jika memungkinkan, tandai `// TODO: menunggu keputusan bisnis/arsitektur`.
6. **Setiap penambahan/pergantian package wajib disinkronkan** ke dokumen ini — dependency yang benar-benar dipakai di codebase tapi tidak tercatat di sini dianggap menyimpang dari governance proyek.

---

## 8. Maintenance Plan

| Aktivitas | Strategi |
|---|---|
| **Update dependency (rutin)** | Review bulanan terhadap update minor/patch package Bagian 2 & 3; jalankan lint + type-check + test otomatis + E2E sebelum merge update apa pun (selaras CI gate `PROJECT-CONSTITUTION.md` Bagian 21). |
| **Security patch** | Prioritas tinggi — patch keamanan (terutama pada `next`, `react`, `@supabase/supabase-js`) diterapkan segera setelah rilis resmi tersedia, di luar siklus review bulanan biasa; pantau advisory resmi vendor (mis. Vercel security release notes untuk Next.js/React). |
| **Major version upgrade** | Direncanakan sebagai pekerjaan terpisah (bukan disisipkan di PR fitur) — wajib melalui staging, pengujian regresi penuh (unit + component + E2E), dan review dampak terhadap `technology-decisions.md` (apakah alasan pemilihan masih berlaku). |
| **Deprecation handling** | Jika sebuah package resmi dinyatakan deprecated oleh maintainer-nya (seperti kasus `react-beautiful-dnd` yang sudah dihindari sejak awal), migrasi ke pengganti direncanakan sebagai revisi `technology-decisions.md` & `dependency-manifest.md` secara bersamaan, bukan penggantian diam-diam oleh satu kontributor/AI Coding Assistant. |
| **Vulnerability review** | Audit dependency (`npm audit`/setara) dijalankan sebagai bagian CI, minimal setiap PR yang mengubah lockfile; kerentanan tingkat tinggi/kritis memblokir merge sampai diperbaiki atau di-mitigasi dengan alasan terdokumentasi. |

---

## 9. Open Questions

1. **Package/wrapper spesifik untuk Google Maps Platform di React** belum ditentukan — kandidat: `@vis.gl/react-google-maps` (wrapper resmi Google, lebih baru) vs `@react-google-maps/api` (lebih matang secara komunitas/riwayat penggunaan). Perlu keputusan tim sebelum implementasi Modul 3 (form lokasi listing) dimulai.
2. **Package Search Engine belum ditentukan** karena Search Engine sendiri belum menjadi bagian Official Technology Stack (lihat `technology-decisions.md` Bagian 9 poin 2) — jika keputusan mengarah ke Typesense, package klien terkait (`typesense` Node/JS client) perlu ditambahkan ke dokumen ini melalui revisi.
3. **Package Cache/Rate Limiting belum ditentukan** (lihat `technology-decisions.md` Bagian 9 poin 3) — jika keputusan mengarah ke Redis, package terkait (mis. `ioredis`, atau SDK rate-limiting edge seperti `@upstash/ratelimit` + `@upstash/redis` jika memakai Redis terkelola serverless) perlu ditambahkan melalui revisi.
4. **Package Job Queue belum ditentukan** (lihat `technology-decisions.md` Bagian 9 poin 4) — jika keputusan mengarah ke BullMQ, package `bullmq` + `ioredis` perlu ditambahkan; jika mengarah ke Supabase Edge Functions + cron, tidak ada package npm tambahan yang diperlukan di sisi aplikasi Next.js.
5. **Versi presisi (minor/patch) tiap package** sengaja tidak dipin di dokumen ini — hanya rentang mayor yang dicantumkan agar dokumen tidak cepat usang; versi presisi ditentukan saat instalasi nyata dan dikunci di lockfile proyek.

---

*Dokumen ini adalah katalog dependency resmi turunan dari `technology-decisions.md`. Dipakai sebagai rujukan package yang sah oleh seluruh AI Coding Assistant (termasuk Bolt.new) dan developer manusia. Wajib direvisi bersamaan setiap kali `technology-decisions.md` direvisi, terutama saat Bagian 9 (Open Questions) di kedua dokumen diselesaikan.*
