# DOCUMENT GOVERNANCE & BASELINE REGISTER
## Platform Web Real Estate Agency

---

# 1. Document Information

| Field | Value |
|---|---|
| **Name** | Document Governance & Baseline Register — Platform Web Real Estate Agency |
| **Version** | 1.0 |
| **Status** | Draft — menunggu review & pengesahan tim (konsisten dengan status dokumen governance turunan lain seperti `technology-decisions.md`, `DEVELOPMENT-ROADMAP.md`, `AI-DEVELOPMENT-BLUEPRINT.md`; belum berstatus "BERLAKU" seperti `PROJECT-CONSTITUTION.md`) |
| **Effective Date** | Menyusul, setelah Approval (lihat Bagian 9) |
| **Last Updated** | 29 Juli 2026 — sinkronisasi penambahan resolusi `architecture-decision-records.md` `ADR-006` (Job Queue Strategy, Approved) ke Baseline Register, Version Matrix, Review Schedule, Source of Truth Matrix, Document Status, dan Dependency Matrix; versi terbaru dokumen turunan (`technology-decisions.md` v1.3, `SYSTEM-ARCHITECTURE.md` v1.3, `dependency-manifest.md` v1.3, `PROJECT-CONSTITUTION.md` v1.4, `development-playbook.md`/`AI-DEVELOPMENT-BLUEPRINT.md` v1.3, `CHANGELOG.md` 0.1.3, `decision-log.md` entry `ADR-040`) direfleksikan penuh di Bagian 10. Nomor versi dokumen ini **tidak naik** (masih Draft — lihat Bagian 5, "nomor versi hanya naik saat memasuki status Baseline"). |
| **Owner** | Software Configuration Manager / Technical Documentation Architect (nama individu penanggung jawab belum ditetapkan — konsisten dengan catatan terbuka yang sama di `technology-decisions.md` Bagian 1 dan `decision-log.md` Bagian 1) |
| **Purpose** | Menjadi **satu-satunya referensi resmi** mengenai status lifecycle, versi, baseline, kepemilikan (ownership), jadwal review/approval, hubungan source-of-truth, dan dependency antar seluruh dokumen proyek — agar seluruh dokumen dikelola secara konsisten sepanjang umur proyek (desain → development → testing → deployment → maintenance) oleh kontributor manusia maupun AI Coding Assistant. |
| **Related Documents** | Seluruh **19** dokumen proyek yang terdaftar di Bagian 10 (Baseline Register) — bertambah dari 18 dengan masuknya `architecture-decision-records.md` |
| **Does Not Replace** | `decision-log.md` (mencatat **mengapa** sebuah keputusan diambil) dan `CHANGELOG.md` (mencatat **riwayat** perubahan yang sudah terjadi). Dokumen ini murni mengatur **status, versi, dan lifecycle** setiap dokumen — bukan isi keputusan atau riwayat perubahan itu sendiri. |

> **Kedudukan dokumen dalam hierarki governance:** Dokumen ini adalah **meta-dokumen** — ia mengatur *cara mengelola dokumen lain*, bukan mengambil keputusan arsitektur/bisnis baru. Jika terjadi ketidaksesuaian antara dokumen ini dan `PROJECT-CONSTITUTION.md`, Constitution yang berlaku (lihat Bagian 7 — Source of Truth Matrix). Dokumen ini boleh dijadikan rujukan status oleh dokumen governance lain, tetapi tidak menggantikan otoritas isi dokumen mana pun.

---

# 2. Documentation Principles

Prinsip berikut mengikat seluruh dokumen proyek — baik yang sudah ada maupun dokumen baru yang akan dibuat di fase mendatang (Functional Specification, UI Specification, Technical Specification, dsb.):

| Prinsip | Definisi | Penerapan di Proyek Ini |
|---|---|---|
| **Single Source of Truth (SSoT)** | Setiap jenis informasi (kebutuhan bisnis, skema data, kontrak API, dsb.) hanya punya **satu** dokumen yang berwenang menjadi rujukan akhir. | Lihat Bagian 7 — Source of Truth Matrix. Dokumen lain boleh **mengutip/meringkas**, tidak boleh **mendefinisikan ulang** secara independen. |
| **One Responsibility per Document** | Setiap dokumen punya satu tanggung jawab utama yang jelas, tidak tumpang tindih fungsi dengan dokumen lain. | Mis. `technology-decisions.md` menjelaskan **mengapa** teknologi dipilih; `dependency-manifest.md` mendaftar **package apa** yang boleh dipakai — keduanya tidak saling menduplikasi isi. |
| **Traceability** | Setiap keputusan/isi dokumen dapat ditelusuri asalnya — dokumen sumber mana yang melahirkannya, dan dokumen turunan mana yang menerapkannya. | Hampir seluruh dokumen proyek sudah mencantumkan tabel "Dokumen Sumber"/"Related Documents" — praktik ini wajib dipertahankan untuk dokumen baru. |
| **Consistency** | Istilah, penomoran role/modul, dan status yang sama harus konsisten lintas dokumen — tidak ada dua dokumen "final" yang saling bertentangan tanpa tercatat sebagai Open Decision. | Ditegakkan lewat Bagian 8 (Dependency Matrix) dan Bagian 11 (Change Management) — perubahan pada satu dokumen wajib memicu pengecekan dampak ke dokumen dependen. |
| **Version Control** | Setiap dokumen memiliki nomor versi eksplisit mengikuti Semantic Versioning (Bagian 5), bukan tanggal saja. | Diterapkan penuh; lihat Bagian 10 untuk versi aktual tiap dokumen saat ini. |
| **Change Control** | Perubahan pada dokumen yang sudah **Baseline** tidak boleh dilakukan diam-diam — wajib melalui prosedur resmi (Bagian 11). | Konsisten dengan `decision-log.md` Bagian 8 (Decision Review Process) yang sudah menetapkan AI tidak berwenang melompati tahap Approval. |
| **AI Readability** | Dokumen ditulis dengan struktur eksplisit (tabel, heading bernomor, status jelas) agar dapat diparse dan diikuti AI Coding Assistant tanpa ambiguitas. | Seluruh dokumen proyek sudah mengikuti pola ini; dokumen ini sendiri disusun dengan pola yang sama. |
| **Human Readability** | Dokumen tetap dapat dibaca dan diaudit manusia non-teknis (Product Manager, QA, stakeholder bisnis) tanpa memerlukan AI sebagai perantara. | Bahasa Indonesia naratif dipertahankan untuk konteks/alasan; istilah teknis industri (Baseline, Draft, SemVer, dsb.) dipakai apa adanya tanpa terjemahan paksa. |

---

# 3. Documentation Lifecycle

Lifecycle resmi yang berlaku untuk **seluruh** dokumen proyek, dari dokumen governance hingga dokumen turunan modul (Functional/UI/Technical Specification bila dibuat di kemudian hari):

```
Planned
   ↓
Draft
   ↓
In Review
   ↓
Approved
   ↓
Baseline
   ↓
Revision Requested
   ↓
Updated
   ↓
Review
   ↓
Baseline (versi baru)
   ↓
Archived
```

**Arti setiap status:**

| Status | Arti | Boleh Dijadikan Rujukan Implementasi? |
|---|---|---|
| **Planned** | Dokumen baru sudah diidentifikasi kebutuhannya (mis. karena ditandai gap oleh dokumen lain) namun penulisannya belum dimulai. Tidak ada isi. | Tidak — belum ada konten untuk dirujuk. |
| **Draft** | Penulisan sedang berlangsung atau baru selesai draf pertama. Isi dapat berubah signifikan. Belum melalui review formal. | Tidak, kecuali sebagai *configurable placeholder* dengan penanda `// TODO` — tidak pernah sebagai keputusan final. |
| **In Review** | Draf sudah lengkap dan sedang direview (oleh Reviewer yang ditunjuk — Bagian 9). Perubahan pada tahap ini bersifat hasil review, bukan penulisan baru. | Tidak — masih dapat berubah akibat hasil review. |
| **Approved** | Isi dokumen sudah disetujui secara arsitektural/bisnis oleh Approver yang berwenang. Mengikat untuk implementasi berikutnya, namun **belum tentu** sudah menjadi versi definitif yang dikunci (lihat perbedaan dengan Baseline di Bagian 4). | Ya — mengikat, tetapi masih dapat direvisi tanpa proses Baseline formal jika revisi minor. |
| **Baseline** | Versi dokumen **dikunci** sebagai titik acuan resmi — tidak boleh diubah langsung; setiap perubahan wajib melalui prosedur Bagian 11 dan menghasilkan versi Baseline baru. | Ya — ini adalah versi **paling otoritatif** yang boleh dirujuk AI Coding Assistant maupun developer manusia (lihat Bagian 13). |
| **Revision Requested** | Sebuah Baseline yang sudah ada menerima permintaan perubahan resmi (lewat Change Request/ADR baru di `decision-log.md`). Versi Baseline lama **tetap berlaku** sampai versi baru selesai proses ini. | Ya (versi Baseline lama yang masih berlaku) — bukan revisi yang sedang berjalan. |
| **Updated** | Perubahan sudah diterapkan ke isi dokumen berdasarkan Revision Requested, menghasilkan draf revisi baru. | Tidak — sama seperti Draft, harus melalui Review lagi. |
| **Review** (siklus ulang) | Draf revisi (Updated) direview ulang sebelum disetujui menjadi Baseline versi baru. | Tidak. |
| **Baseline (versi baru)** | Versi revisi disetujui dan dikunci sebagai Baseline baru, menggantikan Baseline sebelumnya. Baseline lama diarsipkan (bukan dihapus). | Ya — menggantikan Baseline versi sebelumnya sebagai rujukan aktif. |
| **Deprecated** | Dokumen/bagian dokumen masih ada tetapi tidak lagi direkomendasikan sebagai rujukan aktif — biasanya karena digantikan versi baru atau karena keputusan yang dicatatnya sudah usang. Dipertahankan sebagai sejarah. | Tidak — hanya untuk konteks historis. |
| **Archived** | Dokumen sepenuhnya keluar dari siklus rujukan aktif (mis. versi Blueprint Session 3 yang digantikan versi upload di Session 5 — lihat `CHANGELOG.md` Session 4–5). Disimpan untuk audit trail, tidak pernah dihapus. | Tidak. |

