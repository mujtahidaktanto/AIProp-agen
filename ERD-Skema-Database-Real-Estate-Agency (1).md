# ERD & Skema Database — Platform Web Real Estate Agency

**Dokumen pendamping:** PRD-Real-Estate-Agency-Platform.md, User-Flow-Real-Estate-Agency-Platform.md
**Versi:** 1.0
**Tanggal:** 25 Juli 2026

Diagram visual ERD tersedia terpisah di file `ERD-Diagram.mermaid`. Dokumen ini berisi data dictionary lengkap (tabel, field, tipe data, constraint, relasi) per modul.

---

## 1. Daftar Entitas per Modul

| Modul | Entitas/Tabel |
|---|---|
| M1 — Auth & Registrasi | `users`, `agent_verification_documents` |
| M2 — Profil Agen | `agent_profiles` |
| M3 — Listing | `listings`, `listing_photos`, `listing_videos`, `amenities`, `listing_amenities`, `listing_price_history`, `listing_leads`, `listing_views` |
| M4 — Learning Center | `courses`, `course_lessons`, `quizzes`, `quiz_questions`, `quiz_options`, `enrollments`, `quiz_attempts`, `certificates` |
| M5 — Kalender Event | `events`, `event_registrations` |
| M6 — Katalog Developer | `developer_partners`, `developer_projects`, `developer_project_media`, `agent_project_claims` |
| M7 — Scoring DBR | `dbr_simulations`, `dbr_config` |
| M8 — Dashboard & Notifikasi | `notifications` |
| M9 — Admin/Sistem | `system_configs`, `audit_logs` |
| M10 — Role & Hak Akses (RBAC) | `roles`, `permissions`, `role_permissions` |
| Referensi Wilayah Indonesia | `ref_provinces`, `ref_cities`, `ref_districts`, `ref_villages` |

---

## 2. Data Dictionary Detail

### 2.1 `users` (Modul 1 — tabel induk semua role)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| email | VARCHAR(255) | UNIQUE, NOT NULL | |
| phone | VARCHAR(20) | UNIQUE | |
| password_hash | VARCHAR(255) | NOT NULL | |
| role_id | UUID/BIGINT | FK → roles.id, NOT NULL | Merujuk ke tabel `roles` (superadmin/manager/admin/agent/instructor/developer_partner) agar mendukung role kustom (Modul 10) |
| status | ENUM | NOT NULL, default `pending_review` | `pending_review`, `active`, `suspended`, `rejected` |
| email_verified_at | TIMESTAMP | NULLABLE | |
| last_login_at | TIMESTAMP | NULLABLE | |
| created_at | TIMESTAMP | NOT NULL | |
| updated_at | TIMESTAMP | NOT NULL | |

### 2.2 `agent_verification_documents` (Modul 1)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| user_id | UUID/BIGINT | FK → users.id | |
| doc_type | ENUM | NOT NULL | `ktp`, `npwp`, `sertifikasi_rei`, `lainnya` |
| file_url | VARCHAR(500) | NOT NULL | |
| encrypted | BOOLEAN | default true | Data sensitif wajib terenkripsi |
| review_status | ENUM | default `pending` | `pending`, `approved`, `rejected` |
| reviewed_by | UUID/BIGINT | FK → users.id, NULLABLE | Admin yang review |
| rejection_reason | TEXT | NULLABLE | |
| created_at | TIMESTAMP | | |

### 2.3 `agent_profiles` (Modul 2)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| user_id | UUID/BIGINT | FK → users.id, UNIQUE | 1:1 dengan users |
| full_name | VARCHAR(150) | NOT NULL | |
| avatar_url | VARCHAR(500) | NULLABLE | |
| bio | TEXT | NULLABLE | |
| specialization | ENUM/ARRAY | NULLABLE | `residensial`, `komersial`, `tanah`, `sewa` |
| coverage_area | VARCHAR(255) | NULLABLE | Area operasional (multi-kota, disimpan JSON/array) |
| office_name | VARCHAR(150) | NULLABLE | Nama kantor/brokerage |
| license_number | VARCHAR(50) | NULLABLE | |
| whatsapp_number | VARCHAR(20) | NOT NULL | Default untuk CTA listing |
| contact_visibility | ENUM | default `public` | `public`, `hidden` |
| public_slug | VARCHAR(150) | UNIQUE | Untuk URL profil publik |
| total_listings_sold | INT | default 0 | Dihitung otomatis (trigger/cron dari `listings`) |
| total_listings_rented | INT | default 0 | |
| created_at / updated_at | TIMESTAMP | | |

