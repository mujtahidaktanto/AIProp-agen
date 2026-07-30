# TECHNOLOGY DECISIONS
## Platform Web Real Estate Agency

---

## 1. Document Information

| Field | Value |
|---|---|
| **Name** | Technology Decisions — Platform Web Real Estate Agency |
| **Version** | 1.1 |
| **Status** | Draft — menunggu review & pengesahan tim (belum berstatus "BERLAKU" seperti `PROJECT-CONSTITUTION.md`; **belum eligible untuk status Baseline** di `document-governance-baseline-register.md` karena masih ada Open Decision yang secara langsung memengaruhi isi dokumen ini — ADR-005 Search Strategy, ADR-006 Job Queue Strategy, ADR-018 Caching Strategy, seluruhnya masih **OPEN**. Satu Open Decision yang sebelumnya memengaruhi dokumen ini — ADR-001 Backend Architecture — telah **Approved** dan diintegrasikan penuh ke revisi ini.) |
| **Last Updated** | 27 Juli 2026 — direvisi (v1.0 → v1.1) untuk mengintegrasikan resolusi ADR-001 (Backend Architecture, Approved) dari `architecture-decision-records.md` |
| **Owner** | Principal Software Architect / Technical Lead (nama individu penanggung jawab belum ditetapkan — lihat Bagian 9, Open Questions) |

**Kedudukan dokumen dalam hierarki governance proyek:** Dokumen ini adalah turunan operasional dari `PROJECT-CONSTITUTION.md` Bagian 4 (Tech Stack & Framework) dan `SYSTEM-ARCHITECTURE.md` Bagian 4 (Technology Stack). Jika terjadi ketidaksesuaian pada level katalog/deskripsi, **`PROJECT-CONSTITUTION.md` yang menang**. Namun untuk **keputusan arsitektur/teknis yang sudah dicatat sebagai ADR berstatus Approved** di `architecture-decision-records.md`, ADR tersebut menjadi **referensi utama** sampai `PROJECT-CONSTITUTION.md`/`SYSTEM-ARCHITECTURE.md` diperbarui secara resmi untuk mencerminkannya (`architecture-decision-records.md` Bagian 10, AI Usage Rules poin 3) — dokumen ini **wajib dibaca setelah** ADR terkait, bukan sebagai sumber keputusan independen. Dokumen ini **tidak menggantikan** Constitution atau ADR — fungsinya adalah memperdalam *alasan* di balik setiap pilihan teknologi resmi agar konsisten dipahami oleh seluruh AI Coding Assistant (Claude, Bolt.new, ChatGPT, Cursor) dan developer manusia.

**Dokumen sumber yang menjadi rujukan:** `architecture-decision-records.md` (sumber kebenaran keputusan arsitektur — dibaca **sebelum** dokumen ini), `PROJECT-CONSTITUTION.md`, `SYSTEM-ARCHITECTURE.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, `AI-CONTEXT-PACK.md`, `PRD-Real-Estate-Agency-Platform-v1.1.md`, `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`, `API-Specification-Real-Estate-Agency-Platform-v1.1.md`, `SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md`.

---

## 2. Technology Decision Principles

Setiap teknologi pada Official Technology Stack (Bagian 3) dipilih berdasarkan sepuluh prinsip berikut secara konsisten — bukan preferensi individu, melainkan kriteria yang dapat diaudit ulang jika suatu saat ada usulan perubahan:

| Prinsip | Penerapan |
|---|---|
| **Simplicity** | Diutamakan solusi dengan permukaan API sekecil mungkin dan konvensi yang jelas, agar tim kecil dan AI Coding Assistant dapat produktif tanpa banyak *boilerplate* atau konfigurasi tersembunyi. |
| **Maintainability** | Teknologi dengan pola idiomatis yang stabil dari waktu ke waktu, dokumentasi tipe (TypeScript) end-to-end, dan minim "sihir" implisit yang menyulitkan debugging jangka panjang. |
| **Scalability** | Mampu bertumbuh dari MVP satu agensi ke volume listing/agen yang jauh lebih besar tanpa perombakan arsitektur (lihat `SYSTEM-ARCHITECTURE.md` Bagian 16 & 20). |
| **AI Friendly** | Teknologi arus utama dengan pola kode yang banyak terwakili di data pelatihan model AI (Next.js, TypeScript, Zod, dsb.) sehingga AI Coding Assistant menghasilkan kode yang akurat dan idiomatis, mengurangi halusinasi API. |
| **Community Support** | Ekosistem aktif, rilis rutin, dan basis pengguna besar — mengurangi risiko proyek open-source terbengkalai. |
| **Documentation Quality** | Dokumentasi resmi lengkap dan terpelihara, penting karena dokumentasi adalah *ground truth* yang dipakai AI Coding Assistant maupun developer baru. |
| **Performance** | Selaras target Core Web Vitals wajib di `SEO-Analytics-Specification` Bagian 5 (LCP < 2.5s, CLS < 0.1, INP < 200ms, TTFB < 600ms). |
| **Security** | Dukungan bawaan untuk praktik keamanan wajib di `PROJECT-CONSTITUTION.md` Bagian 20 (enkripsi at-rest, RLS, signed URL, rate limiting). |
| **Cost Efficiency** | Model harga dapat diprediksi pada tahap awal (banyak *free tier* yang cukup untuk MVP), tanpa mengorbankan jalur upgrade saat traffic bertumbuh. |
| **Long Term Support** | Diprioritaskan vendor/proyek dengan rekam jejak rilis jangka panjang dan kebijakan dukungan versi yang jelas (LTS), bukan proyek eksperimental. |

> Prinsip ini juga menjadi kriteria wajib bagi AI Coding Assistant maupun developer manusia ketika mengajukan penambahan teknologi baru di luar daftar resmi (lihat Bagian 6 & 7).

---

## 3. Official Technology Stack

Tabel berikut adalah **keputusan resmi dan final** proyek — identik dengan `PROJECT-CONSTITUTION.md` Bagian 4, tidak boleh disimpangi tanpa keputusan arsitektur eksplisit yang disetujui. **Satu pengecualian ditandai `*`**: baris tersebut mencerminkan arah yang condong dipilih namun ADR yang menaunginya masih berstatus **OPEN** (belum Approved) — lihat catatan di bawah tabel.

| Layer | Teknologi Resmi |
|---|---|
| Frontend | Next.js (App Router) |
| Language | TypeScript |
| UI | shadcn/ui |
| CSS | Tailwind CSS |
| Icons | Lucide React |
| Backend/API | Next.js Route Handlers (BFF tipis) + Supabase (BaaS) — **final, ADR-001 Approved** |
| Database | PostgreSQL (Supabase) |
| Authentication | Supabase Auth |
| Authorization | Supabase RLS + RBAC (kustom aplikasi) |
| Storage | Supabase Storage |
| Hosting | Vercel — **final, ADR-010 Approved** |
| Repository | GitHub |
| Deployment | GitHub → Vercel |
| Transactional Email | Resend |
| Monitoring | Sentry |
| Server State | TanStack Query |
| UI State | Zustand |
| Forms | React Hook Form |
| Validation | Zod |
| Unit Testing | Vitest |
| Component Testing | React Testing Library |
| E2E Testing | Playwright |
| Charts | Recharts |
| Table | TanStack Table |
| Drag & Drop | dnd-kit |
| Date Library | date-fns |
| PDF | pdf-lib |
| Image Compression | browser-image-compression |
| Maps | Google Maps Platform * — **condong dipilih, ADR-008 masih OPEN** |

> **Keputusan resmi (ADR-001, Approved 27 Juli 2026):** Backend/API dikunci final sebagai **Next.js Route Handlers (BFF tipis), terintegrasi langsung dengan Supabase** — seluruh endpoint `API-Specification-v1.1.md` diimplementasikan di `app/api/v1/**/route.ts` dalam satu aplikasi (`apps/web`), tanpa service backend Node.js terpisah (NestJS/Express). Keputusan ini tercatat lengkap (Context/Rationale/Alternatives/Consequences) di `architecture-decision-records.md` ADR-001 dan `decision-log.md` ADR-038, serta telah disinkronkan ke `PROJECT-CONSTITUTION.md` Bagian 4 & `SYSTEM-ARCHITECTURE.md` Bagian 4/9/11/23. Tidak ada lagi status "opsi terbuka" untuk keputusan ini — lihat Bagian 6 (Architecture Constraints poin 10) untuk larangan menambahkan service backend terpisah tanpa ADR baru yang men-supersede ADR-001.
>
> **`*` Pengecualian (ADR-008, masih OPEN):** Baris **Maps** berbeda dari seluruh baris lain di tabel ini — Google Maps Platform **belum** berstatus Approved secara arsitektural. `architecture-decision-records.md` ADR-008 (Maps Provider) mencatat provider ini baru "condong dipilih", menunggu konfirmasi biaya per-request dari tim bisnis sebelum dapat dikunci final (target: sebelum Sprint S4/S9). Implementasi wajib tetap *provider-agnostic* sampai ADR-008 Approved — lihat Bagian 4.29 dan Bagian 9 (Open Questions poin 4).

---

## 4. Decision Detail

### 4.1 Next.js (App Router) — Frontend Framework
- **Purpose:** Framework React full-stack untuk seluruh halaman publik (SSR/SSG/ISR) dan privat (CSR), serta BFF lewat Route Handlers.
- **Why Selected:** Satu-satunya pilihan yang memenuhi syarat SSR/SSG/ISR wajib di `SEO-Analytics-Specification` Bagian 1.1 untuk Homepage, Search, Detail Listing, Profil Agen, dan Detail Proyek Developer — ditetapkan final di `PROJECT-CONSTITUTION.md` Riwayat Keputusan Arsitektur #6.
- **Advantages:** App Router Server Components mengurangi JS yang dikirim ke client (bagus untuk Core Web Vitals); ISR memungkinkan halaman listing tetap cepat tanpa render ulang penuh; ekosistem Vercel terintegrasi native; dukungan TypeScript kelas satu; Route Handlers menghilangkan kebutuhan server Node.js terpisah untuk BFF ringan — dikunci final sebagai keputusan arsitektur backend proyek (`architecture-decision-records.md` ADR-001, Approved).
- **Limitations:** App Router (Server Components, caching model) memiliki kurva belajar tersendiri dibanding Pages Router lama; strategi caching butuh pemahaman eksplisit (`fetch` cache directives) agar tidak salah men-cache data privat.
- **Alternative Considered:** Remix, Astro, SPA React murni (Vite) + backend terpisah.
- **Why Alternative Was Rejected:** Remix/Astro memiliki ekosistem lebih kecil dan dukungan Vercel-native lebih lemah; SPA murni gagal memenuhi syarat SSR wajib untuk SEO tanpa menambah kompleksitas prerendering terpisah.
- **Integration Notes:** Route group `(public)` wajib Server Component untuk data-fetching utama (lihat `PROJECT-CONSTITUTION.md` Bagian 5); `(dashboard)`/`(admin)` CSR dengan `noindex, nofollow`.
- **AI Development Notes:** AI Coding Assistant wajib menempatkan halaman sesuai pola rendering per Bagian 2 `AI-DEVELOPMENT-BLUEPRINT.md` — jangan membuat halaman publik baru sebagai client component murni dengan `useEffect` + `fetch` sebagai sumber data utama.

### 4.2 TypeScript — Language
- **Purpose:** Bahasa utama seluruh codebase (frontend, Route Handlers, skema validasi, shared types).
- **Why Selected:** Satu sumber kebenaran tipe data (`packages/shared-types`) yang disyaratkan `PROJECT-CONSTITUTION.md` Bagian 2 (Single Source of Truth) hanya mungkin dijaga konsisten dengan bahasa bertipe statis di FE & BE sekaligus.
- **Advantages:** Deteksi error saat compile-time, IDE/AI autocomplete jauh lebih akurat, refactor besar lebih aman.
- **Limitations:** Build time lebih lambat dibanding JavaScript murni; butuh disiplin tim agar tidak memakai `any` implisit.
- **Alternative Considered:** JavaScript murni (tanpa tipe statis).
- **Why Alternative Was Rejected:** Tidak mendukung "Single Source of Truth" tipe data lintas layer yang menjadi prinsip arsitektur eksplisit proyek; risiko bug runtime jauh lebih tinggi pada skema RBAC/ownership yang kompleks.
- **Integration Notes:** `strict: true` wajib, tanpa `any` implisit (`PROJECT-CONSTITUTION.md`/`AI-CONTEXT-PACK.md` Bagian 11 poin 7).
- **AI Development Notes:** AI Coding Assistant dilarang menonaktifkan strict mode atau membubuhkan `// @ts-ignore` untuk "membuat kode jalan" tanpa menyelesaikan akar masalah tipe.

