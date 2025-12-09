# 📊 Passing Data to Views

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Mengirim berbagai jenis data ke view
- ✅ Mengakses data di dalam view
- ✅ Mengirim object dan array
- ✅ Menggunakan data untuk multiple views

---

## 🤔 Bagaimana Data Dikirim?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

Data dikirim dari Controller ke View melalui **array `$data`**.

**Setiap key dalam array menjadi variabel di view:**
- `$data['title']` → `$title`
- `$data['user']` → `$user`
- `$data['products']` → `$products`

</div>

---

## 📋 Syntax Dasar

```php
// Controller
$data['key'] = 'value';
$this->load->view('nama_view', $data);

// View
<?= $key ?>  // Output: value
```

---

## 🔧 Jenis Data yang Bisa Dikirim

### 1️⃣ String

```php
<?php
// Controller
public function index() {
    $data['title'] = 'Selamat Datang';
    $data['message'] = 'Ini adalah pesan dari controller.';
    $data['username'] = 'John Doe';
    
    $this->load->view('home', $data);
}
```

```php
<!-- View: home.php -->
<h1><?= $title ?></h1>
<p><?= $message ?></p>
<p>Halo, <?= $username ?>!</p>
```

**Output:**
```html
<h1>Selamat Datang</h1>
<p>Ini adalah pesan dari controller.</p>
<p>Halo, John Doe!</p>
```

---

### 2️⃣ Number

```php
<?php
// Controller
$data['total_users'] = 150;
$data['price'] = 250000;
$data['discount'] = 0.15;
$data['rating'] = 4.5;

$this->load->view('stats', $data);
```

```php
<!-- View -->
<p>Total Users: <?= $total_users ?></p>
<p>Harga: Rp <?= number_format($price) ?></p>
<p>Diskon: <?= $discount * 100 ?>%</p>
<p>Rating: <?= $rating ?>/5</p>
```

---

### 3️⃣ Array

```php
<?php
// Controller
$data['colors'] = ['Red', 'Green', 'Blue'];

$data['user'] = [
    'name' => 'John Doe',
    'email' => 'john@mail.com',
    'age' => 25
];

$this->load->view('example', $data);
```

```php
<!-- View -->
<!-- Indexed Array -->
<ul>
<?php foreach ($colors as $color): ?>
    <li><?= $color ?></li>
<?php endforeach; ?>
</ul>

<!-- Associative Array -->
<p>Nama: <?= $user['name'] ?></p>
<p>Email: <?= $user['email'] ?></p>
<p>Umur: <?= $user['age'] ?> tahun</p>
```

---

### 4️⃣ Array of Arrays (List Data)

```php
<?php
// Controller
$data['products'] = [
    ['id' => 1, 'name' => 'Laptop', 'price' => 15000000],
    ['id' => 2, 'name' => 'Mouse', 'price' => 250000],
    ['id' => 3, 'name' => 'Keyboard', 'price' => 500000],
];

$this->load->view('product/list', $data);
```

```php
<!-- View -->
<table>
    <thead>
        <tr>
            <th>ID</th>
            <th>Nama</th>
            <th>Harga</th>
        </tr>
    </thead>
    <tbody>
        <?php foreach ($products as $product): ?>
        <tr>
            <td><?= $product['id'] ?></td>
            <td><?= $product['name'] ?></td>
            <td>Rp <?= number_format($product['price']) ?></td>
        </tr>
        <?php endforeach; ?>
    </tbody>
</table>
```

---

### 5️⃣ Object

```php
<?php
// Controller - Data dari Model (biasanya object)
$data['user'] = $this->user_model->get_by_id(5);
// Return: object dengan properties: id, name, email, created_at

$this->load->view('user/profile', $data);
```

```php
<!-- View -->
<!-- Akses property object dengan -> -->
<div class="profile">
    <h1><?= $user->name ?></h1>
    <p>Email: <?= $user->email ?></p>
    <p>Member sejak: <?= $user->created_at ?></p>
</div>
```

---

### 6️⃣ Array of Objects

```php
<?php
// Controller - Hasil query biasanya array of objects
$data['users'] = $this->user_model->get_all();
// Return: array of user objects

$this->load->view('user/list', $data);
```

```php
<!-- View -->
<?php foreach ($users as $user): ?>
<div class="user-card">
    <h3><?= $user->name ?></h3>
    <p><?= $user->email ?></p>
</div>
<?php endforeach; ?>
```

---

### 7️⃣ Boolean

```php
<?php
// Controller
$data['is_logged_in'] = $this->session->userdata('logged_in');
$data['is_admin'] = $this->session->userdata('role') === 'admin';
$data['show_sidebar'] = true;

$this->load->view('dashboard', $data);
```

