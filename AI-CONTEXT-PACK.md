# AI CONTEXT PACK
## Platform Web Real Estate Agency

**Versi:** 1.0
**Tanggal:** 27 Juli 2026
**Fungsi dokumen:** Context tetap (persistent context) untuk seluruh AI Coding Assistant yang bekerja pada proyek ini — Bolt.new, Claude Code, Cursor, GitHub Copilot, ChatGPT, dan sejenisnya. Dokumen ini dirancang untuk **ditempelkan/di-load ulang** di setiap sesi baru agar konsistensi proyek terjaga selama pengembangan berbulan-bulan, tanpa perlu AI membaca ulang seluruh dokumen sumber yang panjang.
**Turunan dari:** `PROJECT-CONSTITUTION.md`, `PRD-Real-Estate-Agency-Platform-v1.1.md`, `ERD-Skema-Database-Real-Estate-Agency-v1.1.md`, `ERD-Diagram-v1.1.mermaid`, `API-Specification-Real-Estate-Agency-Platform-v1.1.md`, `User-Flow-Real-Estate-Agency-Platform-v1.1.md`, `SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md`, dan `AI-DEVELOPMENT-BLUEPRINT.md`.
**Catatan penting:** Dokumen ini adalah **ringkasan operasional**, bukan pengganti dokumen sumber. Jika AI butuh detail teknis mendalam (skema field lengkap, endpoint spesifik, alur UI langkah-demi-langkah), tetap rujuk dokumen sumber di `/docs`. Jika terjadi ketidaksesuaian, dokumen sumber yang menang — lihat "Potential Conflict" di bagian akhir.

---

## 1. PROJECT IDENTITY

**Nama Project:** Platform Web Real Estate Agency (nama brand final belum ditentukan — gunakan placeholder `{nama_platform}` di kode/konten sampai keputusan bisnis turun).

**Tujuan:** Mendigitalisasi operasional agensi properti Indonesia secara end-to-end — mulai dari onboarding & administrasi agen, manajemen listing, edukasi agen, kolaborasi dengan developer properti, hingga alat bantu pre-screening kelayakan KPR (DBR Scoring) — sambil memastikan seluruh halaman publik terindeks mesin pencari secepat dan seakurat mungkin sejak hari pertama rilis.

**Visi:** Menjadi platform operasional utama bagi agensi properti untuk mengelola seluruh siklus kerja agen (rekrutmen mandiri → sertifikasi → listing → closing) dalam satu sistem, sekaligus menjadi kanal akuisisi lead organik (SEO) untuk agennya.

**Nilai Bisnis:**
- Mempercepat & mendigitalkan proses rekrutmen dan administrasi agen (self-service registration).
- Meningkatkan kualitas closing lewat kualifikasi awal calon pembeli (DBR Scoring) sebelum diajukan ke bank.
- Membuka kanal kolaborasi bisnis baru antara agen dan developer properti (katalog proyek, skema komisi).
- Meningkatkan kapabilitas jual agen lewat Learning Center gratis (sertifikasi internal).
- Membangun aset SEO jangka panjang (listing terindeks, profil agen publik) sebagai sumber lead organik berkelanjutan, bukan hanya bergantung pada iklan berbayar.

---

## 2. BUSINESS DOMAIN

**Jenis Platform:** PropTech / Real Estate Agency SaaS — model **B2B2C**. Platform dipakai secara internal oleh agensi & agennya (B2B side), sekaligus dikonsumsi publik oleh calon pembeli/penyewa properti (B2C side).

**Model Transaksi:** Platform **tidak** memproses transaksi jual-beli properti secara langsung dan **bukan** payment gateway untuk closing properti. Nilai jual utama adalah **lead generation** (klik CTA WhatsApp ke agen) dan **tooling operasional** (Learning Center, DBR Scoring, katalog developer). Monetisasi platform (komisi, tier keanggotaan, boost listing berbayar) **belum final** — tidak ada logika pembayaran wajib di MVP.

