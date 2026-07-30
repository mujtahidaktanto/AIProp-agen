# API Specification — Platform Web Real Estate Agency

**Dokumen pendamping:** PRD, User Flow, ERD & Skema Database
**Versi:** 1.0
**Tanggal:** 26 Juli 2026
**Base URL:** `https://api.<domain-anda>.id/api/v1`

---

## 0. Konvensi Umum

### 0.1 Autentikasi
- **JWT Bearer Token** untuk seluruh endpoint yang butuh login: header `Authorization: Bearer {access_token}`.
- **Access token**: umur pendek (mis. 15–60 menit). **Refresh token**: umur panjang (mis. 30 hari), disimpan sbg httpOnly cookie atau secure storage di client.
- **OAuth2 Google Login** didukung sebagai alternatif registrasi/login (lihat 1.3) — hasil akhirnya tetap menerbitkan JWT internal platform, sehingga seluruh endpoint lain tidak perlu tahu apakah user login via password atau Google.
- Setiap endpoint di dokumen ini diberi label **Auth**: `Public` (tanpa login), `Authenticated` (butuh login, role apa pun), atau **role spesifik** (`Superadmin`, `Manager`, `Admin`, `Agen`, `Buyer`, `Developer Partner`) sesuai matriks RBAC di PRD Modul 10.

### 0.2 Format Response Standar
```json
// Sukses
{
  "success": true,
  "data": { ... },
  "meta": { "page": 1, "per_page": 20, "total": 134 }
}

// Gagal
{
  "success": false,
  "error": {
    "code": "LISTING_NOT_FOUND",
    "message": "Listing tidak ditemukan atau Anda tidak memiliki akses.",
    "details": null
  }
}
```

### 0.3 Kode Status HTTP Utama
| Kode | Arti |
|---|---|
| 200 / 201 | Sukses (GET/PUT/PATCH) / Sukses dibuat (POST) |
| 400 | Bad Request — validasi input gagal |
| 401 | Unauthorized — token tidak ada/invalid/expired |
| 403 | Forbidden — token valid tapi tidak punya permission (lihat Modul 10 RBAC) |
| 404 | Resource tidak ditemukan (atau ditemukan tapi bukan milik user → disamarkan jadi 404 untuk data privat) |
| 409 | Konflik (mis. email sudah terdaftar, klaim proyek duplikat) |
| 422 | Unprocessable Entity — validasi bisnis gagal (mis. DBR melebihi threshold saat submit final) |
| 429 | Too Many Requests — rate limit |

### 0.4 Pagination & Filtering
Query param standar untuk semua endpoint list: `?page=1&per_page=20&sort=created_at&order=desc`. Filter spesifik dijelaskan per endpoint.

### 0.5 Rate Limiting
- Publik (unauthenticated): 60 req/menit/IP.
- Authenticated: 300 req/menit/user.
- Endpoint sensitif (login, register, OTP, forgot-password): 5 req/menit/IP+identifier untuk mencegah brute-force.

### 0.6 Penegakan RBAC di Level API
Setiap request diverifikasi middleware terhadap tabel `role_permissions` (lihat ERD Modul 10) sebelum handler dijalankan:
1. Cek token valid → identifikasi `user.role_id`.
2. Cek permission untuk kombinasi `module_code + action_code` dari endpoint yang diakses.
3. Jika `granted_scope = own`, query otomatis difilter `WHERE agent_id = current_user.id`.
4. Jika `granted_scope = all`, tidak ada filter kepemilikan (berlaku Superadmin/Manager/Admin).
5. Role `superadmin` selalu bypass (lihat catatan ERD).

---

## 1. Authentication & User Management API
*(Selaras dengan PRD Modul 1 — Registrasi & Autentikasi, Modul 2 — Profil Agen, dan Modul 10 — RBAC)*

> **Catatan penyesuaian peran:** dokumen ini memakai istilah teknis yang konsisten dengan RBAC di PRD — `Public User` = **Guest** (tanpa akun), `Buyer` = akun terdaftar ringan untuk pencari properti (baru ditambahkan agar mendukung fitur simpan listing/lead tracking pribadi), `Registered Agent` = **Agen**, dan `Super Admin` tetap **Superadmin**. Untuk `Agency/Principal` (kantor broker): **telah dikonfirmasi tidak menjadi role sistem tersendiri** — cukup memakai atribut `office_name` pada profil agen (lihat ERD `agent_profiles`) untuk mengelompokkan agen per kantor, tanpa perlu login/akses khusus level kantor.

