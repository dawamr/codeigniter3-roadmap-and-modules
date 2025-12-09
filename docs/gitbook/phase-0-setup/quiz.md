# ❓ Quiz: Phase 0 - Environment & Setup

## 🎯 Tentang Quiz Ini

Quiz ini menguji pemahaman Anda tentang instalasi, struktur, konfigurasi, dan konsep dasar CodeIgniter 3. Terdapat **15 pertanyaan**.

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### 📊 Passing Score

| Skor | Status | Rekomendasi |
|------|--------|-------------|
| **12-15** | 🟢 Excellent | Lanjut ke Phase 1! |
| **9-11** | 🟡 Good | Review materi yang kurang |
| **6-8** | 🟠 Need Review | Pelajari ulang beberapa bagian |
| **0-5** | 🔴 Try Again | Ulangi Phase 0 dari awal |

</div>

---

## 📁 Bagian 1: Project Structure (5 Pertanyaan)

### Pertanyaan 1
**Folder mana yang berisi file-file yang Anda buat (controllers, models, views)?**

- [ ] A. `system/`
- [ ] B. `application/`
- [ ] C. `user_guide/`
- [ ] D. `vendor/`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `application/`**

**Penjelasan:**
- `application/` → Folder kerja utama Anda (controllers, models, views, config)
- `system/` → Core CI3, JANGAN diubah!
- `user_guide/` → Dokumentasi offline
- `vendor/` → Untuk Composer (jika digunakan)

</details>

---

### Pertanyaan 2
**Mengapa folder `system/` tidak boleh diubah?**

- [ ] A. Karena read-only
- [ ] B. Karena berisi core framework yang akan hilang saat update
- [ ] C. Karena hanya untuk Linux
- [ ] D. Karena berisi database

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Karena berisi core framework yang akan hilang saat update**

**Penjelasan:**
Folder `system/` berisi kode inti CodeIgniter. Jika Anda mengubahnya:
- Perubahan akan **hilang** saat update CI3
- Bisa menyebabkan **bug** yang sulit dilacak
- Tidak sesuai **best practice**

**Solusi:** Jika perlu extend core class, buat di `application/core/` dengan prefix `MY_`.

</details>

---

### Pertanyaan 3
**Di mana Anda meletakkan file CSS, JavaScript, dan images?**

- [ ] A. `application/assets/`
- [ ] B. `system/assets/`
- [ ] C. Buat folder `assets/` di root project
- [ ] D. `application/views/assets/`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Buat folder `assets/` di root project**

**Penjelasan:**
CI3 tidak menyediakan folder assets. Best practice adalah membuat folder `assets/` di **root project** (sejajar dengan `index.php`):

```
project/
├── application/
├── system/
├── assets/       ← Buat di sini
│   ├── css/
│   ├── js/
│   └── images/
└── index.php
```

Akses dengan: `<?= base_url('assets/css/style.css') ?>`

</details>

---

### Pertanyaan 4
**File `index.php` di root project berfungsi sebagai?**

- [ ] A. Halaman utama website
- [ ] B. Front Controller (pintu masuk semua request)
- [ ] C. File konfigurasi
- [ ] D. View template

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Front Controller (pintu masuk semua request)**

**Penjelasan:**
`index.php` adalah **Front Controller** - SEMUA request masuk melalui file ini, kemudian diarahkan ke controller yang sesuai.

```
http://site.com/user/profile
       ↓
    index.php (Front Controller)
       ↓
    Router menentukan: User controller, profile method
       ↓
    User.php -> profile()
```

</details>

---

### Pertanyaan 5
**Folder `application/config/` berisi?**

- [ ] A. File CSS dan JavaScript
- [ ] B. File konfigurasi aplikasi
- [ ] C. File controller
- [ ] D. File database

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. File konfigurasi aplikasi**

**Penjelasan:**
Folder `config/` berisi semua pengaturan aplikasi:

| File | Fungsi |
|------|--------|
| `config.php` | Base URL, session, encryption |
| `database.php` | Koneksi database |
| `autoload.php` | Auto-load libraries/helpers |
| `routes.php` | Custom URL routing |

</details>

---

## ⚙️ Bagian 2: Configuration (5 Pertanyaan)

### Pertanyaan 6
**Konfigurasi `base_url` yang benar untuk localhost adalah?**

- [ ] A. `$config['base_url'] = 'localhost/myapp';`
- [ ] B. `$config['base_url'] = 'http://localhost/myapp/';`
- [ ] C. `$config['base_url'] = 'http://localhost/myapp';`
- [ ] D. `$config['base_url'] = '/myapp/';`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$config['base_url'] = 'http://localhost/myapp/';`**

**Penjelasan:**
Base URL harus:
- ✅ Diawali dengan protocol (`http://` atau `https://`)
- ✅ Diakhiri dengan trailing slash (`/`)

