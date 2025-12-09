# 📁 Project Structure

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami struktur folder CodeIgniter 3
- ✅ Mengetahui fungsi setiap folder utama
- ✅ Paham di mana meletakkan file yang tepat
- ✅ Mengenal file-file penting di CI3

---

## 🤔 Mengapa Perlu Memahami Struktur?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Analogi:** Bayangkan CI3 seperti **kantor perusahaan** 🏢

- 📁 `application/` = Ruang kerja utama (tempat Anda coding)
- 📁 `system/` = Ruang mesin (jangan disentuh!)
- 📄 `index.php` = Pintu masuk utama

Dengan memahami struktur, Anda tahu persis **di mana meletakkan apa** - seperti tahu di mana gudang, ruang meeting, dan meja kerja Anda!

</div>

---

## 📊 Struktur Folder Lengkap

```
codeigniter3/
│
├── 📁 application/          ⭐ FOLDER KERJA UTAMA
│   ├── 📁 cache/            # Cache files
│   ├── 📁 config/           # Konfigurasi aplikasi
│   ├── 📁 controllers/      # Controller files
│   ├── 📁 core/             # Extended core classes
│   ├── 📁 helpers/          # Custom helper functions
│   ├── 📁 hooks/            # Hook scripts
│   ├── 📁 language/         # Language files
│   ├── 📁 libraries/        # Custom libraries
│   ├── 📁 logs/             # Log files
│   ├── 📁 models/           # Model files
│   ├── 📁 third_party/      # Third-party libraries
│   └── 📁 views/            # View files (HTML)
│
├── 📁 system/               🔒 JANGAN DIUBAH!
│   ├── 📁 core/             # CI3 core classes
│   ├── 📁 database/         # Database drivers
│   ├── 📁 helpers/          # Built-in helpers
│   └── 📁 libraries/        # Built-in libraries
│
├── 📁 user_guide/           # Dokumentasi offline
│
├── 📄 index.php             🚪 PINTU MASUK
├── 📄 .htaccess             # URL rewriting rules
└── 📄 composer.json         # (Opsional) Composer config
```

---

## 📂 Folder Application (Detail)

Ini adalah folder utama tempat Anda akan bekerja. Mari bahas satu per satu:

### 🎮 controllers/

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menerima request dari user dan menentukan apa yang harus dilakukan.

**Analogi:** Seperti **resepsionis** yang menerima tamu dan mengarahkan ke ruangan yang tepat.

</div>

```
application/controllers/
├── Welcome.php        # Controller default
├── User.php           # Handle user-related actions
├── Product.php        # Handle product actions
└── 📁 admin/          # Sub-folder untuk admin
    ├── Dashboard.php
    └── Settings.php
```

**Contoh Controller:**
```php
<?php
// application/controllers/User.php

class User extends CI_Controller {
    
    public function index() {
        // Halaman utama user
    }
    
    public function profile($id) {
        // Tampilkan profil user
    }
    
    public function login() {
        // Proses login
    }
}
```

**URL Mapping:**
| URL | Controller | Method |
|-----|------------|--------|
| `/user` | User.php | index() |
| `/user/profile/5` | User.php | profile(5) |
| `/user/login` | User.php | login() |

---

### 👁️ views/

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menampilkan data ke user dalam bentuk HTML.

**Analogi:** Seperti **layar TV** yang menampilkan hasil kerja di belakang layar.

</div>

```
application/views/
├── welcome_message.php    # View default
├── 📁 user/               # Views untuk user
│   ├── index.php
│   ├── profile.php
│   └── login.php
├── 📁 product/            # Views untuk product
│   ├── list.php
│   └── detail.php
└── 📁 templates/          # Template layout
    ├── header.php
    ├── footer.php
    └── sidebar.php
```

**Contoh View:**
```php
<!-- application/views/user/profile.php -->
<!DOCTYPE html>
<html>
<head>
    <title>Profil <?= $user['nama'] ?></title>
</head>
<body>
    <h1>Profil User</h1>
    <p>Nama: <?= $user['nama'] ?></p>
    <p>Email: <?= $user['email'] ?></p>
</body>
</html>
```

---

### 💾 models/

<div style="background: #FFF3E0; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Berinteraksi dengan database (CRUD operations).

**Analogi:** Seperti **staf gudang** yang mengambil dan menyimpan barang.

</div>

```
application/models/
├── User_model.php         # Model untuk tabel users
├── Product_model.php      # Model untuk tabel products
└── Order_model.php        # Model untuk tabel orders
```

**Contoh Model:**
```php
<?php
// application/models/User_model.php

class User_model extends CI_Model {
    
    private $table = 'users';
    
    public function get_all() {
        return $this->db->get($this->table)->result();
    }
    
    public function get_by_id($id) {
        return $this->db->get_where($this->table, ['id' => $id])->row();
    }
    
    public function create($data) {
        return $this->db->insert($this->table, $data);
    }
}
```

---

### ⚙️ config/

<div style="background: #FCE4EC; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menyimpan semua konfigurasi aplikasi.

**Analogi:** Seperti **buku aturan** perusahaan yang mengatur cara kerja.

