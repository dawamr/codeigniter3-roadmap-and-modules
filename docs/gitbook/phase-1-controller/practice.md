# 💻 Practice Lab: Controller & Routing

## 🎯 Tujuan

Lab ini akan memandu Anda membuat controller pertama dan memahami routing di CI3.

---

## 📋 Persiapan

Pastikan Anda sudah:
- [ ] Menyelesaikan Phase 0 (CI3 terinstall)
- [ ] Database `belajar_ci3` sudah dibuat
- [ ] Base URL sudah dikonfigurasi

---

## 🚀 Lab 1: Controller Pertama

### Langkah 1.1: Buat Controller Pages

Buat file baru: `application/controllers/Pages.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Pages extends CI_Controller {
    
    public function index() {
        echo "<h1>Selamat datang di Pages Controller!</h1>";
        echo "<p>Ini adalah method index (default).</p>";
    }
    
    public function about() {
        echo "<h1>About Us</h1>";
        echo "<p>Ini adalah halaman tentang kami.</p>";
    }
    
    public function contact() {
        echo "<h1>Contact Us</h1>";
        echo "<p>Email: contact@example.com</p>";
    }
}
```

### Langkah 1.2: Test Controller

Akses URL berikut di browser:

| URL | Expected Output |
|-----|-----------------|
| `http://localhost/belajar-ci3/pages` | Selamat datang... |
| `http://localhost/belajar-ci3/pages/index` | Selamat datang... |
| `http://localhost/belajar-ci3/pages/about` | About Us |
| `http://localhost/belajar-ci3/pages/contact` | Contact Us |

### ✅ Checkpoint 1

Semua URL menampilkan output yang sesuai.

---

## 🎯 Lab 2: Controller dengan Parameter

### Langkah 2.1: Buat Controller User

Buat file: `application/controllers/User.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class User extends CI_Controller {
    
    public function index() {
        echo "<h1>Daftar User</h1>";
        echo "<ul>";
        echo "<li><a href='".base_url('user/profile/1')."'>User 1</a></li>";
        echo "<li><a href='".base_url('user/profile/2')."'>User 2</a></li>";
        echo "<li><a href='".base_url('user/profile/3')."'>User 3</a></li>";
        echo "</ul>";
    }
    
    // Method dengan 1 parameter
    public function profile($id) {
        echo "<h1>Profile User</h1>";
        echo "<p>User ID: $id</p>";
        echo "<a href='".base_url('user')."'>Kembali</a>";
    }
    
    // Method dengan 2 parameter
    public function compare($id1, $id2) {
        echo "<h1>Compare Users</h1>";
        echo "<p>Comparing User $id1 with User $id2</p>";
    }
    
    // Method dengan default parameter
    public function posts($user_id, $page = 1) {
        echo "<h1>Posts by User $user_id</h1>";
        echo "<p>Page: $page</p>";
    }
}
```

### Langkah 2.2: Test Parameter

| URL | Output |
|-----|--------|
| `/user` | Daftar User |
| `/user/profile/5` | User ID: 5 |
| `/user/compare/1/2` | Comparing User 1 with User 2 |
| `/user/posts/10` | Posts by User 10, Page: 1 |
| `/user/posts/10/3` | Posts by User 10, Page: 3 |

### ✅ Checkpoint 2

Parameter dari URL berhasil diterima di method.

---

## 📤 Lab 3: Input GET dan POST

### Langkah 3.1: Buat Controller Search

Buat file: `application/controllers/Search.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Search extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->helper('url');
    }
    
    // Form pencarian
    public function index() {
        ?>
        <!DOCTYPE html>
        <html>
        <head>
            <title>Search</title>
            <style>
                body { font-family: Arial; padding: 20px; }
                .form-group { margin-bottom: 15px; }
                input, select { padding: 10px; width: 300px; }
                button { padding: 10px 20px; background: #4CAF50; color: white; border: none; cursor: pointer; }
            </style>
        </head>
        <body>
            <h1>🔍 Search Products</h1>
            
            <!-- GET Form -->
            <form action="<?= base_url('search/results') ?>" method="GET">
                <div class="form-group">
                    <input type="text" name="keyword" placeholder="Kata kunci...">
                </div>
                <div class="form-group">
                    <select name="category">
                        <option value="">Semua Kategori</option>
                        <option value="electronics">Electronics</option>
                        <option value="fashion">Fashion</option>
                        <option value="food">Food</option>
                    </select>
                </div>
                <button type="submit">Cari</button>
            </form>
        </body>
        </html>
        <?php
    }
    
    // Hasil pencarian (GET)
    public function results() {
        // Ambil data GET
        $keyword = $this->input->get('keyword');
        $category = $this->input->get('category');
        
        echo "<h1>Hasil Pencarian</h1>";
        echo "<p><strong>Keyword:</strong> " . ($keyword ?: '(kosong)') . "</p>";
        echo "<p><strong>Category:</strong> " . ($category ?: '(semua)') . "</p>";
        echo "<p><a href='".base_url('search')."'>← Cari Lagi</a></p>";
    }
}
```

