# 🌍 Environment Settings

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami perbedaan environment (development, testing, production)
- ✅ Mengkonfigurasi environment yang tepat
- ✅ Mengelola error handling per environment
- ✅ Menerapkan best practices untuk keamanan

---

## 🤔 Apa itu Environment?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Environment** adalah **mode operasi** aplikasi yang menentukan bagaimana aplikasi berperilaku.

**Analogi:** Seperti mode di smartphone 📱
- **Development** = Mode Debug (semua info ditampilkan)
- **Testing** = Mode Test (untuk QA)
- **Production** = Mode Normal (aman untuk publik)

</div>

---

## 📊 Tiga Environment di CI3

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; text-align: center; border-top: 4px solid #4CAF50;">
<h3>🛠️ Development</h3>
<p><strong>Untuk coding</strong></p>
<hr>
<ul style="text-align: left; font-size: 0.9em;">
<li>✅ Error ditampilkan</li>
<li>✅ Debug info aktif</li>
<li>✅ Log level: semua</li>
<li>❌ Tidak untuk publik</li>
</ul>
</div>

<div style="background: #FFF3E0; padding: 20px; border-radius: 10px; text-align: center; border-top: 4px solid #FF9800;">
<h3>🧪 Testing</h3>
<p><strong>Untuk QA/UAT</strong></p>
<hr>
<ul style="text-align: left; font-size: 0.9em;">
<li>⚠️ Error partial</li>
<li>✅ Test data</li>
<li>✅ Log level: error</li>
<li>❌ Tidak untuk publik</li>
</ul>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; text-align: center; border-top: 4px solid #F44336;">
<h3>🚀 Production</h3>
<p><strong>Untuk publik</strong></p>
<hr>
<ul style="text-align: left; font-size: 0.9em;">
<li>❌ Error disembunyikan</li>
<li>✅ Performa optimal</li>
<li>✅ Log level: error</li>
<li>✅ Aman untuk publik</li>
</ul>
</div>

</div>

---

## ⚙️ Mengatur Environment

### 📄 Di index.php

```php
<?php
// index.php - Baris paling atas

/*
|---------------------------------------------------------------
| APPLICATION ENVIRONMENT
|---------------------------------------------------------------
|
| Pilihan: 'development', 'testing', 'production'
|
*/
define('ENVIRONMENT', 'development');  // Ubah sesuai kebutuhan
```

### 🔐 Cara Lebih Aman: Environment Variable

Jangan hardcode di index.php! Gunakan environment variable:

```php
<?php
// index.php

// Ambil dari environment variable, default 'production' (lebih aman)
define('ENVIRONMENT', isset($_SERVER['CI_ENV']) ? $_SERVER['CI_ENV'] : 'production');
```

**Set Environment Variable:**

**Apache (.htaccess atau vhost):**
```apache
SetEnv CI_ENV development
```

**Nginx:**
```nginx
fastcgi_param CI_ENV development;
```

**Linux/Mac (terminal):**
```bash
export CI_ENV=development
```

**Windows (command prompt):**
```cmd
set CI_ENV=development
```

---

## 🚨 Error Handling per Environment

### Di index.php

```php
<?php
// index.php - Setelah define ENVIRONMENT

switch (ENVIRONMENT)
{
    case 'development':
        error_reporting(-1);           // Tampilkan SEMUA error
        ini_set('display_errors', 1);  // Tampilkan di browser
        break;

    case 'testing':
    case 'production':
        ini_set('display_errors', 0);  // Sembunyikan dari browser
        error_reporting(E_ALL & ~E_NOTICE & ~E_DEPRECATED & ~E_STRICT & ~E_USER_NOTICE & ~E_USER_DEPRECATED);
        break;

    default:
        header('HTTP/1.1 503 Service Unavailable.', TRUE, 503);
        echo 'Environment tidak valid.';
        exit(1);
}
```

### 📊 Perbandingan Error Handling

| Aspek | Development | Production |
|-------|-------------|------------|
| PHP Errors | ✅ Ditampilkan | ❌ Disembunyikan |
| Database Errors | ✅ Detail lengkap | ❌ Pesan generic |
| Stack Trace | ✅ Ya | ❌ Tidak |
| Log Files | ✅ Verbose | ✅ Error only |

---

## 🔧 Konfigurasi Berbasis Environment

### 📁 Database Config

```php
<?php
// application/config/database.php

// Konfigurasi berbeda untuk setiap environment
if (ENVIRONMENT === 'development') {
    $db['default'] = array(
        'hostname' => 'localhost',
        'username' => 'root',
        'password' => '',
        'database' => 'myapp_dev',
        'db_debug' => TRUE,  // Tampilkan error query
    );
} 
elseif (ENVIRONMENT === 'production') {
    $db['default'] = array(
        'hostname' => 'db.server.com',
        'username' => 'prod_user',
        'password' => 'super_secret_password',
        'database' => 'myapp_prod',
        'db_debug' => FALSE,  // Sembunyikan error query!
    );
}
```

### 📁 Config Berbasis Folder

CI3 mendukung konfigurasi per environment melalui folder:

```
application/config/
├── config.php              # Default config
├── database.php            # Default database
│
├── 📁 development/         # Override untuk development
│   ├── config.php
│   └── database.php
│
├── 📁 testing/             # Override untuk testing
│   └── database.php
│
└── 📁 production/          # Override untuk production
    ├── config.php
    └── database.php
```

**Contoh: config khusus development**

```php
<?php
// application/config/development/config.php

// Override hanya yang berbeda
$config['log_threshold'] = 4;  // Log semua
$config['csrf_protection'] = FALSE;  // Matikan CSRF di development
```

---

## 📝 Logging

### Konfigurasi Log

```php
<?php
// application/config/config.php

/*
| Log Threshold:
| 0 = Disables logging
| 1 = Error Messages (termasuk PHP errors)
| 2 = Debug Messages
| 3 = Informational Messages
| 4 = All Messages
*/
$config['log_threshold'] = 1;  // Production: hanya error
// $config['log_threshold'] = 4;  // Development: semua

$config['log_path'] = '';  // Default: application/logs/

$config['log_file_extension'] = '';  // Default: .php (tersembunyi)

$config['log_date_format'] = 'Y-m-d H:i:s';
```

### Menulis Log

```php
<?php
// Di controller atau model

// Error - masalah yang harus diperbaiki
log_message('error', 'Database connection failed: ' . $error);

// Debug - informasi untuk debugging
log_message('debug', 'User ID: ' . $user_id . ' accessed page');

// Info - informasi umum
log_message('info', 'Cron job completed successfully');
```

### Lokasi File Log

```
application/logs/
├── log-2024-01-15.php
├── log-2024-01-16.php
└── log-2024-01-17.php
```

**Format Log:**
```
ERROR - 2024-01-15 10:30:45 --> Database connection failed: Access denied
DEBUG - 2024-01-15 10:30:50 --> User ID: 5 logged in
INFO  - 2024-01-15 10:31:00 --> Email sent to john@mail.com
```

---

## 🔐 Best Practices Keamanan

### 1️⃣ Production Checklist

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Sebelum deploy ke production, pastikan:**

- [ ] `ENVIRONMENT` = `'production'`
- [ ] `db_debug` = `FALSE`
- [ ] `log_threshold` = `1` (error only)
- [ ] `csrf_protection` = `TRUE`
- [ ] Encryption key sudah diset
- [ ] Tidak ada password di code (gunakan env variable)
- [ ] Folder `application/logs/` tidak bisa diakses publik

</div>

### 2️⃣ Sembunyikan Error dari User

```php
<?php
// application/views/errors/html/error_general.php (production)

<!DOCTYPE html>
<html>
<head>
    <title>Oops! Something went wrong</title>
</head>
<body>
    <h1>Maaf, terjadi kesalahan</h1>
    <p>Tim kami sedang memperbaiki masalah ini.</p>
    <a href="<?= base_url() ?>">Kembali ke Home</a>
    
    <!-- JANGAN tampilkan detail error! -->
</body>
</html>
```

### 3️⃣ Protect Sensitive Folders

**.htaccess untuk folder application/:**
```apache
<IfModule authz_core_module>
    Require all denied
</IfModule>
<IfModule !authz_core_module>
    Deny from all
</IfModule>
```

### 4️⃣ Environment-Specific Features

```php
<?php
// Di controller

class Debug extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        
        // Hanya bisa diakses di development
        if (ENVIRONMENT !== 'development') {
            show_404();
        }
    }
    
    public function info() {
        phpinfo();  // Berbahaya di production!
    }
    
    public function logs() {
        // Tampilkan log files
    }
}
```

---

## 🔄 Workflow Development → Production

```
┌─────────────────────────────────────────────────────────────────┐
│                        DEVELOPMENT                               │
│  • localhost                                                     │
│  • ENVIRONMENT = 'development'                                   │
│  • Error ditampilkan                                             │
│  • Test dengan data dummy                                        │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                          TESTING                                 │
│  • staging.domain.com                                            │
│  • ENVIRONMENT = 'testing'                                       │
│  • QA testing                                                    │
│  • Data mirip production                                         │
└─────────────────────────────────────────────────────────────────┘
                              │
                              ▼
┌─────────────────────────────────────────────────────────────────┐
│                        PRODUCTION                                │
│  • www.domain.com                                                │
│  • ENVIRONMENT = 'production'                                    │
│  • Error disembunyikan                                           │
│  • Data real user                                                │
└─────────────────────────────────────────────────────────────────┘
```

---

## ✅ Checklist Environment

### Development
- [ ] ENVIRONMENT = 'development'
- [ ] Error reporting aktif
- [ ] db_debug = TRUE
- [ ] log_threshold = 4
- [ ] Bisa akses phpinfo()

### Production
- [ ] ENVIRONMENT = 'production'
- [ ] Error disembunyikan
- [ ] db_debug = FALSE
- [ ] log_threshold = 1
- [ ] CSRF aktif
- [ ] HTTPS aktif
- [ ] Sensitive folders protected

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="request-flow.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Request Flow
      </button>
    </a>
  </div>
  <div>
    <a href="practice.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Practice Lab →
      </button>
    </a>
  </div>
</div>