```php
// ✅ Benar
$config['base_url'] = 'http://localhost/myapp/';

// ❌ Salah
$config['base_url'] = 'localhost/myapp';     // Tanpa protocol
$config['base_url'] = 'http://localhost/myapp'; // Tanpa trailing slash
```

</details>

---

### Pertanyaan 7
**Untuk menghilangkan `index.php` dari URL, apa yang perlu dilakukan?**

- [ ] A. Hapus file index.php
- [ ] B. Buat .htaccess dan set `$config['index_page'] = ''`
- [ ] C. Ubah nama index.php
- [ ] D. Edit file di folder system/

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Buat .htaccess dan set `$config['index_page'] = ''`**

**Penjelasan:**
Dua langkah yang diperlukan:

**1. Buat `.htaccess`:**
```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```

**2. Edit `config.php`:**
```php
$config['index_page'] = '';
```

Hasil:
- Sebelum: `http://site.com/index.php/user/profile`
- Sesudah: `http://site.com/user/profile`

</details>

---

### Pertanyaan 8
**Di file mana konfigurasi koneksi database berada?**

- [ ] A. `application/config/config.php`
- [ ] B. `application/config/database.php`
- [ ] C. `application/config/autoload.php`
- [ ] D. `system/database/config.php`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `application/config/database.php`**

**Penjelasan:**
File `database.php` berisi semua konfigurasi database:

```php
$db['default'] = array(
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',
    'database' => 'myapp',
    'dbdriver' => 'mysqli',
    // ...
);
```

</details>

---

### Pertanyaan 9
**Fungsi `autoload.php` adalah?**

- [ ] A. Mengload file secara otomatis saat upload
- [ ] B. Mengload libraries/helpers secara otomatis di setiap request
- [ ] C. Mengload database otomatis
- [ ] D. Mengload view otomatis

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Mengload libraries/helpers secara otomatis di setiap request**

**Penjelasan:**
Dengan autoload, Anda tidak perlu menulis `$this->load->...` berulang-ulang:

```php
// application/config/autoload.php

$autoload['libraries'] = array('database', 'session');
$autoload['helper'] = array('url', 'form');
```

Sekarang `database`, `session`, `url`, dan `form` helper tersedia di **semua controller** tanpa perlu load manual.

</details>

---

### Pertanyaan 10
**Apa fungsi `encryption_key` di config.php?**

- [ ] A. Password database
- [ ] B. Enkripsi untuk session dan data sensitif
- [ ] C. API key
- [ ] D. Password admin

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Enkripsi untuk session dan data sensitif**

**Penjelasan:**
`encryption_key` digunakan untuk:
- ✅ Mengenkripsi session data
- ✅ Mengenkripsi cookie
- ✅ Encrypt library untuk data sensitif

```php
$config['encryption_key'] = 'your-32-character-key-here!!';
```

⚠️ **Penting:** Harus diisi dengan string random minimal 32 karakter!

</details>

---

## 🔄 Bagian 3: Request Flow & Environment (5 Pertanyaan)

### Pertanyaan 11
**Urutan yang benar saat request masuk ke CI3 adalah?**

- [ ] A. View → Controller → Model → Output
- [ ] B. index.php → Router → Controller → Model/View → Output
- [ ] C. Controller → Router → Model → View
- [ ] D. Database → Model → Controller → View

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. index.php → Router → Controller → Model/View → Output**

**Penjelasan:**
Alur lengkap:
```
1. Request masuk → index.php (Front Controller)
2. Router parse URL → Tentukan controller & method
3. Controller dipanggil
4. Controller load Model (jika perlu database)
5. Controller load View (kirim data)
6. Output dikirim ke browser
```

</details>

---

### Pertanyaan 12
**Dalam MVC, komponen mana yang berinteraksi dengan database?**

- [ ] A. View
- [ ] B. Controller
- [ ] C. Model
- [ ] D. Router

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Model**

**Penjelasan:**
Pembagian tugas MVC:
- **Model** → Data layer (query database)
- **View** → Presentation layer (tampilkan HTML)
- **Controller** → Logic layer (koordinasi M & V)

```php
// Model - berinteraksi dengan database
class User_model extends CI_Model {
    public function get_all() {
        return $this->db->get('users')->result();
    }
}
```

</details>

---

### Pertanyaan 13
**Apa perbedaan environment `development` dan `production`?**

- [ ] A. Tidak ada perbedaan
- [ ] B. Development menampilkan error, production menyembunyikan
- [ ] C. Production lebih lambat
- [ ] D. Development tidak bisa akses database

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Development menampilkan error, production menyembunyikan**