> Badge & sertifikat agen **tidak disimpan sebagai field terpisah** di sini — ditampilkan via query relasi ke `certificates` (Modul 4) berdasarkan `user_id`.

---

### 2.4 `listings` (Modul 3 — tabel inti)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| agent_id | UUID/BIGINT | FK → users.id, NOT NULL | Ownership per-agen |
| developer_project_id | UUID/BIGINT | FK → developer_projects.id, NULLABLE | Terisi jika kategori Primary & tertaut proyek |
| category | ENUM | NOT NULL | `primary`, `secondary` |
| transaction_type | ENUM | NOT NULL | `sale`, `rent` |
| title | VARCHAR(200) | NOT NULL | |
| description | TEXT | NULLABLE | |
| property_type | ENUM | NOT NULL | `rumah`, `apartemen`, `ruko`, `tanah`, `gudang`, `kavling`, `lainnya` |
| price | DECIMAL(18,2) | NOT NULL | |
| price_unit | ENUM | NULLABLE | `total`, `per_bulan`, `per_tahun` (relevan utk sewa) |
| is_negotiable | BOOLEAN | default false | |
| address | VARCHAR(500) | NOT NULL | Jalan & nomor (freetext) |
| province_id | UUID/BIGINT | FK → ref_provinces.id, NOT NULL | Dipilih dari database referensi wilayah, bukan isi bebas |
| city_id | UUID/BIGINT | FK → ref_cities.id, NOT NULL | Opsi mengikuti `province_id` terpilih (cascading) |
| district_id | UUID/BIGINT | FK → ref_districts.id, NOT NULL | Opsi mengikuti `city_id` terpilih (cascading) |
| area_keyword | VARCHAR(20) | NULLABLE | Nama wilayah/kawasan freetext pelengkap (mis. "BSD City") — keyword tambahan untuk pencarian, bukan pengganti data administratif |
| latitude | DECIMAL(10,7) | NULLABLE | |
| longitude | DECIMAL(10,7) | NULLABLE | |
| land_area | DECIMAL(10,2) | NULLABLE | m² |
| building_area | DECIMAL(10,2) | NULLABLE | m² |
| bedrooms | SMALLINT | NULLABLE | |
| bathrooms | SMALLINT | NULLABLE | |
| floors | SMALLINT | NULLABLE | |
| carport_capacity | SMALLINT | NULLABLE | |
| electrical_power | INT | NULLABLE | Watt |
| water_source | ENUM | NULLABLE | `pdam`, `sumur`, `lainnya` |
| furnishing | ENUM | NULLABLE | `unfurnished`, `semi_furnished`, `fully_furnished` |
| year_built | SMALLINT | NULLABLE | |
| certificate_type | ENUM | NULLABLE | `shm`, `hgb`, `girik`, `ppjb`, `strata_title`, `lainnya` |
| certificate_transferred | BOOLEAN | NULLABLE | Sudah balik nama atau belum |
| imb_status | ENUM | NULLABLE | `ada`, `tidak_ada`, `dalam_proses` |
| dispute_free_declared | BOOLEAN | default false | Pernyataan agen bebas sengketa |
| whatsapp_number | VARCHAR(20) | NOT NULL | Override dari agent_profiles jika diisi |
| status | ENUM | NOT NULL, default `draft` | `draft`, `pending_review`, `published`, `sold`, `rented`, `expired`, `rejected` |
| rejection_reason | TEXT | NULLABLE | |
| view_count | INT | default 0 | |
| cta_click_count | INT | default 0 | Denormalized counter dari `listing_leads` |
| published_at | TIMESTAMP | NULLABLE | |
| expired_at | TIMESTAMP | NULLABLE | |
| sold_or_rented_at | TIMESTAMP | NULLABLE | |
| created_at / updated_at | TIMESTAMP | | |

### 2.5 `listing_photos`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| listing_id | UUID/BIGINT | FK → listings.id |
| url | VARCHAR(500) | NOT NULL |
| is_cover | BOOLEAN | default false |
| sort_order | SMALLINT | default 0 |