### 1.1 Registrasi & Login

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| POST | `/auth/register` | Public | Registrasi akun baru dengan pilihan `role`: `buyer` atau `agent` |
| POST | `/auth/verify-otp` | Public | Verifikasi kode OTP (email/SMS) setelah registrasi |
| POST | `/auth/resend-otp` | Public | Kirim ulang OTP |
| POST | `/auth/login` | Public | Login email/HP + password → menerbitkan JWT |
| POST | `/auth/oauth/google` | Public | Login/registrasi via Google Sign-In |
| POST | `/auth/refresh` | Public (butuh refresh token valid) | Tukar refresh token → access token baru |
| POST | `/auth/logout` | Authenticated | Invalidasi refresh token (single device) |
| POST | `/auth/logout-all` | Authenticated | Invalidasi seluruh sesi/device |
| POST | `/auth/forgot-password` | Public | Kirim link reset password ke email |
| POST | `/auth/reset-password` | Public (butuh reset token) | Set password baru |

**Contoh — Registrasi:**
```json
POST /api/v1/auth/register
{
  "role": "agent",                // "buyer" | "agent"
  "full_name": "Andi Wijaya",
  "email": "andi@example.com",
  "phone": "+6281234567890",
  "password": "********"
}

201 Created
{
  "success": true,
  "data": {
    "user_id": "usr_8f2a...",
    "role": "agent",
    "status": "pending_review",     // agent → wajib approval; buyer → langsung active setelah verifikasi OTP
    "otp_sent_to": "andi@example.com"
  }
}
```

**Contoh — Login via Google (OAuth2):**
```json
POST /api/v1/auth/oauth/google
{
  "id_token": "eyJhbGciOi...",      // Google ID Token dari Google Sign-In SDK (client-side)
  "intended_role": "buyer"          // hanya dipakai jika akun belum pernah terdaftar
}

200 OK
{
  "success": true,
  "data": {
    "access_token": "...",
    "refresh_token": "...",
    "user": { "id": "usr_...", "role": "buyer", "is_new_user": false }
  }
}
```
> Server memverifikasi `id_token` ke Google Token Info endpoint (server-side), lalu mencocokkan `email` dengan user existing (link akun) atau membuat user baru berstatus `active` (untuk role `buyer`) — role `agent` via Google tetap melalui alur `pending_review` karena wajib upload dokumen legalitas.

### 1.2 Profil & Verifikasi (Modul 2, Modul 1)

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| GET | `/users/me` | Authenticated | Data akun & profil user yang sedang login |
| PUT | `/users/profile` | Authenticated | Update profil (bio, foto, spesialisasi, WA, dsb) |
| POST | `/users/verification-documents` | Agen | Upload dokumen legalitas (KTP/NPWP/sertifikasi) |
| GET | `/agents/{id}` | Public | Profil publik agen (untuk halaman `domain.com/agen/{slug}`) |
| GET | `/agents/{id}/credentials` | Public | Lencana verifikasi, badge sertifikasi (Modul 4), statistik listing terjual/tersewa |
| GET | `/agents/{id}/reviews` | Public | Daftar ulasan/rating dari klien (jika fitur review diaktifkan) |
| GET | `/admin/agents/pending` | Superadmin, Manager, Admin | Daftar agen menunggu approval registrasi |
| PUT | `/admin/agents/{id}/approve` | Superadmin, Manager, Admin | Approve registrasi agen |
| PUT | `/admin/agents/{id}/reject` | Superadmin, Manager, Admin | Reject dengan alasan |
| PUT | `/admin/agents/{id}/suspend` | Superadmin, Manager, Admin | Suspend akun agen |

### 1.3 Manajemen Role & Permission (Modul 10)

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| GET | `/admin/roles` | Superadmin, Manager | Daftar role & jumlah user per role |
| GET | `/admin/permissions/matrix` | Superadmin | Ambil seluruh matriks permission (semua role) |
| GET | `/admin/permissions/matrix/agent` | Superadmin, Manager | Ambil matriks permission **khusus role Agen** |
| PUT | `/admin/permissions/matrix` | Superadmin | Update permission role apa pun (termasuk Manager/Admin) |
| PUT | `/admin/permissions/matrix/agent` | Superadmin, Manager | Update permission **khusus role Agen** (Manager hanya boleh menyentuh baris ini — divalidasi via `editable_by_role_code`, lihat ERD) |
| PUT | `/admin/users/{id}/role` | Superadmin (semua role), Manager (khusus Agen ↔ Admin) | Ubah role seorang user |
| GET | `/admin/audit-logs` | Superadmin, Manager (khusus perubahan yg relevan) | Riwayat perubahan permission/role/data sensitif |