**Target Pengguna:**
- Agen properti independen/bernaung kantor yang butuh alat kelola listing & profil profesional.
- Tim internal agensi (Superadmin/Manager/Admin/Instructor) yang mengelola operasional platform.
- Developer properti yang ingin memasarkan proyek lewat jaringan agen.
- Calon pembeli/penyewa properti (Guest maupun Buyer terdaftar) yang mencari listing dan ingin dihubungkan ke agen.

**Masalah yang Diselesaikan:**
1. Proses rekrutmen & verifikasi agen yang masih manual/lambat.
2. Listing properti tersebar tidak terstruktur, sulit ditemukan mesin pencari.
3. Minimnya alat kualifikasi awal calon pembeli KPR sebelum diajukan ke bank (potensi penolakan tinggi di tahap lanjut).
4. Kurangnya kanal edukasi/sertifikasi gratis yang terstandardisasi untuk agen.
5. Kolaborasi agen–developer yang tidak tersentral (data proyek/harga sering tidak sinkron).
6. Reputasi agen (rating/review) belum terverifikasi secara transparan ke publik.

**Yurisdiksi & Kepatuhan:** Indonesia — mengacu UU PDP (Perlindungan Data Pribadi), memakai data wilayah administratif resmi Kemendagri/BPS, standar perhitungan DBR/DSR perbankan Indonesia, mata uang IDR.

---

## 3. USER ROLES

Hierarki role internal (dari tertinggi ke terendah): **Superadmin → Manager → Admin → Instructor → Agen**, ditambah role eksternal (Developer Partner, Buyer), dan pengunjung publik tanpa akun (Guest/Lead).

| Role | Ringkasan |
|---|---|
| **Superadmin** | Akses penuh tanpa batas ke seluruh fitur, termasuk konfigurasi sistem inti & keamanan web. Satu-satunya yang boleh mengubah permission Admin/Manager/Superadmin. Tidak dapat dibatasi/dihapus; minimal 1 akun aktif wajib selalu ada. |
| **Manager** | Seluruh fungsi Admin + akses **global** (semua agen, semua listing, semua wilayah — tanpa pengecualian tim/wilayah). Berwenang mengubah permission role Agen saja, dan promote/demote Agen ↔ Admin. Tidak dapat menyentuh konfigurasi sistem inti/keamanan atau permission Admin/Manager/Superadmin. |
| **Admin** | Operasional harian: approval agen, moderasi listing, kelola konten Event/Developer, laporan, moderasi review agen. Tidak dapat mengelola akun Admin/Manager/Superadmin lain atau ubah konfigurasi sistem inti. |
| **Instructor** | Role internal terbatas ke Modul Learning Center saja (kelola kursus, bank soal, pantau progress) — setara Admin hanya di lingkup modul tsb. Tidak punya akses moderasi listing/RBAC/konfigurasi sistem. |
| **Agen (Independent Agent)** | Kelola profil & listing **miliknya sendiri**, ikut pelatihan, pakai kalkulator DBR, klaim proyek developer. Tidak pernah bisa melihat/edit/hapus data milik agen lain — ini hard rule aplikasi, bukan permission yang bisa dilonggarkan. |
| **Developer Partner** | Eksternal, akses terbatas ke portal pengajuan proyek/event miliknya sendiri. Login opsional. Event yang diajukan wajib approval sebelum tayang. |
| **Buyer** | Akun terdaftar ringan (opsional) untuk simpan listing favorit, tracking lead pribadi, dan submit review/rating agen (berstatus `pending` sampai dimoderasi). Bukan prasyarat untuk melihat listing atau menghubungi agen. |
| **Guest/Lead** | Publik tanpa akun — melihat listing publik, klik CTA WhatsApp, submit form inquiry tanpa perlu akun sama sekali. |