### 4.3 shadcn/ui — UI Component Library
- **Purpose:** Kumpulan komponen UI headless (berbasis Radix UI primitives) yang di-generate langsung ke dalam repo, bukan dependency npm biasa.
- **Why Selected:** Konsisten dengan kebutuhan desain sistem yang dapat diaudit dan performa (tanpa CSS-in-JS runtime) sesuai `PROJECT-CONSTITUTION.md` Bagian 4.
- **Advantages:** Kode komponen sepenuhnya berada di repo (`components/ui/`) sehingga dapat dikustomisasi bebas tanpa "fighting the library"; aksesibilitas bawaan dari Radix UI; kompatibel penuh dengan Tailwind v4 & React 19 (App Router Server/Client Components).
- **Limitations:** Bukan package versi tunggal yang di-`npm update`; update komponen upstream harus ditarik manual via CLI (`npx shadcn add`), sehingga tim wajib disiplin tidak memodifikasi struktur dasar komponen secara sembarangan agar tetap bisa disinkronkan.
- **Alternative Considered:** Material UI (MUI), Ant Design, Chakra UI.
- **Why Alternative Was Rejected:** MUI/Ant Design membawa opini desain visual yang kuat (sulit dikustomisasi total) dan bundel lebih berat; keduanya eksplisit **dilarang** di Bagian 6 (Architecture Constraints).
- **Integration Notes:** Bergantung pada `@radix-ui/*` (per komponen), `class-variance-authority`, `clsx`, `tailwind-merge` — lihat `dependency-manifest.md` Bagian 2.
- **AI Development Notes:** Selalu cek `components/ui/` sebelum membuat komponen custom baru yang fungsinya serupa (`AI-DEVELOPMENT-BLUEPRINT.md` Bagian 32 poin 5).

### 4.4 Tailwind CSS — CSS Framework
- **Purpose:** Utility-first styling untuk seluruh UI, termasuk basis styling shadcn/ui.
- **Why Selected:** Cocok dengan kebutuhan Core Web Vitals rendah-JS (tidak ada CSS-in-JS runtime) dan desain sistem yang konsisten & dapat diaudit (`PROJECT-CONSTITUTION.md` Bagian 4).
- **Advantages:** CSS-first configuration (v4) menghilangkan `tailwind.config.js` untuk kasus umum; build sangat cepat (mesin Oxide); ukuran CSS akhir kecil karena purging otomatis.
- **Limitations:** Markup dapat terlihat padat kelas utilitas; migrasi dari v3 ke v4 memerlukan penyesuaian variabel warna (HSL → OKLCH) bila di kemudian hari proyek meng-upgrade dari basis awal.
- **Alternative Considered:** CSS Modules murni, styled-components/Emotion (CSS-in-JS).
- **Why Alternative Was Rejected:** CSS-in-JS runtime menambah beban JS di client (bertentangan prinsip Performance); CSS Modules murni tidak menyediakan sistem desain token yang konsisten out-of-the-box seperti Tailwind + shadcn/ui.
- **Integration Notes:** Versi yang direkomendasikan adalah **Tailwind v4**, karena shadcn/ui kini mendukung penuh Tailwind v4 + React 19 (lihat `dependency-manifest.md`).
- **AI Development Notes:** Jangan menambahkan library CSS-in-JS (styled-components, Emotion) sebagai "pelengkap" — seluruh styling baru wajib memakai kelas Tailwind/komponen shadcn/ui yang sudah ada.

### 4.5 Lucide React — Icons
- **Purpose:** Set ikon SVG untuk seluruh UI.
- **Why Selected:** Ikon default resmi ekosistem shadcn/ui, tree-shakeable per-ikon (tidak mengimpor seluruh set).
- **Advantages:** Ringan, konsisten secara visual, aktif dipelihara, kompatibel React Server/Client Components.
- **Limitations:** Gaya ikon tunggal (line icons) — jika kebutuhan desain memerlukan gaya lain (filled/duotone), perlu keputusan tambahan.
- **Alternative Considered:** Heroicons, React Icons (agregator banyak set ikon).
- **Why Alternative Was Rejected:** React Icons menggabungkan banyak set berbeda gaya dalam satu dependency besar (risiko inkonsistensi visual & bundle lebih besar); Heroicons kurang terintegrasi rapat dengan shadcn/ui dibanding Lucide.
- **Integration Notes:** Import per-ikon: `import { Home } from "lucide-react"`.
- **AI Development Notes:** Jangan mencampur set ikon lain (Font Awesome, Heroicons, dsb.) dalam satu halaman/komponen — konsistensi visual adalah bagian dari Definition of Done.

