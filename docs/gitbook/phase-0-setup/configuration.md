# ⚡ Configuration

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Mengkonfigurasi `config.php` dengan benar
- ✅ Setup koneksi database di `database.php`
- ✅ Menggunakan `autoload.php` untuk load otomatis
- ✅ Memahami konfigurasi penting lainnya

---

## 🤔 Mengapa Konfigurasi Penting?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Analogi:** Konfigurasi adalah seperti **pengaturan di smartphone** 📱

- Base URL = Nomor telepon Anda
- Database = Koneksi ke server cloud
- Autoload = Apps yang otomatis jalan saat startup

Tanpa konfigurasi yang benar, aplikasi Anda tidak akan berjalan dengan baik!

</div>

---

## 📄 config.php - Konfigurasi Utama

File: `application/config/config.php`

### 🌐 Base URL (WAJIB!)

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Ini adalah konfigurasi PALING PENTING!** Jika salah, link dan redirect tidak akan berfungsi.

</div>

```php
<?php
// application/config/config.php

/*
|--------------------------------------------------------------------------
| Base Site URL
|--------------------------------------------------------------------------
| URL to your CodeIgniter root. Include trailing slash!
*/
$config['base_url'] = 'http://localhost/belajar-ci3/';
```

**Aturan Base URL:**
| Environment | Contoh Base URL |
|-------------|-----------------|
| Localhost | `http://localhost/nama_project/` |
| Subdomain | `http://ci3.localhost/` |
| Production | `https://www.domain.com/` |

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Tips:**
- Selalu akhiri dengan `/` (trailing slash)
- Gunakan `https://` untuk production
- Untuk dynamic URL:
```php
$config['base_url'] = ((isset($_SERVER['HTTPS']) && $_SERVER['HTTPS'] == "on") ? "https" : "http");
$config['base_url'] .= "://" . $_SERVER['HTTP_HOST'];
$config['base_url'] .= str_replace(basename($_SERVER['SCRIPT_NAME']), "", $_SERVER['SCRIPT_NAME']);
```

</div>

### 🔗 Index Page

```php
/*
|--------------------------------------------------------------------------
| Index File
|--------------------------------------------------------------------------
| If you are using mod_rewrite to remove index.php, set this to empty
*/
$config['index_page'] = 'index.php';  // Default
$config['index_page'] = '';            // Setelah setup .htaccess
```

**Perbedaan:**
- **Dengan index.php:** `http://localhost/ci3/index.php/user/profile`
- **Tanpa index.php:** `http://localhost/ci3/user/profile` ✨ (lebih bersih!)

### 🔐 Encryption Key

```php
/*
|--------------------------------------------------------------------------
| Encryption Key
|--------------------------------------------------------------------------
| Used for session & cookie encryption. WAJIB diisi!
*/
$config['encryption_key'] = 'your-32-character-secret-key!!';
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Cara Generate Encryption Key:**

**Opsi 1 - Online Generator:**
Cari "random string generator 32 characters"

**Opsi 2 - PHP:**
```php
echo bin2hex(random_bytes(16));  // Output: 32 karakter hex
```

**Opsi 3 - Manual:**
Ketik 32 karakter random: `aB3$kL9#mN7@pQ1!rS5*tU8^vW2&xY4`

</div>

### 🔧 Konfigurasi Lainnya

```php
// Timezone
date_default_timezone_set('Asia/Jakarta');

// Session
$config['sess_driver'] = 'files';           // Driver: files, database, redis
$config['sess_save_path'] = NULL;           // Path untuk session files
$config['sess_cookie_name'] = 'ci_session';
$config['sess_expiration'] = 7200;          // 2 jam dalam detik

// Cookie
$config['cookie_prefix'] = '';
$config['cookie_domain'] = '';
$config['cookie_path'] = '/';
$config['cookie_secure'] = FALSE;           // TRUE untuk HTTPS only

// CSRF Protection
$config['csrf_protection'] = TRUE;          // Aktifkan untuk keamanan
$config['csrf_token_name'] = 'csrf_token';
$config['csrf_cookie_name'] = 'csrf_cookie';

// Global XSS Filtering
$config['global_xss_filtering'] = FALSE;    // TRUE = filter semua input
```