**Hard rule lintas role (tidak dapat dilonggarkan oleh konfigurasi permission apa pun):**
- Agen tidak pernah dapat mengedit/menghapus listing atau profil agen lain.
- Superadmin selalu bypass pengecekan permission.
- Sistem wajib mencegah penghapusan/downgrade akun Superadmin terakhir yang aktif.
- Manager hanya boleh mengubah baris permission milik role Agen.
- Setiap perubahan role/permission wajib tercatat di audit log.

---

## 4. CORE MODULES

| Modul | Ringkasan Fungsi | Fase |
|---|---|---|
| **Authentication** | Registrasi mandiri (email/HP + OTP), login (password/Google OAuth), reset password, manajemen sesi multi-device, upload dokumen legalitas agen, approval manual sebelum agen dapat posting listing. | 1 |
| **Profil Agen** | Halaman publik & privat sebagai "kartu nama digital" — bio, spesialisasi, area jangkauan, statistik listing terjual/tersewa, badge sertifikasi, review/rating dari Buyer (moderasi wajib), link share publik. | 1 |
| **Listing** | Inti transaksi platform — CRUD listing per-agen, kategori Primary/Secondary, tujuan Jual/Sewa, form spesifikasi lengkap, CTA WhatsApp, pencarian & filter publik, mode List/Peta, lifecycle status (`draft` → `pending_review` → `published` → `sold`/`rented`/`expired`). | 1 |
| **Learning** (Learning Center) | Katalog kursus gratis, self-enroll, konten video/PDF/kuis, sertifikat digital otomatis setelah lulus, progress tracking, jadwal kelas live. | 3 |
| **Event** (Kalender Event) | Event training/launching proyek/open house/gathering, RSVP agen, pengajuan event oleh Developer Partner (butuh approval). | 3 |
| **Developer** (Direktori Kerjasama Developer) | Katalog proyek developer, klaim proyek oleh agen untuk dipasarkan, sinkronisasi harga/unit real-time ke listing turunan. | 2 |
| **DBR** (Sistem Scoring DBR/KPR) | Kalkulator kelayakan KPR calon pembeli (formula anuitas + rasio DBR), simulasi what-if, export PDF, riwayat simulasi per agen, threshold dikonfigurasi Superadmin. | 2 |
| **Dashboard** | Ringkasan lintas modul per role — agen (listing/lead/progress kursus/DBR miliknya), admin (statistik operasional global). Tidak menyimpan data sendiri, hanya agregasi. | Lintas fase |
| **Notification** | Notifikasi in-app/email/(push opsional) untuk status approval, listing akan expired, sertifikat baru, reminder event, lead baru — selalu personal per user. | Lintas fase |
| **RBAC** (Manajemen Role & Hak Akses) | Pusat pengaturan multirole — Permission Matrix Editor (Superadmin), Permission Editor terbatas (Manager, khusus role Agen), Assign Role, audit trail. Fondasi wajib bagi semua modul lain. | 1 (dasar) |
| **Admin CMS** | Manajemen user, moderasi listing & konten, kelola Learning Center, kelola Developer & Proyek, konfigurasi parameter sistem, laporan & analitik. | 1 (dasar) |
| **SEO & Analytics** | Strategi rendering SSR/SSG/ISR, slug & structured data, sitemap otomatis, GTM/GA4, Google Search Console & Indexing API — fondasi wajib sejak Fase 1, bukan ditambal belakangan. | 1 (fondasi) |

**Roadmap Fase:** Fase 1 (Authentication, Profil Agen, Listing-dasar, Admin CMS-dasar, RBAC-dasar, fondasi SEO) → Fase 2 (Developer, DBR) → Fase 3 (Learning, Event) → Fase 4 (dashboard analitik lanjutan, gamifikasi, integrasi SLIK/BI Checking, payment/komisi otomatis).

---

## 5. TECH STACK

> Stack berikut adalah **satu-satunya** stack yang boleh dipakai — diambil langsung dari `PROJECT-CONSTITUTION.md` Bagian 4. **Dilarang** menambahkan library/framework/database lain di luar daftar ini tanpa persetujuan eksplisit.