### Langkah 3.2: Buat Controller Contact dengan POST

Buat file: `application/controllers/Contact.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Contact extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->helper(['url', 'form']);
    }
    
    public function index() {
        ?>
        <!DOCTYPE html>
        <html>
        <head>
            <title>Contact Form</title>
            <style>
                body { font-family: Arial; padding: 20px; max-width: 500px; }
                .form-group { margin-bottom: 15px; }
                label { display: block; margin-bottom: 5px; font-weight: bold; }
                input, textarea { width: 100%; padding: 10px; box-sizing: border-box; }
                button { padding: 10px 20px; background: #2196F3; color: white; border: none; cursor: pointer; }
                .success { background: #E8F5E9; padding: 15px; border-radius: 5px; margin-bottom: 20px; }
            </style>
        </head>
        <body>
            <h1>📧 Contact Form</h1>
            
            <?php if ($this->session->flashdata('success')): ?>
                <div class="success">
                    <?= $this->session->flashdata('success') ?>
                </div>
            <?php endif; ?>
            
            <form action="<?= base_url('contact/send') ?>" method="POST">
                <div class="form-group">
                    <label>Nama</label>
                    <input type="text" name="name" required>
                </div>
                <div class="form-group">
                    <label>Email</label>
                    <input type="email" name="email" required>
                </div>
                <div class="form-group">
                    <label>Subject</label>
                    <input type="text" name="subject" required>
                </div>
                <div class="form-group">
                    <label>Pesan</label>
                    <textarea name="message" rows="5" required></textarea>
                </div>
                <button type="submit">Kirim Pesan</button>
            </form>
        </body>
        </html>
        <?php
    }
    
    public function send() {
        // Cek method POST
        if ($this->input->method() !== 'post') {
            redirect('contact');
        }
        
        // Ambil data POST
        $name = $this->input->post('name');
        $email = $this->input->post('email');
        $subject = $this->input->post('subject');
        $message = $this->input->post('message');
        
        // Simulasi simpan/kirim
        // Di real app: simpan ke database atau kirim email
        
        // Flash message
        $this->session->set_flashdata('success', "Terima kasih $name! Pesan Anda telah terkirim.");
        
        // Redirect
        redirect('contact');
    }
}
```

### ✅ Checkpoint 3

- Form search dengan GET berfungsi
- Form contact dengan POST berfungsi
- Flash message muncul setelah submit

---

## 🛣️ Lab 4: Custom Routing

### Langkah 4.1: Edit routes.php

Buka file: `application/config/routes.php`

Tambahkan routes berikut:

```php
<?php
// Setelah default routes

// Static pages
$route['about'] = 'pages/about';
$route['contact-us'] = 'contact/index';

// User routes
$route['profile/(:num)'] = 'user/profile/$1';
$route['u/(:num)/posts'] = 'user/posts/$1';
$route['u/(:num)/posts/(:num)'] = 'user/posts/$1/$2';

// Search
$route['cari'] = 'search/index';

// API-style routes
$route['api/users/(:num)'] = 'user/profile/$1';
```

### Langkah 4.2: Test Routes

| New URL | Maps To |
|---------|---------|
| `/about` | `pages/about` |
| `/contact-us` | `contact/index` |
| `/profile/5` | `user/profile/5` |
| `/u/10/posts` | `user/posts/10` |
| `/u/10/posts/2` | `user/posts/10/2` |
| `/cari` | `search/index` |
| `/api/users/123` | `user/profile/123` |

### ✅ Checkpoint 4

Custom routes berfungsi dengan baik.

---

## 👁️ Lab 5: Controller dengan View

### Langkah 5.1: Buat Views