### 2.6 `listing_videos`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| listing_id | UUID/BIGINT | FK → listings.id |
| url | VARCHAR(500) | NOT NULL |
| type | ENUM | `video`, `virtual_tour` |

### 2.7 `amenities` (master) & `listing_amenities` (pivot)
```
amenities: id (PK), name (VARCHAR, e.g. "Kolam Renang", "Keamanan 24 Jam")
listing_amenities: listing_id (FK), amenity_id (FK)  -- composite PK
```

### 2.8 `listing_price_history`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| listing_id | UUID/BIGINT | FK → listings.id |
| old_price | DECIMAL(18,2) | |
| new_price | DECIMAL(18,2) | |
| changed_at | TIMESTAMP | |

### 2.9 `listing_leads` (pencatatan klik CTA WhatsApp)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| listing_id | UUID/BIGINT | FK → listings.id | |
| agent_id | UUID/BIGINT | FK → users.id | Denormalized untuk query cepat dashboard |
| source | ENUM | default `whatsapp_cta` | |
| ip_address | VARCHAR(45) | NULLABLE | |
| user_agent | VARCHAR(255) | NULLABLE | |
| created_at | TIMESTAMP | | |

### 2.10 `listing_views` (opsional, analitik traffic)
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| listing_id | UUID/BIGINT | FK → listings.id |
| viewed_at | TIMESTAMP | |

---

### 2.11 `developer_partners` (Modul 6)
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| company_name | VARCHAR(200) | NOT NULL |
| pic_name | VARCHAR(150) | NULLABLE |
| pic_contact | VARCHAR(50) | NULLABLE |
| user_id | UUID/BIGINT | FK → users.id, NULLABLE (jika partner punya login) |
| status | ENUM | `active`, `inactive` |
| created_at | TIMESTAMP | |

### 2.12 `developer_projects`
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| developer_id | UUID/BIGINT | FK → developer_partners.id | |
| name | VARCHAR(200) | NOT NULL | |
| location | VARCHAR(255) | | |
| city | VARCHAR(100) | | |
| property_type | ENUM | | Sama seperti listings.property_type |
| price_min | DECIMAL(18,2) | | |
| price_max | DECIMAL(18,2) | | |
| unit_availability | INT | | Jumlah unit tersedia |
| commission_scheme | VARCHAR(255) | | Deskripsi/persentase komisi |
| is_exclusive_by_region | BOOLEAN | default false | |
| status | ENUM | `active`, `coming_soon`, `sold_out`, `inactive` | |
| created_at / updated_at | TIMESTAMP | | |

### 2.13 `developer_project_media`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| project_id | UUID/BIGINT | FK → developer_projects.id |
| type | ENUM | `photo`, `video`, `brochure`, `price_list` |
| url | VARCHAR(500) | NOT NULL |

### 2.14 `agent_project_claims`
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| agent_id | UUID/BIGINT | FK → users.id | |
| project_id | UUID/BIGINT | FK → developer_projects.id | |
| claimed_at | TIMESTAMP | | |
| UNIQUE | (agent_id, project_id) | | Agen tidak bisa klaim proyek yang sama 2x |

---

### 2.15 `courses` (Modul 4)
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| title | VARCHAR(200) | NOT NULL |
| category | ENUM | `sales_skill`, `legal_regulasi`, `produk_developer`, `financial_kpr`, `lainnya` |
| description | TEXT | |
| prerequisite_course_id | UUID/BIGINT | FK → courses.id, NULLABLE |
| passing_grade | SMALLINT | default 70 |
| status | ENUM | `draft`, `published`, `archived` |
| created_by | UUID/BIGINT | FK → users.id |
| created_at / updated_at | TIMESTAMP | |

### 2.16 `course_lessons`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| course_id | UUID/BIGINT | FK → courses.id |
| title | VARCHAR(200) | |
| content_type | ENUM | `video`, `pdf`, `slide` |
| content_url | VARCHAR(500) | |
| sort_order | SMALLINT | |

### 2.17 `quizzes`, `quiz_questions`, `quiz_options`
```
quizzes: id (PK), course_id (FK → courses.id), title

quiz_questions: id (PK), quiz_id (FK → quizzes.id), question_text, question_type (single_choice/multi_choice)

quiz_options: id (PK), question_id (FK → quiz_questions.id), option_text, is_correct (BOOLEAN)
```

