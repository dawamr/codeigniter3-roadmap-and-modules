Pendekatan kamu **sangat tepat** ✅
CI3 **idealnya dipelajari dari sisi *fundamental framework***, bukan dari alur bisnis.
Bisnis flow baru masuk **setelah semua “alat tempur” CI3 dikuasai**.

Di bawah ini aku susun **roadmap pembelajaran CI3 untuk pemula** dengan prinsip:

* ✅ Fokus **fundamental CI3**
* ✅ Berbasis **practice** (bukan teori panjang)
* ✅ Tidak terikat bisnis dulu
* ✅ Buat perbandingan jika dibandingkan dengan PHP Pure(Native) jika memungkinkan
* ✅ Progression jelas (mudah → menengah → siap bikin mini app)
* ✅ Diakhiri **mini app Pemasukan & Pengeluaran Bulanan**
* ✅ Support **Web View & JSON API**

---

# 🧭 Roadmap Pembelajaran CodeIgniter 3 (Fundamental-Focused)

## PHASE 0 — Pondasi Wajib (Environment & Mindset)

> Target: bisa menjalankan CI3 tanpa “magic”

### Materi

* Instalasi CI3 (manual, bukan composer)
* Struktur folder CI3:

  * `application/controllers`
  * `application/models`
  * `application/views`
  * `config/`
* Alur request CI3 (Request → Controller → Model → View)
* `index.php` dan `.htaccess`
* Konfigurasi dasar:

  * `base_url`
  * `database.php`
  * `autoload.php`

### Practice

* Menjalankan CI3 + halaman “Hello CI3”
* 1 controller sederhana + 1 view

---

## PHASE 1 — Controller & Routing (Fundamental Utama)

> Target: paham bagaimana CI3 menerima request

### Materi

* Controller:

  * Method default (`index`)
  * Public vs private method
* Routing dasar (`routes.php`)
* Parameter URL:

  * `/user/detail/1`
* Constructor controller
* Menggunakan `$this->load`

### Practice

* Controller dengan banyak method
* Route custom
* Kirim parameter lewat URL

---

## PHASE 2 — View & Template (Presentation Layer)

> Target: pisahkan logic & tampilan

### Materi

* Load view
* Passing data ke view
* Multiple view:

  * header
  * footer
  * content
* Struktur folder view yang rapi
* Dasar PHP di view (if, foreach)

### Practice

* Template sederhana
* Dynamic data di view
* Layout dengan header/footer

---

## PHASE 3 — Database & Model (Data Layer)

> Target: CRUD tanpa SQL di controller

### Materi

* Database config (MySQL)
* Active Record / Query Builder
* Model CI3
* CRUD:

  * insert
  * select
  * update
  * delete
* Return data: `row()`, `result()`, `array()`

### Practice

* Model CRUD sederhana
* Controller panggil model
* Tampilkan data di view

---

## PHASE 4 — Form Handling & Validation

> Target: bisa menangani input user dengan benar

### Materi

* Form helper
* Method POST & GET
* Form Validation library
* Error message
* Old input (repopulate form)
* Sanitasi dasar input

### Practice

* Form tambah data
* Validasi field wajib
* Menampilkan error di view

---

## PHASE 5 — Session & Authentication Dasar

> Target: memahami state & login logic

### Materi

* Session CI3
* Flashdata
* Login / logout logic
* Password hashing (`password_hash`)
* Middleware versi CI3 (cek login di constructor)

### Practice

* Login form
* Proteksi halaman
* Flash message success/error

---

## PHASE 6 — Helper, Library & Reusability

> Target: tidak menulis kode berulang

### Materi

* Helper:

  * custom helper
  * autoload helper
* Library:

  * custom library
* `MY_Controller`
* `MY_Model` (konsep dasar)

### Practice

* Helper format tanggal / rupiah
* Base controller untuk auth check
* Reusable method

---

## PHASE 7 — Response Handling (Web vs JSON API)

> **FUNDAMENTAL PENTING untuk CI3 modern**

### Materi

* Output HTML vs JSON
* `json_encode`
* Set header response
* HTTP status code
* Conditional response:

  * web
  * api

### Practice

* 1 controller → 2 output:

  * view
  * JSON
* Endpoint API sederhana

---

## PHASE 8 — Security Fundamental

> Target: aman di level pemula–menengah

### Materi

* XSS filtering
* CSRF protection
* Escaping output
* SQL Injection protection (Query Builder)
* Basic access control

### Practice

* Aktifkan CSRF
* Filter input
* Secure form

---

## PHASE 9 — Pagination, Search & Filter

> Target: data handling realistic

### Materi

* Pagination library
* Search data
* Filter by parameter
* Query Builder lanjutan

### Practice

* List data paginated
* Search keyword
* Filter rentang tanggal

---

## PHASE 10 — Mini App: Pemasukan & Pengeluaran Bulanan ✅

> Semua fundamental digabung

---

# 📌 Mini App Specification (Practice Final)

### Goal

Aplikasi sederhana untuk:

* Mencatat **pemasukan & pengeluaran**
* Rekap bulanan
* Support:

  * ✅ Web UI
  * ✅ JSON API

---

## Fitur

* CRUD transaksi
* Kategori transaksi
* Tanggal transaksi
* Tipe: `income | expense`
* Rekap bulanan
* Auth sederhana (1 user)

---

## Web Response

* List transaksi
* Tambah/edit/hapus
* Rekap per bulan

---

## JSON API

* `GET /api/transactions`
* `POST /api/transactions`
* `GET /api/summary?month=2025-01`

---

## Fundamental yang Dipakai

✅ Routing
✅ Controller
✅ Model
✅ View
✅ Validation
✅ Session
✅ Response JSON
✅ Security dasar
✅ Pagination
✅ Helper & Library

---

## Kenapa Roadmap Ini Cocok

* Tidak lompat ke “bisnis”
* Fokus **alat CI3**
* Semua materi terpakai di mini app
* Cocok untuk:

  * Akademik
  * Self-learning
  * Bootcamp
  * Onboarding junior

Kalau kamu mau, aku bisa lanjut ke:

* 📂 **Struktur folder final mini app CI3**
* 📘 **Silabus per minggu (minggu 1–6)**
* 🧪 **Checklist kompetensi sebelum lanjut ke mini app**
* 🧱 **Skeleton project CI3 (starter template)**