### 4.6 Supabase — Backend (BaaS)
- **Purpose:** Platform backend-as-a-service yang menyediakan Postgres, Auth, Storage, Row Level Security, dan Edge Functions dalam satu layanan terkelola.
- **Why Selected:** Memberi Auth + Storage + RLS siap pakai tanpa membangun ulang dari nol, sekaligus database relasional penuh (PostgreSQL) yang dibutuhkan skema ERD proyek yang kaya relasi (`PROJECT-CONSTITUTION.md` Bagian 4).
- **Advantages:** RLS sebagai lapisan pertahanan kedua di level database (selaras `PROJECT-CONSTITUTION.md` Bagian 20 poin 2); Realtime subscriptions untuk notifikasi in-app; Edge Functions untuk logic dekat-DB (trigger sitemap/indexing); mengurangi jumlah vendor/infrastruktur yang perlu dikelola tim kecil.
- **Limitations:** Vendor lock-in relatif terhadap API/konvensi Supabase; skala sangat besar (>>jutaan baris/detik) mungkin memerlukan strategi tambahan (read replica, sharding) di fase lanjutan; RLS policy yang kompleks (RBAC kustom proyek ini) butuh disiplin penulisan policy yang benar agar tidak membocorkan data.
- **Alternative Considered:** Firebase (Firestore/NoSQL), backend custom (NestJS/Express + Postgres terkelola sendiri seperti AWS RDS).
- **Why Alternative Was Rejected:** Firebase berbasis NoSQL tidak cocok dengan skema ERD relasional ketat proyek ini (FK, ENUM, UNIQUE composite — lihat ERD Bagian 2); backend custom penuh menambah beban operasional (mengelola server, auth, storage dari nol) yang bertentangan dengan prinsip Simplicity & Cost Efficiency untuk tahap MVP — dikonfirmasi final sebagai bagian keputusan arsitektur backend proyek (`architecture-decision-records.md` ADR-001, Approved: Route Handlers + Supabase, tanpa service Node.js terpisah).
- **Integration Notes:** Wajib dua lapis pertahanan — RBAC middleware aplikasi **dan** RLS (`AI-DEVELOPMENT-BLUEPRINT.md` Bagian 2); service role key **hanya** di server-side, tidak pernah ke client (`PROJECT-CONSTITUTION.md` Bagian 12 & 17).
- **AI Development Notes:** Jangan pernah meng-expose `SUPABASE_SERVICE_ROLE_KEY` ke bundle client-side; role/permission tetap dikelola di tabel `roles`/`role_permissions` aplikasi, bukan hanya metadata `auth.users` Supabase (`PROJECT-CONSTITUTION.md` Bagian 12).

### 4.7 PostgreSQL (Supabase) — Database
- **Purpose:** Database relasional utama seluruh entitas proyek (37+ tabel per ERD).
- **Why Selected:** ERD proyek memakai relasi ketat (FK, ENUM, UNIQUE composite) yang cocok RDBMS, bukan skema fleksibel NoSQL.
- **Advantages:** ACID compliance, dukungan indexing kaya (composite, trigram/full-text untuk `area_keyword`), ekosistem tooling matang (migration, backup).
- **Limitations:** Scaling horizontal butuh strategi eksplisit (partitioning/sharding) jika volume tumbuh sangat besar — dicatat sebagai keputusan arsitektur terpisah di masa depan (`SYSTEM-ARCHITECTURE.md` Bagian 20).
- **Alternative Considered:** MongoDB/NoSQL document store.
- **Why Alternative Was Rejected:** Tidak cocok dengan kebutuhan relasi ketat & constraint (CHECK, UNIQUE, FK cascading) yang menjadi tulang punggung RBAC dan ownership hard rule proyek ini.
- **Integration Notes:** Migration dikelola via Supabase CLI, disimpan di repo (`/apps/api/migrations`), tidak boleh diedit langsung lewat Supabase Studio di production (`PROJECT-CONSTITUTION.md` Bagian 12).
- **AI Development Notes:** Jangan mengganti nama tabel/field yang sudah ada; setiap perubahan skema wajib migration file + sinkronisasi `ERD-Skema-Database.md`/`ERD-Diagram.mermaid`.

### 4.8 Supabase Auth — Authentication
- **Purpose:** Mekanisme login (email/password, OTP, Google OAuth2), dibungkus JWT internal platform.
- **Why Selected:** Selaras `API-Specification` §0.1 & §1.1 — hasil akhir tetap JWT platform sendiri, sehingga endpoint lain tidak perlu tahu metode login yang dipakai.
- **Advantages:** OTP & OAuth2 siap pakai tanpa membangun ulang; terintegrasi rapat dengan RLS Supabase (`auth.uid()`).
- **Limitations:** Role/permission aplikasi (RBAC kustom 7 role) tidak sepenuhnya native di Supabase Auth — wajib tetap dikelola di tabel `roles`/`role_permissions` aplikasi sendiri.
- **Alternative Considered:** Auth0, Clerk, NextAuth.js/Auth.js custom.
- **Why Alternative Was Rejected:** Menambah vendor terpisah dari database (kompleksitas & biaya tambahan) padahal Supabase Auth sudah terintegrasi langsung dengan Postgres/RLS yang sudah dipilih sebagai database.
- **Integration Notes:** Verifikasi `id_token` Google OAuth wajib server-side dengan Google Auth Library resmi (`API-Specification` §9.5).
- **AI Development Notes:** Jangan menyimpan role di JWT/metadata Supabase sebagai satu-satunya sumber kebenaran — selalu validasi ulang terhadap tabel `role_permissions` aplikasi.

### 4.9 Supabase RLS + RBAC (kustom aplikasi) — Authorization
- **Purpose:** Lapisan otorisasi ganda — RBAC kustom (7 role: Superadmin/Manager/Admin/Instructor/Agen/Developer Partner/Buyer) di level aplikasi, RLS sebagai lapisan kedua di level database.
- **Why Selected:** `PROJECT-CONSTITUTION.md` Bagian 20 poin 2 mewajibkan RLS + middleware RBAC berlapis, tidak pernah hanya mengandalkan satu lapisan.
- **Advantages:** Ownership (`agent_id`) sebagai hard boundary ditegakkan dua kali (aplikasi & DB) — mengurangi risiko kebocoran data lintas agen meski ada bug di satu lapisan.
- **Limitations:** Kompleksitas ganda berarti setiap perubahan skema permission harus disinkronkan hati-hati di kedua lapisan agar tidak saling bertentangan.
- **Alternative Considered:** RBAC aplikasi saja tanpa RLS (percaya penuh pada middleware).
- **Why Alternative Was Rejected:** Bertentangan langsung dengan hard rule keamanan proyek (dua lapisan pertahanan wajib) — satu lapisan gagal (mis. bug middleware) akan langsung membocorkan seluruh data tanpa RLS sebagai jaring pengaman.
- **Integration Notes:** `granted_scope` (`own`/`all`/`none`) diterapkan di layer service/repository, bukan controller (`AI-DEVELOPMENT-BLUEPRINT.md` Bagian 31).
- **AI Development Notes:** Superadmin selalu bypass (short-circuit `true`); Manager selalu `granted_scope = 'all'` tanpa mode scoped tim/wilayah — ini keputusan final, jangan ditanyakan ulang.

### 4.10 Supabase Storage — Storage
- **Purpose:** Penyimpanan file foto/video listing (bucket publik) dan dokumen legalitas agen (bucket privat terenkripsi).
- **Why Selected:** Terintegrasi langsung dengan Supabase Auth/RLS untuk kontrol akses signed URL, mengurangi vendor tambahan.
- **Advantages:** Bucket publik vs privat terpisah tegas sesuai kebutuhan keamanan (`PROJECT-CONSTITUTION.md` Bagian 12); signed URL berumur pendek untuk dokumen KTP/NPWP.
- **Limitations:** Transformasi gambar (resize/format modern WebP/AVIF) tidak sekaya CDN gambar khusus (Cloudinary/ImageKit) — kompresi sisi client ditangani terpisah oleh `browser-image-compression` (Bagian 4.28).
- **Alternative Considered:** Cloudinary, ImageKit, AWS S3 + CloudFront.
- **Why Alternative Was Rejected:** Menambah vendor CDN gambar terpisah untuk MVP dianggap belum perlu selama Supabase Storage + kompresi client-side mencukupi target Core Web Vitals; opsi ini tetap dicatat sebagai evaluasi fase lanjutan (Bagian 8) jika kebutuhan transformasi gambar bertumbuh kompleks.
- **Integration Notes:** Bucket publik (`listing-photos`, `listing-videos`) vs privat (`agent-verification-documents`) — lihat `PROJECT-CONSTITUTION.md` Bagian 12.
- **AI Development Notes:** Dokumen legalitas tidak pernah lewat CDN publik; akses hanya via signed URL untuk role `superadmin`/`manager`/`admin` saat review.

### 4.11 Vercel — Hosting
- **Purpose:** Platform hosting & deployment untuk aplikasi Next.js.
- **Why Selected:** Kombinasi Next.js + Vercel adalah pasangan native (pembuat framework yang sama), memberi dukungan penuh fitur App Router (ISR, Edge Middleware, Image Optimization) tanpa konfigurasi tambahan.
- **Advantages:** Zero-config deploy dari GitHub, preview deployment per PR, edge caching bawaan mendukung target TTFB < 600ms.
- **Limitations:** Model harga berbasis fungsi serverless/edge dapat menjadi signifikan pada traffic sangat tinggi — perlu dipantau seiring pertumbuhan.
- **Alternative Considered:** Self-hosted (Docker + VPS/Kubernetes), Netlify, AWS Amplify.
- **Why Alternative Was Rejected:** Self-hosted menambah beban operasional DevOps yang tidak sepadan untuk tim kecil di tahap MVP; Netlify/Amplify memiliki dukungan fitur App Router Next.js (khususnya fitur terbaru seperti Server Actions/Cache Components) yang kurang seketat Vercel sebagai pembuat framework.
- **Integration Notes:** Deployment pipeline: GitHub → Vercel (lihat 4.13).
- **AI Development Notes:** Perhatikan `NODE_ENV`/environment variable per environment (staging/production) dikelola di dashboard Vercel, tidak pernah di-commit ke repo.