---

## 2. Property & Listing Management API
*(Selaras dengan PRD Modul 3)*

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| POST | `/listings` | Agen | Buat listing baru (kategori Primary/Secondary, tujuan Jual/Sewa, seluruh field spesifikasi rumah sesuai PRD 3.2) |
| GET | `/listings/{id}` | Public | Detail listing (untuk halaman detail; jumlah `view_count` bertambah otomatis) |
| PUT | `/listings/{id}` | Agen (pemilik), Superadmin/Manager/Admin | Update listing. Untuk kategori Primary tertaut developer, field harga/spesifikasi resmi read-only bagi Agen |
| PATCH | `/listings/{id}/status` | Agen (pemilik), Superadmin/Manager/Admin | Ubah status: `draft`, `pending_review`, `sold`, `rented`, `expired` |
| DELETE | `/listings/{id}` | Agen (pemilik), Superadmin/Manager/Admin | Hapus/arsipkan (soft delete) |
| POST | `/listings/{id}/media` | Agen (pemilik) | Upload foto (multi), video/virtual tour — lihat integrasi Storage/CDN di Bagian 9 |
| DELETE | `/listings/{id}/media/{media_id}` | Agen (pemilik) | Hapus 1 foto/video |
| PUT | `/listings/{id}/media/{media_id}/set-cover` | Agen (pemilik) | Tetapkan sebagai foto cover |
| GET | `/listings/{id}/price-history` | Agen (pemilik), Superadmin/Manager/Admin | Riwayat perubahan harga |
| POST | `/listings/from-project/{project_id}` | Agen | Auto-generate listing Primary dari katalog developer (Modul 6) — men-copy harga/spesifikasi resmi |
| GET | `/agents/me/listings` | Agen | Daftar listing milik agen yang login (semua status) |
| GET | `/admin/listings/pending` | Superadmin, Manager, Admin | Antrean moderasi listing |
| PUT | `/admin/listings/{id}/approve` | Superadmin, Manager, Admin | Approve listing → `published` |
| PUT | `/admin/listings/{id}/reject` | Superadmin, Manager, Admin | Reject dengan alasan |

**Contoh — Buat Listing:**
```json
POST /api/v1/listings
{
  "category": "secondary",
  "transaction_type": "sale",
  "property_type": "rumah",
  "title": "Rumah Minimalis 2 Lantai di BSD",
  "description": "...",
  "price": 1850000000,
  "is_negotiable": true,
  "address": "Jl. Kenanga No. 12",
  "province": "Banten",
  "city": "Tangerang Selatan",
  "district": "Serpong",
  "latitude": -6.301,
  "longitude": 106.673,
  "land_area": 120,
  "building_area": 150,
  "bedrooms": 3,
  "bathrooms": 2,
  "floors": 2,
  "certificate_type": "shm",
  "certificate_transferred": true,
  "amenities": ["kolam_renang", "keamanan_24_jam"],
  "whatsapp_number": "+6281234567890"
}
```

---

## 3. Advanced Search & Filtering API
*(Selaras dengan PRD Modul 3.4 — direkomendasikan didukung mesin pencari Typesense/Elasticsearch untuk performa & typo-tolerance)*

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| GET | `/properties/search` | Public | Pencarian multi-filter (lihat query param di bawah) |
| GET | `/properties/map-bounds` | Public | Pencarian geospasial dalam kotak koordinat (NE/SW bounds) untuk render pin di peta |
| GET | `/properties/nearby` | Public | Pencarian berdasarkan titik GPS pengguna (`lat`, `lng`, `radius_km`) — lihat catatan geolokasi di Bagian 9.4 |
| GET | `/properties/autocomplete` | Public | Saran lokasi/keyword saat mengetik di search bar |
| GET | `/properties/{id}/similar` | Public | Rekomendasi listing serupa (lokasi/harga/tipe berdekatan) |