> **Catatan konsistensi dengan `decision-log.md`:** Status *Approved*, *Implemented*, *Deprecated*, *Replaced* di `decision-log.md` Bagian 3 mengacu pada **entri keputusan (ADR)**, bukan status **dokumen** secara keseluruhan. Kedua sistem status ini melengkapi, bukan menggantikan satu sama lain — satu ADR bisa berstatus *Approved* di dalam sebuah dokumen yang status keseluruhannya masih *Draft* (lihat Governance Notes Bagian 15, poin 1).

---

# 4. Baseline Rules

## 4.1 Kapan Dokumen Boleh Menjadi Baseline

Sebuah dokumen **hanya** boleh naik status menjadi **Baseline** jika **seluruh** syarat berikut terpenuhi:

1. **Seluruh dependency-nya sudah Baseline atau tidak memiliki dependency yang memblokir.** Rujuk Bagian 8 (Document Dependency Matrix) — dokumen tidak boleh di-baseline mendahului dokumen yang menjadi prasyaratnya (mis. `ERD-Skema-Database` tidak boleh Baseline jika `technology-decisions.md` yang menjadi dasarnya masih Draft dan isinya berdampak langsung ke skema).
2. **Sudah melalui tahap Review formal** oleh Reviewer yang ditunjuk (Bagian 9) — bukan sekadar ditulis oleh satu peran/AI tanpa validasi silang.
3. **Tidak ada Open Decision yang secara langsung memengaruhi isi dokumen tersebut** (rujuk `decision-log.md` Bagian 11 — Open Decisions, dan `CHANGELOG.md` — Known Issues). Jika ada Open Decision yang relevan, dokumen dapat tetap naik ke Baseline **hanya** jika bagian yang terdampak sudah diimplementasikan sebagai *configurable placeholder* yang eksplisit ditandai, bukan diasumsikan selesai.
4. **Sudah disetujui (Approved)** oleh Approver yang berwenang sesuai Bagian 9.
5. **Tidak bertentangan dengan dokumen berhierarki lebih tinggi** yang sudah berstatus Baseline (rujuk Bagian 7 — Source of Truth Matrix). Jika ditemukan pertentangan, dokumen tidak boleh di-baseline sampai pertentangan tercatat sebagai ADR di `decision-log.md` dan diselesaikan.

## 4.2 Kapan Baseline Boleh Diubah Kembali

Sebuah dokumen yang sudah berstatus **Baseline tidak boleh diedit langsung**. Perubahan hanya sah melalui jalur berikut:

1. **Perubahan minor/editorial** (typo, perbaikan redaksi yang tidak mengubah keputusan/kontrak) — boleh dilakukan sebagai **PATCH** (Bagian 5) tanpa perlu ADR baru, namun tetap wajib dicatat di `CHANGELOG.md` dan tidak mengubah status Baseline (versi PATCH tetap Baseline, hanya nomor versi yang naik).
2. **Perubahan substantif** (menambah/mengubah keputusan, kontrak data/API, ruang lingkup) — wajib melalui **Change Management Rules penuh** (Bagian 11): Request → Decision Log (ADR baru) → Impact Analysis → Update Document → Review → Approval → **New Baseline**. Baseline lama tidak dihapus, melainkan diberi status **Deprecated**, lalu **Archived** setelah Baseline baru resmi berlaku.
3. **Tidak ada pengecualian** untuk AI Coding Assistant — AI dapat mengusulkan (Proposed) dan membantu Impact Analysis, tetapi **tidak berwenang** mengubah dokumen berstatus Baseline atas inisiatif sendiri, konsisten dengan `decision-log.md` Bagian 8 dan Bagian 9 poin 3.
4. **Baseline yang saling bertentangan tidak boleh berdampingan** — jika sebuah revisi menghasilkan Baseline baru, Baseline lama **wajib** segera diberi status Deprecated pada hari yang sama, untuk mencegah dua "versi final" yang aktif bersamaan (ini adalah akar dari beberapa Known Issue di `CHANGELOG.md`, lihat Governance Notes Bagian 15).

---

# 5. Versioning Rules

Seluruh dokumen proyek mengikuti **Semantic Versioning** (`MAJOR.MINOR`, tanpa PATCH kecuali diperlukan granularitas lebih — lihat catatan di bawah), selaras dengan prinsip yang sudah dipakai `CHANGELOG.md` untuk versi rilis kode proyek (`MAJOR.MINOR.PATCH`).

| Level | Kapan Digunakan | Contoh di Proyek Ini |
|---|---|---|
| **MAJOR** (`1.x` → `2.0`) | Perubahan yang mengubah **keputusan inti/kontrak** dokumen tsb — mis. perubahan struktur role, perubahan kontrak field wajib API, perubahan arsitektur backend. Menghasilkan Baseline baru yang **tidak backward-compatible** secara konsep dengan versi sebelumnya. | Revisi v1.0 → v1.1 pada PRD/ERD/API Spec/User Flow/SEO Spec (resolusi 7 konflik lintas dokumen) **secara substansi** memenuhi kriteria MAJOR menurut definisi ini — dicatat sebagai catatan konsistensi di Governance Notes (Bagian 15, poin 2), karena dokumen sumber menyebutnya "Minor Revision" sedangkan isinya mengandung breaking change konseptual (formalisasi role baru, migrasi `city` → `city_id`). |
| **MINOR** (`1.0` → `1.1`) | Penambahan konten baru yang **tidak mengubah** keputusan yang sudah ada — mis. penambahan bagian baru, penambahan tabel/entitas baru yang belum pernah didefinisikan sebelumnya, penambahan klarifikasi yang memperjelas (bukan mengubah) makna sebelumnya. | Penambahan bagian "Riwayat Keputusan Arsitektur" di `PROJECT-CONSTITUTION.md` v1.1 tanpa mengubah keputusan v1.0 yang sudah ada. |
| **PATCH** (`1.1` → `1.1.1`, opsional) | Perbaikan redaksional, typo, perbaikan tautan/referensi silang yang tidak mengubah makna/keputusan apa pun. | Digunakan hanya jika tim memilih granularitas tambahan; saat ini seluruh dokumen proyek memakai penomoran dua digit (`1.0`, `1.1`) — PATCH disediakan sebagai opsi, bukan kewajiban, untuk dokumen governance (berbeda dengan versi rilis **kode** proyek di `CHANGELOG.md` yang wajib tiga digit). |