### 2.18 `enrollments`
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| agent_id | UUID/BIGINT | FK → users.id | |
| course_id | UUID/BIGINT | FK → courses.id | |
| status | ENUM | `in_progress`, `completed` | |
| progress_percent | SMALLINT | default 0 | |
| enrolled_at | TIMESTAMP | | |
| completed_at | TIMESTAMP | NULLABLE | |
| UNIQUE | (agent_id, course_id) | | |

### 2.19 `quiz_attempts`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| enrollment_id | UUID/BIGINT | FK → enrollments.id |
| quiz_id | UUID/BIGINT | FK → quizzes.id |
| score | DECIMAL(5,2) | |
| passed | BOOLEAN | |
| attempted_at | TIMESTAMP | |

### 2.20 `certificates`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| agent_id | UUID/BIGINT | FK → users.id |
| course_id | UUID/BIGINT | FK → courses.id |
| certificate_url | VARCHAR(500) | |
| issued_at | TIMESTAMP | |
| UNIQUE | (agent_id, course_id) | |

---

### 2.21 `events` (Modul 5)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| title | VARCHAR(200) | NOT NULL | |
| category | ENUM | `training`, `launching_proyek`, `open_house`, `gathering` | |
| description | TEXT | | |
| is_online | BOOLEAN | default false | |
| location | VARCHAR(255) | NULLABLE | |
| meeting_link | VARCHAR(500) | NULLABLE | |
| host | VARCHAR(150) | NULLABLE | |
| quota | INT | NULLABLE | NULL = tanpa batas |
| related_course_id | UUID/BIGINT | FK → courses.id, NULLABLE | Jika event = kelas live |
| related_project_id | UUID/BIGINT | FK → developer_projects.id, NULLABLE | Jika event = launching proyek |
| submitted_by | UUID/BIGINT | FK → users.id, NULLABLE | Developer partner pengaju |
| status | ENUM | `pending_approval`, `published`, `rejected`, `cancelled` | |
| start_at | TIMESTAMP | NOT NULL | |
| end_at | TIMESTAMP | NULLABLE | |
| created_at / updated_at | TIMESTAMP | | |

### 2.22 `event_registrations`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| event_id | UUID/BIGINT | FK → events.id |
| agent_id | UUID/BIGINT | FK → users.id |
| status | ENUM | `registered`, `waitlist`, `attended`, `cancelled` |
| registered_at | TIMESTAMP | |
| UNIQUE | (event_id, agent_id) | |

---

### 2.23 `dbr_simulations` (Modul 7)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| agent_id | UUID/BIGINT | FK → users.id | |
| listing_id | UUID/BIGINT | FK → listings.id, NULLABLE | Jika simulasi dibuka dari listing |
| prospect_name | VARCHAR(150) | NULLABLE | |
| prospect_phone | VARCHAR(20) | NULLABLE | |
| net_income | DECIMAL(18,2) | NOT NULL | *(data sensitif — akses terbatas agen pemilik & admin)* |
| existing_installments | DECIMAL(18,2) | default 0 | *(data sensitif)* |
| property_price | DECIMAL(18,2) | NOT NULL | |
| down_payment | DECIMAL(18,2) | NOT NULL | |
| loan_amount | DECIMAL(18,2) | NOT NULL | Computed: property_price - down_payment |
| tenor_months | SMALLINT | NOT NULL | |
| interest_rate_annual | DECIMAL(5,2) | NOT NULL | |
| monthly_installment | DECIMAL(18,2) | NOT NULL | Hasil kalkulasi anuitas |
| dbr_percent | DECIMAL(5,2) | NOT NULL | |
| eligibility_status | ENUM | NOT NULL | `layak`, `perlu_review`, `tidak_layak` |
| pdf_export_url | VARCHAR(500) | NULLABLE | |
| created_at | TIMESTAMP | | |

### 2.24 `dbr_config` (parameter global, dikelola Admin — Modul 9)
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| dbr_threshold_percent | DECIMAL(5,2) | default 35.00 |
| default_interest_rate | DECIMAL(5,2) | default 8.50 |
| updated_by | UUID/BIGINT | FK → users.id |
| updated_at | TIMESTAMP | |