> **Catatan governance:** Keputusan teknologi Vercel sebagai hosting resmi sudah **Approved** (`architecture-decision-records.md` ADR-010, Deployment Strategy, 27 Juli 2026) — bukan lagi Open Question di dokumen ini (lihat Bagian 9). Sesuai `AI-CONTEXT-PACK.md` bagian "Potential Conflict" poin 3, satu-satunya sisa pekerjaan adalah administratif: keputusan ini **belum dibackfill formal** ke `PROJECT-CONSTITUTION.md` sebagai keputusan arsitektur (baru muncul di `SYSTEM-ARCHITECTURE.md` dan ADR ini) — tugas sinkronisasi dokumen governance, bukan keputusan teknologi yang masih terbuka.

### 4.12 GitHub — Repository
- **Purpose:** Version control & kolaborasi kode sumber.
- **Why Selected:** Integrasi native dengan Vercel (auto-deploy per push/PR) dan GitHub Actions untuk CI/CD.
- **Advantages:** Ekosistem terbesar untuk code review, Actions, dan integrasi pihak ketiga (Sentry, dsb.).
- **Limitations:** Tidak relevan untuk tim yang sudah terkunci di platform Git lain (GitLab/Bitbucket) — tidak berlaku di proyek ini.
- **Alternative Considered:** GitLab, Bitbucket.
- **Why Alternative Was Rejected:** Tidak memberi keuntungan tambahan dibanding GitHub untuk kombinasi stack ini, sementara integrasi Vercel paling matang lewat GitHub.
- **Integration Notes:** Branch protection + status check (lint/type-check/test/migration) wajib lolos sebelum merge ke `main` (`PROJECT-CONSTITUTION.md` Bagian 21).
- **AI Development Notes:** Commit message mengikuti Conventional Commits (`AI-DEVELOPMENT-BLUEPRINT.md` Bagian 19).

### 4.13 GitHub → Vercel — Deployment
- **Purpose:** Pipeline deployment otomatis dari commit/PR ke lingkungan preview & production.
- **Why Selected:** Menghilangkan langkah deploy manual; setiap PR otomatis mendapat preview URL untuk review visual sebelum merge.
- **Advantages:** Rollback instan ke deployment sebelumnya jika production bermasalah; preview environment memudahkan QA fitur baru termasuk oleh AI Coding Assistant untuk verifikasi visual.
- **Limitations:** Bergantung pada GitHub Actions/Vercel checks berjalan tepat waktu — CI yang lambat dapat menghambat kecepatan iterasi tim.
- **Alternative Considered:** Deployment manual via CLI, pipeline custom (Jenkins/GitLab CI di infra sendiri).
- **Why Alternative Was Rejected:** Menambah kompleksitas operasional tanpa manfaat signifikan dibanding integrasi native GitHub → Vercel untuk stack Next.js.
- **Integration Notes:** Lint + type-check + test otomatis + migration check wajib lolos sebagai gate sebelum merge (`PROJECT-CONSTITUTION.md` Bagian 21 poin 2).
- **AI Development Notes:** Jangan bypass CI check untuk "mempercepat" merge, termasuk saat bekerja sebagai AI Coding Assistant.

### 4.14 Resend — Transactional Email
- **Purpose:** Pengiriman email transaksional (OTP, notifikasi status approval, reminder).
- **Why Selected:** API modern berbasis developer-experience tinggi, terintegrasi baik dengan ekosistem TypeScript/React (React Email templates), mengisi kekosongan yang dicatat `SYSTEM-ARCHITECTURE.md` Bagian 23 poin 10 (provider email transaksional belum ditetapkan di dokumen sumber v1.1).
- **Advantages:** Deliverability baik, template berbasis komponen React, dashboard log pengiriman untuk debugging.
- **Limitations:** Harga per volume email perlu dipantau seiring pertumbuhan basis pengguna (agen + buyer).
- **Alternative Considered:** SendGrid, Amazon SES, Postmark.
- **Why Alternative Was Rejected:** SES membutuhkan konfigurasi infrastruktur tambahan (domain warm-up, DKIM manual) yang lebih berat untuk tim kecil; SendGrid/Postmark valid tetapi Resend dipilih karena DX (developer experience) TypeScript-nya paling selaras dengan stack Next.js/React yang sudah dipilih.
- **Integration Notes:** Dipakai untuk OTP (Modul 1), notifikasi status (Modul 8), bukan untuk marketing/bulk email.
- **AI Development Notes:** Jangan mengirim data sensitif (`net_income`, dokumen legalitas) sebagai lampiran/isi email — hanya notifikasi status & link aman.

### 4.15 Sentry — Monitoring
- **Purpose:** Error tracking & performance monitoring untuk frontend (Next.js) dan Route Handlers.
- **Why Selected:** Mengisi kekosongan tooling monitoring/observability yang dicatat sebagai open question di `SYSTEM-ARCHITECTURE.md` Bagian 23 poin 11; SDK resmi `@sentry/nextjs` terintegrasi rapat dengan App Router (server & client components, edge runtime).
- **Advantages:** Source map otomatis untuk stack trace production yang terbaca; performance tracing untuk mendeteksi regresi Core Web Vitals/TTFB.
- **Limitations:** Volume error/tracing tinggi dapat memakan kuota paket berbayar — perlu sampling rate dikonfigurasi wajar.
- **Alternative Considered:** LogRocket, Datadog, self-hosted (Grafana + Loki).
- **Why Alternative Was Rejected:** Datadog/self-hosted jauh lebih kompleks & mahal untuk kebutuhan MVP; LogRocket lebih berfokus session replay dibanding error/performance tracing yang jadi prioritas utama proyek ini.
- **Integration Notes:** `request_id`/`correlation_id` di error backend wajib konsisten dengan yang dikembalikan ke client (`PROJECT-CONSTITUTION.md` Bagian 13).
- **AI Development Notes:** Jangan pernah mengirim data sensitif (`net_income`, KTP/NPWP, token JWT penuh) ke Sentry breadcrumb/context — scrub sebelum log terkirim.

### 4.16 TanStack Query — Server State
- **Purpose:** Fetching, caching, dan sinkronisasi data server di sisi client (dashboard, admin panel).
- **Why Selected:** Mengelola cache server-state secara deklaratif (loading/error/stale state) tanpa reinventing logic tersebut secara manual, terintegrasi baik dengan Supabase client & Route Handlers.
- **Advantages:** Automatic refetching, cache invalidation granular, devtools bawaan untuk debugging; mengurangi boilerplate `useEffect` + `useState` manual untuk data fetching.
- **Limitations:** Konsep cache key & invalidation butuh pemahaman tim agar tidak terjadi stale data yang tidak disadari, khususnya pada dashboard real-time-ish (jumlah lead, notifikasi).
- **Alternative Considered:** SWR.
- **Why Alternative Was Rejected:** **SWR eksplisit dilarang** di Bagian 6 — untuk menghindari dua library server-state berfungsi sama berjalan berdampingan; TanStack Query dipilih karena fitur mutation & devtools lebih lengkap untuk kebutuhan CRUD dashboard yang kompleks (listing, DBR, admin panel).
- **Integration Notes:** Dipakai khusus di route group `(dashboard)`/`(admin)` (CSR) — halaman publik `(public)` tetap mengandalkan Server Component fetch, bukan TanStack Query, untuk menjaga SSR.
- **AI Development Notes:** Jangan mencampur pola fetch manual (`useEffect` + `fetch`) dengan TanStack Query dalam komponen yang sama — pilih satu pola secara konsisten per halaman.

### 4.17 Zustand — UI State
- **Purpose:** State management untuk state UI lokal/lintas komponen yang bukan server-state (mis. state wizard form multi-step, filter UI sementara, modal state global).
- **Why Selected:** API minimal (tanpa boilerplate reducer/action/dispatch), ukuran bundle sangat kecil, cocok dengan prinsip Simplicity.
- **Advantages:** Tidak memerlukan Context Provider bertingkat; mudah dipadukan dengan TypeScript untuk tipe store yang ketat; performa baik karena hanya me-render ulang komponen yang subscribe ke slice state terkait.
- **Limitations:** Tanpa struktur/disiplin tim, store bisa jadi "keranjang sampah" state acak — perlu konvensi jelas (satu store per domain UI, bukan satu store raksasa global).
- **Alternative Considered:** Redux (Toolkit), Context API murni, Jotai/Recoil.
- **Why Alternative Was Rejected:** **Redux eksplisit dilarang** di Bagian 6 karena boilerplate berlebihan untuk kebutuhan UI state proyek ini; Context API murni tidak dioptimasi untuk update frekuensi tinggi (re-render berlebihan); Jotai/Recoil valid tapi tidak dipilih agar tidak menambah satu lagi library state tanpa kebutuhan jelas di luar yang sudah dipilih (TanStack Query untuk server state sudah menangani sebagian besar kebutuhan).
- **Integration Notes:** Server state (data dari API) **tidak** disimpan di Zustand — itu domain TanStack Query; Zustand murni untuk UI state.
- **AI Development Notes:** Sebelum membuat store Zustand baru, pastikan state yang dimaksud benar-benar UI state, bukan server state yang seharusnya lewat TanStack Query.

