# 🏗️ Template System

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Membuat layout template yang reusable
- ✅ Menggunakan header dan footer
- ✅ Membuat template library sendiri
- ✅ Implementasi master layout pattern

---

## 🤔 Mengapa Butuh Template?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Masalah:** Setiap halaman punya struktur yang sama (header, footer, sidebar), tapi kita harus copy-paste di setiap view.

**Solusi:** Gunakan **template system** untuk membagi layout menjadi komponen reusable.

</div>

---

## 📊 Template Patterns di CI3

### Pattern 1: Header + Content + Footer

Pattern paling sederhana dan umum digunakan.

```
┌─────────────────────────────────────────────────────────────┐
│                      header.php                              │
│  <!DOCTYPE html><head>...</head><body><nav>...</nav>        │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│                      content.php                             │
│  <main>Content halaman...</main>                             │
└─────────────────────────────────────────────────────────────┘
┌─────────────────────────────────────────────────────────────┐
│                      footer.php                              │
│  <footer>...</footer></body></html>                          │
└─────────────────────────────────────────────────────────────┘
```

---

## 📁 Struktur Folder Template

```
application/views/
├── templates/
│   ├── header.php          # Head, navbar
│   ├── footer.php          # Footer, scripts
│   ├── sidebar.php         # Sidebar (opsional)
│   └── admin_header.php    # Header untuk admin
│
├── home.php                # Content halaman
├── user/
│   ├── index.php
│   └── profile.php
└── product/
    ├── list.php
    └── detail.php
```

---

## 🔧 Implementasi Basic Template

### Step 1: Buat Header Template

```php
<!-- application/views/templates/header.php -->
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= $title ?? 'My App' ?> | My Application</title>
    
    <!-- CSS -->
    <link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
    
    <!-- Additional CSS from controller -->
    <?php if (isset($extra_css)): ?>
        <?php foreach ($extra_css as $css): ?>
            <link rel="stylesheet" href="<?= base_url('assets/css/' . $css) ?>">
        <?php endforeach; ?>
    <?php endif; ?>
</head>
<body>
    <!-- Navigation -->
    <nav class="navbar">
        <div class="container">
            <a href="<?= base_url() ?>" class="logo">My App</a>
            
            <ul class="nav-menu">
                <li><a href="<?= base_url() ?>" class="<?= ($active_menu ?? '') == 'home' ? 'active' : '' ?>">Home</a></li>
                <li><a href="<?= base_url('product') ?>" class="<?= ($active_menu ?? '') == 'products' ? 'active' : '' ?>">Products</a></li>
                <li><a href="<?= base_url('about') ?>" class="<?= ($active_menu ?? '') == 'about' ? 'active' : '' ?>">About</a></li>
            </ul>
            
            <div class="nav-auth">
                <?php if ($this->session->userdata('logged_in')): ?>
                    <span>Hi, <?= $this->session->userdata('name') ?></span>
                    <a href="<?= base_url('logout') ?>">Logout</a>
                <?php else: ?>
                    <a href="<?= base_url('login') ?>">Login</a>
                <?php endif; ?>
            </div>
        </div>
    </nav>
    
    <!-- Flash Messages -->
    <?php if ($this->session->flashdata('success')): ?>
        <div class="alert alert-success">
            <?= $this->session->flashdata('success') ?>
        </div>
    <?php endif; ?>
    
    <?php if ($this->session->flashdata('error')): ?>
        <div class="alert alert-error">
            <?= $this->session->flashdata('error') ?>
        </div>
    <?php endif; ?>
    
    <!-- Main Content Start -->
    <main class="container">
```

### Step 2: Buat Footer Template

```php
<!-- application/views/templates/footer.php -->
    </main>
    <!-- Main Content End -->
    
    <!-- Footer -->
    <footer class="footer">
        <div class="container">
            <p>&copy; <?= date('Y') ?> My Application. All rights reserved.</p>
        </div>
    </footer>
    
    <!-- JavaScript -->
    <script src="<?= base_url('assets/js/script.js') ?>"></script>
    
    <!-- Additional JS from controller -->
    <?php if (isset($extra_js)): ?>
        <?php foreach ($extra_js as $js): ?>
            <script src="<?= base_url('assets/js/' . $js) ?>"></script>
        <?php endforeach; ?>
    <?php endif; ?>
    
    <!-- Inline JS from controller -->
    <?php if (isset($inline_js)): ?>
        <script><?= $inline_js ?></script>
    <?php endif; ?>
</body>
</html>
```

### Step 3: Buat Content View

```php
<!-- application/views/product/index.php -->
<div class="page-header">
    <h1><?= $title ?></h1>
    <a href="<?= base_url('product/create') ?>" class="btn btn-primary">+ Tambah Produk</a>
</div>

<div class="product-grid">
    <?php if (!empty($products)): ?>
        <?php foreach ($products as $product): ?>
            <div class="product-card">
                <img src="<?= base_url('uploads/' . $product->image) ?>" alt="<?= $product->name ?>">
                <h3><?= $product->name ?></h3>
                <p class="price">Rp <?= number_format($product->price) ?></p>
                <a href="<?= base_url('product/detail/' . $product->id) ?>" class="btn">Detail</a>
            </div>
        <?php endforeach; ?>
    <?php else: ?>
        <div class="empty-state">
            <p>Belum ada produk.</p>
        </div>
    <?php endif; ?>
</div>
```

### Step 4: Load di Controller