---

### 2.25 `notifications` (Modul 8)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| user_id | UUID/BIGINT | FK → users.id | Penerima |
| type | ENUM | `approval_status`, `event_reminder`, `listing_expiring`, `certificate_issued`, `lead_new`, `lainnya` | |
| title | VARCHAR(200) | | |
| message | TEXT | | |
| related_entity_type | VARCHAR(50) | NULLABLE | mis. `listing`, `event`, `course` |
| related_entity_id | UUID/BIGINT | NULLABLE | |
| is_read | BOOLEAN | default false | |
| created_at | TIMESTAMP | | |

---

### 2.26 `system_configs` (Modul 9)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| config_key | VARCHAR(100) | UNIQUE | mis. `listing_expiry_days`, `max_active_listing_per_tier` |
| config_value | VARCHAR(255) | | |
| updated_by | UUID/BIGINT | FK → users.id | |
| updated_at | TIMESTAMP | | |

### 2.27 `audit_logs`
| Field | Tipe | Constraint |
|---|---|---|
| id | UUID/BIGINT | PK |
| user_id | UUID/BIGINT | FK → users.id |
| action | VARCHAR(100) | mis. `approve_listing`, `reject_agent` |
| entity_type | VARCHAR(50) | |
| entity_id | UUID/BIGINT | |
| old_value | JSON | NULLABLE |
| new_value | JSON | NULLABLE |
| created_at | TIMESTAMP | |

---

### 2.28 `roles` (Modul 10)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| code | VARCHAR(50) | UNIQUE, NOT NULL | `superadmin`, `admin`, `manager`, `agent`, `instructor`, `developer_partner`, atau kode role kustom |
| name | VARCHAR(100) | NOT NULL | Nama tampilan |
| is_system_role | BOOLEAN | default true | `true` untuk 4 role inti + 2 role eksternal bawaan; `false` untuk role kustom buatan Superadmin |
| is_protected | BOOLEAN | default false | `true` khusus untuk `superadmin` — mencegah role ini dihapus/di-nonaktifkan |
| created_at / updated_at | TIMESTAMP | | |

### 2.29 `permissions` (Modul 10)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| module_code | VARCHAR(50) | NOT NULL | mis. `M1_registration`, `M3_listing`, `M7_dbr` |
| action_code | VARCHAR(50) | NOT NULL | mis. `create`, `read`, `update`, `delete`, `approve` |
| scope_type | ENUM | default `own` | `all` (global — dipakai Superadmin/Manager/Admin), `own` (data milik sendiri — dipakai Agen), `none` |
| description | VARCHAR(255) | | Deskripsi human-readable, mis. "Approve registrasi agen baru" |
| UNIQUE | (module_code, action_code) | | Satu baris permission per kombinasi modul+aksi |

### 2.30 `role_permissions` (pivot, Modul 10)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| role_id | UUID/BIGINT | FK → roles.id | |
| permission_id | UUID/BIGINT | FK → permissions.id | |
| granted_scope | ENUM | `all`, `own`, `none` | Override scope default dari `permissions.scope_type` — inilah yang diedit lewat Permission Matrix Editor |
| editable_by_role_code | VARCHAR(50) | default `superadmin` | Menentukan role mana yang **berwenang mengubah baris ini**. Baris dengan `role_id` merujuk ke role `agent` bernilai `superadmin,manager` (baik Superadmin maupun Manager boleh mengubah); baris lain (role Admin/Manager/Superadmin, serta seluruh baris di modul konfigurasi sistem/keamanan) bernilai `superadmin` saja |
| updated_by | UUID/BIGINT | FK → users.id | User yang terakhir mengubah |
| updated_at | TIMESTAMP | | |
| UNIQUE | (role_id, permission_id) | | |

> Baris untuk `role_id` = `superadmin` **tidak pernah dibaca untuk pembatasan** — di level aplikasi, role `superadmin` selalu bypass pengecekan tabel ini (hardcoded full-access), sesuai business rule di PRD Modul 10.
> Field `editable_by_role_code` adalah mekanisme utama yang menegakkan aturan "Manager hanya boleh mengubah permission role Agen" — dicek di level aplikasi sebelum mengizinkan request `UPDATE` ke tabel ini.