| Layer | Teknologi Wajib |
|---|---|
| Frontend Framework | **Next.js** (App Router, versi stabil terbaru) |
| Bahasa | **TypeScript** (`strict: true` di seluruh layer, tanpa `any` implisit) |
| Styling | **Tailwind CSS** + komponen headless (**shadcn/ui** atau setara) |
| Backend/API | **Node.js (TypeScript)** — via Next.js Route Handlers (BFF ringan) **atau** service backend terpisah (NestJS/Express TypeScript). *Pilih salah satu secara eksplisit, tidak boleh dicampur untuk domain yang sama.* |
| Database | **PostgreSQL** (di-host via **Supabase**) |
| Auth Provider | **Supabase Auth** (email/password, OTP, Google OAuth2), dibungkus JWT internal platform |
| Search Engine | **Typesense** (rekomendasi) atau Elasticsearch |
| Storage/CDN | **Supabase Storage** / Cloudinary / ImageKit (bucket publik & privat terpisah) |
| Cache & Rate Limit | **Redis** |
| Job Queue | Redis-based queue (**BullMQ**) atau **Supabase Edge Functions** + cron |
| Validasi Data | **Zod** (satu sumber skema, dipakai client & server) |
| Maps/Geocoding | Google Maps Platform atau Mapbox *(belum final — implementasikan provider-agnostic)* |
| Analytics | GTM + GA4 (konfigurasi via `system_configs`, dikelola Superadmin) |
| CI/CD | Git-based pipeline (GitHub Actions/GitLab CI) — lint, type-check, test, migration check wajib lolos |

**Larangan eksplisit:** jangan menambahkan ORM auto-sync untuk production migration, jangan mengganti PostgreSQL/Supabase dengan database lain, jangan menambahkan state management library (Redux/Zustand, dsb.) tanpa justifikasi kompleksitas yang jelas.

---

## 6. DATABASE SUMMARY

> Ringkasan tabel inti & relasi sederhana. **ERD lengkap (37+ entitas) tidak ditampilkan di sini** — rujuk `ERD-Skema-Database-Real-Estate-Agency-v1.1.md` dan `ERD-Diagram-v1.1.mermaid` untuk detail field, constraint, dan index.

**Tabel Inti (per domain):**

| Domain | Tabel Inti | Relasi Sederhana |
|---|---|---|
| Identitas & RBAC | `users`, `roles`, `permissions`, `role_permissions` | `users.role_id → roles.id`; `role_permissions` menghubungkan `roles` ↔ `permissions` |
| Verifikasi Agen | `agent_verification_documents` | `→ users.id` |
| Profil Agen | `agent_profiles`, `agent_reviews` | `agent_profiles.user_id → users.id` (1:1); `agent_reviews.agent_id/buyer_id → users.id` |
| Listing | `listings`, `listing_photos`, `listing_videos`, `listing_leads`, `listing_price_history` | Semua anak `→ listings.id`; `listings.agent_id → users.id`; `listings.developer_project_id → developer_projects.id` (nullable) |
| Wilayah | `ref_provinces`, `ref_cities`, `ref_districts`, `ref_villages` | Cascading: `ref_cities.province_id → ref_provinces`, `ref_districts.city_id → ref_cities`, `ref_villages.district_id → ref_districts`; dipakai `listings` & `developer_projects` |
| Developer | `developer_partners`, `developer_projects`, `developer_project_media`, `agent_project_claims` | `developer_projects.developer_id → developer_partners.id`; `developer_projects.city_id → ref_cities.id`; `agent_project_claims` = pivot agen ↔ proyek |
| Learning Center | `courses`, `course_lessons`, `quizzes`, `quiz_questions`, `enrollments`, `certificates` | `enrollments`/`certificates` = pivot `users` (agen) ↔ `courses` |
| Event | `events`, `event_registrations` | `event_registrations` = pivot `events` ↔ `users` (agen) |
| DBR Scoring | `dbr_simulations`, `dbr_config` | `dbr_simulations.agent_id → users.id`; `.listing_id → listings.id` (nullable); parameter global di `dbr_config` |
| Dashboard/Notifikasi | `notifications` | `→ users.id` |
| Admin/Sistem | `system_configs`, `audit_logs` | Konfigurasi & jejak audit lintas modul |
| SEO | `url_redirects` | `entity_type`/`entity_id` merujuk fleksibel ke `listings`/`developer_projects` |