**Penjelasan:**

| Aspek | Development | Production |
|-------|-------------|------------|
| Error display | ✅ Ditampilkan | ❌ Disembunyikan |
| Debug info | ✅ Ya | ❌ Tidak |
| db_debug | TRUE | FALSE |
| Log level | All | Error only |

```php
// index.php
define('ENVIRONMENT', 'development');  // Coding
define('ENVIRONMENT', 'production');   // Live
```

</details>

---

### Pertanyaan 14
**Di file mana environment aplikasi diset?**

- [ ] A. `application/config/config.php`
- [ ] B. `application/config/database.php`
- [ ] C. `index.php`
- [ ] D. `.htaccess`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `index.php`**

**Penjelasan:**
Environment diset di bagian atas `index.php`:

```php
<?php
// index.php - baris pertama

define('ENVIRONMENT', 'development');
// atau
define('ENVIRONMENT', 'production');
```

Ini mempengaruhi:
- Error reporting
- Logging level
- Cache behavior

</details>

---

### Pertanyaan 15
**Apa yang terjadi jika `db_debug` = TRUE di production?**

- [ ] A. Database lebih cepat
- [ ] B. Error database ditampilkan ke user (berbahaya!)
- [ ] C. Database tidak bisa diakses
- [ ] D. Tidak ada efek

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Error database ditampilkan ke user (berbahaya!)**

**Penjelasan:**
Jika `db_debug = TRUE` di production:
- ❌ Error SQL ditampilkan ke user
- ❌ Nama tabel, kolom terekspos
- ❌ Potential SQL injection info leak
- ❌ Security vulnerability!

```php
// application/config/database.php

// ✅ Cara aman
'db_debug' => (ENVIRONMENT !== 'production'),

// atau
'db_debug' => FALSE,  // Di production
```

</details>

---

## 📊 Hitung Skor Anda

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

### 📝 Scorecard

| Bagian | Benar | Total |
|--------|-------|-------|
| Project Structure (1-5) | ___ | 5 |
| Configuration (6-10) | ___ | 5 |
| Request Flow & Environment (11-15) | ___ | 5 |
| **TOTAL** | ___ | **15** |

</div>

---

## 🎯 Hasil Quiz

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; border-left: 4px solid #4CAF50;">
<h3>🟢 12-15 Benar</h3>
<p><strong>Excellent!</strong></p>
<p>Anda menguasai Phase 0 dengan baik. Siap lanjut ke Phase 1!</p>
<a href="../phase-1-controller/README.md">➡️ Lanjut ke Phase 1</a>
</div>

<div style="background: #FFF9C4; padding: 20px; border-radius: 10px; border-left: 4px solid #FDD835;">
<h3>🟡 9-11 Benar</h3>
<p><strong>Good!</strong></p>
<p>Hampir sempurna. Review materi yang kurang, lalu lanjut.</p>
<a href="README.md">📖 Review Phase 0</a>
</div>

<div style="background: #FFE0B2; padding: 20px; border-radius: 10px; border-left: 4px solid #FF9800;">
<h3>🟠 6-8 Benar</h3>
<p><strong>Need Review</strong></p>
<p>Pelajari ulang beberapa bagian sebelum lanjut.</p>
<a href="structure.md">📁 Review Structure</a>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; border-left: 4px solid #F44336;">
<h3>🔴 0-5 Benar</h3>
<p><strong>Try Again</strong></p>
<p>Ulangi Phase 0 dari awal untuk pemahaman lebih baik.</p>
<a href="installation.md">📥 Mulai dari Installation</a>
</div>

</div>

---

## 📚 Area yang Perlu Diperbaiki

| Jika salah di... | Review |
|------------------|--------|
| Pertanyaan 1-5 | [📁 Project Structure](structure.md) |
| Pertanyaan 6-10 | [⚡ Configuration](configuration.md) |
| Pertanyaan 11-13 | [🔄 Request Flow](request-flow.md) |
| Pertanyaan 14-15 | [🌍 Environment](environment.md) |

---

## 🚀 Siap Lanjut?

<div style="text-align: center; margin: 40px 0;">

<p>Jika skor Anda 12+, Anda siap untuk Phase 1!</p>

<a href="../phase-1-controller/README.md">
<button style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 15px 40px; border: none; border-radius: 25px; font-size: 18px; cursor: pointer;">
🎮 Mulai Phase 1: Controller & Routing
</button>
</a>

</div>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="practice.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Practice Lab
      </button>
    </a>
  </div>
  <div>
    <a href="../phase-1-controller/README.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Phase 1: Controller →
      </button>
    </a>
  </div>
</div>