```php
<!-- View -->
<?php if ($is_logged_in): ?>
    <p>Selamat datang kembali!</p>
    
    <?php if ($is_admin): ?>
        <a href="<?= base_url('admin') ?>">Admin Panel</a>
    <?php endif; ?>
<?php else: ?>
    <a href="<?= base_url('login') ?>">Login</a>
<?php endif; ?>

<?php if ($show_sidebar): ?>
    <aside>Sidebar content...</aside>
<?php endif; ?>
```

---

## 📊 Passing Data ke Multiple Views

Ketika load multiple views, data bisa di-share:

```php
<?php
// Controller
public function profile($id) {
    // Siapkan semua data
    $data['title'] = 'User Profile';
    $data['user'] = $this->user_model->get_by_id($id);
    $data['posts'] = $this->post_model->get_by_user($id);
    $data['is_logged_in'] = $this->session->userdata('logged_in');
    
    // Semua view menerima $data yang sama
    $this->load->view('templates/header', $data);  // Akses: $title, $is_logged_in
    $this->load->view('user/profile', $data);      // Akses: $user, $posts
    $this->load->view('templates/footer', $data);  // Akses: semua jika perlu
}
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Tips:** View hanya menggunakan variabel yang dibutuhkan. Mengirim semua data ke semua views tidak masalah - view akan mengabaikan variabel yang tidak dipakai.

</div>

---

## 🎯 Contoh Lengkap

### Controller

```php
<?php
// application/controllers/Product.php

class Product extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->model('product_model');
        $this->load->model('category_model');
    }
    
    public function index() {
        // Data untuk header
        $data['title'] = 'Daftar Produk';
        $data['active_menu'] = 'products';
        
        // Data untuk content
        $data['products'] = $this->product_model->get_all();
        $data['total_products'] = count($data['products']);
        
        // Data untuk sidebar
        $data['categories'] = $this->category_model->get_all();
        
        // Data tambahan
        $data['show_promo'] = true;
        $data['promo_message'] = 'Diskon 20% untuk semua produk!';
        
        // Load views
        $this->load->view('templates/header', $data);
        $this->load->view('templates/sidebar', $data);
        $this->load->view('product/index', $data);
        $this->load->view('templates/footer');
    }
}
```

### View: templates/header.php

```php
<!DOCTYPE html>
<html>
<head>
    <title><?= $title ?> | My Store</title>
</head>
<body>
    <nav>
        <a href="/" class="<?= $active_menu == 'home' ? 'active' : '' ?>">Home</a>
        <a href="/products" class="<?= $active_menu == 'products' ? 'active' : '' ?>">Products</a>
    </nav>
    
    <?php if (isset($show_promo) && $show_promo): ?>
        <div class="promo-banner">
            <?= $promo_message ?>
        </div>
    <?php endif; ?>
```

### View: templates/sidebar.php

```php
<aside class="sidebar">
    <h3>Kategori</h3>
    <ul>
        <?php foreach ($categories as $cat): ?>
            <li><a href="<?= base_url('products/category/' . $cat->slug) ?>">
                <?= $cat->name ?>
            </a></li>
        <?php endforeach; ?>
    </ul>
</aside>
```

### View: product/index.php

```php
<main class="content">
    <h1><?= $title ?></h1>
    <p>Total: <?= $total_products ?> produk</p>
    
    <div class="product-grid">
        <?php foreach ($products as $product): ?>
            <div class="product-card">
                <img src="<?= base_url('uploads/' . $product->image) ?>">
                <h3><?= $product->name ?></h3>
                <p class="price">Rp <?= number_format($product->price) ?></p>
                <a href="<?= base_url('product/detail/' . $product->id) ?>" class="btn">
                    Lihat Detail
                </a>
            </div>
        <?php endforeach; ?>
    </div>
</main>
```

---

## ⚠️ Handling Missing Data

```php
<!-- Cek apakah variabel ada sebelum digunakan -->

<!-- Cara 1: isset() -->
<?php if (isset($user)): ?>
    <p><?= $user->name ?></p>
<?php endif; ?>

<!-- Cara 2: empty() untuk array -->
<?php if (!empty($products)): ?>
    <?php foreach ($products as $product): ?>
        <p><?= $product->name ?></p>
    <?php endforeach; ?>
<?php else: ?>
    <p>Tidak ada produk.</p>
<?php endif; ?>

<!-- Cara 3: Null coalescing -->
<p><?= $title ?? 'Default Title' ?></p>
<p><?= $user->name ?? 'Anonymous' ?></p>
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Cara mengirim string, number, boolean
- [ ] Cara mengirim array dan object
- [ ] Perbedaan akses array `['key']` dan object `->`
- [ ] Loop untuk array of arrays/objects
- [ ] Share data ke multiple views
- [ ] Handle missing data dengan isset/empty

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="loading-views.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Loading Views
      </button>
    </a>
  </div>
  <div>
    <a href="php-in-views.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        PHP in Views →
      </button>
    </a>
  </div>
</div>