### 4.18 React Hook Form — Forms
- **Purpose:** Manajemen state form (registrasi, listing, kalkulator DBR, admin panel).
- **Why Selected:** Performa tinggi (uncontrolled inputs, minim re-render), terintegrasi rapat dengan Zod lewat resolver resmi.
- **Advantages:** API deklaratif dengan validasi real-time; mendukung form kompleks (nested fields, field array) yang dibutuhkan form listing (foto multi-upload, field lokasi cascading).
- **Limitations:** Pola uncontrolled berbeda dari form berbasis state React biasa — perlu penyesuaian pola pikir tim yang terbiasa controlled input penuh.
- **Alternative Considered:** Formik.
- **Why Alternative Was Rejected:** **Formik eksplisit dilarang** di Bagian 6 — performa re-render Formik lebih rendah dibanding React Hook Form pada form besar/kompleks seperti form listing multi-step proyek ini.
- **Integration Notes:** Dipasangkan dengan `@hookform/resolvers` + Zod (lihat 4.19) sebagai satu sumber skema validasi FE & BE.
- **AI Development Notes:** Skema validasi Zod ditulis sekali di `lib/validation`/`shared-types`, dipakai sebagai resolver React Hook Form — jangan duplikasi aturan validasi manual di dalam komponen form.

### 4.19 Zod — Validation
- **Purpose:** Skema validasi tunggal untuk seluruh input — dipakai baik di client (real-time form validation) maupun server (validasi ulang sebelum tulis DB).
- **Why Selected:** TypeScript-first (skema otomatis menghasilkan tipe statis lewat `z.infer`), memenuhi prinsip Single Source of Truth validasi (`PROJECT-CONSTITUTION.md` Bagian 14).
- **Advantages:** Satu definisi skema untuk validasi + tipe, mengurangi duplikasi & drift antara tipe TypeScript manual dan aturan validasi runtime.
- **Limitations:** Skema kompleks (validasi kondisional lintas field, mis. konversi tenor tahun→bulan) butuh `.refine()`/`.transform()` yang perlu didokumentasikan agar mudah dipahami AI Coding Assistant berikutnya.
- **Alternative Considered:** Yup, Joi.
- **Why Alternative Was Rejected:** Yup/Joi tidak memiliki inferensi tipe TypeScript native sekuat Zod (`z.infer`), sehingga tetap membutuhkan definisi tipe terpisah yang berisiko drift dari skema validasi.
- **Integration Notes:** Backend tidak boleh mempercayai validasi frontend — validasi ulang di server wajib untuk semua endpoint mutating (`POST`/`PUT`/`PATCH`).
- **AI Development Notes:** Field wajib PRD Modul 3.2 (judul, lokasi cascading, harga, minimal 3 foto, status legalitas, no. WA) divalidasi Zod **sebelum** status listing berubah ke `pending_review`.

### 4.20 Vitest — Unit Testing
- **Purpose:** Unit test untuk business logic (service layer, utility functions, skema Zod).
- **Why Selected:** Kompatibel native dengan tooling Vite/Next.js modern, konfigurasi minimal, kecepatan eksekusi tinggi dibanding Jest pada proyek berbasis ESM/TypeScript.
- **Advantages:** API mirip Jest (kurva belajar rendah bagi tim yang familiar Jest), watch mode sangat cepat, dukungan native TypeScript/ESM tanpa transpilasi tambahan.
- **Limitations:** Ekosistem plugin sedikit lebih muda dibanding Jest — kebutuhan sangat niche mungkin butuh workaround.
- **Alternative Considered:** Jest.
- **Why Alternative Was Rejected:** Jest tetap valid secara fungsional, namun Vitest dipilih untuk performa & kompatibilitas ESM/TypeScript yang lebih mulus dengan stack Next.js modern — menghindari dua test runner berjalan berdampingan tanpa alasan kuat.
- **Integration Notes:** Dijalankan sebagai CI gate wajib sebelum merge (`PROJECT-CONSTITUTION.md` Bagian 21).
- **AI Development Notes:** Business logic sensitif (perhitungan DBR, filter `granted_scope`, ownership check) wajib memiliki unit test eksplisit, bukan hanya diuji manual.

### 4.21 React Testing Library — Component Testing
- **Purpose:** Pengujian komponen React dari perspektif interaksi pengguna (bukan detail implementasi internal).
- **Why Selected:** Standar industri untuk pengujian komponen React, filosofi "test seperti pengguna memakai aplikasi" cocok untuk memverifikasi form/dashboard yang kompleks.
- **Advantages:** Query berbasis accessibility role/label mendorong komponen yang lebih aksesibel; terintegrasi mulus dengan Vitest.
- **Limitations:** Tidak menguji end-to-end lintas halaman/navigasi nyata (itu domain Playwright).
- **Alternative Considered:** Enzyme.
- **Why Alternative Was Rejected:** Enzyme sudah tidak lagi dipelihara aktif untuk versi React modern (Server Components) — tidak kompatibel dengan arsitektur App Router.
- **Integration Notes:** Dipasangkan dengan `@testing-library/jest-dom` untuk matcher tambahan (`toBeInTheDocument`, dsb.).
- **AI Development Notes:** Query elemen berdasarkan role/label (`getByRole`), bukan `data-testid` sebagai default pertama, agar test turut memverifikasi aksesibilitas.

### 4.22 Playwright — E2E Testing
- **Purpose:** Pengujian end-to-end lintas browser untuk alur kritis (registrasi agen, publish listing, submit DBR, moderasi admin).
- **Why Selected:** Dukungan multi-browser (Chromium, Firefox, WebKit) dalam satu API, auto-wait bawaan mengurangi flaky test, dan dukungan resmi Next.js/Vercel yang matang.
- **Advantages:** Trace viewer untuk debugging test gagal, dapat dijalankan di CI (GitHub Actions) dengan mudah, mendukung pengujian visual & network mocking.
- **Limitations:** Waktu eksekusi E2E lebih lambat dibanding unit test — perlu strategi seleksi alur kritis saja, bukan menguji seluruh permutasi UI lewat E2E.
- **Alternative Considered:** Cypress.
- **Why Alternative Was Rejected:** Cypress secara historis lebih terbatas pada multi-tab/multi-origin testing dan dukungan browser WebKit dibanding Playwright, yang relevan untuk alur OAuth Google (redirect antar origin).
- **Integration Notes:** Dijalankan sebagai bagian CI gate untuk alur kritis sebelum merge ke `main`.
- **AI Development Notes:** Prioritaskan E2E untuk *acceptance criteria* modul di PRD (mis. "Agen baru dapat submit form registrasi lengkap"), bukan menduplikasi seluruh unit test di level E2E.

### 4.23 Recharts — Charts
- **Purpose:** Visualisasi data (dashboard agen: jumlah lead 7/30 hari; dashboard admin: statistik agen/listing/proyek).
- **Why Selected:** Berbasis React/SVG deklaratif (komponen React biasa, bukan wrapper canvas library eksternal), dokumentasi baik, cukup untuk kebutuhan chart standar (bar, line, pie) proyek ini.
- **Advantages:** API deklaratif konsisten dengan pola komponen React lain di proyek; responsif bawaan (`ResponsiveContainer`).
- **Limitations:** Untuk visualisasi sangat kompleks/custom (mis. chart geospasial lanjutan), mungkin kurang fleksibel dibanding D3 murni — belum menjadi kebutuhan di scope Fase 1-2.
- **Alternative Considered:** Chart.js, Victory, D3 murni.
- **Why Alternative Was Rejected:** Chart.js berbasis Canvas API imperatif (kurang idiomatis dalam paradigma komponen React deklaratif); D3 murni terlalu low-level untuk kebutuhan dashboard standar dan menambah kompleksitas pengembangan tanpa manfaat sepadan di tahap ini.
- **Integration Notes:** Dipakai di route group `(dashboard)`/`(admin)` (CSR), bukan halaman publik.
- **AI Development Notes:** Jangan menambah library chart kedua (mis. Chart.js) untuk kasus penggunaan yang sudah bisa dipenuhi Recharts.

### 4.24 TanStack Table — Table
- **Purpose:** Tabel data kompleks (daftar listing admin, daftar agen, laporan) dengan sorting/filtering/pagination sisi client maupun server-driven.
- **Why Selected:** Headless (tanpa opini UI, dipadukan bebas dengan shadcn/ui `Table` component), mendukung pagination server-side yang wajib diterapkan di seluruh query list (`PROJECT-CONSTITUTION.md` Bagian 19).
- **Advantages:** Sangat fleksibel untuk kebutuhan tabel kompleks (kolom dinamis, row selection, expandable rows) yang relevan untuk Admin Panel/CMS.
- **Limitations:** Headless berarti styling & markup sepenuhnya tanggung jawab tim (dipadukan dengan shadcn/ui `Table`) — bukan tabel "siap pakai" bergaya seperti AG Grid.
- **Alternative Considered:** AG Grid, react-table v7 (versi lama).
- **Why Alternative Was Rejected:** AG Grid membawa opini UI berat & lisensi berbayar untuk fitur enterprise yang tidak dibutuhkan skala proyek ini; react-table v7 adalah versi API lama — TanStack Table adalah penerus resminya (v8+).
- **Integration Notes:** Query list API selalu paginated (`?page=1&per_page=20`) — TanStack Table dikonfigurasi mode `manualPagination` untuk tabel berbasis data server.
- **AI Development Notes:** Jangan mengambil seluruh baris tanpa limit ke client lalu memfilter di frontend — filter/paginate wajib di level API sesuai kontrak `API-Specification` §0.4.