**Prinsip desain data yang wajib diingat AI:**
- PK selalu UUID, bukan auto-increment.
- Soft delete (`deleted_at`) untuk `listings`, `users`, `developer_projects` — tidak ada `DELETE` fisik.
- `agent_id` adalah **ownership boundary** hard-coded, bukan sekadar permission.
- Field lokasi selalu cascading (`province_id → city_id → district_id`), bukan freetext (kecuali `area_keyword`, maks 20 karakter).
- Counter agregat (`cta_click_count`, `total_listings_sold/rented`) didenormalisasi via trigger/job, bukan `COUNT()` on-the-fly.

---

## 7. PROJECT PRINCIPLES

1. **Single Source of Truth** — satu definisi tipe data (`packages/shared-types`), satu skema validasi (Zod), satu dokumen ERD/API Spec sebagai rujukan; tidak boleh ada definisi ganda yang berpotensi berbeda.
2. **Ownership sebagai Hard Boundary** — `agent_id` adalah batas kepemilikan yang tidak bisa dilewati permission apa pun; desain apa pun berangkat dari asumsi ini dulu, baru permission tambahan di atasnya.
3. **Reusable Components** — komponen UI dasar dan pola service/repository dipakai ulang lintas modul, bukan ditulis ulang per fitur.
4. **Modular Development** — setiap modul (Auth, Listing, RBAC, dst.) punya batas tanggung jawab jelas dan dependency yang eksplisit (lihat Bagian 4).
5. **SEO-First, bukan SEO-Afterthought** — setiap halaman publik baru dicek terhadap checklist SEO **sebelum** dianggap selesai.
6. **Konfigurasi di atas Hard-code** — parameter yang bisa berubah karena keputusan bisnis (threshold DBR, masa expired listing, passing grade) selalu configurable lewat Admin Panel.
7. **Progressive Disclosure untuk RBAC** — role dengan akses lebih sempit tidak melihat menu/opsi yang tidak relevan, disembunyikan penuh, bukan sekadar disabled.
8. **Data Lokasi Terstruktur** — field lokasi baru mengikuti pola cascading wilayah yang sudah ada.
9. **Scalable & Maintainable** — struktur folder, penamaan, dan pola kode konsisten agar mudah di-scale dan dipelihara AI/human di masa depan.
10. **Security First** — enkripsi data sensitif, RBAC berlapis (middleware + RLS), tidak ada trust terhadap input client.
11. **Mobile-First & PWA-Aware** — agen bekerja di lapangan; form listing, kalkulator DBR, dashboard harus nyaman di layar kecil/koneksi tidak stabil.
12. **Graceful Degradation Komunikasi** — CTA WhatsApp adalah jalur utama Buyer↔Agen di MVP; tidak ada asumsi chat in-app sudah tersedia.

---

## 8. UI PRINCIPLES