**Aturan tambahan:**
- Nomor versi **hanya** naik ketika dokumen resmi memasuki status **Baseline** baru (Bagian 3) — status Draft/In Review yang masih dalam proses penulisan versi berikutnya **tidak** menaikkan nomor versi publik; gunakan penanda internal (mis. "v1.2-draft") jika diperlukan untuk kolaborasi.
- Dokumen yang statusnya masih **Draft** pada versi pertamanya (mis. `technology-decisions.md`, `dependency-manifest.md`, `DEVELOPMENT-ROADMAP.md`, `AI-DEVELOPMENT-BLUEPRINT.md` — seluruhnya tercatat versi "1.0" tetapi status "Draft — menunggu pengesahan tim") **tetap** memakai penomoran versi sejak draf pertama — versi tidak menunggu sampai Approved untuk mulai dihitung.
- Downgrade nomor versi **tidak pernah** dilakukan — koreksi tetap dicatat sebagai kenaikan versi baru (konsisten dengan Aturan Wajib #1 `CHANGELOG.md`: "History tidak boleh dihapus").

---

# 6. Document Status Definitions

Ringkasan definisi status dokumen (melengkapi penjelasan naratif Bagian 3), dipakai sebagai nilai baku kolom **Status** di Bagian 10 (Baseline Register):

| Status | Definisi Ringkas |
|---|---|
| **Planned** | Kebutuhan dokumen sudah diidentifikasi (mis. ditandai sebagai gap oleh dokumen lain), belum ditulis. |
| **Draft** | Sedang ditulis / draf pertama selesai, belum direview formal. |
| **In Review** | Sedang dalam proses review oleh Reviewer yang ditunjuk. |
| **Approved** | Disetujui isi/keputusannya, mengikat untuk implementasi, namun belum tentu dikunci sebagai Baseline formal. |
| **Baseline** | Versi dikunci sebagai acuan resmi tertinggi saat ini — tidak boleh diubah langsung. |
| **Deprecated** | Tidak lagi direkomendasikan sebagai rujukan aktif, digantikan versi/dokumen lain, dipertahankan untuk sejarah. |
| **Archived** | Sepenuhnya keluar dari siklus rujukan aktif, disimpan permanen untuk audit trail. |

---

# 7. Source of Truth Matrix

| Jenis Informasi | Dokumen Source of Truth | Dokumen Boleh Mengutip (Tidak Boleh Mendefinisikan Ulang) |
|---|---|---|
| **Business Requirement** | `PRD-Real-Estate-Agency-Platform-v1.1.md` | AI-CONTEXT-PACK, DEVELOPMENT-ROADMAP, TASK-TEMPLATE |
| **Governance Tertinggi / Engineering Guidelines** | `PROJECT-CONSTITUTION.md` | Seluruh dokumen lain (mengalahkan seluruhnya jika terjadi konflik) |
| **Architecture (High-Level)** | `SYSTEM-ARCHITECTURE.md` | technology-decisions, dependency-manifest, AI-DEVELOPMENT-BLUEPRINT |
| **Database (Logis/ERD)** | `ERD-Skema-Database-Real-Estate-Agency-v1.1.md` + `ERD-Diagram-v1.1.mermaid` | API-Specification, PRD (referensi field), SYSTEM-ARCHITECTURE §7 |
| **Database (Fisik/Migration)** | **TBD** — belum ada dokumen "Database Schema" fisik terpisah dari ERD dalam repositori yang direview (lihat Governance Notes Bagian 15, poin 3). Sampai dokumen ini dibuat, migration file aktual di `/apps/api/migrations` (setelah Sprint S0 berjalan) menjadi rujukan fisik, dengan ERD sebagai rujukan desain logis. | — |
| **API Contract** | `API-Specification-Real-Estate-Agency-Platform-v1.1.md` | SYSTEM-ARCHITECTURE §9, User-Flow |
| **User Interaction Flow** | `User-Flow-Real-Estate-Agency-Platform-v1.1.md` | PRD (acceptance criteria), Functional Specification (bila dibuat) |
| **SEO & Analytics Strategy** | `SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md` | PROJECT-CONSTITUTION §18–19, DEVELOPMENT-ROADMAP |
| **Technology Stack & Rasionalisasi** | `technology-decisions.md` | dependency-manifest, SYSTEM-ARCHITECTURE §4 |
| **Dependency/Package Katalog** | `dependency-manifest.md` | AI-DEVELOPMENT-BLUEPRINT, TASK-TEMPLATE |
| **Arsitektur & Keputusan Teknis per Topik (ADR)** | `architecture-decision-records.md` — **25 ADR** (`ADR-001`–`ADR-025`), disusun per topik arsitektur (Backend, Auth, RBAC, Database, Search, Job Queue, Email, Maps, Storage, Deployment, State Management, API Architecture, Error Handling, Logging, Monitoring, Testing, Security, Caching, File Upload, Notification, Frontend Framework, Schema Conventions, Multi-Tenancy, RBAC Role Scope, Type Safety). **Status per 29 Jul 2026: 23 Approved** (termasuk `ADR-001` Backend Architecture, `ADR-005` Search Strategy, & `ADR-006` Job Queue Strategy), **2 OPEN** (`ADR-008` Maps, `ADR-018` Caching). Dibaca sebelum `technology-decisions.md` — menjelaskan alasan di balik apa yang tercantum di sana. | `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md` §4, `AI-DEVELOPMENT-BLUEPRINT.md`, `dependency-manifest.md` |
| **Roadmap & Sprint Plan** | `DEVELOPMENT-ROADMAP.md` | TASK-TEMPLATE, CURRENT-PROJECT-STATE |
| **Format/Unit Kerja Task** | `TASK-TEMPLATE.md` | AI-DEVELOPMENT-BLUEPRINT §21 |
| **Keputusan & Rasionalisasi — Jurnal Kronologis Lintas Proyek (ADR)** | `decision-log.md` — jurnal **kronologis** seluruh keputusan proyek (teknis maupun non-teknis, mis. penetapan versi Blueprint aktif, keputusan governance dokumen), disusun per urutan waktu terjadinya, **bukan** per topik arsitektur. Penomoran ADR-nya independen dari `architecture-decision-records.md` (lihat catatan penomoran di dokumen ADR tsb, Bagian 2). | Seluruh dokumen lain yang merujuk alasan keputusan |
| **Riwayat Perubahan (Rilis Kode)** | `CHANGELOG.md` | — (tidak dikutip dokumen lain sebagai rujukan keputusan, hanya riwayat) |
| **Status Implementasi Nyata (Living)** | `CURRENT-PROJECT-STATE.md` | TASK-TEMPLATE, AI-DEVELOPMENT-BLUEPRINT — **wajib dibaca pertama** di setiap sesi AI |
| **Context Ringkas Proyek** | `AI-CONTEXT-PACK.md` (Bagian 1–2) digabung `PRD-...v1.1.md` (Bagian 1) — konsisten dengan definisi "Project Overview" di `AI-DEVELOPMENT-BLUEPRINT.md` §5 | Seluruh dokumen lain sebagai ringkasan cepat |
| **AI Instruction / Prosedur Kerja AI** | `AI-DEVELOPMENT-BLUEPRINT.md` (versi upload 24 bagian — ditetapkan sebagai acuan aktif sejak Session 5, menggantikan versi Session 3) | AI-CONTEXT-PACK §11–12, TASK-TEMPLATE |
| **Validasi Kesiapan Fondasi** | `foundation-validation-report.md` | decision-log (Open Decisions), CHANGELOG (Known Issues) |
| **Keputusan Eksekutif Tingkat Tinggi** | **TBD** — dokumen "Executive Architecture Review" disebutkan pada daftar dokumen yang sudah ada, namun **tidak ditemukan sebagai file** dalam repositori yang direview untuk penyusunan dokumen ini (lihat Governance Notes Bagian 15, poin 3). | — |
| **Tata Kelola Dokumen Itu Sendiri** | **Dokumen ini** (`document-governance-baseline-register.md`) | Seluruh dokumen lain (untuk pertanyaan "dokumen mana yang berwenang atas X") |

---

# 8. Document Dependency Matrix

Diagram berikut menunjukkan **arah dependency** — dokumen di atas panah harus stabil/Baseline lebih dulu sebelum dokumen di bawahnya dapat dianggap solid, konsisten dengan urutan yang sudah dibangun `DEVELOPMENT-ROADMAP.md` dan `foundation-validation-report.md` Bagian 16–18:

```
PROJECT-CONSTITUTION.md (Engineering Guidelines)
        ↓
Architecture Decision Records (architecture-decision-records.md — 25 ADR per topik arsitektur,
        23 Approved termasuk ADR-001 Backend Architecture, ADR-005 Search Strategy & ADR-006 Job Queue Strategy,
        2 OPEN: ADR-008, ADR-018)
        ↓
Technology Decisions
        ↓
System Architecture
        ↓
ERD (Skema Database Logis) + ERD Diagram
        ↓
Database Schema (fisik — TBD, menyusul saat Sprint S0)
        ↓
API Specification
        ↓
User Flow
        ↓
PRD Alignment  (PRD sudah ada v1.1 — baris ini merepresentasikan proses verifikasi silang
                berkelanjutan antara PRD dan seluruh dokumen teknis di atasnya)
        ↓
Functional Specification   (Planned — belum ada file, lihat Governance Notes Bagian 15, poin 4)
        ↓
UI Specification           (Planned — belum ada file)
        ↓
Technical Specification    (Ready with Notes — bahan baku tersebar di System Architecture +
                             Technology Decisions + Dependency Manifest, perlu konsolidasi)
        ↓
Module Planning            (Development Roadmap sudah memenuhi fungsi ini — status Ready)
```

**Dependency dokumen operasional (paralel, tidak linear terhadap rantai di atas):**

| Dokumen | Bergantung Pada |
|---|---|
| `Architecture Decision Records` (`architecture-decision-records.md`) | `PROJECT-CONSTITUTION.md` (Engineering Guidelines tertinggi) + dokumen sumber v1.1 (PRD/ERD/API Spec) sebagai konteks kebutuhan per topik ADR. Menjadi prasyarat penjelas bagi `Technology Decisions` dan `System Architecture` (setiap baris "Official Technology Stack" seharusnya dapat ditelusuri balik ke tepat satu ADR di dokumen ini). |
| `Dependency Manifest` | `Technology Decisions` (satu-ke-satu, setiap package memetakan ke satu keputusan teknologi) |
| `AI Development Blueprint` (Development Playbook) | Seluruh dokumen sumber v1.1 + System Architecture + Technology Decisions + Dependency Manifest + AI Context Pack |
| `AI Context Pack` | Seluruh dokumen sumber v1.1 + AI Development Blueprint |
| `Task Template` | Constitution + dokumen sumber v1.1 + System Architecture + Technology Decisions + Dependency Manifest + AI Development Blueprint + AI Context Pack + Development Roadmap |
| `Decision Log` | Tidak bergantung struktural pada dokumen lain (mencatat keputusan lintas semua dokumen), tetapi **menjelaskan alasan** di balik keputusan yang tercantum di dokumen lain |
| `Changelog` | `Current Project State` (perubahan riil yang dicatat harus konsisten dengan status implementasi nyata) |
| `Current Project State` | Tidak bergantung struktural — merefleksikan kondisi **fisik** repositori, independen dari dokumen desain mana pun |
| `Foundation Validation Report` | Seluruh 17 dokumen yang diaudit (snapshot pada tanggal audit) |
| `Document Governance & Baseline Register` (dokumen ini) | Seluruh dokumen di atas — sebagai meta-layer yang mengatur status/versi, bukan isi |

---

# 9. Review & Approval Matrix

> **Catatan:** Kolom **Reviewer**/**Approver** saat ini diisi dengan **peran** (bukan nama individu), konsisten dengan status "Owner belum ditetapkan sebagai nama spesifik" yang sudah tercatat terbuka di `technology-decisions.md` Bagian 1, `decision-log.md` Bagian 1, dan `DEVELOPMENT-ROADMAP.md` Bagian 1. Ini ditandai eksplisit sebagai TBD-untuk-nama-individu, bukan diasumsikan.

| Document | Reviewer (Peran) | Approver (Peran) | Review Frequency | Approval Required |
|---|---|---|---|---|
| PROJECT-CONSTITUTION.md | Principal Software Architect + Technical Lead | Technical Lead / Product Owner (nama TBD) | Setiap ada keputusan bisnis besar turun (per Bagian 21 poin 8 dokumen tsb) | Ya — wajib |
| PRD v1.1 | Senior Business Analyst + Product Manager | Product Owner (nama TBD) | Setiap perubahan requirement bisnis | Ya — wajib |
| ERD + ERD Diagram v1.1 | Database Architect | Technical Lead (nama TBD) | Setiap penambahan/perubahan entitas | Ya — wajib |
| API Specification v1.1 | API Architect | Technical Lead (nama TBD) | Setiap penambahan/perubahan endpoint | Ya — wajib |
| User Flow v1.1 | Senior Business Analyst + UX Reviewer (peran belum ada di proyek — TBD) | Product Owner (nama TBD) | Setiap perubahan alur pengguna signifikan | Ya — wajib |
| SEO & Analytics Specification v1.1 | SEO/Performance Reviewer (peran belum ditunjuk — TBD) | Technical Lead (nama TBD) | Setiap perubahan strategi rendering/SEO | Ya — wajib |
| SYSTEM-ARCHITECTURE.md | Enterprise Solution Architect | Technical Lead / CTO (nama TBD) | Setiap keputusan arsitektur besar | Ya — wajib |
| architecture-decision-records.md | Architecture Review Board (CTO, Principal Software Architect, Enterprise Solution Architect, Senior Backend/Frontend Architect, Cloud Architect, DevOps Architect, Database Architect, Security Architect, AI Development Architect, Technical Lead) | Technical Lead / CTO (nama TBD) — pengesahan formal final per-ADR tetap memerlukan konfirmasi manusia | Per ADR (setiap kali topik arsitektur baru dibahas atau ADR ber-status Open/Proposed diselesaikan) | Ya — wajib, per-entry ADR (bukan per-dokumen), konsisten dengan pola `decision-log.md` |
| technology-decisions.md | Principal Software Architect | Technical Lead (nama TBD) — **belum pernah disahkan formal** (lihat Governance Notes Bagian 15, poin 1) | Setiap Open Question (Bagian 9 dokumen tsb) diselesaikan | Ya — wajib, **belum terpenuhi saat ini** |
| dependency-manifest.md | Principal Software Architect | Technical Lead (nama TBD) | Bersamaan setiap kali technology-decisions.md direvisi | Ya — wajib |
| AI-DEVELOPMENT-BLUEPRINT.md | Reviewer AI (Claude, peran ditetapkan di Bagian 3.1 dokumen tsb) + Technical Lead manusia | Technical Lead (nama TBD) | Setiap kali AI Workflow/AI Rules berubah | Ya — wajib |
| AI-CONTEXT-PACK.md | Technical Writer (Claude — Bagian 3.1 Blueprint) | Technical Lead (nama TBD) | Setiap kali dokumen sumber v1.1 berubah signifikan | Ya — direkomendasikan |
| DEVELOPMENT-ROADMAP.md | Engineering Manager / Senior Technical PM | Technical Lead / Product Owner (nama TBD) | Setiap kali Constitution/PRD/System Architecture berubah signifikan | Ya — wajib |
| TASK-TEMPLATE.md | Staff Software Engineer | Technical Lead (nama TBD) | Jika ditemukan gap berulang pada jenis task tertentu | Direkomendasikan, tidak wajib per-task |
| decision-log.md | Self-maintained (Living Document) | Technical Lead untuk setiap ADR berstatus Approved (nama TBD) | Berkelanjutan (setiap ADR baru) | Ya, per-entry (bukan per-dokumen) |
| CHANGELOG.md | Release Manager | Technical Lead (nama TBD) | Setiap rilis versi baru | Ya, per-entry rilis |
| CURRENT-PROJECT-STATE.md | Technical Project Manager | Technical Lead (nama TBD) | Akhir setiap sesi development yang mengubah kode nyata | Ya — wajib sebelum sesi ditutup |
| foundation-validation-report.md | AI Audit Panel (peran gabungan — lihat dokumen tsb Bagian 1) | CTO / Technical Lead (nama TBD) | Sekali per audit besar (mis. sebelum fase baru dimulai) | Ya — sebagai gate, bukan approval berkelanjutan |
| Executive Architecture Review | **TBD** — dokumen tidak ditemukan dalam repositori yang direview (lihat Governance Notes Bagian 15, poin 3) | **TBD** | **TBD** | **TBD** |
| Database Schema (fisik) | **TBD** — belum ada sebagai dokumen/migration fisik (proyek masih 0% kode per `CURRENT-PROJECT-STATE.md`) | **TBD** | **TBD** | **TBD** |
| document-governance-baseline-register.md (dokumen ini) | Software Configuration Manager | Technical Lead / CTO (nama TBD) | Setiap kali status/versi dokumen lain berubah material | Ya — wajib |

---

# 10. Baseline Register

> Data diisi berdasarkan isi aktual dokumen yang tersedia pada tanggal penyusunan (27 Juli 2026), **disinkronkan ulang 29 Juli 2026** dengan resolusi `architecture-decision-records.md` `ADR-006` (Job Queue Strategy, Approved) dan rilis `CHANGELOG.md` `0.1.3` / `decision-log.md` `ADR-040`. Kolom yang informasinya tidak tersedia secara eksplisit di dokumen sumber ditandai **TBD**. Baris terdampak ditandai eksplisit di kolom **Last Review**, bukan diedit diam-diam.

| Document | Current Version | Status | Baseline Version | Owner | Last Review | Next Review | Dependencies | Source of Truth Untuk |
|---|---|---|---|---|---|---|---|---|
| PROJECT-CONSTITUTION.md | 1.4 | Baseline (dinyatakan "BERLAKU") | 1.4 | Principal Software Architect (nama TBD) | 29 Jul 2026 (revisi v1.3→v1.4, sinkron `ADR-006`) | Setiap keputusan bisnis besar turun | — (tertinggi) | Governance / Engineering Guidelines |
| PRD-Real-Estate-Agency-Platform-v1.1.md | 1.1 | Baseline (dinyatakan "Disetujui") | 1.1 | Senior Business Analyst / Product Manager (nama TBD) | 26 Jul 2026 | Saat requirement bisnis berubah | PROJECT-CONSTITUTION.md | Business Requirement |
| ERD-Skema-Database-Real-Estate-Agency-v1.1.md | 1.1 | Approved (belum eksplisit disebut "Baseline" — direkomendasikan naik status setelah Database Schema Alignment, lihat `foundation-validation-report.md` §18) | 1.1 (kandidat) | Database Architect (nama TBD) | 26 Jul 2026 | Saat Database Schema Alignment dieksekusi | PRD, PROJECT-CONSTITUTION.md | Database (Logis) |
| ERD-Diagram-v1.1.mermaid | 1.1 | Approved (companion ERD, status sama) | 1.1 (kandidat) | Database Architect (nama TBD) | 26 Jul 2026 | Bersamaan dengan ERD Skema | ERD-Skema-Database v1.1 | Database (Visual) |
| API-Specification-Real-Estate-Agency-Platform-v1.1.md | 1.1 | Approved (direkomendasikan naik Baseline setelah API Alignment) | 1.1 (kandidat) | API Architect (nama TBD) | 26 Jul 2026 | Saat API Alignment dieksekusi | ERD, PRD | API Contract |
| User-Flow-Real-Estate-Agency-Platform-v1.1.md | 1.1 | Approved | 1.1 (kandidat) | Senior Business Analyst (nama TBD) | 26 Jul 2026 | Saat User Flow Alignment dieksekusi | PRD, API Specification | User Interaction Flow |
| SEO-Analytics-Specification-Real-Estate-Agency-Platform-v1.1.md | 1.1 | Approved | 1.1 (kandidat) | Technical Lead (nama TBD) | 26 Jul 2026 | Saat strategi rendering berubah | PRD, System Architecture | SEO & Analytics Strategy |
| SYSTEM-ARCHITECTURE.md | 1.3 | Approved ("Referensi teknis utama — mengikat"), **belum Baseline formal** (menunggu pengesahan nama individu Reviewer/Approver) | TBD (menunggu sinkronisasi) | Enterprise Solution Architect (nama TBD) | 29 Jul 2026 (naik v1.2→v1.3, sinkron `ADR-006`) | Setelah Open Decision H1–H3 (`foundation-validation-report.md` §16) diselesaikan | PROJECT-CONSTITUTION.md, dokumen sumber v1.1, **architecture-decision-records.md** | Architecture (High-Level) |
| architecture-decision-records.md | 1.0 | **Draft** (status dokumen sendiri) — meski `ADR-001` (Backend Architecture), `ADR-005` (Search Strategy), dan `ADR-006` (Job Queue Strategy) di dalamnya sudah berstatus entry-level **Approved** (hasil Architecture Review Board, 27, 28 & 29 Jul 2026). 22 ADR lain bercampur status Approved/Open/Rejected/Superseded per-entry — lihat dokumen tsb Bagian 5 (Open Decisions Summary). | Belum ada (blocked oleh status dokumen Draft — sama seperti pola `technology-decisions.md`) | Principal Enterprise Software Architect / Enterprise Solution Architect (nama TBD) | 29 Jul 2026 (ADR-006 dikunci Approved) | Setelah pengesahan tim formal + seluruh ADR ber-status Open (ADR-008 Maps, ADR-018 Caching) diselesaikan | PROJECT-CONSTITUTION.md, dokumen sumber v1.1 | Arsitektur & Keputusan Teknis per Topik (ADR) |
| technology-decisions.md | 1.3 | **Draft** (status dokumen sendiri) — meski isinya disebut "keputusan resmi dan final" | Belum ada (blocked oleh status Draft) | Principal Software Architect / Technical Lead (nama TBD) | 29 Jul 2026 (naik v1.2→v1.3, sinkron `ADR-006`) | Setelah pengesahan tim + Open Questions §9 diselesaikan **dan** disinkronkan dengan `architecture-decision-records.md` `ADR-008`/`ADR-018` (masih Open); penambahan **Bolt.new** sebagai toolchain resmi (catatan kondisional Board `ADR-001`) masih belum dieksekusi | SYSTEM-ARCHITECTURE.md, PROJECT-CONSTITUTION.md, **architecture-decision-records.md** | Technology Stack & Rasionalisasi |
| dependency-manifest.md | 1.3 | Draft (mengikuti status technology-decisions.md) | Belum ada | Principal Software Architect (nama TBD) | 29 Jul 2026 (naik v1.2→v1.3, sinkron `ADR-006` — tidak ada package npm baru, hanya klarifikasi Vercel Cron/Postgres Trigger native; `bullmq`/`ioredis` ditolak permanen) | Bersamaan dengan technology-decisions.md | technology-decisions.md | Dependency/Package Katalog |
| AI-DEVELOPMENT-BLUEPRINT.md (versi upload, 24 bagian — kini juga dirujuk sebagai `development-playbook.md`) | 1.3 | Draft (status dokumen), namun **ditetapkan sebagai acuan aktif** oleh keputusan user (CHANGELOG Session 5) menggantikan versi Session 3 (32 bagian, kini Archived) | Belum ada Baseline formal | Principal Software Architect (nama TBD) | 29 Jul 2026 (naik v1.2→v1.3, sinkron `ADR-006`) | Saat AI Workflow/Rules berubah | Seluruh dokumen sumber v1.1 + System Architecture + Technology Decisions | AI Instruction / Development Playbook |
| AI-DEVELOPMENT-BLUEPRINT.md (versi Session 3, 32 bagian) | 1.0 (superseded) | **Archived** (digantikan versi upload, per keputusan Session 5) | — | — | 26–27 Jul 2026 | Tidak ada — arsip permanen | — | Historis saja |
| AI-CONTEXT-PACK.md | 1.0 | Approved | 1.0 (kandidat) | Technical Writer (nama TBD) | 27 Jul 2026 | Saat dokumen sumber v1.1 berubah signifikan | Dokumen sumber v1.1 + AI-DEVELOPMENT-BLUEPRINT.md | Context Ringkas Proyek |
| DEVELOPMENT-ROADMAP.md | 1.0 | Draft | Belum ada | Engineering Manager / Senior Technical PM (nama TBD) | 27 Jul 2026 | Setelah pengesahan tim | Seluruh dokumen sumber + System Architecture + Technology Decisions | Roadmap & Sprint Plan |
| TASK-TEMPLATE.md | 1.0 | Baseline (dinyatakan "BERLAKU") | 1.0 | Staff Software Engineer (nama TBD) | 27 Jul 2026 | Jika ditemukan gap berulang | Seluruh dokumen governance | Format/Unit Kerja Task |
| CURRENT-PROJECT-STATE.md | 0.1 | Baseline (Living Document — berlaku sejak dibuat, wajib update tiap sesi) | 0.1 (per-sesi, berubah dinamis) | Technical Project Manager (nama TBD) | 29 Jul 2026 (snapshot ADR & Governance diperbarui, sinkron `ADR-006`) | Akhir setiap sesi development | Kondisi fisik repositori (independen dokumen desain) | Status Implementasi Nyata |
| CHANGELOG.md | 0.1.3 (versi rilis proyek yang dicatat di dalamnya; dokumen itu sendiri tidak diberi nomor versi terpisah) | Baseline (Living Document) | 0.1.3 | Release Manager (nama TBD) | 29 Jul 2026 (rilis `0.1.3` — Governance Sync: `ADR-006` Job Queue Strategy diselesaikan, Known Issue #5 kini sepenuhnya RESOLVED) | Setiap rilis versi baru | CURRENT-PROJECT-STATE.md | History |
| decision-log.md | 1.0 | Baseline (Living Document — "BERLAKU sejak dibuat") | 1.0 (per-ADR, berkembang) | Principal Software Architect / Technical Lead (nama TBD) | 29 Jul 2026 (entry `ADR-040` ditambahkan — sinkronisasi `architecture-decision-records.md` `ADR-006` Job Queue Strategy) | Berkelanjutan (tiap ADR baru) | Seluruh dokumen governance, **architecture-decision-records.md** (sebagai sumber sinkronisasi ADR-038, ADR-039 & ADR-040) | Decision (jurnal kronologis) |
| foundation-validation-report.md | 1.0 | Baseline (dinyatakan "Final — Quality Gate Deliverable") | 1.0 | AI Audit Panel (peran gabungan) | 27 Jul 2026 | Sebelum fase besar berikutnya dimulai | 17 dokumen yang diaudit | Validation |
| Executive Architecture Review | **TBD** | **TBD — dokumen tidak ditemukan dalam repositori yang direview** | **TBD** | **TBD** | **TBD** | **TBD** | **TBD** | Executive Decision (belum ada rujukan aktual) |
| Database Schema (fisik) | **TBD** | **Planned** — belum ada migration/skema fisik (proyek 0% kode) | **TBD** | Database Architect (nama TBD) | **TBD** | Setelah Sprint S0 dieksekusi | ERD-Skema-Database v1.1 | Database (Fisik) |
| document-governance-baseline-register.md (dokumen ini) | 1.0 | Draft | Belum ada | Software Configuration Manager (nama TBD) | 29 Jul 2026 | Setiap kali status/versi dokumen lain berubah material | Seluruh dokumen di atas | Tata Kelola Dokumen |

---

# 11. Change Management Rules

Prosedur wajib setiap kali ada permintaan perubahan terhadap dokumen berstatus **Baseline**:

```
Request
   ↓
Decision Log (ADR baru dibuat berstatus "Proposed" — decision-log.md §2, §8)
   ↓
Impact Analysis (cek Document Dependency Matrix §8 — dokumen turunan mana yang ikut terdampak;
                 cek juga Module Dependency & Feature Dependency di AI-DEVELOPMENT-BLUEPRINT.md
                 sesuai catatan decision-log.md §8)
   ↓
Update Document (isi dokumen direvisi menjadi status "Updated", bukan langsung menimpa Baseline)
   ↓
Review (oleh Reviewer sesuai Bagian 9 — Review & Approval Matrix)
   ↓
Approval (oleh Approver berwenang; ADR di decision-log.md naik status menjadi "Approved")
   ↓
New Baseline (nomor versi naik sesuai Bagian 5; Baseline lama diberi status Deprecated →
              lalu Archived setelah masa transisi wajar)
   ↓
Changelog (entri baru ditambahkan di CHANGELOG.md — tidak pernah menimpa entri lama,
           sesuai Aturan Wajib #1–#2 CHANGELOG.md)
```

**Aturan pelengkap:**

1. **AI Coding Assistant tidak berwenang melompati tahap Approval** — ini menegaskan kembali `decision-log.md` Bagian 8 poin terakhir. AI dapat mengisi Request dan membantu Impact Analysis/Update Document, tetapi Approval selalu memerlukan konfirmasi manusia berwenang.
2. **Setiap perubahan wajib menyebut ADR yang menjadi dasarnya** — dokumen yang diperbarui harus mencantumkan nomor ADR terkait di catatan revisinya (pola yang sudah dipakai `PROJECT-CONSTITUTION.md` Bagian "Riwayat Keputusan Arsitektur").
3. **Perubahan yang berdampak ke dokumen berhierarki lebih tinggi tidak sah** — mis. jika perubahan pada `technology-decisions.md` ternyata memerlukan perubahan `PROJECT-CONSTITUTION.md`, Impact Analysis wajib mengeskalasi ADR tersebut sebagai perubahan pada dokumen yang lebih tinggi, bukan menerapkannya sepihak hanya di dokumen turunan.
4. **Tidak ada perubahan "silent"** — perubahan pada dokumen apa pun yang tidak melalui alur ini (mis. edit langsung tanpa ADR) dianggap pelanggaran governance, sesuai semangat `decision-log.md` Bagian 9 poin 4.

---

# 12. Document Update Priority

Ketika terjadi perubahan pada area berikut, dokumen-dokumen ini **wajib diperbarui pertama** (dalam urutan), sebelum dokumen turunan lain menyusul:

| Area Perubahan | Dokumen yang Diperbarui Pertama | Dokumen Turunan yang Menyusul |
|---|---|---|
| **Business Rule** | `PRD-...v1.1.md` → `PROJECT-CONSTITUTION.md` (jika hard rule ikut berubah) | User Flow, API Specification, Development Roadmap, Task terkait |
| **Database** | `ERD-Skema-Database-...v1.1.md` + `ERD-Diagram-...v1.1.mermaid` | API Specification (kontrak field), Database Schema fisik/migration, System Architecture §7 |
| **API** | `API-Specification-...v1.1.md` | User Flow (jika alur ikut berubah), dependency-manifest (jika perlu SDK baru), System Architecture §9 |
| **Security** | `PROJECT-CONSTITUTION.md` Bagian 20 (Security Rules) | SYSTEM-ARCHITECTURE.md §14, AI-CONTEXT-PACK §10, ERD (kolom enkripsi/RLS), API Specification (label Auth) |
| **UI** | User Flow (bila alur berubah) → UI Specification (setelah dibuat — lihat Bagian 15 poin 4) | Functional Specification, Task terkait modul |
| **Architecture Decision (per topik)** | `architecture-decision-records.md` (ADR ber-status Approved terlebih dahulu dikunci) | `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `dependency-manifest.md`, `AI-DEVELOPMENT-BLUEPRINT.md`, `decision-log.md` (entry sinkronisasi kronologis) |
| **Technology** | `technology-decisions.md` → `dependency-manifest.md` | SYSTEM-ARCHITECTURE.md §4, AI-DEVELOPMENT-BLUEPRINT.md, PROJECT-CONSTITUTION.md §4 (jika keputusan naik jadi hard rule tertinggi) |
| **Deployment** | `SYSTEM-ARCHITECTURE.md` §18 (Deployment Architecture) → `DEVELOPMENT-ROADMAP.md` (Go Live Checklist) | CURRENT-PROJECT-STATE.md, CHANGELOG.md (entri rilis) |

**Prinsip umum:** dokumen yang menjadi **Source of Truth** (Bagian 7) untuk area yang berubah **selalu** diperbarui lebih dulu — dokumen yang hanya **mengutip** menyusul setelahnya. Ini mencegah pola yang sudah tercatat sebagai Known Issue di `CHANGELOG.md` (keputusan final di satu dokumen turunan namun belum tersinkron ke dokumen sumber yang lebih tinggi).

---

# 13. AI Usage Rules

Aturan wajib bagi AI Coding Assistant apa pun (Claude, Bolt.new, ChatGPT, Cursor, GitHub Copilot, dsb.) saat membaca dan menggunakan dokumentasi proyek:

1. **Selalu mulai dari Project Overview** (`AI-CONTEXT-PACK.md` Bagian 1–2 + `PRD` Bagian 1), lalu `CURRENT-PROJECT-STATE.md`, sebelum membaca dokumen teknis mendalam — konsisten dengan `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 5 (Documentation Reading Order).
2. **Gunakan Source of Truth Matrix (Bagian 7 dokumen ini)** untuk menentukan dokumen mana yang berwenang menjawab suatu pertanyaan — jangan mencampur informasi dari dua dokumen yang membahas topik sama tanpa mengecek mana yang menang.
3. **Jangan mengambil keputusan dari dokumen berstatus Deprecated atau Archived** (mis. `AI-DEVELOPMENT-BLUEPRINT.md` versi Session 3 yang sudah digantikan) — selalu verifikasi status dokumen di Bagian 10 (Baseline Register) sebelum menjadikannya rujukan.
4. **Jangan menggunakan dokumen berstatus Draft sebagai rujukan keputusan final** — perlakukan isinya sebagai *configurable placeholder*, tandai `// TODO: menunggu Baseline`, konsisten dengan penanganan Open Question di seluruh dokumen sumber.
5. **Selalu gunakan versi Baseline terbaru** yang tercatat di Bagian 10 — jika sebuah dokumen memiliki lebih dari satu versi yang beredar (draf revisi sedang berjalan + Baseline lama), Baseline lama yang masih berlaku sampai Baseline baru resmi disahkan (lihat Bagian 4.2).
6. **Jika ditemukan pertentangan antar dokumen yang belum tercatat sebagai Open Decision** — laporkan sebagai temuan ke pengguna/tim, **jangan** memilih salah satu sisi secara sepihak (menegaskan kembali `decision-log.md` Bagian 9 poin 6).
7. **Ikuti hierarki kemenangan dokumen** (`PROJECT-CONSTITUTION.md` > dokumen sumber v1.1 > `SYSTEM-ARCHITECTURE.md` > `architecture-decision-records.md` > `technology-decisions.md` > `dependency-manifest.md` > `AI-DEVELOPMENT-BLUEPRINT.md`) setiap kali dua dokumen tampak memberi instruksi berbeda. **Catatan:** `architecture-decision-records.md` dibaca **sebelum** `technology-decisions.md` (ADR menjelaskan alasan di balik katalog stack), namun ADR ber-status `Open`/`Proposed` di dalamnya tidak mengalahkan apa pun — hanya ADR ber-status `Approved` yang mengikat.
8. **Dokumen ini (`document-governance-baseline-register.md`) tidak dipakai untuk mengambil keputusan isi/teknis** — hanya dipakai untuk menjawab pertanyaan status/versi/ownership/lifecycle dokumen. Untuk isi teknis, tetap rujuk dokumen sumber langsung sesuai Source of Truth Matrix.

---

# 14. Governance Checklist

Checklist wajib diverifikasi **sebelum** Development (Sprint S0 dan seterusnya) dieksekusi penuh ke tahap implementasi kode:

- [ ] Seluruh dokumen inti (Constitution, PRD, ERD, API Spec, User Flow, SEO Spec) berstatus **Baseline** — saat ini: PROJECT-CONSTITUTION.md dan PRD sudah Baseline; ERD/API Spec/User Flow/SEO Spec masih **Approved** (kandidat Baseline, lihat Bagian 10) — **belum sepenuhnya terpenuhi**.
- [ ] Tidak ada **Open Decision** yang memblokir modul yang akan dikerjakan — cek `decision-log.md` Bagian 11 dan `CHANGELOG.md` Known Issues sebelum memulai setiap sprint — **saat ini masih tercatat 8 baris di `decision-log.md` §11, namun secara substansi tinggal 5 yang benar-benar terbuka**: butir #1 (arsitektur backend) sudah dikunci **Approved** via `architecture-decision-records.md` `ADR-001`/`decision-log.md` `ADR-038` (27 Jul 2026); butir #5 (Search Engine **dan** Job Queue) sudah dikunci **Approved** sepenuhnya via `ADR-005`/`ADR-039` (28 Jul 2026) dan `ADR-006`/`ADR-040` (29 Jul 2026) — `CHANGELOG.md` Known Issue #1 ditandai `RESOLVED (0.1.1)` dan Known Issue #5 kini sepenuhnya ditandai `RESOLVED (0.1.3)`. Baris #1 dan #5 di `decision-log.md` §11 itu sendiri **belum diformat ulang** sepenuhnya menjadi status Resolved terpisah (masih memakai strikethrough sebagian, sesuai aturan "tidak menghapus histori"). Sebagian sisanya bersifat blocking untuk sprint tertentu (lihat tabel di `decision-log.md` §11).
- [ ] **Decision Log** dalam kondisi terbaru — seluruh keputusan yang relevan dengan sprint yang akan dikerjakan sudah tercatat sebagai ADR. **Terkini**: entry `ADR-040` (29 Jul 2026) sudah menyinkronkan keputusan Job Queue Strategy dari `architecture-decision-records.md` `ADR-006`.
- [ ] **Changelog** dalam kondisi terbaru — versi proyek saat ini (`0.1.3`) mencerminkan kondisi nyata repositori (rilis Governance Sync: resolusi `ADR-006` + sinkronisasi berantai ke `technology-decisions.md`, `SYSTEM-ARCHITECTURE.md`, `dependency-manifest.md`, `PROJECT-CONSTITUTION.md`, `AI-DEVELOPMENT-BLUEPRINT.md`/`development-playbook.md`).
- [ ] **Dependency antar dokumen konsisten** — tidak ada dokumen turunan yang mengklaim sesuatu "final" sementara dokumen sumbernya masih mencatat "terbuka" (lihat Governance Notes Bagian 15 untuk daftar ketidaksesuaian yang masih berjalan). **Item baru:** `technology-decisions.md`/`dependency-manifest.md` belum mencantumkan **Bolt.new** sebagai toolchain resmi, sesuai catatan kondisional Board di `ADR-001` — masih terbuka (lihat Governance Notes poin 7).
- [ ] **Source of Truth jelas** untuk setiap topik yang akan disentuh sprint — verifikasi lewat Bagian 7 dokumen ini sebelum menulis kode. **Diperbarui**: Bagian 7 kini membedakan `architecture-decision-records.md` (ADR per topik arsitektur) dari `decision-log.md` (jurnal kronologis lintas proyek).
- [ ] **Siap digunakan AI** — AI Context Pack, AI Development Blueprint, Task Template tersedia dan konsisten satu sama lain.
- [ ] **Functional Specification & UI Specification** — status Planned, belum ada file — direkomendasikan disusun sebelum atau paralel dengan Module Planning penuh (konsisten dengan `foundation-validation-report.md` §16, §18).
- [ ] **Nama individu Owner/Reviewer/Approver** ditetapkan untuk setiap dokumen di Bagian 9 & 10 — saat ini seluruhnya masih diisi peran, bukan nama (TBD), termasuk baris baru `architecture-decision-records.md`.

> **Status checklist saat ini:** **CONDITIONAL PASS** (istilah dipinjam dari `foundation-validation-report.md` Bagian 19) — proyek boleh melanjutkan ke Sprint S0 karena tidak ada temuan Critical yang memblokir fondasi teknis, namun item yang belum tercentang di atas sebaiknya diselesaikan paralel, terutama sebelum Module Planning penuh menyentuh modul yang berdampak pada Open Decision aktif (lihat Bagian 15).

---

# 15. Governance Notes

> Bagian ini murni **catatan rekomendasi hasil pengamatan** selama penyusunan dokumen governance ini. **Tidak ada isi dokumen sumber mana pun yang diubah** untuk menyusun catatan berikut — seluruhnya bersifat advisory, menunggu keputusan eksplisit manusia berwenang.

1. **Status "Draft" vs isi yang disebut "final"** — `technology-decisions.md`, `dependency-manifest.md`, `DEVELOPMENT-ROADMAP.md`, dan `AI-DEVELOPMENT-BLUEPRINT.md` seluruhnya berstatus dokumen "Draft — menunggu pengesahan tim", namun isinya berulang kali menyebut keputusan di dalamnya sebagai "resmi dan final" atau "mengikat". Ini menciptakan ambiguitas status Baseline (lihat Bagian 10) — direkomendasikan tim menetapkan Approval formal secara eksplisit (nama individu + tanggal) agar keempat dokumen ini dapat naik status menjadi Baseline yang konsisten dengan isinya.
2. **Klasifikasi versi v1.0 → v1.1 pada dokumen sumber** — berdasarkan kriteria Semantic Versioning di Bagian 5 dokumen ini, revisi v1.0→v1.1 pada PRD/ERD/API Spec/User Flow/SEO Spec (migrasi `city`→`city_id`, formalisasi role baru) mengandung elemen yang secara definisi lebih dekat ke **MAJOR** (breaking change konseptual) dibanding **MINOR**. Ini bukan kesalahan — `CHANGELOG.md` Aturan Wajib #3 sendiri sudah mengantisipasi hal ini ("selama fase Initial Development 0.y.z, kenaikan minor dapat menyertakan perubahan lebih besar dari biasanya"). Dicatat di sini murni sebagai referensi silang, bukan usulan mengubah penomoran yang sudah dipakai proyek.
3. **Dua dokumen yang disebutkan dalam daftar dokumen proyek namun tidak ditemukan sebagai file terpisah**: (a) **"Database Schema"** — dokumen ini disebut terpisah dari ERD pada permintaan penyusunan, namun repositori yang direview hanya berisi `ERD-Skema-Database-...v1.1.md` (skema **logis**) dan `ERD-Diagram-...v1.1.mermaid`; belum ada skema **fisik**/migration terpisah — ini konsisten dengan `CURRENT-PROJECT-STATE.md` yang menyatakan database fisik belum diinisialisasi (0% kode), sehingga kekosongan ini **diharapkan** pada tahap ini. (b) **"Executive Architecture Review"** — disebut ada dalam daftar dokumen proyek pada permintaan penyusunan, namun tidak ditemukan sebagai file dalam repositori yang direview untuk menyusun dokumen ini. Direkomendasikan tim mengklarifikasi apakah dokumen ini sudah ada di lokasi lain (belum diupload) atau memang belum dibuat — jika belum dibuat, statusnya di Bagian 10 tetap **Planned/TBD** sampai file tersedia.
4. **Functional Specification & UI Specification** — konsisten dengan temuan `foundation-validation-report.md` Bagian 4, 17, dan 18, kedua dokumen ini secara eksplisit **belum ada sebagai file** di repositori manapun yang direview. `AI-DEVELOPMENT-BLUEPRINT.md` Bagian 5 sendiri sudah menandai gap ini secara sadar ("bukan kesalahan penyusunan, melainkan keputusan sadar agar Blueprint tidak menciptakan sumber kebenaran palsu"). Baseline Register (Bagian 10) mencerminkan status ini apa adanya sebagai **Planned**, bukan memberi status yang tidak dapat diverifikasi.
5. **Duplikasi Known Issues dan Open Decisions** — `CHANGELOG.md` (bagian "Known Issues") dan `decision-log.md` (Bagian 11 "Open Decisions") mencatat sebagian besar item yang **sama** (arsitektur backend, Search Engine, Job Queue, Vercel, Google Maps, provider Email/Monitoring) dengan penomoran dan redaksi yang sedikit berbeda. `foundation-validation-report.md` Bagian 17 (LOW priority) sudah merekomendasikan konsolidasi keduanya menjadi satu daftar kanonik bersilang-referensi — dokumen governance ini tidak mengambil keputusan konsolidasi tersebut, hanya meneruskan rekomendasi yang sudah ada.
6. **Kepemilikan (Owner) seluruh dokumen governance masih berupa peran, bukan nama individu** — konsisten di `technology-decisions.md` §1, `decision-log.md` §1, `DEVELOPMENT-ROADMAP.md` §1 header. Direkomendasikan sebagai item tindak lanjut administratif sebelum Sprint S1 dimulai, agar Review & Approval Matrix (Bagian 9 dokumen ini) dapat diisi dengan nama nyata, bukan TBD.
7. **Sinkronisasi 27 Juli 2026 — `architecture-decision-records.md` masuk Baseline Register.** Dokumen ini sebelumnya **tidak terdaftar** di Bagian 10 meski sudah ada sebagai file terpisah — gap ini kini ditutup (baris baru ditambahkan, jumlah dokumen terdaftar naik dari 18 menjadi 19). ADR terbaru di dalamnya, `ADR-001` (Backend Architecture: Next.js Route Handlers, tanpa service Node terpisah), berstatus **Approved** (27 Jul 2026, hasil Architecture Review Board) dan telah disinkronkan secara kronologis ke `decision-log.md` sebagai `ADR-038`, serta ditandai `RESOLVED (0.1.1)` di `CHANGELOG.md` Known Issues #1. **Dua item turunan masih terbuka**, bukan diasumsikan selesai: (a) **Bolt.new** sebagai toolchain resmi proyek belum ditambahkan eksplisit ke `technology-decisions.md`/`dependency-manifest.md` — ini adalah catatan kondisional Architecture Review Board pada `ADR-001` itu sendiri; (b) redaksi usang di `SYSTEM-ARCHITECTURE.md` (batas eksekusi serverless Vercel) yang menjadi catatan kondisional kedua dari Board belum dieksekusi. Kedua item ini direkomendasikan menjadi prioritas sinkronisasi berikutnya sebelum `technology-decisions.md`/`SYSTEM-ARCHITECTURE.md` dapat naik status Baseline.
8. **Ambiguitas penomoran "ADR-" antar dua dokumen berbeda** — `architecture-decision-records.md` memakai penomoran independen `ADR-001`–`ADR-025` (per topik arsitektur), sedangkan `decision-log.md` memakai penomoran independen `ADR-001`–`ADR-039` (kronologis, lintas seluruh keputusan proyek termasuk non-teknis). Kedua ruang nomor **tidak berbagi identitas** meski memakai prefiks sama — dicatat secara eksplisit oleh `architecture-decision-records.md` sendiri (Bagian 2) sebagai potensi ambiguitas penamaan, **belum diputuskan** solusinya (mis. prefiks pembeda seperti `TADR-` vs `DADR-`) karena itu adalah keputusan tata kelola dokumen, bukan keputusan arsitektur — diteruskan di sini sebagai rekomendasi terbuka, bukan diputuskan sepihak oleh dokumen ini.
9. **Sinkronisasi 28 Juli 2026 — `ADR-005` (Search Strategy) diselesaikan Approved dan disinkronkan berantai.** Menyusul pola yang sama dengan `ADR-001` (lihat poin 7), `architecture-decision-records.md` `ADR-005` mengunci strategi bertahap — PostgreSQL Full-Text Search + `pg_trgm` untuk Fase 1, migrasi terjadwal ke Typesense di Fase 2 berdasarkan kriteria ambang eksplisit — sebagai keputusan **Approved** (hasil Architecture Review Board, 28 Jul 2026). Disinkronkan secara kronologis ke `decision-log.md` sebagai `ADR-039`, ditandai `RESOLVED (0.1.2)` di `CHANGELOG.md` Known Issues bagian Search Engine (dari butir #5), dan disinkronkan berantai ke `technology-decisions.md` (v1.1→v1.2), `SYSTEM-ARCHITECTURE.md` (v1.1→v1.2), `dependency-manifest.md` (v1.1→v1.2), `PROJECT-CONSTITUTION.md` (v1.2→v1.3), dan `AI-DEVELOPMENT-BLUEPRINT.md` (v1.1→v1.2) pada sesi yang sama — **bukan dua pekerjaan terpisah yang boleh drift**, konsisten dengan `PROJECT-CONSTITUTION.md` Bagian 25 poin 4. **Dua item turunan masih terbuka**, diwariskan dari catatan kondisional Board `ADR-005` itu sendiri: (a) proyeksi volume listing realistis 6–12 bulan pertama perlu dikonfirmasi tim bisnis untuk memvalidasi angka ambang migrasi 50.000 baris; (b) kapasitas DevOps/anggaran Typesense untuk Fase 2 perlu dikonfirmasi sebelum kriteria ambang tercapai. Dengan resolusi ini, **22 dari 25 ADR** kini berstatus Approved (naik dari 21), menyisakan **3 ADR OPEN** (`ADR-006` Job Queue, `ADR-008` Maps, `ADR-018` Caching) — item turunan `ADR-001` yang masih terbuka (Bolt.new, redaksi serverless) **tetap belum dieksekusi**, tidak diselesaikan oleh sesi ini.
10. **Sinkronisasi 29 Juli 2026 — `ADR-006` (Job Queue Strategy) diselesaikan Approved dan disinkronkan berantai.** Menyusul pola yang sama dengan `ADR-001` dan `ADR-005` (lihat poin 7 & 9), `architecture-decision-records.md` `ADR-006` mengunci strategi hybrid native — Vercel Cron Jobs (tugas terjadwal periodik) + Postgres Trigger/Database Webhook (tugas event-driven instan) untuk Fase 1, migrasi terjadwal ke QStash (Upstash) di Fase 2 berdasarkan kriteria ambang eksplisit — sebagai keputusan **Approved** (hasil Architecture Review Board, 29 Jul 2026). **BullMQ+Redis ditolak** karena worker long-running-nya tidak kompatibel dengan model serverless Vercel yang dikunci `ADR-001`. Disinkronkan secara kronologis ke `decision-log.md` sebagai `ADR-040`, ditandai `RESOLVED (0.1.3)` di `CHANGELOG.md` Known Issues #5 (kini sepenuhnya resolved bersama bagian Search Engine), dan disinkronkan berantai ke `technology-decisions.md` (v1.2→v1.3), `SYSTEM-ARCHITECTURE.md` (v1.2→v1.3), `dependency-manifest.md` (v1.2→v1.3), `PROJECT-CONSTITUTION.md` (v1.3→v1.4), dan `AI-DEVELOPMENT-BLUEPRINT.md`/`development-playbook.md` (v1.2→v1.3) pada sesi yang sama — **bukan dua pekerjaan terpisah yang boleh drift**, konsisten dengan `PROJECT-CONSTITUTION.md` Bagian 25 poin 4. **Dua item turunan masih terbuka**, diwariskan dari catatan kondisional Board `ADR-006` itu sendiri: (a) tier Vercel produksi (Hobby/Pro/Enterprise) perlu dikonfirmasi — menentukan batas jumlah/frekuensi Cron Jobs; (b) status resmi fitur Agent Workspace di roadmap perlu dikonfirmasi tim produk. Dengan resolusi ini, **23 dari 25 ADR** kini berstatus Approved (naik dari 22), menyisakan **2 ADR OPEN** (`ADR-008` Maps, `ADR-018` Caching) — item turunan `ADR-001` dan `ADR-005` yang masih terbuka (Bolt.new, redaksi serverless, proyeksi volume listing, anggaran Typesense) **tetap belum dieksekusi**, tidak diselesaikan oleh sesi ini. **Catatan penamaan:** `AI-DEVELOPMENT-BLUEPRINT.md` kini juga dirujuk sebagai `development-playbook.md` di dokumen-dokumen terbaru (lihat Bagian 10) — keduanya merujuk file yang sama, bukan dua dokumen terpisah; direkomendasikan konsolidasi nama tunggal pada revisi berikutnya.

---

*Dokumen ini adalah Document Governance & Baseline Register resmi proyek — meta-dokumen yang mengatur status, versi, baseline, ownership, dan lifecycle seluruh dokumen proyek. Tidak menggantikan `decision-log.md` (alasan keputusan) maupun `CHANGELOG.md` (riwayat perubahan). Wajib direview ulang setiap kali status/versi dokumen mana pun di Bagian 10 berubah material, dan menjadi acuan tetap bagi AI Coding Assistant maupun kontributor manusia dalam menentukan dokumen mana yang berwenang atas suatu informasi sepanjang siklus hidup proyek.*