**Query Parameter `GET /properties/search`:**
```
?category=secondary
&transaction_type=sale
&property_type=rumah,apartemen
&province=Banten&city=Tangerang+Selatan
&price_min=500000000&price_max=2000000000
&land_area_min=80&building_area_min=100
&bedrooms_min=2&bathrooms_min=1
&certificate_type=shm,hgb
&sort=newest              // newest | price_asc | price_desc | popular
&page=1&per_page=20
```

**Query Parameter `GET /properties/map-bounds`:**
```
?ne_lat=-6.20&ne_lng=106.85&sw_lat=-6.35&sw_lng=106.60&filters=... (parameter sama seperti /search)
```
Response berisi array minimal `{ id, lat, lng, price, category, cover_photo_url }` (payload ringan untuk render pin, detail lengkap diambil terpisah saat pin diklik via `GET /listings/{id}`).

---

## 4. Lead Generation & CRM Integration API
*(Selaras dengan PRD Modul 3.3 — CTA WhatsApp, diperluas dengan formulir inquiry in-app)*

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| POST | `/leads` | Public / Buyer | Kirim inquiry dari form di halaman listing (alternatif selain klik CTA WA langsung) |
| POST | `/listings/{id}/cta-click` | Public | Catat event klik tombol "Chat via WhatsApp" (lead pasif, tanpa form) sebelum redirect ke `wa.me` |
| GET | `/agents/me/leads` | Agen | Daftar leads/prospek masuk milik agen yang login |
| GET | `/admin/leads` | Superadmin, Manager, Admin | Daftar leads seluruh agen (global) |
| GET | `/leads/{id}` | Agen (pemilik lead), Superadmin/Manager/Admin | Detail 1 lead |
| PUT | `/leads/{id}/status` | Agen (pemilik lead) | Update funnel: `new`, `contacted`, `site_visit_scheduled`, `closed_won`, `closed_lost` |
| GET | `/agents/me/leads/stats` | Agen | Ringkasan jumlah lead per status/periode (untuk Dashboard M8) |

**Contoh — Kirim Lead:**
```json
POST /api/v1/leads
{
  "listing_id": "lst_9f21...",
  "name": "Budi Santoso",
  "phone": "+6285712345678",
  "email": "budi@example.com",
  "message": "Apakah masih bisa nego harga?",
  "preferred_contact": "whatsapp"
}
```

---

## 5. Communication & Chat API — **[FASE LANJUTAN / NON-MVP]**
*(Disepakati: untuk rilis awal, komunikasi Buyer↔Agen cukup memakai CTA WhatsApp yang sudah tersedia di setiap listing — lihat Bagian 4. Bagian ini didokumentasikan sebagai referensi desain untuk fase berikutnya, saat platform ingin menyediakan percakapan in-app tanpa Buyer perlu membagikan nomor pribadi.)*

Mekanisme utama saat ini: setiap listing menampilkan nomor WhatsApp agen (Modul 3), dan setiap klik tombol "Chat via WhatsApp" tercatat sebagai lead event lewat `POST /listings/{id}/cta-click` (Bagian 4) sebelum pengguna diarahkan ke `wa.me/{nomor_agen}`. Seluruh percakapan lanjutan terjadi di WhatsApp, di luar sistem.

Spesifikasi berikut **baru diimplementasikan saat fase in-app chat diaktifkan**:

| Protokol/Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| WebSocket | `wss://api.<domain>.id/ws/chat` | Authenticated (token via query/header saat handshake) | Koneksi real-time untuk kirim/terima pesan instan |
| GET | `/chats/conversations` | Authenticated | Daftar percakapan aktif milik user (Buyer atau Agen) |
| GET | `/chats/conversations/{id}/messages` | Authenticated (partisipan chat) | Riwayat pesan dalam 1 percakapan (paginated) |
| POST | `/chats/conversations` | Buyer, Agen | Mulai percakapan baru terkait listing tertentu |
| POST | `/chats/schedule-visit` | Buyer, Agen | Ajukan/konfirmasi jadwal site visit langsung dari dalam chat — otomatis membuat entri di Kalender Event (Modul 5) sbg tipe `open_house` personal |
| PUT | `/chats/messages/{id}/read` | Authenticated | Tandai pesan sudah dibaca |