---

### 2.33 `ref_provinces` (Referensi Wilayah — mendukung API Bagian 8)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| code | VARCHAR(10) | UNIQUE, NOT NULL | Kode wilayah resmi (mis. kode Kemendagri) |
| name | VARCHAR(100) | NOT NULL | mis. "Banten", "Jawa Barat" |

### 2.34 `ref_cities`
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| province_id | UUID/BIGINT | FK → ref_provinces.id, NOT NULL | |
| code | VARCHAR(10) | UNIQUE, NOT NULL | |
| name | VARCHAR(100) | NOT NULL | mis. "Tangerang Selatan" |
| type | ENUM | NOT NULL | `kota`, `kabupaten` |

### 2.35 `ref_districts`
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| city_id | UUID/BIGINT | FK → ref_cities.id, NOT NULL | |
| code | VARCHAR(10) | UNIQUE, NOT NULL | |
| name | VARCHAR(100) | NOT NULL | mis. "Serpong" |

### 2.36 `ref_villages` (kelurahan/desa — disiapkan untuk API Bagian 8 & kebutuhan lain seperti alamat profil, belum dipakai `listings` saat ini karena form listing hanya sampai level Kecamatan)
| Field | Tipe | Constraint | Keterangan |
|---|---|---|---|
| id | UUID/BIGINT | PK | |
| district_id | UUID/BIGINT | FK → ref_districts.id, NOT NULL | |
| code | VARCHAR(10) | UNIQUE, NOT NULL | |
| name | VARCHAR(100) | NOT NULL | |
| postal_code | VARCHAR(6) | NULLABLE | |

> **Sumber data:** di-seed sekali dari dataset wilayah administratif resmi (mis. data Kemendagri) dan di-*host* di database internal — bukan dipanggil ke API pihak ketiga tiap request (sudah disepakati di API Specification Bagian 8), karena data ini relatif statis.

---

## 3. Ringkasan Relasi Kunci (Foreign Key Map)

| Tabel Anak | Foreign Key | Tabel Induk | Kardinalitas |
|---|---|---|---|
| agent_profiles | user_id | users | 1 : 1 |
| agent_verification_documents | user_id | users | N : 1 |
| listings | agent_id | users | N : 1 |
| listings | developer_project_id | developer_projects | N : 1 (nullable) |
| listings | province_id | ref_provinces | N : 1 |
| listings | city_id | ref_cities | N : 1 |
| listings | district_id | ref_districts | N : 1 |
| ref_cities | province_id | ref_provinces | N : 1 |
| ref_districts | city_id | ref_cities | N : 1 |
| ref_villages | district_id | ref_districts | N : 1 |
| listing_photos / videos | listing_id | listings | N : 1 |
| listing_amenities | listing_id, amenity_id | listings, amenities | N : N |
| listing_price_history | listing_id | listings | N : 1 |
| listing_leads | listing_id, agent_id | listings, users | N : 1 |
| developer_projects | developer_id | developer_partners | N : 1 |
| developer_project_media | project_id | developer_projects | N : 1 |
| agent_project_claims | agent_id, project_id | users, developer_projects | N : N |
| course_lessons | course_id | courses | N : 1 |
| quizzes | course_id | courses | N : 1 |
| quiz_questions | quiz_id | quizzes | N : 1 |
| quiz_options | question_id | quiz_questions | N : 1 |
| enrollments | agent_id, course_id | users, courses | N : N (via tabel ini) |
| quiz_attempts | enrollment_id | enrollments | N : 1 |
| certificates | agent_id, course_id | users, courses | N : N (via tabel ini) |
| events | related_course_id | courses | N : 1 (nullable) |
| events | related_project_id | developer_projects | N : 1 (nullable) |
| event_registrations | event_id, agent_id | events, users | N : N (via tabel ini) |
| dbr_simulations | agent_id | users | N : 1 |
| dbr_simulations | listing_id | listings | N : 1 (nullable) |
| notifications | user_id | users | N : 1 |
| users | role_id | roles | N : 1 |
| role_permissions | role_id, permission_id | roles, permissions | N : N (via tabel ini) |

---

## 4. Catatan Desain & Keamanan Data