### 4.25 dnd-kit — Drag & Drop
- **Purpose:** Interaksi drag-and-drop (mis. mengurutkan foto listing, menyusun ulang urutan modul kursus/kuis di Learning Center).
- **Why Selected:** Library drag-and-drop modern untuk React yang aktif dipelihara, dengan dukungan aksesibilitas (keyboard navigation) bawaan.
- **Advantages:** Modular (core + sortable preset terpisah, hanya impor yang dibutuhkan), performa baik dengan sensor pointer/keyboard.
- **Limitations:** API berbasis primitives (butuh sedikit lebih banyak setup dibanding library "drop-in" lama) — namun ini trade-off wajar untuk fleksibilitas & maintenance jangka panjang.
- **Alternative Considered:** react-beautiful-dnd.
- **Why Alternative Was Rejected:** `react-beautiful-dnd` sudah **dinyatakan deprecated** oleh maintainer aslinya (Atlassian) dan tidak lagi menerima update kompatibilitas React versi baru — bertentangan dengan prinsip Long Term Support.
- **Integration Notes:** Dipakai di form upload foto listing (reorder foto & pilih cover) dan admin Learning Center (reorder lesson/soal).
- **AI Development Notes:** Pastikan urutan hasil drag-drop disimpan eksplisit (kolom `order`/`position`) di skema DB terkait, bukan hanya diasumsikan dari urutan array response API.

### 4.26 date-fns — Date Library
- **Purpose:** Manipulasi & formatting tanggal (masa berlaku listing, expiry, jadwal event, tenor DBR).
- **Why Selected:** Modular (tree-shakeable per fungsi, bukan satu objek besar), immutable by design, ukuran bundle jauh lebih kecil dibanding alternatif monolitik.
- **Advantages:** Fungsi murni per kebutuhan (`format`, `addMonths`, `differenceInDays`) memudahkan tree-shaking; dukungan locale Indonesia (`id`) untuk format tanggal lokal.
- **Limitations:** Penanganan timezone kompleks (lintas zona waktu) membutuhkan paket pendamping (`date-fns-tz`) jika suatu saat dibutuhkan — belum menjadi kebutuhan eksplisit di dokumen sumber (aplikasi berbasis WIB/lokal Indonesia).
- **Alternative Considered:** Moment.js, Day.js, Luxon.
- **Why Alternative Was Rejected:** **Moment.js eksplisit dilarang** di Bagian 6 (sudah dinyatakan dalam mode maintenance oleh tim intinya, mutable API rawan bug, ukuran besar); Day.js/Luxon valid secara teknis namun tidak dipilih agar tidak ada dua library date-utility berfungsi tumpang tindih tanpa Architecture Decision eksplisit.
- **Integration Notes:** Konversi tenor tahun→bulan (keputusan final `tenor_months`) sebaiknya memakai utility murni, bukan objek Date, karena tenor adalah angka bulan, bukan rentang tanggal.
- **AI Development Notes:** Jangan mengimpor seluruh `date-fns` sebagai satu objek — impor fungsi spesifik (`import { format } from "date-fns"`) untuk menjaga tree-shaking optimal.

### 4.27 pdf-lib — PDF
- **Purpose:** Generate PDF (export hasil simulasi DBR untuk lampiran pengajuan KPR ke bank, per PRD Modul 7 User Flow 5.7).
- **Why Selected:** Library PDF murni JavaScript/TypeScript yang berjalan baik di lingkungan Node.js (Route Handlers) tanpa dependency native/binary eksternal (mis. headless Chrome).
- **Advantages:** Dapat membuat & memodifikasi PDF terprogram (isi form, tambah teks/gambar) sepenuhnya di sisi server, ringan untuk dijalankan di lingkungan serverless (Vercel Functions).
- **Limitations:** Tidak dirancang untuk "convert HTML ke PDF" — layout kompleks harus disusun manual lewat API (koordinat teks/gambar), bukan dari template HTML/CSS existing.
- **Alternative Considered:** Puppeteer/Playwright (render HTML → PDF), jsPDF.
- **Why Alternative Was Rejected:** Puppeteer/headless Chrome jauh lebih berat untuk lingkungan serverless (ukuran binary besar, cold start lambat di Vercel Functions); jsPDF valid secara fungsi tapi tumpang tindih penuh dengan pdf-lib — memilih satu untuk menghindari duplikasi library dengan fungsi sama (Bagian 6).
- **Integration Notes:** Dipanggil dari Route Handler saat agen klik "Export ke PDF" di kalkulator DBR (User Flow Modul 7).
- **AI Development Notes:** Data finansial (`net_income`, `existing_installments`) yang masuk ke PDF tetap tunduk pada aturan data sensitif — PDF hasil generate tidak boleh disimpan di storage publik.

### 4.28 browser-image-compression — Image Compression
- **Purpose:** Kompresi gambar di sisi client sebelum upload (foto listing) agar ukuran file lebih kecil sebelum mencapai Supabase Storage.
- **Why Selected:** Mengurangi beban upload & bandwidth, mendukung target performa (LCP) dengan memastikan gambar sudah dioptimasi sebelum tersimpan, mengurangi kebutuhan CDN transformasi pihak ketiga yang berat di sisi server.
- **Advantages:** Berjalan di Web Worker (tidak memblokir main thread saat kompresi), konfigurasi target ukuran file maksimal & resolusi maksimal.
- **Limitations:** Kompresi sisi client bergantung kemampuan device pengguna (perangkat agen di lapangan mungkin bervariasi) — tetap perlu validasi ukuran & tipe file ulang di server (`PROJECT-CONSTITUTION.md` Bagian 16).
- **Alternative Considered:** Kompresi server-side penuh (Sharp) atau CDN transformation (Cloudinary/ImageKit) sebagai satu-satunya lapisan optimasi.
- **Why Alternative Was Rejected:** Server-side-only compression menambah beban proses di Route Handlers/serverless function (biaya & waktu proses lebih tinggi); pendekatan hybrid (kompresi client + Supabase Storage) dipilih agar sejalan dengan keputusan tidak menambah vendor CDN gambar terpisah di Bagian 4.10.
- **Integration Notes:** Dijalankan sebelum `POST /listings/{id}/media`, bukan pengganti validasi MIME/magic-bytes di server.
- **AI Development Notes:** Validasi tipe file **wajib** tetap dilakukan di server (cek magic bytes), kompresi client bukan pengganti validasi keamanan upload.

### 4.29 Google Maps Platform — Maps (⚠️ Status: Condong Dipilih, Belum Final — lihat ADR-008)
- **Purpose:** Autocomplete alamat (client-side), reverse geocoding & distance matrix (server-side) untuk fitur lokasi listing.
- **Why Selected (sementara/condong):** Dokumen ini condong memilih Google Maps Platform, **namun status resminya tetap OPEN** — `architecture-decision-records.md` ADR-008 (Maps Provider) mencatat keputusan ini **belum final** karena `PROJECT-CONSTITUTION.md` dan `API-Specification-v1.1.md` §13/§9.1 (dokumen berhierarki lebih tinggi) masih mencatat provider Maps sebagai "belum final (Google Maps Platform atau Mapbox)", dan konfirmasi biaya bisnis per-request belum diterima. **Baris ini bukan pengecualian dari Bagian 3 — melainkan satu-satunya baris di Official Technology Stack yang masih menunggu ADR naik status ke Approved.**
- **Advantages:** Akurasi data alamat/POI Indonesia yang matang, dokumentasi lengkap, satu vendor untuk seluruh kebutuhan (Autocomplete, Geocoding, Distance Matrix, Places Nearby).
- **Limitations:** Model harga per-request setelah kuota gratis terlampaui — perlu pemisahan tegas client-key (dibatasi domain/referrer, kuota rendah) vs server-key (rahasia, kuota penuh) sesuai `PROJECT-CONSTITUTION.md` Bagian 20 poin 5.
- **Alternative Considered:** Mapbox — tetap alternatif valid secara teknis per ADR-008, belum dieksplorasi mendalam di dokumen sumber, belum ditolak secara definitif.
- **Why Alternative Was Rejected:** **Belum ditolak secara final** — Mapbox tetap tercatat sebagai alternatif sah di ADR-008 selama status ADR masih OPEN. Google Maps Platform condong dipilih atas dasar akurasi data Indonesia & konsolidasi vendor, namun keputusan final menunggu konfirmasi biaya tim bisnis (lihat Bagian 9, Open Questions poin 4).
- **Integration Notes:** Reverse geocoding & Distance Matrix dipanggil server-side (API key rahasia); Autocomplete client-side (API key dibatasi domain/referrer). **Implementasi wajib dibuat provider-agnostic/configurable** sampai ADR-008 Approved, agar penggantian ke Mapbox (bila terjadi) tidak memerlukan rewrite besar.
- **AI Development Notes:** Jangan pernah mengekspos `GOOGLE_MAPS_API_KEY_SERVER` ke client — hanya `GOOGLE_MAPS_API_KEY_CLIENT` yang boleh dibundel ke frontend. Jangan mengimplementasikan integrasi Maps sebagai keputusan final yang tidak dapat diubah — tandai `// TODO: menunggu resolusi ADR-008` pada titik integrasi kunci.

---

## 5. Compatibility Matrix