**Event WebSocket (contoh payload):**
```json
// Client → Server
{ "type": "send_message", "conversation_id": "cvs_...", "text": "Apakah unit ini masih tersedia?" }

// Server → Client (broadcast ke partisipan)
{ "type": "new_message", "conversation_id": "cvs_...", "message": { "id": "msg_...", "sender_id": "usr_...", "text": "...", "sent_at": "2026-07-26T10:00:00Z" } }
```
> Fallback: jika koneksi WebSocket gagal (mis. jaringan agen di lapangan tidak stabil), client dapat polling `GET /chats/conversations/{id}/messages?after={last_message_id}` sebagai mekanisme graceful degradation.

---

## 6. Financial & Valuation Calculator API
*(Selaras dengan PRD Modul 7 — Sistem Scoring DBR)*

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| POST | `/calculator/dbr` | Agen (alias lama: `/calculator/kpr`) | Hitung simulasi DBR & kelayakan KPR |
| GET | `/calculator/dbr/config` | Public | Ambil parameter aktif (threshold DBR, suku bunga default) — untuk ditampilkan sbg default di form |
| POST | `/calculator/dbr/{id}/save-as-prospect` | Agen | Simpan hasil simulasi sbg Lead/Prospek |
| GET | `/agents/me/dbr-simulations` | Agen | Riwayat simulasi milik sendiri |
| GET | `/admin/dbr-simulations` | Superadmin, Manager, Admin | Riwayat simulasi seluruh agen (global) |
| GET | `/calculator/dbr/{id}/export-pdf` | Agen (pemilik simulasi) | Export hasil ke PDF |
| PUT | `/admin/config/dbr` | **Superadmin only** | Ubah threshold DBR & suku bunga default sistem |
| GET | `/market-insights/suburb` | Public | Tren harga rata-rata per m² di suatu kecamatan/kelurahan |

**Contoh — Hitung DBR:**
```json
POST /api/v1/calculator/dbr
{
  "prospect_name": "Rina Kartika",
  "prospect_phone": "+6281298765432",
  "net_income": 15000000,
  "existing_installments": 1200000,
  "property_price": 850000000,
  "down_payment": 170000000,
  "tenor_months": 180,
  "interest_rate_annual": 8.5,
  "listing_id": "lst_9f21..."          // opsional, jika dibuka dari halaman listing
}

200 OK
{
  "success": true,
  "data": {
    "loan_amount": 680000000,
    "monthly_installment": 6698421,
    "dbr_percent": 52.66,
    "eligibility_status": "tidak_layak",
    "threshold_used": 35,
    "disclaimer": "Hasil ini estimasi awal, bukan keputusan final bank."
  }
}
```

---

## 7. Notification & Content Management API
*(Selaras dengan PRD Modul 8 — Dashboard & Notifikasi, serta banner promosi dari Modul 5/6)*

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| GET | `/notifications` | Authenticated | Daftar notifikasi milik user yang login |
| PUT | `/notifications/{id}/read` | Authenticated | Tandai satu notifikasi dibaca |
| PUT | `/notifications/read-all` | Authenticated | Tandai semua dibaca |
| POST | `/admin/notifications/push` | Superadmin, Manager, Admin (sistem juga dapat memicu otomatis) | Kirim notifikasi push/email manual (mis. broadcast pengumuman) |
| GET | `/banners/promotions` | Public | Banner promosi featured listing/hot deals untuk beranda |
| POST | `/admin/banners` | Superadmin, Manager, Admin | Buat/kelola banner promosi |
| GET | `/dashboard/summary` | Authenticated | Ringkasan dashboard sesuai scope role (own untuk Agen, global untuk Superadmin/Manager/Admin) — menggabungkan data listing, lead, event, kursus (Modul 8) |

---

## 8. Wilayah Indonesia (Reference Data) API
*(Data alamat administratif Indonesia — Provinsi, Kota/Kabupaten, Kecamatan, Kelurahan/Desa, Kode Pos)*

| Method | Endpoint | Auth | Deskripsi |
|---|---|---|---|
| GET | `/regions/provinces` | Public | Daftar 38 provinsi |
| GET | `/regions/cities?province_id={id}` | Public | Kota/kabupaten dalam 1 provinsi |
| GET | `/regions/districts?city_id={id}` | Public | Kecamatan dalam 1 kota/kabupaten |
| GET | `/regions/villages?district_id={id}` | Public | Kelurahan/desa dalam 1 kecamatan |
| GET | `/regions/postal-code?village_id={id}` | Public | Kode pos untuk kelurahan/desa tsb |
| GET | `/regions/search?q={keyword}` | Public | Pencarian bebas lintas level (mis. ketik "Serpong" → hasil kecamatan + kota induk) |