1. **Enkripsi field sensitif**: `agent_verification_documents.file_url` (dokumen KTP/NPWP) dan seluruh field finansial di `dbr_simulations` (`net_income`, `existing_installments`) wajib dienkripsi at-rest dan dibatasi akses hanya untuk pemilik data (agent_id) & role `admin`.
2. **Denormalisasi terkontrol**: `listings.cta_click_count` dan `agent_profiles.total_listings_sold/rented` adalah counter yang didenormalisasi dari tabel transaksional (`listing_leads`, `listings`) untuk performa query dashboard — perlu update via trigger/scheduled job, bukan dihitung on-the-fly setiap request.
3. **Soft delete** disarankan (kolom `deleted_at`) untuk tabel `listings`, `users`, `developer_projects` agar data riwayat (statistik, audit) tidak hilang saat "dihapus".
4. **Indexing prioritas**: `listings(status, category, transaction_type, city_id, price)` untuk mendukung filter pencarian; `listing_leads(listing_id, created_at)` untuk agregasi dashboard; `dbr_simulations(agent_id, created_at)` untuk riwayat prospek. Kolom `area_keyword` disarankan memakai index full-text/trigram terpisah agar pencarian keyword kawasan tetap cepat meski bersifat freetext.
5. **Konsistensi data Primary Listing**: field `listings.price`, spesifikasi, dan materi pada listing berkategori `primary` yang tertaut `developer_project_id` sebaiknya divalidasi di level aplikasi (bukan hard constraint DB) agar tetap sinkron dengan `developer_projects`, namun tetap mengizinkan agen menambah deskripsi/foto tambahan.
6. **Enforcement RBAC di level backend**: setiap query/endpoint yang menyentuh data ber-scope (`listings`, `dbr_simulations`, `agent_profiles`, dsb) wajib menambahkan filter `WHERE agent_id = :current_user_id` ketika `role_permissions.granted_scope = 'own'` (berlaku untuk role Agen). Untuk role Superadmin/Manager/Admin dengan `granted_scope = 'all'`, query berjalan tanpa filter kepemilikan (akses global) — namun **tetap wajib melalui pengecekan permission**, bukan hanya "tidak ada filter". Middleware juga wajib menolak (403) setiap permintaan `UPDATE`/`DELETE` dari Agen terhadap baris `listings`/`agent_profiles` yang `agent_id`-nya bukan miliknya, terlepas dari nilai permission apa pun — aturan ini bersifat hard rule di kode aplikasi, bukan sekadar konfigurasi.
7. **Enforcement pembatasan Manager pada `role_permissions`**: sebelum mengizinkan request `UPDATE` ke tabel `role_permissions`, aplikasi wajib memvalidasi `editable_by_role_code` pada baris target memuat kode role si pengubah — mis. Manager hanya boleh `UPDATE` baris dengan `role_id` = role `agent`; percobaan mengubah baris lain (role Admin/Manager/Superadmin, atau modul `M9_system_config`/`M9_security`) ditolak (403) meski request datang dari akun Manager yang valid.
8. **Role `superadmin` sebagai bypass tunggal**: di level kode aplikasi, cek permission untuk role `superadmin` sebaiknya di-short-circuit (selalu `true`) sebelum melakukan query ke `role_permissions`, agar konsisten dengan business rule "Superadmin tidak dapat dibatasi" tanpa bergantung pada data yang bisa saja tidak lengkap/salah konfigurasi.
9. **Safety guard akun Superadmin terakhir**: tambahkan constraint aplikasi (bukan constraint SQL) yang mencegah `UPDATE`/`DELETE` pada `users` jika hasilnya membuat jumlah user aktif dengan `role_id = (SELECT id FROM roles WHERE code='superadmin')` menjadi 0.
10. **Konsistensi referensi wilayah belum menyeluruh**: saat ini hanya `listings` yang bermigrasi memakai `province_id`/`city_id`/`district_id`. Tabel `developer_projects.city` masih freetext (VARCHAR) — direkomendasikan ikut dimigrasi ke `city_id` (FK → `ref_cities`) pada iterasi berikutnya agar data lokasi proyek developer konsisten dan bisa difilter bersama listing biasa di pencarian.

---

*Diagram visual (ERD) tersedia di file terpisah `ERD-Diagram.mermaid`. Dokumen ini menjadi acuan untuk implementasi migrasi database pada tahap development.*