```php
<?php
class Product extends CI_Controller {
    
    public function index() {
        $data['title'] = 'Daftar Produk';
        $data['active_menu'] = 'products';
        $data['products'] = $this->product_model->get_all();
        
        // Extra assets (opsional)
        $data['extra_css'] = ['product.css'];
        $data['extra_js'] = ['product.js'];
        
        // Load views dengan template
        $this->load->view('templates/header', $data);
        $this->load->view('product/index', $data);
        $this->load->view('templates/footer', $data);
    }
}
```

---

## 📦 Pattern 2: Template Library

Untuk menghindari load 3 views berulang, buat library:

### Buat Template Library

```php
<?php
// application/libraries/Template.php

class Template {
    
    protected $CI;
    protected $data = [];
    
    public function __construct() {
        $this->CI =& get_instance();
    }
    
    /**
     * Set data untuk template
     */
    public function set($key, $value) {
        $this->data[$key] = $value;
        return $this;
    }
    
    /**
     * Load view dengan template
     */
    public function load($view, $data = []) {
        // Merge data
        $data = array_merge($this->data, $data);
        
        // Load views
        $this->CI->load->view('templates/header', $data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('templates/footer', $data);
    }
    
    /**
     * Load view dengan sidebar
     */
    public function load_with_sidebar($view, $data = []) {
        $data = array_merge($this->data, $data);
        
        $this->CI->load->view('templates/header', $data);
        $this->CI->load->view('templates/sidebar', $data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('templates/footer', $data);
    }
    
    /**
     * Load admin template
     */
    public function admin($view, $data = []) {
        $data = array_merge($this->data, $data);
        
        $this->CI->load->view('templates/admin_header', $data);
        $this->CI->load->view('templates/admin_sidebar', $data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('templates/admin_footer', $data);
    }
}
```

### Gunakan Template Library

```php
<?php
class Product extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->library('template');
        $this->load->model('product_model');
    }
    
    public function index() {
        $data['title'] = 'Daftar Produk';
        $data['active_menu'] = 'products';
        $data['products'] = $this->product_model->get_all();
        
        // Satu baris saja!
        $this->template->load('product/index', $data);
    }
    
    public function detail($id) {
        $data['title'] = 'Detail Produk';
        $data['product'] = $this->product_model->get_by_id($id);
        
        // Dengan sidebar
        $this->template->load_with_sidebar('product/detail', $data);
    }
}
```

---

## 🎨 Pattern 3: Master Layout

Pattern lebih advanced dengan content section:

### Master Layout View

```php
<!-- application/views/layouts/master.php -->
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <title><?= $title ?? 'My App' ?></title>
    <link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
</head>
<body>
    <nav class="navbar">
        <!-- Navigation -->
    </nav>
    
    <div class="wrapper">
        <?php if (isset($sidebar)): ?>
            <aside class="sidebar">
                <?= $sidebar ?>
            </aside>
        <?php endif; ?>
        
        <main class="content">
            <?= $content ?>
        </main>
    </div>
    
    <footer>
        <!-- Footer -->
    </footer>
    
    <script src="<?= base_url('assets/js/script.js') ?>"></script>
</body>
</html>
```

### Template Library dengan Master Layout

```php
<?php
// application/libraries/Template.php

class Template {
    
    protected $CI;
    
    public function __construct() {
        $this->CI =& get_instance();
    }
    
    public function render($view, $data = [], $layout = 'layouts/master') {
        // Render content view sebagai string
        $data['content'] = $this->CI->load->view($view, $data, TRUE);
        
        // Render sidebar jika ada
        if (isset($data['sidebar_view'])) {
            $data['sidebar'] = $this->CI->load->view($data['sidebar_view'], $data, TRUE);
        }
        
        // Load master layout
        $this->CI->load->view($layout, $data);
    }
}
```

### Penggunaan

```php
<?php
public function index() {
    $data['title'] = 'Products';
    $data['products'] = $this->product_model->get_all();
    $data['sidebar_view'] = 'partials/category_sidebar';
    
    $this->template->render('product/index', $data);
}
```

---

## 📱 Template untuk Admin

### Struktur Terpisah

```
views/
├── templates/              # Public templates
│   ├── header.php
│   └── footer.php
│
├── admin/                  # Admin views
│   ├── templates/          # Admin templates
│   │   ├── header.php
│   │   ├── sidebar.php
│   │   └── footer.php
│   ├── dashboard.php
│   └── users/
│       ├── index.php
│       └── edit.php
```

### Admin Base Controller

```php
<?php
// application/core/MY_Controller.php

class MY_Controller extends CI_Controller {
    public function __construct() {
        parent::__construct();
    }
}

class Admin_Controller extends MY_Controller {
    
    public function __construct() {
        parent::__construct();
        
        // Cek login admin
        if (!$this->session->userdata('is_admin')) {
            redirect('auth/login');
        }
        
        // Load template library
        $this->load->library('template');
    }
    
    protected function render($view, $data = []) {
        $this->load->view('admin/templates/header', $data);
        $this->load->view('admin/templates/sidebar', $data);
        $this->load->view('admin/' . $view, $data);
        $this->load->view('admin/templates/footer', $data);
    }
}
```

### Admin Controller

```php
<?php
// application/controllers/admin/Dashboard.php

class Dashboard extends Admin_Controller {
    
    public function index() {
        $data['title'] = 'Dashboard';
        $data['stats'] = $this->dashboard_model->get_stats();
        
        $this->render('dashboard', $data);
    }
}
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Pattern Header + Content + Footer
- [ ] Membuat Template Library
- [ ] Master Layout pattern
- [ ] Struktur folder untuk templates
- [ ] Template terpisah untuk Admin
- [ ] Kapan menggunakan pattern mana

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="php-in-views.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← PHP in Views
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