---

## 💾 database.php - Koneksi Database

File: `application/config/database.php`

### 🔌 Konfigurasi Dasar

```php
<?php
// application/config/database.php

$active_group = 'default';  // Grup koneksi yang aktif
$query_builder = TRUE;      // Aktifkan Query Builder

$db['default'] = array(
    'dsn'      => '',
    'hostname' => 'localhost',           // Host database
    'username' => 'root',                // Username MySQL
    'password' => '',                    // Password MySQL
    'database' => 'belajar_ci3',         // Nama database
    'dbdriver' => 'mysqli',              // Driver (mysqli/pdo)
    'dbprefix' => '',                    // Prefix tabel (optional)
    'pconnect' => FALSE,                 // Persistent connection
    'db_debug' => TRUE,                  // Show error (FALSE di production!)
    'cache_on' => FALSE,                 // Query caching
    'cachedir' => '',
    'char_set' => 'utf8',                // Character set
    'dbcollat' => 'utf8_general_ci',     // Collation
    'swap_pre' => '',
    'encrypt'  => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```

### 📋 Penjelasan Parameter Penting

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Parameter | Deskripsi | Contoh |
|-----------|-----------|--------|
| `hostname` | Alamat server database | `localhost`, `127.0.0.1` |
| `username` | Username MySQL | `root`, `db_user` |
| `password` | Password MySQL | Kosong untuk local, isi untuk production |
| `database` | Nama database | `belajar_ci3` |
| `dbdriver` | Driver database | `mysqli` (MySQL), `pdo` |
| `dbprefix` | Prefix tabel | `ci_` → semua tabel jadi `ci_users` |
| `db_debug` | Tampilkan error | `TRUE` development, `FALSE` production |
| `char_set` | Encoding | `utf8` atau `utf8mb4` (support emoji) |

</div>

### 🔀 Multiple Database Connections

Untuk koneksi ke lebih dari satu database:

```php
<?php
// Database utama
$db['default'] = array(
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',
    'database' => 'app_utama',
    'dbdriver' => 'mysqli',
    // ... config lainnya
);

// Database kedua (misal: legacy system)
$db['legacy'] = array(
    'hostname' => '192.168.1.100',
    'username' => 'legacy_user',
    'password' => 'legacy_pass',
    'database' => 'old_system',
    'dbdriver' => 'mysqli',
    // ... config lainnya
);
```

**Cara Menggunakan:**
```php
<?php
class Some_model extends CI_Model {
    
    public function __construct() {
        parent::__construct();
        
        // Load database default (otomatis jika di autoload)
        $this->load->database();
        
        // Load database legacy
        $this->legacy_db = $this->load->database('legacy', TRUE);
    }
    
    public function get_from_legacy() {
        return $this->legacy_db->get('old_table')->result();
    }
}
```

---

## 📦 autoload.php - Load Otomatis

File: `application/config/autoload.php`

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Apa itu Autoload?**

Autoload memungkinkan library, helper, atau model di-load **otomatis** setiap request. Tidak perlu `$this->load->...` berulang-ulang!

</div>

### 📚 Libraries

```php
<?php
// Load libraries otomatis
$autoload['libraries'] = array(
    'database',      // Koneksi database
    'session',       // Session handling
    'form_validation' // Form validation
);
```

**Libraries yang umum di-autoload:**
| Library | Fungsi | Kapan Dibutuhkan |
|---------|--------|------------------|
| `database` | Koneksi DB | Hampir selalu |
| `session` | Manage session | Jika pakai login/cart |
| `form_validation` | Validasi form | Jika banyak form |

### 🔧 Helpers

```php
<?php
// Load helpers otomatis
$autoload['helper'] = array(
    'url',       // base_url(), site_url(), redirect()
    'form',      // form_open(), form_input()
    'security',  // xss_clean(), do_hash()
    'date'       // now(), mdate()
);
```