Tabel berikut merangkum kompatibilitas & pola integrasi antar komponen inti stack. Simbol: ✅ Terintegrasi langsung/resmi · ⚙️ Terintegrasi via adapter/konfigurasi tambahan · ➖ Tidak berinteraksi langsung (independen).

| | Next.js | Supabase | TanStack Query | Zustand | React Hook Form | Zod | Tailwind/shadcn | Vercel | Sentry | Playwright |
|---|---|---|---|---|---|---|---|---|---|---|
| **Next.js** | — | ⚙️ via `@supabase/ssr` | ✅ | ✅ | ✅ | ✅ | ✅ | ✅ native | ✅ `@sentry/nextjs` | ✅ |
| **Supabase** | ⚙️ | — | ⚙️ sebagai query fn | ➖ | ➖ | ⚙️ validasi payload | ➖ | ➖ | ⚙️ error capture | ⚙️ test DB |
| **TanStack Query** | ✅ | ⚙️ | — | ➖ (beda domain state) | ➖ | ⚙️ validasi response | ➖ | ➖ | ➖ | ⚙️ mocking |
| **Zustand** | ✅ | ➖ | ➖ | — | ⚙️ state form lintas step | ➖ | ➖ | ➖ | ➖ | ➖ |
| **React Hook Form** | ✅ | ➖ | ➖ | ⚙️ | — | ✅ `@hookform/resolvers` | ✅ styling input | ➖ | ➖ | ✅ diuji E2E |
| **Zod** | ✅ | ⚙️ | ⚙️ | ➖ | ✅ | — | ➖ | ➖ | ➖ | ➖ |
| **Tailwind/shadcn** | ✅ | ➖ | ➖ | ➖ | ✅ | ➖ | — | ✅ build native | ➖ | ✅ diuji visual |
| **Vercel** | ✅ native | ⚙️ env var | ➖ | ➖ | ➖ | ➖ | ✅ | — | ⚙️ integrasi | ⚙️ CI |
| **Sentry** | ✅ | ⚙️ | ➖ | ➖ | ➖ | ➖ | ➖ | ⚙️ | — | ➖ |
| **Playwright** | ✅ | ⚙️ seed test data | ⚙️ | ➖ | ✅ | ➖ | ✅ | ⚙️ CI | ➖ | — |

**Catatan penting kompatibilitas:**
- **Next.js ↔ Supabase**: gunakan `@supabase/ssr` untuk client browser vs server terpisah (`PROJECT-CONSTITUTION.md` Bagian 5 — `/lib/supabase/` browser & server terpisah).
- **React Hook Form ↔ Zod**: via `@hookform/resolvers/zod` — satu skema dipakai sebagai `resolver` form sekaligus validasi server.
- **Next.js ↔ Vercel**: pasangan native — fitur ISR/Edge Middleware/Image Optimization bekerja penuh tanpa konfigurasi tambahan.
- **Playwright ↔ Next.js**: dijalankan terhadap build production (`next build && next start`) di CI, bukan `next dev`, agar hasil pengujian merepresentasikan kondisi production.
- **TanStack Query ↔ Zustand**: keduanya sengaja **tidak tumpang tindih tanggung jawab** — Query untuk server state, Zustand untuk UI state; mencampur keduanya untuk domain yang sama adalah anti-pattern.
- **Tailwind v4 ↔ shadcn/ui**: shadcn/ui versi terbaru mendukung penuh Tailwind v4 (CSS-first `@theme`) & React 19 — pastikan proyek diinisialisasi langsung dengan kombinasi versi terbaru ini agar tidak perlu migrasi v3→v4 di kemudian hari.

---

## 6. Architecture Constraints

Aturan berikut **wajib dipatuhi** — pelanggaran dianggap gagal Definition of Done (`AI-DEVELOPMENT-BLUEPRINT.md` Bagian 30):

1. **Tidak boleh menggunakan Redux** (termasuk Redux Toolkit) — Zustand adalah satu-satunya UI state management resmi.
2. **Tidak boleh menggunakan Material UI (MUI).**
3. **Tidak boleh menggunakan Ant Design.**
4. **Tidak boleh menggunakan Formik** — React Hook Form adalah satu-satunya form library resmi.
5. **Tidak boleh menggunakan SWR** — TanStack Query adalah satu-satunya server-state library resmi.
6. **Tidak boleh menggunakan Moment.js** — date-fns adalah satu-satunya date library resmi.
7. **Tidak boleh menggunakan react-beautiful-dnd** (deprecated) — dnd-kit adalah pengganti resminya.
8. **Tidak boleh menggunakan CSS-in-JS runtime** (styled-components, Emotion, dsb.) — Tailwind CSS + shadcn/ui adalah satu-satunya pendekatan styling resmi.
9. **Tidak boleh menggunakan Axios** sebagai HTTP client tambahan — gunakan `fetch` native (didukung penuh Next.js/TanStack Query) kecuali ada kebutuhan teknis spesifik yang didokumentasikan sebagai Architecture Decision.
10. **Tidak boleh menambahkan backend service Node.js terpisah** (NestJS/Express, dsb.) — Backend resmi dikunci final sebagai Supabase + Next.js Route Handlers (`architecture-decision-records.md` ADR-001, **Approved**, lihat Bagian 3). Perubahan atas keputusan ini hanya sah melalui ADR baru yang secara eksplisit men-supersede ADR-001, bukan keputusan sepihak AI Coding Assistant maupun implementasi ad-hoc.
11. **Tidak boleh menambahkan library chart/table/PDF/date kedua** yang fungsinya sudah dipenuhi Recharts/TanStack Table/pdf-lib/date-fns tanpa Architecture Decision.
12. **Tidak boleh menggunakan library dengan fungsi yang sama** dengan yang sudah resmi di Bagian 3 tanpa Architecture Decision (ADR) tertulis yang disetujui.
13. **Tidak boleh mengekspos secret/service role key** (`*_SECRET`, `*_SERVICE_ROLE_KEY`, `*_SERVER`) ke bundle client-side — audit build output secara berkala (`PROJECT-CONSTITUTION.md` Bagian 17 & 20).
14. **Tidak boleh mengedit skema database langsung** lewat Supabase Studio di environment production — wajib lewat migration file yang direview.
15. **Tidak boleh mencampur Server Component fetch dan TanStack Query** dalam satu halaman sebagai dua sumber kebenaran data yang berbeda untuk data yang sama.

---

## 7. AI Coding Rules

Aturan berikut **wajib dipatuhi** oleh AI Coding Assistant apa pun (Claude, Bolt.new, ChatGPT, Cursor, GitHub Copilot, dsb.) yang bekerja pada implementasi proyek ini:

1. **Jangan menambahkan dependency baru** di luar Bagian 3 (Official Technology Stack) tanpa alasan tertulis dan justifikasi berbasis 10 prinsip di Bagian 2.
2. **Selalu gunakan library resmi proyek** — cek Bagian 3 & `dependency-manifest.md` sebelum menulis kode yang membutuhkan kapabilitas baru (fetch data, form, chart, dsb.).
3. **Selalu gunakan reusable component** yang sudah ada di `components/ui/` (shadcn/ui) sebelum membuat komponen custom baru yang fungsinya serupa.
4. **Jangan membuat implementasi duplikat** — logic bisnis, skema validasi (Zod), dan tipe data masing-masing punya satu lokasi sumber kebenaran (`lib/`, `packages/shared-types`).
5. **Ikuti seluruh keputusan dokumen ini**, `PROJECT-CONSTITUTION.md`, dan `architecture-decision-records.md` — untuk keputusan arsitektur/teknis, ADR berstatus **Approved** menjadi rujukan tertinggi; untuk hal lain di luar cakupan ADR, jika dokumen ini tampak berbeda dari `PROJECT-CONSTITUTION.md`, **`PROJECT-CONSTITUTION.md` yang menang**; laporkan sebagai `// TODO: konflik technology-decisions vs constitution/ADR`.
6. **Jangan mengambil keputusan arsitektur baru secara sepihak** untuk item yang tercantum di Bagian 9 (Open Questions) — implementasikan sebagai *configurable placeholder*, tandai `// TODO: menunggu keputusan bisnis/arsitektur`, dan laporkan ke manusia jika keputusan tersebut memblokir progres.
7. **Jika instruksi user bertentangan dengan Architecture Constraints (Bagian 6)**, tanyakan konfirmasi eksplisit sebelum menyimpang — jangan diam-diam menambah library terlarang.
8. **Selalu jaga TypeScript `strict: true` tanpa `any` implisit** di seluruh kode baru.
9. **Selalu validasi ulang di server** untuk endpoint mutating (`POST`/`PUT`/`PATCH`) meskipun sudah divalidasi Zod di client — backend tidak boleh mempercayai input client.
10. **Sebelum menambah package baru**, cek dulu apakah kapabilitas yang dibutuhkan sudah tersedia dari salah satu library resmi di Bagian 3 (banyak kebutuhan "baru" sebenarnya sudah tercakup, mis. jangan menambah library HTTP client baru jika `fetch` + TanStack Query sudah cukup).

---

## 8. Future Evaluation

Teknologi berikut **belum** menjadi bagian Official Technology Stack (Bagian 3), namun dicatat sebagai kandidat evaluasi untuk versi/fase pengembangan selanjutnya, konsisten dengan `SYSTEM-ARCHITECTURE.md` Bagian 20 (Future Architecture) dan Bagian 4 (masih terbuka di dokumen sumber):

