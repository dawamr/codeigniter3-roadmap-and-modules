# 🔧 Database Configuration

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Mengkonfigurasi koneksi database di CI3
- ✅ Memahami setiap opsi konfigurasi
- ✅ Setup multiple database connections
- ✅ Menggunakan environment-based config

---

## 🤔 Mengapa Konfigurasi Penting?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

Konfigurasi database adalah **jembatan** antara aplikasi CI3 dengan database server.

**Analogi:** Seperti mengisi formulir untuk masuk ke gedung 🏢
- Nama gedung → hostname
- Nomor ID → username
- Password → password
- Lantai/ruangan → database name

</div>

---

## 📁 Lokasi File Konfigurasi

```
application/
└── config/
    └── database.php    ← File konfigurasi database
```

---

## 📋 Struktur Konfigurasi Dasar

```php
<?php
// application/config/database.php

defined('BASEPATH') OR exit('No direct script access allowed');

$active_group = 'default';
$query_builder = TRUE;

$db['default'] = array(
    'dsn'      => '',
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',
    'database' => 'nama_database',
    'dbdriver' => 'mysqli',
    'dbprefix' => '',
    'pconnect' => FALSE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt'  => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```

---

## 🔍 Penjelasan Setiap Opsi

### Opsi Wajib (Harus Diisi)

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Opsi | Penjelasan | Contoh |
|------|------------|--------|
| `hostname` | Alamat server database | `localhost`, `127.0.0.1`, `db.server.com` |
| `username` | Username database | `root`, `admin`, `db_user` |
| `password` | Password database | `secret123` |
| `database` | Nama database | `toko_online`, `blog_ci3` |
| `dbdriver` | Driver database | `mysqli`, `pdo`, `postgre`, `sqlite3` |

</div>

### Opsi Penting Lainnya

```php
<?php
$db['default'] = array(
    // === KONEKSI ===
    'hostname' => 'localhost',      // Server database
    'username' => 'root',           // Username
    'password' => '',               // Password (kosong untuk XAMPP default)
    'database' => 'belajar_ci3',    // Nama database
    'dbdriver' => 'mysqli',         // Driver: mysqli, pdo, postgre, sqlite3
    
    // === PREFIX ===
    'dbprefix' => 'tbl_',           // Prefix untuk semua tabel
    // Contoh: 'users' menjadi 'tbl_users'
    
    // === DEBUG ===
    'db_debug' => (ENVIRONMENT !== 'production'),
    // TRUE: Error ditampilkan (development)
    // FALSE: Error disembunyikan (production)
    
    // === CHARSET ===
    'char_set' => 'utf8',           // Character set
    'dbcollat' => 'utf8_general_ci', // Collation
    // Penting untuk emoji dan karakter internasional: 'utf8mb4'
    
    // === CACHE ===
    'cache_on' => FALSE,            // Query caching
    'cachedir' => '',               // Direktori cache
    
    // === CONNECTION ===
    'pconnect' => FALSE,            // Persistent connection
    // FALSE recommended untuk shared hosting
    
    // === SECURITY ===
    'encrypt'  => FALSE,            // SSL/TLS encryption
    'compress' => FALSE,            // Kompresi data
    'stricton' => FALSE,            // Strict mode MySQL
);
```

---

## 📊 Driver Database yang Didukung

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>🐬 MySQL/MariaDB</h4>

```php
'dbdriver' => 'mysqli',
```
Paling umum digunakan. Recommended untuk pemula.
</div>

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px;">
<h4>🐘 PostgreSQL</h4>

```php
'dbdriver' => 'postgre',
```
Database enterprise-level.
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px;">
<h4>📦 SQLite</h4>

```php
'dbdriver' => 'sqlite3',
'database' => '/path/to/database.db',
```
File-based, tanpa server.
</div>

<div style="background: #FCE4EC; padding: 15px; border-radius: 8px;">
<h4>🔌 PDO (Universal)</h4>

```php
'dbdriver' => 'pdo',
'dsn' => 'mysql:host=localhost;dbname=test',
```
Bisa connect ke berbagai database.
</div>

</div>

---

## 🔄 Multiple Database Connections

Untuk aplikasi yang butuh koneksi ke lebih dari satu database:

```php
<?php
// application/config/database.php

// Database utama
$db['default'] = array(
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',
    'database' => 'toko_online',
    'dbdriver' => 'mysqli',
    // ... opsi lainnya
);

// Database kedua (untuk analytics)
$db['analytics'] = array(
    'hostname' => 'analytics.server.com',
    'username' => 'analytics_user',
    'password' => 'analytics_pass',
    'database' => 'analytics_db',
    'dbdriver' => 'mysqli',
    // ... opsi lainnya
);

// Database ketiga (untuk legacy system)
$db['legacy'] = array(
    'hostname' => 'legacy.server.com',
    'username' => 'legacy_user',
    'password' => 'legacy_pass',
    'database' => 'old_system',
    'dbdriver' => 'mysqli',
    // ... opsi lainnya
);
```