> **Rekomendasi implementasi:** data wilayah administratif Indonesia relatif statis (update resmi Kemendagri jarang berubah) — sebaiknya **di-seed & di-host sendiri** di database internal (bersumber dari dataset terbuka resmi, mis. data wilayah Kemendagri/BPS) alih-alih memanggil API pihak ketiga di setiap request pencarian, demi performa dan menghindari ketergantungan uptime layanan eksternal. Endpoint di atas sepenuhnya melayani dari database sendiri; Google Places/Maps API (Bagian 9.1) baru dipakai untuk **autocomplete alamat jalan & pin peta presisi**, bukan untuk data wilayah administratif.

---

## 9. Integrasi Pihak Ketiga (External Services)

### 9.1 Maps & Geocoding — Google Maps Platform / Mapbox
| Kebutuhan | Cara Integrasi |
|---|---|
| Autocomplete alamat jalan saat isi form listing | Client-side memanggil Google Places Autocomplete API langsung (API key dibatasi domain/referrer); hasil (alamat + lat/long) dikirim ke `POST /listings` |
| Pin peta di halaman detail listing & hasil pencarian | Google Maps JavaScript API / Mapbox GL JS di sisi frontend, menggunakan `latitude`/`longitude` yang tersimpan di `listings` |
| Perhitungan jarak ke fasilitas umum (sekolah, rumah sakit, tol) | Google Places Nearby Search / Distance Matrix API, dipanggil dari backend (server-side, API key rahasia) agar tidak membebani kuota client & menyembunyikan API key |
| Reverse geocoding (koordinat → alamat) saat agen pin lokasi di peta | Google Geocoding API, dipanggil server-side saat `POST/PUT /listings` untuk validasi & auto-fill kota/kecamatan |

### 9.2 Storage & CDN — AWS S3 / Cloudinary / ImageKit
| Kebutuhan | Cara Integrasi |
|---|---|
| Upload foto/video listing (Bagian 2) | `POST /listings/{id}/media` menerima file → backend upload ke bucket S3/Cloudinary → simpan URL CDN di `listing_photos.url`/`listing_videos.url` |
| Kompresi & optimasi otomatis | Dilakukan oleh layanan pihak ketiga (Cloudinary/ImageKit transformation API) saat upload — backend hanya menyimpan URL dengan parameter transformasi (mis. `?w=800&q=auto`) |
| Dokumen legalitas agen (KTP/NPWP) | Upload ke bucket **terpisah & privat** (tidak lewat CDN publik), dengan enkripsi at-rest, akses via signed URL berumur pendek saja untuk Superadmin/Admin/Manager saat review |

### 9.3 Payment Gateway — untuk Fitur Membership Premium Agen (Fase Lanjutan)
| Kebutuhan | Cara Integrasi |
|---|---|
| Langganan keanggotaan agen premium (mis. boost listing, kuota listing lebih besar) | Integrasi Midtrans/Xendit (payment gateway populer Indonesia, mendukung VA, e-wallet, kartu kredit) |
| Endpoint terkait (disiapkan sbg placeholder fase lanjutan) | `POST /billing/subscriptions`, `GET /billing/invoices`, `POST /billing/webhook` (menerima callback status pembayaran dari payment gateway) |
> Modul ini **belum termasuk cakupan wajib saat ini** — didesain sbg ekstensi non-breaking (tabel & endpoint baru) yang bisa ditambahkan tanpa mengubah struktur listing/agen yang sudah ada.

### 9.4 Geolokasi Pengguna (GPS Browser)
- **Server tidak dapat "membaca" GPS pengguna secara langsung** — pembacaan lokasi terjadi di **sisi client** melalui Browser Geolocation API (`navigator.geolocation.getCurrentPosition()`), setelah pengguna memberi izin akses lokasi.
- Setelah client mendapat `{ lat, lng }`, nilai tersebut dikirim sebagai query param ke `GET /properties/nearby?lat=...&lng=...&radius_km=5` (Bagian 3) untuk menampilkan listing terdekat.
- Fallback jika pengguna menolak izin lokasi: gunakan estimasi kota dari IP (mis. via layanan IP-geolocation) sebagai default kasar, atau minta pengguna memilih kota secara manual dari Bagian 8.