| Kategori | Kandidat (dari dokumen sumber) | Kapan Relevan |
|---|---|---|
| **Redis** | Cache & rate limiting | Jika rate limiting endpoint sensitif (auth/OTP) & cache edge Vercel/Supabase bawaan dinilai belum cukup saat traffic bertumbuh — keputusan formal masih **OPEN** (`architecture-decision-records.md` ADR-018, lihat Bagian 9 poin 2). |
| **Queue** | BullMQ (butuh Redis) atau Supabase Edge Functions + cron | Untuk regenerasi sitemap event-driven, sinkronisasi counter denormalisasi, pengiriman notifikasi terjadwal — keputusan formal masih **OPEN** (`architecture-decision-records.md` ADR-006, lihat Bagian 9 poin 3). Dengan ADR-001 (Backend Architecture) kini Approved ke arah Route Handlers + Supabase, opsi Edge Functions+cron menguat secara arsitektural. |
| **Search Engine** | Typesense (direkomendasikan dokumen sumber) atau Elasticsearch | Untuk `/properties/search` & `/properties/autocomplete` dengan typo-tolerance — keputusan formal masih **OPEN** (`architecture-decision-records.md` ADR-005, lihat Bagian 9 poin 1). |
| **Payment Gateway** | Midtrans/Xendit | Fase membership premium agen (boost listing, kuota lebih besar) — tidak masuk cakupan wajib MVP (`API-Specification` §9.3). |
| **AI Service** | Rekomendasi listing personalisasi, auto-deskripsi listing, penilaian kualitas foto, chatbot FAQ | Fase lanjutan sesuai `SYSTEM-ARCHITECTURE.md` Bagian 20 — wajib tetap menghormati SSR untuk konten AI-generated agar tidak mengorbankan SEO. |
| **Analytics** | Dashboard analitik custom, funnel lead-to-closing lanjutan | Dibangun di atas data `listing_leads`/`listing_views` yang sudah terstruktur sejak Fase 1 (fondasi GTM/GA4 sudah ada). |
| **Background Job** | Integrasi SLIK/BI Checking (validasi cicilan otomatis DBR), WA Business API | Fase 4 sesuai roadmap PRD Bagian 6. |
| **CDN (gambar khusus)** | Cloudinary/ImageKit sebagai lapisan transformasi tambahan di atas Supabase Storage | Jika kebutuhan transformasi gambar (resize dinamis multi-varian, video streaming) melampaui kapasitas Supabase Storage + kompresi client-side. |

---

## 9. Open Questions

Sesuai instruksi pembuatan dokumen ini — **tidak ada asumsi yang dibuat** untuk poin-poin berikut; seluruhnya wajib dikonfirmasi tim sebelum atau selama development, dan diimplementasikan sebagai *configurable placeholder* dengan penanda `// TODO` di kode. Status setiap poin di bawah ini disinkronkan terhadap `architecture-decision-records.md` sebagai sumber kebenaran — poin yang ADR-nya sudah **Approved** tidak lagi dicantumkan di sini (lihat Bagian 3 & 6 untuk keputusan finalnya).

> **Item yang telah diselesaikan dan dihapus dari daftar ini:** *Arsitektur backend/API* — **RESOLVED** via `architecture-decision-records.md` ADR-001 (Approved, 27 Juli 2026): Next.js Route Handlers + Supabase, tanpa service Node.js terpisah. Lihat Bagian 3 (catatan di bawah tabel stack) dan Bagian 6 poin 10.

1. **Search Engine belum ada di Official Technology Stack** (Bagian 3), padahal `API-Specification` mensyaratkan `/properties/search` dan `/properties/autocomplete` dengan typo-tolerance yang secara teknis membutuhkan mesin pencari khusus (Typesense/Elasticsearch direkomendasikan dokumen sumber) — **query database Postgres murni (full-text/trigram index) kemungkinan tidak cukup** untuk kebutuhan ini pada volume listing besar. Status resmi: **`architecture-decision-records.md` ADR-005 — OPEN.** Perlu keputusan eksplisit: apakah cukup mengandalkan Postgres full-text search di awal, atau menambah Typesense sejak Fase 1.
2. **Cache/Rate limiting level aplikasi (Redis) belum ada di Official Technology Stack**, padahal `PROJECT-CONSTITUTION.md` Bagian 20 poin 6 mewajibkan rate limiting endpoint auth/OTP untuk mencegah brute-force. Status resmi: **`architecture-decision-records.md` ADR-018 — OPEN** (prioritas paling rendah di antara seluruh ADR Open, tidak memblokir Sprint S0–S1; caching edge/CDN halaman publik sudah tercakup inheren oleh ADR-021 & ADR-010, **tidak** memerlukan keputusan terpisah). Perlu keputusan: menggunakan fitur rate-limiting bawaan Vercel/edge middleware, atau menambah Redis. Terkait erat ADR-006 — jika Job Queue mengarah ke BullMQ, Redis otomatis dibutuhkan dan menyelesaikan dua keputusan sekaligus.
3. **Job Queue/scheduled job belum ada di Official Technology Stack**, padahal beberapa proses wajib bersifat asinkron/terjadwal: regenerasi sitemap event-driven, sinkronisasi counter denormalisasi (`total_listings_sold`, `cta_click_count`), pemanggilan Google Indexing API. Status resmi: **`architecture-decision-records.md` ADR-006 — OPEN**, namun kini condong lebih kuat ke arah **Supabase Edge Functions + cron** (tanpa Redis tambahan) sejak ADR-001 (Backend Architecture) dikunci final ke Route Handlers + Supabase — opsi ini paling konsisten secara arsitektural. Alternatif tetap BullMQ (butuh Redis, lihat poin 2). Perlu pengesahan eksplisit sebelum Sprint S6/S13.
4. **Google Maps Platform dicatat di Bagian 3 sebagai provider Maps** untuk kebutuhan autocomplete alamat, reverse geocoding, dan distance matrix (Modul 3 & 6) — namun ini **bukan keputusan final**. Status resmi: **`architecture-decision-records.md` ADR-008 — OPEN.** Dokumen ini condong ke Google Maps Platform (akurasi data alamat/POI Indonesia matang, satu vendor untuk seluruh kebutuhan), dengan Mapbox sebagai alternatif yang tetap valid secara teknis namun belum dieksplorasi mendalam. **Perlu konfirmasi eksplisit tim bisnis** atas estimasi biaya per-request pada volume listing yang diproyeksikan sebelum ADR-008 dapat naik status menjadi Approved — wajib diselesaikan sebelum Sprint S4/S9.
5. **Package/library wrapper spesifik untuk Google Maps Platform di React** (mis. `@vis.gl/react-google-maps` — wrapper resmi Google — vs `@react-google-maps/api` yang lebih lama) belum ditentukan; bergantung pada resolusi ADR-008 (poin 4) terlebih dahulu — lihat catatan di `dependency-manifest.md` Bagian 9.
6. **Threshold DBR final, model monetisasi, kebijakan eksklusivitas developer per wilayah, kepemilikan akun GTM/GSC/GA4** — seluruhnya tetap berstatus terbuka sesuai `PROJECT-CONSTITUTION.md` (bukan konflik teknologi maupun cakupan ADR arsitektur/teknis — ini keputusan bisnis murni yang memang belum diambil); tidak memengaruhi Official Technology Stack di dokumen ini, namun tetap wajib diimplementasikan sebagai *configurable* (`system_configs`/`dbr_config`).
7. **Owner/penanggung jawab individu dokumen ini** (Bagian 1) belum ditentukan sebagai nama spesifik — saat ini hanya diisi peran (Principal Software Architect/Technical Lead), konsisten dengan gap yang sama di `architecture-decision-records.md` Bagian 1.

> **Catatan (Vercel sebagai hosting resmi):** Keputusan teknologinya sendiri **sudah Approved** — `architecture-decision-records.md` ADR-010 (Deployment Strategy, Approved 27 Juli 2026): Vercel + GitHub + GitHub Actions. Item ini karenanya **dihapus dari daftar Open Questions** (bukan lagi keputusan teknologi yang terbuka). Satu-satunya sisa pekerjaan adalah administratif — ADR-010 sendiri mencatat sebagai *Notes*: belum dibackfill formal ke `PROJECT-CONSTITUTION.md` Bagian 4 — ini tugas sinkronisasi dokumen governance, bukan Open Question teknologi di dokumen ini.

---

*Dokumen ini disusun sebagai referensi teknologi resmi turunan dari `PROJECT-CONSTITUTION.md` v1.1, `architecture-decision-records.md`, dan seluruh dokumen sumber proyek (26–27 Juli 2026). Versi 1.1 (27 Juli 2026) mengintegrasikan resolusi ADR-001 (Backend Architecture, Approved) dan mengoreksi status Maps Provider/Vercel agar konsisten dengan `architecture-decision-records.md` sebagai sumber kebenaran. Berpasangan dengan `dependency-manifest.md`. Wajib direview ulang setiap kali status ADR di `architecture-decision-records.md` berubah, atau setiap kali ada keputusan pada Bagian 9 (Open Questions) yang diselesaikan — perubahan wajib disinkronkan balik ke `PROJECT-CONSTITUTION.md` & `SYSTEM-ARCHITECTURE.md`.*