### Menggunakan Multiple Database

```php
<?php
class Report_model extends CI_Model {
    
    private $db_analytics;
    
    public function __construct() {
        parent::__construct();
        
        // Load database kedua
        $this->db_analytics = $this->load->database('analytics', TRUE);
    }
    
    public function get_analytics() {
        // Query ke database analytics
        return $this->db_analytics->get('page_views')->result();
    }
    
    public function get_products() {
        // Query ke database default
        return $this->db->get('products')->result();
    }
}
```

---

## 🌍 Environment-Based Configuration

### Metode 1: Conditional dalam database.php

```php
<?php
// application/config/database.php

// Konfigurasi berbeda berdasarkan environment
if (ENVIRONMENT === 'development') {
    $db['default'] = array(
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => '',
        'database' => 'dev_database',
        'db_debug' => TRUE,
        // ...
    );
} 
elseif (ENVIRONMENT === 'testing') {
    $db['default'] = array(
        'hostname' => 'test.server.com',
        'username' => 'test_user',
        'password' => 'test_pass',
        'database' => 'test_database',
        'db_debug' => TRUE,
        // ...
    );
}
else { // production
    $db['default'] = array(
        'hostname' => 'prod.server.com',
        'username' => 'prod_user',
        'password' => 'super_secret',
        'database' => 'production_db',
        'db_debug' => FALSE,  // PENTING: FALSE di production!
        // ...
    );
}
```

### Metode 2: File Config per Environment

```
application/config/
├── database.php           # Default/fallback
├── development/
│   └── database.php       # Development config
├── testing/
│   └── database.php       # Testing config
└── production/
    └── database.php       # Production config
```

CI3 otomatis memilih file berdasarkan `ENVIRONMENT` constant.

### Metode 3: Environment Variables

```php
<?php
// application/config/database.php

$db['default'] = array(
    'hostname' => getenv('DB_HOST') ?: 'localhost',
    'username' => getenv('DB_USER') ?: 'root',
    'password' => getenv('DB_PASS') ?: '',
    'database' => getenv('DB_NAME') ?: 'default_db',
    'dbdriver' => 'mysqli',
    'db_debug' => (ENVIRONMENT !== 'production'),
    // ...
);
```

Set environment variable di server:
```bash
export DB_HOST=production.server.com
export DB_USER=prod_user
export DB_PASS=super_secret
export DB_NAME=prod_database
```

---

## 🔒 Security Best Practices

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px;">
<h3>✅ DO (Lakukan)</h3>
<ul>
<li>Set <code>db_debug = FALSE</code> di production</li>
<li>Gunakan user dengan permission minimal</li>
<li>Gunakan environment variables untuk credentials</li>
<li>Gunakan SSL untuk remote database</li>
<li>Backup database secara regular</li>
</ul>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px;">
<h3>❌ DON'T (Hindari)</h3>
<ul>
<li>Commit password ke Git</li>
<li>Gunakan user 'root' di production</li>
<li>Tampilkan error database ke user</li>
<li>Simpan password plain text di file</li>
<li>Grant ALL PRIVILEGES ke user app</li>
</ul>
</div>

</div>

---

## 🔌 Loading Database

### Autoload (Recommended)

```php
<?php
// application/config/autoload.php

$autoload['libraries'] = array('database');
```

### Manual Load

```php
<?php
// Di controller atau model
$this->load->database();
```

### Cek Koneksi

```php
<?php
class Test extends CI_Controller {
    
    public function db_check() {
        $this->load->database();
        
        if ($this->db->conn_id) {
            echo "✅ Database connected!";
            echo "<br>Database: " . $this->db->database;
        } else {
            echo "❌ Connection failed!";
        }
    }
}
```

---

## ⚠️ Troubleshooting

### Error: Unable to connect to database

<details>
<summary>🔧 Solusi</summary>

1. **Cek service MySQL running:**
   ```bash
   # Linux/Mac
   sudo service mysql status
   
   # XAMPP
   # Pastikan MySQL sudah di-Start
   ```

2. **Cek credentials:**
   ```php
   'hostname' => 'localhost',  // atau 127.0.0.1
   'username' => 'root',
   'password' => '',  // kosong untuk XAMPP default
   ```

3. **Cek database exists:**
   ```sql
   CREATE DATABASE IF NOT EXISTS nama_database;
   ```

</details>

### Error: No database selected

<details>
<summary>🔧 Solusi</summary>

Pastikan nama database sudah diisi:
```php
'database' => 'nama_database',  // Jangan kosong!
```

</details>

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Lokasi file konfigurasi database
- [ ] Opsi konfigurasi wajib (hostname, username, password, database, dbdriver)
- [ ] Perbedaan db_debug di development vs production
- [ ] Cara setup multiple database
- [ ] Environment-based configuration
- [ ] Security best practices

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="README.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Overview
      </button>
    </a>
  </div>
  <div>
    <a href="model-basics.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Model Fundamentals →
      </button>
    </a>
  </div>
</div>
