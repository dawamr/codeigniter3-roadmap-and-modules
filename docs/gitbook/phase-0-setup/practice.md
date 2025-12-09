# 🔧 Practice Lab

## 🎯 Tujuan

Lab ini akan memandu Anda melakukan setup project CodeIgniter 3 dari awal hingga siap digunakan. Ikuti langkah-langkah berikut secara berurutan.

---

## 📋 Persiapan

Pastikan Anda sudah memiliki:
- [ ] XAMPP/Laragon terinstall dan berjalan
- [ ] Browser (Chrome/Firefox)
- [ ] Code editor (VS Code)
- [ ] Download CodeIgniter 3 dari [codeigniter.com](https://codeigniter.com/download)

---

## 🚀 Lab 1: Instalasi CodeIgniter 3

### Langkah 1.1: Extract dan Rename

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

1. Extract file `CodeIgniter-3.x.x.zip`
2. Rename folder menjadi `belajar-ci3`
3. Pindahkan ke folder htdocs:
   - Windows: `C:\xampp\htdocs\belajar-ci3`
   - Mac: `/Applications/MAMP/htdocs/belajar-ci3`
   - Laragon: `C:\laragon\www\belajar-ci3`

</div>

### Langkah 1.2: Test Instalasi

1. Buka browser
2. Akses: `http://localhost/belajar-ci3`
3. Anda harus melihat halaman **"Welcome to CodeIgniter!"**

<details>
<summary>❌ Tidak bisa diakses? Klik untuk troubleshooting</summary>

**Masalah: "Not Found"**
- Pastikan Apache sudah running
- Cek nama folder (case-sensitive di Linux/Mac)

**Masalah: Blank page**
- Cek error log di `application/logs/`
- Pastikan PHP sudah terinstall

**Masalah: Access Denied**
- Cek permission folder (chmod 755)

</details>

### ✅ Checkpoint 1
```
[Screenshot halaman Welcome to CodeIgniter]
URL: http://localhost/belajar-ci3/
```

---

## ⚙️ Lab 2: Konfigurasi Dasar

### Langkah 2.1: Set Base URL

Buka file: `application/config/config.php`

```php
<?php
// Cari baris ini (sekitar baris 26):
$config['base_url'] = '';

// Ubah menjadi:
$config['base_url'] = 'http://localhost/belajar-ci3/';
```

### Langkah 2.2: Set Encryption Key

Masih di `config.php`:

```php
<?php
// Cari baris ini (sekitar baris 315):
$config['encryption_key'] = '';

// Ubah menjadi (generate key random Anda sendiri):
$config['encryption_key'] = 'BelajarCI3-2024-RandomKey123456';
```

### Langkah 2.3: Setup Autoload

Buka file: `application/config/autoload.php`

```php
<?php
// Cari dan ubah:

// Libraries (sekitar baris 61)
$autoload['libraries'] = array('database', 'session');

// Helpers (sekitar baris 92)
$autoload['helper'] = array('url', 'form');
```

### ✅ Checkpoint 2

Refresh halaman - harus tetap menampilkan Welcome page tanpa error.

---

## 💾 Lab 3: Setup Database

### Langkah 3.1: Buat Database

1. Buka phpMyAdmin: `http://localhost/phpmyadmin`
2. Klik **"New"** di sidebar kiri
3. Isi nama database: `belajar_ci3`
4. Pilih Collation: `utf8mb4_general_ci`
5. Klik **"Create"**

### Langkah 3.2: Konfigurasi Koneksi

Buka file: `application/config/database.php`

```php
<?php
// Cari dan ubah (sekitar baris 78):

$db['default'] = array(
    'dsn'      => '',
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',            // Kosong untuk XAMPP default
    'database' => 'belajar_ci3', // Nama database yang dibuat
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

### Langkah 3.3: Test Koneksi Database

Buka file: `application/controllers/Welcome.php`

Tambahkan method baru:

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Welcome extends CI_Controller {

    public function index()
    {
        $this->load->view('welcome_message');
    }
    
    // Tambahkan method ini untuk test database
    public function test_db()
    {
        // Coba koneksi ke database
        $this->load->database();
        
        if ($this->db->conn_id) {
            echo "<h2>✅ Koneksi Database Berhasil!</h2>";
            echo "<p>Database: " . $this->db->database . "</p>";
            echo "<p>Driver: " . $this->db->dbdriver . "</p>";
        } else {
            echo "<h2>❌ Koneksi Database Gagal!</h2>";
        }
    }
}
```

### Langkah 3.4: Test

Akses: `http://localhost/belajar-ci3/index.php/welcome/test_db`

Anda harus melihat: **"✅ Koneksi Database Berhasil!"**

<details>
<summary>❌ Koneksi gagal? Klik untuk troubleshooting</summary>

**Error: "Unable to connect to database"**
- Pastikan MySQL sudah running di XAMPP
- Cek username dan password
- Pastikan database `belajar_ci3` sudah dibuat

**Error: "No database selected"**
- Cek nama database di config (typo?)

</details>

### ✅ Checkpoint 3
```
[Screenshot "Koneksi Database Berhasil"]
URL: http://localhost/belajar-ci3/index.php/welcome/test_db
```

---

## 📁 Lab 4: Buat Struktur Folder

### Langkah 4.1: Buat Folder Assets

Di **root project** (sejajar dengan index.php), buat struktur folder:

```
belajar-ci3/
├── application/
├── system/
├── assets/          ← Buat folder ini
│   ├── css/
│   ├── js/
│   └── images/
└── index.php
```

### Langkah 4.2: Buat File CSS

Buat file: `assets/css/style.css`

```css
/* assets/css/style.css */

* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, sans-serif;
    background-color: #f5f5f5;
    color: #333;
    line-height: 1.6;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 20px;
}

.card {
    background: white;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    padding: 20px;
    margin-bottom: 20px;
}

.btn {
    display: inline-block;
    padding: 10px 20px;
    background: #4CAF50;
    color: white;
    text-decoration: none;
    border-radius: 5px;
    border: none;
    cursor: pointer;
}

.btn:hover {
    background: #45a049;
}

.header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 40px 20px;
    text-align: center;
    margin-bottom: 30px;
}
```

### ✅ Checkpoint 4

File `assets/css/style.css` sudah dibuat.

---

## 👁️ Lab 5: Buat View Pertama

### Langkah 5.1: Buat Template Layout

Buat folder: `application/views/templates/`

**File: `application/views/templates/header.php`**
```php
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= isset($title) ? $title : 'Belajar CI3' ?></title>
    <link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
</head>
<body>
    <div class="header">
        <h1>🎓 Belajar CodeIgniter 3</h1>
        <p>Tutorial Lengkap untuk Pemula</p>
    </div>
    <div class="container">
```

**File: `application/views/templates/footer.php`**
```php
    </div><!-- /.container -->
    
    <footer style="text-align: center; padding: 20px; color: #666;">
        <p>&copy; <?= date('Y') ?> Belajar CI3. Made with ❤️</p>
    </footer>
    
    <script src="<?= base_url('assets/js/script.js') ?>"></script>
</body>
</html>
```

### Langkah 5.2: Buat Home View

**File: `application/views/home.php`**
```php
<div class="card">
    <h2>👋 Selamat Datang!</h2>
    <p>Anda telah berhasil menginstall dan mengkonfigurasi CodeIgniter 3.</p>
    
    <h3>✅ Checklist Setup:</h3>
    <ul>
        <li>✔️ CodeIgniter 3 terinstall</li>
        <li>✔️ Base URL dikonfigurasi</li>
        <li>✔️ Database terkoneksi</li>
        <li>✔️ Assets folder dibuat</li>
        <li>✔️ Template layout siap</li>
    </ul>
</div>

<div class="card">
    <h2>📚 Informasi System</h2>
    <table style="width: 100%; border-collapse: collapse;">
        <tr>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><strong>CI Version:</strong></td>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><?= CI_VERSION ?></td>
        </tr>
        <tr>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><strong>PHP Version:</strong></td>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><?= phpversion() ?></td>
        </tr>
        <tr>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><strong>Environment:</strong></td>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><?= ENVIRONMENT ?></td>
        </tr>
        <tr>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><strong>Base URL:</strong></td>
            <td style="padding: 10px; border-bottom: 1px solid #eee;"><?= base_url() ?></td>
        </tr>
    </table>
</div>

<div class="card">
    <h2>🚀 Langkah Selanjutnya</h2>
    <p>Anda siap untuk mulai belajar!</p>
    <a href="<?= base_url('index.php/welcome/about') ?>" class="btn">Pelajari Lebih Lanjut</a>
</div>
```

### Langkah 5.3: Update Controller

Edit file: `application/controllers/Welcome.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Welcome extends CI_Controller {

    public function index()
    {
        $data['title'] = 'Home - Belajar CI3';
        
        $this->load->view('templates/header', $data);
        $this->load->view('home', $data);
        $this->load->view('templates/footer');
    }
    
    public function about()
    {
        $data['title'] = 'About - Belajar CI3';
        
        $this->load->view('templates/header', $data);
        $this->load->view('about', $data);
        $this->load->view('templates/footer');
    }
    
    public function test_db()
    {
        if ($this->db->conn_id) {
            echo "✅ Database Connected: " . $this->db->database;
        } else {
            echo "❌ Database Connection Failed";
        }
    }
}
```

### Langkah 5.4: Buat About View

**File: `application/views/about.php`**
```php
<div class="card">
    <h2>📖 Tentang Tutorial Ini</h2>
    <p>Tutorial ini dibuat untuk membantu pemula mempelajari CodeIgniter 3 dari dasar.</p>
    
    <h3>Yang Akan Dipelajari:</h3>
    <ul>
        <li>🎮 Controllers & Routing</li>
        <li>👁️ Views & Templates</li>
        <li>💾 Models & Database</li>
        <li>📝 Form Handling</li>
        <li>🔐 Security</li>
        <li>Dan masih banyak lagi!</li>
    </ul>
    
    <a href="<?= base_url() ?>" class="btn">← Kembali ke Home</a>
</div>
```

### ✅ Checkpoint 5

Akses: `http://localhost/belajar-ci3/`

Anda harus melihat halaman home dengan styling CSS dan informasi system.

---

## 🔗 Lab 6: Hilangkan index.php dari URL

### Langkah 6.1: Buat .htaccess

Buat file `.htaccess` di **root project** (sejajar dengan index.php):

```apache
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```

### Langkah 6.2: Edit config.php

```php
<?php
// Ubah dari:
$config['index_page'] = 'index.php';

// Menjadi:
$config['index_page'] = '';
```

### Langkah 6.3: Test

Akses URL tanpa index.php:
- `http://localhost/belajar-ci3/welcome/about`

Harus menampilkan halaman About!

### ✅ Checkpoint 6
```
URL baru (tanpa index.php):
http://localhost/belajar-ci3/welcome/about
```

---

## 🎉 Hasil Akhir

Selamat! Anda telah berhasil:

- ✅ Menginstall CodeIgniter 3
- ✅ Mengkonfigurasi base_url dan encryption_key
- ✅ Setup koneksi database
- ✅ Membuat struktur folder assets
- ✅ Membuat template layout (header, footer)
- ✅ Membuat controller dan view
- ✅ Menghilangkan index.php dari URL

### 📁 Struktur Project Akhir

```
belajar-ci3/
├── application/
│   ├── config/
│   │   ├── autoload.php       ✏️ Edited
│   │   ├── config.php         ✏️ Edited
│   │   └── database.php       ✏️ Edited
│   ├── controllers/
│   │   └── Welcome.php        ✏️ Edited
│   └── views/
│       ├── templates/         📁 New
│       │   ├── header.php     📄 New
│       │   └── footer.php     📄 New
│       ├── home.php           📄 New
│       └── about.php          📄 New
├── assets/                    📁 New
│   ├── css/
│   │   └── style.css          📄 New
│   ├── js/
│   └── images/
├── system/
├── .htaccess                  📄 New
└── index.php
```

---

## 🏆 Challenge Tambahan

Coba kerjakan tantangan berikut untuk mengasah kemampuan:

<details>
<summary>💪 Challenge 1: Tambah Navigation</summary>

Buat navigation menu di header yang bisa berpindah halaman:

```php
<nav>
    <a href="<?= base_url() ?>">Home</a>
    <a href="<?= base_url('welcome/about') ?>">About</a>
    <a href="<?= base_url('welcome/contact') ?>">Contact</a>
</nav>
```

</details>

<details>
<summary>💪 Challenge 2: Buat Halaman Contact</summary>

1. Tambah method `contact()` di Welcome controller
2. Buat view `contact.php`
3. Tampilkan form contact sederhana

</details>

<details>
<summary>💪 Challenge 3: Buat Table di Database</summary>

1. Buat tabel `users` di phpMyAdmin
2. Buat model `User_model.php`
3. Tampilkan data dari database di view

</details>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="environment.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Environment Settings
      </button>
    </a>
  </div>
  <div>
    <a href="quiz.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Quiz →
      </button>
    </a>
  </div>
</div>