**Helpers yang umum di-autoload:**
| Helper | Fungsi Penting | 
|--------|----------------|
| `url` | `base_url()`, `site_url()`, `redirect()` |
| `form` | `form_open()`, `form_input()`, `form_close()` |
| `html` | `heading()`, `img()`, `link_tag()` |
| `security` | `xss_clean()`, `do_hash()` |

### 🗃️ Models

```php
<?php
// Load models otomatis (jarang digunakan)
$autoload['model'] = array(
    'user_model',
    'product_model'
);
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Tips:** Jangan autoload terlalu banyak model! Load model hanya di controller yang membutuhkan untuk performa lebih baik.

</div>

### ⚙️ Config Files

```php
<?php
// Load config files tambahan
$autoload['config'] = array(
    'email',      // Konfigurasi email
    'pagination'  // Konfigurasi pagination
);
```

### 📋 Contoh autoload.php Lengkap

```php
<?php
// application/config/autoload.php

$autoload['packages'] = array();

$autoload['libraries'] = array(
    'database',
    'session'
);

$autoload['drivers'] = array();

$autoload['helper'] = array(
    'url',
    'form',
    'security'
);

$autoload['config'] = array();

$autoload['language'] = array();

$autoload['model'] = array();
```

---

## 🛣️ routes.php - Custom Routing

File: `application/config/routes.php`

### 🔀 Default Routes

```php
<?php
// application/config/routes.php

// Controller default (home page)
$route['default_controller'] = 'welcome';

// Halaman 404
$route['404_override'] = '';  // Atau 'errors/not_found'

// Translate URI dashes ke underscore
$route['translate_uri_dashes'] = FALSE;
```

### 🎯 Custom Routes

```php
<?php
// Static route
$route['about'] = 'pages/about';
// URL: /about → controller: Pages, method: about

// Route dengan parameter
$route['product/(:num)'] = 'catalog/product_lookup/$1';
// URL: /product/123 → catalog/product_lookup(123)

// Route dengan regex
$route['user/([a-z]+)/(\d+)'] = 'user/view/$1/$2';
// URL: /user/john/5 → user/view('john', 5)

// Route untuk API
$route['api/users']['GET'] = 'api/users/index';
$route['api/users']['POST'] = 'api/users/create';
$route['api/users/(:num)']['PUT'] = 'api/users/update/$1';
$route['api/users/(:num)']['DELETE'] = 'api/users/delete/$1';
```

### 📊 Wildcard Patterns

| Pattern | Cocok Dengan | Contoh |
|---------|--------------|--------|
| `(:num)` | Angka saja | `123`, `456` |
| `(:any)` | Semua karakter | `hello`, `test-123` |
| `(:segment)` | Apapun kecuali `/` | Mirip `(:any)` |

---

## 🔐 Menghilangkan index.php dari URL

### Langkah 1: Buat/Edit .htaccess

```apache
# Buat file .htaccess di root project (sejajar dengan index.php)

RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```

### Langkah 2: Edit config.php

```php
// Ubah dari:
$config['index_page'] = 'index.php';

// Menjadi:
$config['index_page'] = '';
```

### Langkah 3: Aktifkan mod_rewrite

Pastikan `mod_rewrite` aktif di Apache:
```bash
# Linux/Mac
sudo a2enmod rewrite
sudo service apache2 restart

# XAMPP Windows: biasanya sudah aktif
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

✅ **Hasil:**
- Sebelum: `http://localhost/ci3/index.php/user/profile`
- Sesudah: `http://localhost/ci3/user/profile`

</div>

---

## ✅ Checklist Konfigurasi

Sebelum mulai coding, pastikan:

- [ ] `base_url` sudah diset dengan benar
- [ ] `encryption_key` sudah diisi (32 karakter)
- [ ] Database sudah dikonfigurasi dan bisa connect
- [ ] Helpers penting sudah di-autoload (`url`, `form`)
- [ ] Libraries penting sudah di-autoload (`database`, `session`)
- [ ] `.htaccess` sudah dibuat (opsional tapi recommended)

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="structure.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Project Structure
      </button>
    </a>
  </div>
  <div>
    <a href="request-flow.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Request Flow →
      </button>
    </a>
  </div>
</div>