</div>

```
application/config/
├── autoload.php       # Auto-load libraries, helpers
├── config.php         # Konfigurasi utama (base_url, dll)
├── database.php       # Koneksi database
├── routes.php         # Custom URL routing
├── constants.php      # Konstanta aplikasi
├── hooks.php          # Hook configurations
└── migration.php      # Database migration settings
```

<div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0;">

| File | Fungsi | Kapan Diubah |
|------|--------|--------------|
| `config.php` | Base URL, encryption key | Saat setup awal |
| `database.php` | Host, username, password DB | Saat koneksi database |
| `autoload.php` | Load otomatis libraries | Saat butuh helper/library global |
| `routes.php` | Custom URL patterns | Saat butuh URL khusus |

</div>

---

### 📦 libraries/

<div style="background: #E0F7FA; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menyimpan custom library (class) buatan Anda.

**Analogi:** Seperti **mesin khusus** yang Anda buat untuk tugas tertentu.

</div>

```
application/libraries/
├── Template.php       # Library untuk templating
├── Auth.php           # Library untuk authentication
└── Api_response.php   # Library untuk API responses
```

**Contoh Library:**
```php
<?php
// application/libraries/Template.php

class Template {
    protected $CI;
    
    public function __construct() {
        $this->CI =& get_instance();
    }
    
    public function load($view, $data = []) {
        $this->CI->load->view('templates/header', $data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('templates/footer');
    }
}
```

---

### 🔧 helpers/

<div style="background: #F3E5F5; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menyimpan fungsi-fungsi helper buatan Anda.

**Analogi:** Seperti **perkakas** yang membantu pekerjaan sehari-hari.

</div>

```
application/helpers/
├── format_helper.php      # Fungsi formatting
├── auth_helper.php        # Fungsi authentication
└── string_helper.php      # Fungsi string manipulation
```

**Contoh Helper:**
```php
<?php
// application/helpers/format_helper.php

if (!function_exists('format_rupiah')) {
    function format_rupiah($angka) {
        return 'Rp ' . number_format($angka, 0, ',', '.');
    }
}

if (!function_exists('format_tanggal')) {
    function format_tanggal($date) {
        return date('d M Y', strtotime($date));
    }
}
```

---

## 🔒 Folder System

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; margin: 20px 0;">

⚠️ **PENTING: Jangan pernah mengubah file di folder `system/`!**

Folder ini berisi **core framework** CodeIgniter 3. Mengubahnya bisa:
- ❌ Merusak framework
- ❌ Hilang saat update
- ❌ Menyebabkan bug yang sulit dilacak

**Jika perlu extend core:** Gunakan folder `application/core/` untuk membuat `MY_Controller`, `MY_Model`, dll.

</div>

---

## 🚪 File index.php

File `index.php` adalah **pintu masuk utama** (front controller) untuk semua request.

```php
<?php
// index.php - Bagian penting

// Environment (development, testing, production)
define('ENVIRONMENT', 'development');

// Path ke folder system
$system_path = 'system';

// Path ke folder application
$application_folder = 'application';
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Yang perlu diubah di index.php:**
- `ENVIRONMENT` → Ubah ke 'production' saat live
- `$system_path` → Jika folder system dipindah
- `$application_folder` → Jika folder application dipindah

</div>

---

## 🎯 Quick Reference: Di Mana Meletakkan Apa?

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Anda Ingin... | Letakkan di... | Contoh |
|---------------|----------------|--------|
| Menangani URL request | `controllers/` | `User.php` |
| Menampilkan HTML | `views/` | `user/profile.php` |
| Query database | `models/` | `User_model.php` |
| Konfigurasi | `config/` | `config.php` |
| Class reusable | `libraries/` | `Auth.php` |
| Fungsi helper | `helpers/` | `format_helper.php` |
| Extend CI core | `core/` | `MY_Controller.php` |
| Asset (CSS/JS) | `assets/` (buat sendiri) | `css/style.css` |

</div>

---

## 📁 Best Practice: Folder Assets

CI3 tidak menyediakan folder untuk assets (CSS, JS, images). Buat sendiri di **root project**:

```
codeigniter3/
├── 📁 application/
├── 📁 system/
├── 📁 assets/           ⭐ Buat folder ini!
│   ├── 📁 css/
│   │   └── style.css
│   ├── 📁 js/
│   │   └── script.js
│   └── 📁 images/
│       └── logo.png
└── index.php
```

**Akses di View:**
```php
<link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
<script src="<?= base_url('assets/js/script.js') ?>"></script>
<img src="<?= base_url('assets/images/logo.png') ?>">
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Perbedaan folder `application/` dan `system/`
- [ ] Di mana meletakkan Controller, View, Model
- [ ] Fungsi folder `config/`
- [ ] Kapan menggunakan `libraries/` vs `helpers/`
- [ ] Pentingnya tidak mengubah folder `system/`
- [ ] Cara membuat folder `assets/`

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="installation.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Installation
      </button>
    </a>
  </div>
  <div>
    <a href="configuration.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Configuration →
      </button>
    </a>
  </div>
</div>
