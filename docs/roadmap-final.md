# 🧭 Roadmap Pembelajaran CodeIgniter 3
## Modul Fundamental untuk Pemula

> **Tujuan:** Menguasai fundamental framework CI3 dengan pendekatan praktis, diakhiri dengan pembuatan mini aplikasi.

---

## 📋 Daftar Isi

1. [Prerequisite](#prerequisite)
2. [Phase 0 - Environment & Setup](#phase-0---environment--setup)
3. [Phase 1 - Controller & Routing](#phase-1---controller--routing)
4. [Phase 2 - View & Template](#phase-2---view--template)
5. [Phase 3 - Database & Model](#phase-3---database--model)
6. [Phase 4 - Form Handling & Validation](#phase-4---form-handling--validation)
7. [Phase 5 - Input & Security Dasar](#phase-5---input--security-dasar)
8. [Phase 6 - Session & Authentication](#phase-6---session--authentication)
9. [Phase 7 - Helper & Library](#phase-7---helper--library)
10. [Phase 8 - File Upload](#phase-8---file-upload)
11. [Phase 9 - Response Handling (Web & API)](#phase-9---response-handling-web--api)
12. [Phase 10 - Pagination, Search & Filter](#phase-10---pagination-search--filter)
13. [Phase 11 - Error Handling & Debugging](#phase-11---error-handling--debugging)
14. [Phase 12 - Mini Project: Aplikasi Keuangan](#phase-12---mini-project-aplikasi-keuangan)

---

## Prerequisite

### Pengetahuan Dasar
- HTML & CSS dasar
- PHP dasar (variabel, array, function, OOP dasar)
- SQL dasar (SELECT, INSERT, UPDATE, DELETE)
- Pemahaman HTTP Request (GET, POST)

### Tools
- XAMPP / Laragon / MAMP
- VS Code
- Browser + DevTools
- Postman

---

## Phase 0 - Environment & Setup

> **Target:** Mampu menjalankan CI3 dan memahami struktur folder

### 0.1 Instalasi CodeIgniter 3
- Download CI3 dari sumber resmi
- Ekstrak ke folder htdocs/www
- Menjalankan CI3 pertama kali

### 0.2 Struktur Folder CI3
- `application/` - Folder utama development
  - `controllers/`, `models/`, `views/`
  - `config/`, `helpers/`, `libraries/`
  - `hooks/`, `core/`
- `system/` - Core CI3 (jangan diubah)
- `index.php` - Entry point
- `.htaccess` - URL rewriting

### 0.3 Konfigurasi Dasar
- `config/config.php`: `base_url`, `index_page`, `encryption_key`
- `config/autoload.php`: autoload helper, library, model
- `config/database.php`: koneksi database MySQL

### 0.4 Alur Request CI3
```
Request → index.php → Routing → Controller → Model ↔ Database → View → Response
```

### 0.5 Environment Configuration
- Constant `ENVIRONMENT`: development, testing, production

### 📝 Practice
1. Install CI3 dan jalankan welcome page
2. Ubah `base_url` sesuai project
3. Buat halaman "Hello CodeIgniter 3"

### ❓ Quiz (8 Pertanyaan)
1. Apa perbedaan folder `application/` dan `system/`?
2. File apa yang menjadi entry point utama CI3?
3. Di mana kita mengatur koneksi database?
4. Apa fungsi `.htaccess` di root folder CI3?
5. Bagaimana cara menghilangkan `index.php` dari URL?
6. Apa itu `base_url` dan mengapa penting?
7. Sebutkan 3 folder utama di `application/`!
8. Apa yang terjadi jika `ENVIRONMENT` diset ke 'production'?

---

## Phase 1 - Controller & Routing

> **Target:** Memahami cara CI3 menerima dan memproses request

### 1.1 Konsep Controller
- Naming convention (PascalCase, file lowercase)
- Default controller (`Welcome.php`)

### 1.2 Anatomi Controller
- `extends CI_Controller`
- Method `index()` sebagai default
- Public vs Private method

### 1.3 Constructor
- `__construct()` dan `parent::__construct()`
- Load resource di constructor

### 1.4 Routing
- Default routing: `controller/method/param`
- File `config/routes.php`
- Custom routes dengan wildcard: `(:num)`, `(:any)`

### 1.5 Parameter URL
- `$this->uri->segment(n)`
- Parameter sebagai argument method

### 1.6 Load Resource
- `$this->load->view()`, `model()`, `helper()`, `library()`

### 📝 Practice
1. Buat controller dengan multiple method
2. Buat route custom
3. Buat method dengan parameter URL

### ❓ Quiz (10 Pertanyaan)
1. Apa naming convention file controller?
2. URL default untuk method `detail()` di controller `Product`?
3. Perbedaan method public dan private?
4. Cara membuat route custom?
5. Fungsi `parent::__construct()`?
6. Perbedaan wildcard `(:num)` dan `(:any)`?
7. Cara mengambil segment ke-3 dari URL?
8. Method apa yang dipanggil jika URL hanya nama controller?
9. Fungsi `defined('BASEPATH') OR exit(...)`?
10. Cara me-load model di controller?

---

## Phase 2 - View & Template

> **Target:** Memisahkan logic dan tampilan

### 2.1 Konsep View
- Lokasi: `application/views/`
- Naming convention

### 2.2 Load View
- `$this->load->view('nama_view', $data)`
- Multiple view, return as string

### 2.3 Passing Data
- Array associative ke view
- Akses: `$nama_variabel`

### 2.4 PHP di View
- Echo, kondisional, perulangan
- Alternative syntax untuk template

### 2.5 Template System
- Struktur: layouts/, partials/, pages/
- Header, footer, content terpisah

### 📝 Practice
1. Passing data string dan array ke view
2. Template dengan header/content/footer
3. List data dengan `foreach`

### ❓ Quiz (10 Pertanyaan)
1. Di folder mana file view disimpan?
2. Cara passing data dari controller ke view?
3. Parameter ketiga `$this->load->view()` jika TRUE?
4. Alternative syntax PHP untuk `if-else`?
5. Keuntungan memisahkan view?
6. Cara akses variabel `$title` di view?
7. Perbedaan `<?php echo ?>` dan `<?= ?>`?
8. Struktur folder view yang baik?
9. Cara load multiple view?
10. Jika variabel tidak dikirim tapi dipanggil?

---

## Phase 3 - Database & Model

> **Target:** CRUD dengan Query Builder

### 3.1 Konfigurasi Database
- File `config/database.php`
- Parameter koneksi

### 3.2 Konsep Model
- Satu model = satu tabel
- Naming: `Nama_model.php`

### 3.3 Query Builder
- **SELECT:** `get()`, `where()`, `select()`, chaining
- **INSERT:** `insert()`, `insert_id()`
- **UPDATE:** `where()`, `update()`
- **DELETE:** `delete()`

### 3.4 Mengambil Result
- `result()`, `result_array()`, `row()`, `num_rows()`

### 3.5 Where Conditions
- Simple, operator, OR, IN, LIKE

### 3.6 Join, Order, Limit
- `join()`, `order_by()`, `group_by()`, `limit()`

### 3.7 Database Transaction
- `trans_start()`, `trans_complete()`, `trans_status()`

### 📝 Practice
1. Buat tabel products
2. Model CRUD
3. Tampilkan di view

### ❓ Quiz (10 Pertanyaan)
1. File konfigurasi database?
2. Perbedaan `result()` dan `result_array()`?
3. Cara dapat last insert ID?
4. Query Builder untuk SELECT WHERE?
5. Fungsi `affected_rows()`?
6. Cara JOIN dengan Query Builder?
7. Keuntungan Query Builder vs raw SQL?
8. Syntax UPDATE Query Builder?
9. Kapan pakai transaction?
10. Perbedaan `row()` dan `result()`?

---

## Phase 4 - Form Handling & Validation

> **Target:** Menangani input user dengan benar

### 4.1 Form Helper
- `form_open()`, `form_input()`, `form_dropdown()`, dll

### 4.2 Form Validation Library
- `set_rules()`, validation rules

### 4.3 Validation Rules
- `required`, `min_length`, `max_length`, `valid_email`
- `numeric`, `matches`, `is_unique`, `callback_*`

### 4.4 Menjalankan Validasi
- `$this->form_validation->run()`

### 4.5 Error Messages
- `validation_errors()`, `form_error('field')`

### 4.6 Repopulate Form
- `set_value()`, `set_select()`, `set_checkbox()`

### 4.7 Callback Validation
- Custom validation dengan `callback_function_name`

### 📝 Practice
1. Form registrasi dengan validasi
2. Tampilkan error
3. Repopulate form

### ❓ Quiz (10 Pertanyaan)
1. Cara load form_validation library?
2. Fungsi `set_value()`?
3. Cara tampilkan error per field?
4. Perbedaan `validation_errors()` dan `form_error()`?
5. Syntax set validation rule?
6. Sebutkan 5 validation rules!
7. Cara buat custom validation callback?
8. Jika `run()` return FALSE?
9. Cara cek field unik di database?
10. Fungsi `set_error_delimiters()`?

---

## Phase 5 - Input & Security Dasar

> **Target:** Menangani input dengan aman

### 5.1 Input Class
- `post()`, `get()`, `server()`, `cookie()`
- `method()`, `ip_address()`

### 5.2 XSS Filtering
- Global filter, per-input filter
- `$this->security->xss_clean()`

### 5.3 CSRF Protection
- Config, `form_open()` otomatis
- Manual token

### 5.4 SQL Injection Protection
- Query Builder auto escape
- Binding parameter

### 5.5 Output Escaping
- `html_escape()`, `htmlspecialchars()`

### 5.6 Password Hashing
- `password_hash()`, `password_verify()`

### 📝 Practice
1. Form dengan CSRF
2. XSS filter input
3. Hash password

### ❓ Quiz (10 Pertanyaan)
1. Perbedaan `$this->input->post()` dan `$_POST`?
2. Cara aktifkan CSRF?
3. Fungsi parameter TRUE di `post('field', TRUE)`?
4. Cara Query Builder cegah SQL Injection?
5. Kapan pakai `html_escape()`?
6. Fungsi `password_hash()` dan `password_verify()`?
7. Cara dapat IP address user?
8. Apa CSRF dan mengapa perlu dicegah?
9. Cara tambah CSRF token manual?
10. Sebutkan 3 serangan yang dicegah CI3!

---

## Phase 6 - Session & Authentication

> **Target:** State management dan sistem login

### 6.1 Session di CI3
- Driver: files, database
- Konfigurasi session

### 6.2 Session Operations
- `set_userdata()`, `userdata()`, `unset_userdata()`
- `sess_destroy()`

### 6.3 Flashdata
- `set_flashdata()`, `flashdata()`, `keep_flashdata()`

### 6.4 Tempdata
- Session dengan expiry time

### 6.5 Authentication Flow
- Login → Validasi → Cek DB → Verify Password → Set Session → Redirect

### 6.6 Auth Check (Middleware)
- Cek di constructor
- Menggunakan `MY_Controller`

### 📝 Practice
1. Tabel users
2. Form registrasi/login
3. Logout, proteksi halaman

### ❓ Quiz (10 Pertanyaan)
1. Perbedaan `userdata` dan `flashdata`?
2. Cara logout (hapus session)?
3. Konfigurasi session driver?
4. Fungsi `tempdata`?
5. Cara cek user sudah login?
6. Flow authentication?
7. Keuntungan database session driver?
8. Cara buat "middleware" auth?
9. Mengapa `password_hash()` bukan MD5?
10. Apa terjadi flashdata setelah dibaca?

---

## Phase 7 - Helper & Library

> **Target:** Kode reusable

### 7.1 Konsep Helper
- Kumpulan fungsi, bukan class
- Naming: `nama_helper.php`

### 7.2 Built-in Helper
- url, form, html, text, date, file, string

### 7.3 Custom Helper
- Buat fungsi dengan `if (!function_exists())`

### 7.4 Konsep Library
- Class dengan method (OOP)
- Naming: `Namalibrary.php`

### 7.5 Custom Library
- Akses CI dengan `get_instance()`

### 7.6 MY_Controller & MY_Model
- Extending core class untuk reusability

### 7.7 Hooks
- Interceptor di berbagai titik lifecycle

### 📝 Practice
1. Custom helper format rupiah/tanggal
2. Custom library response
3. MY_Controller dengan render()

### ❓ Quiz (10 Pertanyaan)
1. Perbedaan helper dan library?
2. Naming convention file helper?
3. Cara buat custom helper?
4. Fungsi `get_instance()`?
5. Cara autoload helper/library?
6. Keuntungan `MY_Controller`?
7. Sebutkan 3 built-in helper!
8. Cara akses CI di library?
9. Apa hooks dan kapan digunakan?
10. Konsep `MY_Model` untuk CRUD?

---

## Phase 8 - File Upload

> **Target:** Upload file dengan benar dan aman

### 8.1 Konfigurasi Upload
- `upload_path`, `allowed_types`, `max_size`, `encrypt_name`

### 8.2 Upload Process
- `do_upload()`, `display_errors()`, `data()`

### 8.3 Form Upload
- `form_open_multipart()`

### 8.4 Upload Data
- `file_name`, `file_type`, `full_path`, `file_size`, dll

### 8.5 Multiple Upload
- Loop `$_FILES`

### 8.6 Image Manipulation
- Resize, crop, watermark

### 📝 Practice
1. Form upload gambar
2. Validasi tipe/ukuran
3. Generate thumbnail

### ❓ Quiz (10 Pertanyaan)
1. Atribut form wajib untuk upload?
2. Cara batasi tipe file?
3. Fungsi `encrypt_name`?
4. Cara dapat info file setelah upload?
5. Method form helper untuk upload?
6. Cara multiple file upload?
7. Cara buat thumbnail?
8. Cara hapus file uploaded?
9. Return jika upload gagal?
10. Sebutkan 3 validasi penting upload!

---

## Phase 9 - Response Handling (Web & API)

> **Target:** Response HTML dan JSON dari satu codebase

### 9.1 Output JSON
- `set_content_type('application/json')`
- `json_encode()`

### 9.2 HTTP Status Codes
- 200, 201, 400, 401, 404, 500

### 9.3 Standard API Response
```json
{"status": true, "message": "Success", "data": {...}}
```

### 9.4 Response Library
- Standardize JSON output

### 9.5 Dual Controller Pattern
- Web method + API method

### 9.6 API Routes
- Routing berdasarkan HTTP method

### 9.7 Reading JSON Input
- `file_get_contents('php://input')`

### 📝 Practice
1. Response library
2. API GET, POST
3. Routing web vs API

### ❓ Quiz (10 Pertanyaan)
1. Cara set Content-Type JSON?
2. Sebutkan 5 HTTP status code!
3. Perbedaan response web dan API?
4. Cara baca JSON body dari PUT request?
5. Fungsi `set_status_header()`?
6. Format standard API response?
7. Cara tahu HTTP method request?
8. Mengapa perlu response library?
9. Routing berdasarkan HTTP method?
10. Kelebihan dual controller pattern?

---

## Phase 10 - Pagination, Search & Filter

> **Target:** Data list dengan pagination dan pencarian

### 10.1 Pagination Library
- Config: `base_url`, `total_rows`, `per_page`, `uri_segment`
- Styling Bootstrap

### 10.2 Search Implementation
- LIKE query, GET parameter

### 10.3 Filter Implementation
- WHERE conditions dari parameter

### 10.4 Combined Search + Filter + Pagination
- Query builder chaining

### 📝 Practice
1. List products dengan pagination
2. Search by keyword
3. Filter by category, date range

### ❓ Quiz (8 Pertanyaan)
1. Config wajib pagination?
2. Cara hitung offset?
3. Implementasi search dengan LIKE?
4. Kombinasi search + pagination?
5. Cara filter rentang tanggal?
6. Styling pagination Bootstrap?
7. URI segment untuk page number?
8. Cara passing search term di pagination link?

---

## Phase 11 - Error Handling & Debugging

> **Target:** Menangani error dan debugging

### 11.1 Error Reporting
- `ENVIRONMENT` constant
- Error display berdasarkan environment

### 11.2 Logging
- `log_message('level', 'message')`
- Level: error, debug, info

### 11.3 Custom Error Pages
- `application/views/errors/`
- 404, general error

### 11.4 Exception Handling
- try-catch di PHP

### 11.5 Debug Tools
- `print_r()`, `var_dump()`
- `$this->db->last_query()`

### 📝 Practice
1. Custom 404 page
2. Logging di model
3. Debug query

### ❓ Quiz (6 Pertanyaan)
1. Level logging di CI3?
2. Cara lihat query terakhir?
3. Lokasi custom error page?
4. Cara aktifkan logging?
5. Perbedaan error di development vs production?
6. Best practice error handling?

---

## Phase 12 - Mini Project: Aplikasi Keuangan

> **Target:** Implementasi semua fundamental dalam satu aplikasi

### 12.1 Spesifikasi Aplikasi

**Nama:** Aplikasi Pemasukan & Pengeluaran Bulanan

**Fitur:**
- Autentikasi (register, login, logout)
- CRUD Transaksi (income/expense)
- Kategori transaksi
- Rekap bulanan
- Search & filter transaksi
- Pagination
- Dual response: Web View + JSON API

### 12.2 Database Schema

```sql
-- Users
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    email VARCHAR(100) UNIQUE,
    password VARCHAR(255),
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);

-- Categories
CREATE TABLE categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    type ENUM('income', 'expense'),
    user_id INT
);

-- Transactions
CREATE TABLE transactions (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT,
    category_id INT,
    type ENUM('income', 'expense'),
    amount DECIMAL(15,2),
    description TEXT,
    date DATE,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP
);
```

### 12.3 Struktur Folder

```
application/
├── controllers/
│   ├── Auth.php
│   ├── Dashboard.php
│   ├── Transactions.php
│   ├── Categories.php
│   └── Api.php
├── models/
│   ├── User_model.php
│   ├── Transaction_model.php
│   └── Category_model.php
├── views/
│   ├── layouts/
│   ├── auth/
│   ├── dashboard/
│   ├── transactions/
│   └── categories/
├── helpers/
│   └── format_helper.php
├── libraries/
│   └── Api_response.php
└── core/
    ├── MY_Controller.php
    └── MY_Model.php
```

### 12.4 API Endpoints

| Method | Endpoint | Deskripsi |
|--------|----------|-----------|
| POST | /api/auth/login | Login |
| GET | /api/transactions | List transaksi |
| POST | /api/transactions | Tambah transaksi |
| GET | /api/transactions/:id | Detail transaksi |
| PUT | /api/transactions/:id | Update transaksi |
| DELETE | /api/transactions/:id | Hapus transaksi |
| GET | /api/summary?month=2025-01 | Rekap bulanan |

### 12.5 Fundamental yang Digunakan

- ✅ Environment & Config
- ✅ Controller & Routing
- ✅ View & Template
- ✅ Database & Model
- ✅ Form & Validation
- ✅ Input & Security
- ✅ Session & Auth
- ✅ Helper & Library
- ✅ File Upload (bukti transaksi)
- ✅ Response Web & API
- ✅ Pagination, Search, Filter
- ✅ Error Handling

### 12.6 Milestone Development

1. **Setup** - Environment, database, config
2. **Auth** - Register, login, logout, session
3. **Categories** - CRUD kategori
4. **Transactions** - CRUD transaksi
5. **Dashboard** - Rekap, chart bulanan
6. **API** - JSON endpoints
7. **Polish** - Search, filter, pagination, UX

---

## Catatan Penutup

### Tips Belajar
1. **Jangan skip** - Setiap phase adalah pondasi phase berikutnya
2. **Practice > Theory** - Langsung coding, bukan hanya baca
3. **Debug sendiri** - Baca error message, cek log
4. **Baca dokumentasi** - user-guide CI3 sangat lengkap

### Resources
- [CodeIgniter 3 User Guide](https://codeigniter.com/userguide3/)
- [PHP Manual](https://www.php.net/manual/)

### Estimasi Waktu
- Phase 0-4: 1-2 minggu
- Phase 5-9: 2-3 minggu
- Phase 10-11: 1 minggu
- Phase 12: 2-3 minggu
- **Total: 6-9 minggu**

---

*Roadmap ini dirancang untuk pembelajaran terstruktur CodeIgniter 3 dengan fokus fundamental sebelum masuk ke business logic.*