**File: `application/views/products/index.php`**
```php
<!DOCTYPE html>
<html>
<head>
    <title><?= $title ?></title>
    <style>
        body { font-family: Arial; padding: 20px; }
        .product-grid { display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; }
        .product-card { border: 1px solid #ddd; padding: 15px; border-radius: 8px; }
        .product-card h3 { margin-top: 0; }
        .price { color: #4CAF50; font-size: 1.2em; font-weight: bold; }
    </style>
</head>
<body>
    <h1><?= $title ?></h1>
    
    <div class="product-grid">
        <?php foreach ($products as $product): ?>
            <div class="product-card">
                <h3><?= $product['name'] ?></h3>
                <p class="price">Rp <?= number_format($product['price']) ?></p>
                <a href="<?= base_url('product/detail/' . $product['id']) ?>">Detail</a>
            </div>
        <?php endforeach; ?>
    </div>
</body>
</html>
```

**File: `application/views/products/detail.php`**
```php
<!DOCTYPE html>
<html>
<head>
    <title><?= $product['name'] ?></title>
    <style>
        body { font-family: Arial; padding: 20px; max-width: 600px; }
        .price { color: #4CAF50; font-size: 1.5em; }
        .description { color: #666; line-height: 1.6; }
        .back { display: inline-block; margin-top: 20px; color: #2196F3; }
    </style>
</head>
<body>
    <h1><?= $product['name'] ?></h1>
    <p class="price">Rp <?= number_format($product['price']) ?></p>
    <p class="description"><?= $product['description'] ?></p>
    
    <a href="<?= base_url('product') ?>" class="back">← Kembali ke daftar</a>
</body>
</html>
```

### Langkah 5.2: Buat Controller Product

Buat file: `application/controllers/Product.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Product extends CI_Controller {
    
    // Data dummy (nanti diganti dari database)
    private $products = [
        ['id' => 1, 'name' => 'Laptop Gaming', 'price' => 15000000, 'description' => 'Laptop dengan spesifikasi tinggi untuk gaming.'],
        ['id' => 2, 'name' => 'Smartphone Pro', 'price' => 8000000, 'description' => 'Smartphone flagship dengan kamera canggih.'],
        ['id' => 3, 'name' => 'Wireless Earbuds', 'price' => 1500000, 'description' => 'Earbuds wireless dengan noise cancelling.'],
        ['id' => 4, 'name' => 'Smart Watch', 'price' => 3000000, 'description' => 'Jam tangan pintar dengan fitur kesehatan.'],
        ['id' => 5, 'name' => 'Mechanical Keyboard', 'price' => 2000000, 'description' => 'Keyboard mechanical untuk produktivitas.'],
        ['id' => 6, 'name' => 'Gaming Mouse', 'price' => 800000, 'description' => 'Mouse gaming dengan DPI tinggi.'],
    ];
    
    public function __construct() {
        parent::__construct();
        $this->load->helper('url');
    }
    
    public function index() {
        $data['title'] = 'Daftar Produk';
        $data['products'] = $this->products;
        
        $this->load->view('products/index', $data);
    }
    
    public function detail($id) {
        // Cari produk berdasarkan ID
        $product = null;
        foreach ($this->products as $p) {
            if ($p['id'] == $id) {
                $product = $p;
                break;
            }
        }
        
        // 404 jika tidak ditemukan
        if (!$product) {
            show_404();
        }
        
        $data['product'] = $product;
        $this->load->view('products/detail', $data);
    }
}
```

### Langkah 5.3: Test

| URL | Expected |
|-----|----------|
| `/product` | Grid 6 produk |
| `/product/detail/1` | Detail Laptop Gaming |
| `/product/detail/999` | 404 Page |

### ✅ Checkpoint 5

- Halaman list produk menampilkan 6 produk
- Detail produk menampilkan informasi lengkap
- ID tidak valid menampilkan 404

---

## 🏆 Challenge

<details>
<summary>💪 Challenge 1: Buat Controller Blog dengan CRUD Methods</summary>

Buat controller `Blog.php` dengan methods:
- `index()` - List semua post
- `show($id)` - Detail post
- `create()` - Form tambah post
- `store()` - Simpan post (POST)
- `edit($id)` - Form edit post
- `update($id)` - Update post (POST)
- `delete($id)` - Hapus post (POST)

</details>

<details>
<summary>💪 Challenge 2: Buat Sub-folder Controller Admin</summary>

Buat struktur:
```
controllers/admin/
├── Dashboard.php   → /admin/dashboard
├── Users.php       → /admin/users, /admin/users/edit/5
└── Settings.php    → /admin/settings
```

</details>

<details>
<summary>💪 Challenge 3: Buat API Controller dengan JSON Response</summary>

Buat controller `Api.php` yang return JSON:
- `GET /api/products` → List products JSON
- `GET /api/products/5` → Single product JSON
- Error response dengan proper status code

</details>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="loading-resources.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Loading Resources
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