### 9.5 Google OAuth2 (Login)
- Sudah dijabarkan di Bagian 1.1 (`POST /auth/oauth/google`). Memerlukan **Google Cloud OAuth Client ID** terdaftar dengan authorized origins domain platform. Verifikasi `id_token` dilakukan **server-side** memakai Google Auth Library resmi (bukan hanya trust dari client) demi keamanan.

---

## 10. Modul Pendukung Lain (Referensi Silang PRD)

Untuk kelengkapan, berikut ringkasan endpoint modul lain yang sudah menjadi bagian PRD namun di luar 8 kelompok utama yang diminta:

### 10.1 Learning Center API (PRD Modul 4)
| Method | Endpoint | Auth |
|---|---|---|
| GET | `/courses` | Public/Authenticated |
| GET | `/courses/{id}` | Public/Authenticated |
| POST | `/courses/{id}/enroll` | Agen |
| GET | `/agents/me/enrollments` | Agen |
| POST | `/courses/{id}/quiz/submit` | Agen |
| GET | `/agents/me/certificates` | Agen |
| POST | `/admin/courses` / `PUT /admin/courses/{id}` | Superadmin, Manager, Admin |

### 10.2 Kalender Event API (PRD Modul 5)
| Method | Endpoint | Auth |
|---|---|---|
| GET | `/events` | Public/Authenticated |
| POST | `/events/{id}/rsvp` | Agen |
| POST | `/events` | Superadmin, Manager, Admin |
| POST | `/developer-partners/events` | Developer Partner (butuh approval) |

### 10.3 Direktori Kerjasama Developer API (PRD Modul 6)
| Method | Endpoint | Auth |
|---|---|---|
| GET | `/developer-projects` | Public/Authenticated |
| GET | `/developer-projects/{id}` | Public/Authenticated |
| POST | `/developer-projects/{id}/claim` | Agen |
| POST | `/admin/developer-projects` | Superadmin, Manager, Admin |

### 10.4 Admin System Configuration API (PRD Modul 9)
| Method | Endpoint | Auth |
|---|---|---|
| GET / PUT | `/admin/config/system` | **Superadmin only** (mencakup masa expired listing, passing grade default, dsb — bukan threshold DBR yang punya endpoint sendiri di Bagian 6) |
| GET | `/admin/reports/export` | Superadmin, Manager, Admin |

---

## 11. Keputusan yang Sudah Disepakati

| Poin | Keputusan |
|---|---|
| Role "Agency/Principal" | **Tidak diperlukan** sebagai role sistem terpisah — cukup memakai atribut `office_name` di profil agen. |
| Chat in-app (Bagian 5) | **Ditunda ke fase lanjutan** — rilis awal cukup memakai CTA WhatsApp per listing (Bagian 4) sebagai satu-satunya jalur komunikasi Buyer↔Agen. |
| Data Wilayah Indonesia (Bagian 8) | **Di-host sendiri** di database internal (bukan panggil API pihak ketiga tiap request), karena data bersifat statis. |
| Payment Gateway (Bagian 9.3) | Didesain sesuai rekomendasi — placeholder non-breaking untuk fase membership premium agen, belum masuk cakupan wajib rilis awal. |
| Geolokasi GPS (Bagian 9.4) | Disepakati: pembacaan lokasi tetap di sisi browser (client-side), server hanya menerima koordinat lat/lng sebagai parameter. |

## 12. Hal yang Masih Perlu Dikonfirmasi

1. Sumber dataset wilayah Indonesia resmi yang akan dipakai untuk seed data Bagian 8 (mis. dataset Kemendagri terbaru), serta siapa yang bertanggung jawab menjaga data tetap ter-update bila ada pemekaran wilayah.
2. Provider Maps pilihan (Google Maps Platform vs Mapbox) — memengaruhi estimasi biaya operasional karena keduanya berbayar per-request setelah kuota gratis terlampaui.
3. Provider payment gateway pilihan untuk fase membership premium nanti (Midtrans/Xendit/lainnya) — memengaruhi desain detail webhook di Bagian 9.3 saat fase itu tiba.

---

*Dokumen ini menjadi acuan untuk implementasi backend (routing, middleware RBAC, dan integrasi pihak ketiga) pada tahap development.*