- **Rendering sesuai tipe halaman**: halaman publik (homepage, search, detail listing, profil agen, proyek developer) wajib SSR/SSG/ISR; halaman privat (dashboard, admin, hasil kalkulator DBR) CSR + `noindex, nofollow`.
- **Satu `<h1>` per halaman publik**, breadcrumb di setiap halaman detail (`Beranda > Kota > Tipe Properti > Judul`).
- **Komponen dasar (`ui/`) vs komponen fitur (`features/{module}/`) dipisah tegas** — komponen dasar tidak mengandung business logic.
- **Gambar** wajib lewat komponen image Next.js dengan `width`/`height`/`aspect-ratio` reserved (mencegah CLS) dan lazy-load di luar viewport awal.
- **Form panjang** (listing, DBR) mendukung multi-step dan idealnya autosave/draft agar tidak kehilangan input di lapangan.
- **Filter pencarian** memakai debounce (target INP < 200ms) dan disimpan sebagai URL query yang dapat dibagikan.
- **State loading/empty/error** wajib tersedia untuk setiap komponen data-fetch — minimal 4 state: loading, empty, error, success.
- **Konsistensi visual**: satu design system (Tailwind + shadcn/ui), tidak membuat sistem styling paralel.
- **Aksesibilitas dasar**: `alt_text` wajib untuk gambar listing (auto-generate jika kosong), label form jelas dalam Bahasa Indonesia.

---

## 9. CODING PRINCIPLES

- **TypeScript wajib** di seluruh layer, `strict: true`, tanpa `any` implisit.
- **Functional component + hooks saja** — tidak ada class component baru.
- **Business logic terpisah dari UI** — kalkulasi (formula DBR, validasi ownership) selalu di `/lib` atau service layer backend, dapat di-unit-test tanpa render komponen.
- **Satu tanggung jawab per file/modul** — modul backend mengikuti struktur konsisten: `*.controller.ts`, `*.service.ts`, `*.repository.ts`, `*.schema.ts` (Zod), `*.types.ts`.
- **Tidak ada magic number/string** — threshold, expiry, passing grade dibaca dari `system_configs`/`dbr_config`.
- **Linting/formatting seragam** (ESLint + Prettier), pre-commit hook, dan CI gate wajib.
- **Naming konsisten**: `snake_case` untuk DB/JSON API, `camelCase` untuk variabel/fungsi TS, `PascalCase` untuk tipe/komponen, `kebab-case` untuk endpoint REST.
- **Komentar wajib** untuk setiap implementasi hard rule keamanan/RBAC, agar tidak terhapus tidak sengaja saat refactor.
- **Konsistensi FE↔BE↔DB** — field yang sama memakai nama identik di ketiga layer; konversi `camelCase` hanya terjadi di dalam kode TypeScript via mapper/DTO, tidak "bocor" ke response API.

---

## 10. SECURITY PRINCIPLES

1. **Enkripsi data sensitif at-rest** — dokumen legalitas agen (KTP/NPWP) dan field finansial DBR (`net_income`, `existing_installments`) — non-negotiable.
2. **RBAC berlapis** — middleware backend sebagai lapisan pertama, RLS Supabase sebagai lapisan kedua; tidak pernah hanya mengandalkan satu lapisan.
3. **Tidak ada trust terhadap input client** — seluruh validasi bisnis (region, OAuth token, ownership) diulang di server meski sudah divalidasi di client.
4. **Signed URL berumur pendek** untuk dokumen privat — tidak ada URL publik permanen untuk KTP/NPWP.
5. **API key pihak ketiga dipisah** — client-key (dibatasi domain/referrer) vs server-key (rahasia, quota penuh), khususnya Maps & Indexing API.
6. **Rate limiting wajib** khususnya endpoint auth/OTP untuk mencegah brute-force.
7. **Audit trail tidak dapat dihapus** oleh siapa pun kecuali proses retensi resmi terjadwal.
8. **Minimal 1 akun Superadmin aktif** dijamin di level aplikasi, bukan hanya kebijakan dokumentasi.
9. **PII tidak masuk ke Analytics/log teknis** — hanya event & parameter agregat yang dikirim ke GA4/GTM.
10. **Cookie consent + Google Consent Mode** wajib aktif sebelum tracking non-esensial berjalan penuh.
11. **Data privat milik user lain disamarkan sebagai 404**, bukan 403, untuk mencegah enumerasi resource.
12. **Setiap fitur user-generated content baru** (di luar `agent_reviews` yang sudah ada) wajib melalui review keamanan eksplisit sebelum dikembangkan.

---

## 11. AI DEVELOPMENT RULES

> Bagian ini adalah bagian **terpenting** dari dokumen ini. Aturan berikut **wajib dipatuhi** oleh AI Coding Assistant apa pun yang bekerja pada proyek ini, tanpa pengecualian.

1. **Jangan mengubah struktur database tanpa alasan** yang jelas dan disetujui — setiap perubahan skema wajib lewat migration file yang direview.
2. **Jangan mengganti nama tabel** yang sudah ada.
3. **Jangan mengganti nama field** yang sudah dipakai FE/BE/DB.
4. **Jangan membuat tabel baru jika kebutuhan sudah tersedia** — cek dokumentasi ERD dulu sebelum menambah entitas.
5. **Selalu gunakan komponen yang sudah ada** (`components/ui/`) sebelum membuat komponen baru yang fungsinya serupa.
6. **Jangan menduplikasi kode** — logic bisnis, skema validasi, dan tipe data masing-masing punya satu lokasi sumber kebenaran.
7. **Selalu gunakan TypeScript** dengan `strict: true`, tanpa `any` implisit.
8. **Selalu gunakan Supabase** sebagai provider database/auth/storage — tidak beralih ke provider lain tanpa keputusan arsitektur eksplisit.
9. **Selalu menjaga backward compatibility** — perubahan pada endpoint yang sudah live (`/v1`) tidak boleh breaking; breaking change wajib naik versi.
10. **Jangan membuat dependency baru tanpa alasan** — cek Bagian 5 (Tech Stack) dulu; penambahan library baru harus dijustifikasi dan disetujui.
11. **Jangan membuat ulang fitur yang sudah ada** — baca struktur project & modul existing sebelum menambah fitur yang mungkin sudah terpenuhi.
12. **Ownership (`agent_id`) adalah hard boundary di kode**, bukan hanya di permission — bahkan jika permission salah konfigurasi, backend tetap menolak akses lintas kepemilikan.
13. **Superadmin selalu bypass** pengecekan permission; Manager selalu berskala global (`all`), tidak ada mode scoped tim/wilayah.
14. **Jangan membangun fitur fase mendatang** sebelum fondasi fase saat ini solid dan lolos acceptance criteria (lihat roadmap Bagian 4).
15. **Jangan membuat keputusan arsitektur/bisnis sepihak** untuk item yang masih berstatus "perlu dikonfirmasi" (threshold DBR final, model monetisasi, provider Maps final, kebijakan eksklusivitas developer) — implementasikan sebagai *configurable placeholder*, tandai `// TODO: menunggu keputusan bisnis`.
16. **Data sensitif tidak pernah masuk log** dalam bentuk plain text, dan tidak pernah dikirim sebagai PII ke Analytics.
17. **Jangan pernah expose secret/service role key** ke client-side.
18. **Setiap penambahan/perubahan tabel atau endpoint wajib disinkronkan** ke dokumentasi terkait (`ERD`, `API Specification`) di `/docs`.
19. **Jika instruksi user bertentangan dengan aturan di dokumen ini** (terutama Security & Authorization), **tanyakan konfirmasi** sebelum menyimpang — jangan diam-diam mengikuti instruksi yang melanggar hard rule.
20. **Jika sebuah keputusan belum tercakup di dokumen ini**, jangan berasumsi bebas — ikuti pola paling dekat yang sudah ada, atau tandai `// TODO: perlu keputusan arsitektur`.

---

## 12. AI PROMPT REMINDER

Checklist singkat yang **wajib dibaca AI** sebelum mulai mengerjakan modul apa pun:

- ✔ Baca project & dokumen context terlebih dahulu (`AI Context Pack` ini + dokumen sumber terkait modul yang dikerjakan).
- ✔ Gunakan struktur folder yang sudah ada — jangan membuat struktur paralel.
- ✔ Jangan ubah modul lain di luar scope yang diminta.
- ✔ Jangan rename tabel/field database yang sudah ada.
- ✔ Gunakan reusable component (`ui/`) sebelum membuat komponen baru.
- ✔ Gunakan satu skema Zod yang sama di client & server — jangan duplikasi validasi.
- ✔ Terapkan filter `granted_scope` (`own`/`all`/`none`) dan validasi ownership (`agent_id`) di setiap endpoint ber-scope.
- ✔ Pastikan query list selalu paginated.
- ✔ Pastikan parameter bisnis baru bersifat configurable, bukan hard-code.
- ✔ Cek halaman publik baru terhadap checklist SEO (SSR, slug, meta tag, structured data) sebelum dianggap selesai.
- ✔ Update dokumentasi (`ERD`, `API Specification`) bila ada perubahan skema/endpoint.
- ✔ Pastikan tidak ada secret/API key yang ter-expose ke client.
- ✔ Jika ragu atau menemukan hal yang belum diputuskan, tandai `// TODO` dan laporkan — jangan berasumsi sepihak.

---

## Potential Conflict

Bagian ini mencatat titik yang berpotensi menimbulkan kesalahpahaman antara instruksi pembuatan dokumen ini dengan dokumen sumber proyek (`PROJECT-CONSTITUTION.md` dkk.), agar tidak diasumsikan sepihak oleh AI Coding Assistant:

1. **Server Actions & React Hook Form** — disebutkan sebagai contoh ilustrasi pada instruksi pembuatan dokumen ini, namun **tidak tercantum** sebagai stack wajib di `PROJECT-CONSTITUTION.md`. Bagian 5 (Tech Stack) di dokumen ini **sengaja tidak menyertakan** keduanya agar konsisten dengan instruksi "jangan menambahkan teknologi yang tidak ada pada dokumen sumber". Jika tim memang ingin memakai Server Actions (sebagai pengganti/pelengkap Route Handlers) atau React Hook Form (sebagai library form), ini perlu menjadi **keputusan arsitektur eksplisit** yang ditambahkan ke Constitution terlebih dahulu.
2. **Pilihan arsitektur Backend/API** — Constitution belum mengunci antara "Next.js Route Handlers (BFF ringan)" vs "service backend terpisah (NestJS/Express)". Dokumen sumber mewajibkan tim memilih salah satu secara eksplisit sebelum Fase 1 selesai. Dokumen ini menampilkan keduanya sebagai opsi (Bagian 5) sampai keputusan final diambil.
3. **Provider Maps/Geocoding** — belum final antara Google Maps Platform vs Mapbox (masih berstatus "Hal Perlu Dikonfirmasi" di dokumen sumber). Implementasi wajib provider-agnostic sampai keputusan turun.
4. **Model monetisasi** — belum diputuskan (komisi transaksi, tier keanggotaan, atau boost listing berbayar). Tidak ada logika pembayaran wajib di MVP; endpoint billing hanya placeholder non-breaking.
5. **Threshold DBR final & kebijakan per bank rekanan** — masih memerlukan input tim bisnis/legal; wajib configurable, bukan hard-code.
6. **Kebijakan eksklusivitas proyek developer per wilayah/agen** — belum diputuskan; jangan diimplementasikan sebagai batasan permanen tanpa konfirmasi.
7. **Kepemilikan akun organisasi Google Search Console/GTM/GA4** — belum ditentukan tim operasional; tidak memblokir development, namun perlu diselesaikan sebelum go-live.
8. **Kebijakan promosi/demosi role** (mis. apakah Manager dapat mempromosikan Agen langsung menjadi Manager) — belum dikonfirmasi; sesuai hard rule saat ini, Manager hanya bisa promote/demote antara Agen ↔ Admin.

---

*Dokumen ini adalah context pack ringkas turunan dari `PROJECT-CONSTITUTION.md` dan dokumen sumber v1.1 (26 Juli 2026), serta `AI-DEVELOPMENT-BLUEPRINT.md`. Gunakan sebagai referensi cepat di setiap sesi kerja AI Coding Assistant; untuk detail teknis mendalam, rujuk dokumen sumber lengkap di `/docs`.*
