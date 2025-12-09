# 💻 Practice Lab: View & Template

## 🎯 Tujuan

Lab ini akan memandu Anda membuat view dan template system di CodeIgniter 3.

---

## 📋 Persiapan

Pastikan Anda sudah:
- [ ] Menyelesaikan Phase 0 dan Phase 1
- [ ] Memiliki folder `assets/css/` dan `assets/js/`
- [ ] URL helper sudah di-autoload

---

## 🚀 Lab 1: View Pertama dengan Data

### Langkah 1.1: Buat Controller

Buat file: `application/controllers/Blog.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Blog extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->helper('url');
    }
    
    public function index() {
        // Data yang dikirim ke view
        $data['title'] = 'Blog Saya';
        $data['author'] = 'John Doe';
        $data['posts'] = [
            [
                'id' => 1,
                'title' => 'Belajar CodeIgniter 3',
                'excerpt' => 'CodeIgniter adalah framework PHP yang ringan dan cepat.',
                'date' => '2024-01-15'
            ],
            [
                'id' => 2,
                'title' => 'Tips Membuat Website',
                'excerpt' => 'Beberapa tips untuk membuat website yang baik.',
                'date' => '2024-01-10'
            ],
            [
                'id' => 3,
                'title' => 'Database MySQL',
                'excerpt' => 'Pengenalan database MySQL untuk pemula.',
                'date' => '2024-01-05'
            ]
        ];
        
        $this->load->view('blog/index', $data);
    }
}
```

### Langkah 1.2: Buat View

Buat folder dan file: `application/views/blog/index.php`

```php
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= $title ?></title>
    <style>
        * { margin: 0; padding: 0; box-sizing: border-box; }
        body { font-family: 'Segoe UI', sans-serif; background: #f5f5f5; padding: 20px; }
        .container { max-width: 800px; margin: 0 auto; }
        h1 { color: #333; margin-bottom: 10px; }
        .author { color: #666; margin-bottom: 30px; }
        .post { background: white; padding: 20px; border-radius: 8px; margin-bottom: 15px; box-shadow: 0 2px 4px rgba(0,0,0,0.1); }
        .post h2 { color: #2196F3; margin-bottom: 10px; }
        .post p { color: #666; margin-bottom: 10px; }
        .post .date { font-size: 0.9em; color: #999; }
        .post a { color: #4CAF50; text-decoration: none; }
    </style>
</head>
<body>
    <div class="container">
        <h1><?= $title ?></h1>
        <p class="author">Oleh: <?= $author ?></p>
        
        <?php foreach ($posts as $post): ?>
            <article class="post">
                <h2><?= $post['title'] ?></h2>
                <p><?= $post['excerpt'] ?></p>
                <span class="date"><?= date('d M Y', strtotime($post['date'])) ?></span>
                <br><br>
                <a href="<?= base_url('blog/detail/' . $post['id']) ?>">Baca selengkapnya →</a>
            </article>
        <?php endforeach; ?>
    </div>
</body>
</html>
```

### Langkah 1.3: Test

Akses: `http://localhost/belajar-ci3/blog`

### ✅ Checkpoint 1

- Halaman menampilkan judul dan nama author
- 3 artikel ditampilkan dengan loop foreach
- Tanggal diformat dengan benar

---

## 🏗️ Lab 2: Template System

### Langkah 2.1: Buat Header Template

Buat folder dan file: `application/views/templates/header.php`

```php
<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title><?= $title ?? 'My Website' ?></title>
    <link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
</head>
<body>
    <header class="header">
        <div class="container">
            <a href="<?= base_url() ?>" class="logo">🌟 My Website</a>
            <nav>
                <a href="<?= base_url() ?>" class="<?= ($active ?? '') == 'home' ? 'active' : '' ?>">Home</a>
                <a href="<?= base_url('blog') ?>" class="<?= ($active ?? '') == 'blog' ? 'active' : '' ?>">Blog</a>
                <a href="<?= base_url('about') ?>" class="<?= ($active ?? '') == 'about' ? 'active' : '' ?>">About</a>
                <a href="<?= base_url('contact') ?>" class="<?= ($active ?? '') == 'contact' ? 'active' : '' ?>">Contact</a>
            </nav>
        </div>
    </header>
    
    <?php if ($this->session->flashdata('success')): ?>
        <div class="alert alert-success">
            <?= $this->session->flashdata('success') ?>
        </div>
    <?php endif; ?>
    
    <main class="container main-content">
```

### Langkah 2.2: Buat Footer Template

Buat file: `application/views/templates/footer.php`

```php
    </main>
    
    <footer class="footer">
        <div class="container">
            <p>&copy; <?= date('Y') ?> My Website. Made with ❤️ using CodeIgniter 3</p>
        </div>
    </footer>
    
    <script src="<?= base_url('assets/js/script.js') ?>"></script>
</body>
</html>
```

### Langkah 2.3: Buat CSS

Buat file: `assets/css/style.css`

```css
/* Reset */
* {
    margin: 0;
    padding: 0;
    box-sizing: border-box;
}

body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f8f9fa;
    color: #333;
    line-height: 1.6;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    padding: 0 20px;
}

/* Header */
.header {
    background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
    color: white;
    padding: 15px 0;
}

.header .container {
    display: flex;
    justify-content: space-between;
    align-items: center;
}

.header .logo {
    font-size: 1.5em;
    font-weight: bold;
    color: white;
    text-decoration: none;
}

.header nav a {
    color: rgba(255,255,255,0.8);
    text-decoration: none;
    margin-left: 20px;
    transition: color 0.3s;
}

.header nav a:hover,
.header nav a.active {
    color: white;
}

/* Main Content */
.main-content {
    padding: 40px 20px;
    min-height: calc(100vh - 200px);
}

/* Cards */
.card {
    background: white;
    border-radius: 10px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
    padding: 20px;
    margin-bottom: 20px;
}

.card h2 {
    color: #667eea;
    margin-bottom: 10px;
}

/* Grid */
.grid {
    display: grid;
    grid-template-columns: repeat(auto-fill, minmax(300px, 1fr));
    gap: 20px;
}

/* Buttons */
.btn {
    display: inline-block;
    padding: 10px 20px;
    background: #667eea;
    color: white;
    text-decoration: none;
    border-radius: 5px;
    border: none;
    cursor: pointer;
    transition: background 0.3s;
}

.btn:hover {
    background: #5a6fd6;
}

.btn-success { background: #4CAF50; }
.btn-danger { background: #f44336; }

/* Alerts */
.alert {
    padding: 15px 20px;
    margin: 0 auto;
    max-width: 1200px;
    border-radius: 5px;
}

.alert-success {
    background: #d4edda;
    color: #155724;
    border: 1px solid #c3e6cb;
}

.alert-error {
    background: #f8d7da;
    color: #721c24;
    border: 1px solid #f5c6cb;
}

/* Footer */
.footer {
    background: #333;
    color: #aaa;
    text-align: center;
    padding: 20px;
    margin-top: 40px;
}

/* Page Header */
.page-header {
    display: flex;
    justify-content: space-between;
    align-items: center;
    margin-bottom: 30px;
}

.page-header h1 {
    color: #333;
}
```

### Langkah 2.4: Update Blog Controller

```php
<?php
// application/controllers/Blog.php

class Blog extends CI_Controller {
    
    private $posts = [
        ['id' => 1, 'title' => 'Belajar CodeIgniter 3', 'excerpt' => 'CodeIgniter adalah framework PHP yang ringan.', 'content' => 'CodeIgniter adalah framework PHP yang sangat ringan dan mudah dipelajari...', 'date' => '2024-01-15'],
        ['id' => 2, 'title' => 'Tips Web Development', 'excerpt' => 'Tips untuk menjadi web developer.', 'content' => 'Berikut adalah beberapa tips untuk menjadi web developer yang handal...', 'date' => '2024-01-10'],
        ['id' => 3, 'title' => 'Database MySQL', 'excerpt' => 'Pengenalan MySQL untuk pemula.', 'content' => 'MySQL adalah sistem manajemen database relasional yang populer...', 'date' => '2024-01-05'],
    ];
    
    public function __construct() {
        parent::__construct();
        $this->load->helper('url');
    }
    
    public function index() {
        $data['title'] = 'Blog';
        $data['active'] = 'blog';
        $data['posts'] = $this->posts;
        
        // Load dengan template
        $this->load->view('templates/header', $data);
        $this->load->view('blog/list', $data);
        $this->load->view('templates/footer');
    }
    
    public function detail($id) {
        // Cari post
        $post = null;
        foreach ($this->posts as $p) {
            if ($p['id'] == $id) {
                $post = $p;
                break;
            }
        }
        
        if (!$post) {
            show_404();
        }
        
        $data['title'] = $post['title'];
        $data['active'] = 'blog';
        $data['post'] = $post;
        
        $this->load->view('templates/header', $data);
        $this->load->view('blog/detail', $data);
        $this->load->view('templates/footer');
    }
}
```

### Langkah 2.5: Buat Blog Views

**File: `application/views/blog/list.php`**
```php
<div class="page-header">
    <h1>📝 Blog</h1>
</div>

<div class="grid">
    <?php foreach ($posts as $post): ?>
        <div class="card">
            <h2><?= $post['title'] ?></h2>
            <p><?= $post['excerpt'] ?></p>
            <p><small>📅 <?= date('d M Y', strtotime($post['date'])) ?></small></p>
            <br>
            <a href="<?= base_url('blog/detail/' . $post['id']) ?>" class="btn">Baca Selengkapnya</a>
        </div>
    <?php endforeach; ?>
</div>
```

**File: `application/views/blog/detail.php`**
```php
<article>
    <a href="<?= base_url('blog') ?>" class="btn" style="margin-bottom: 20px;">← Kembali</a>
    
    <div class="card">
        <h1><?= $post['title'] ?></h1>
        <p><small>📅 <?= date('l, d F Y', strtotime($post['date'])) ?></small></p>
        <hr style="margin: 20px 0;">
        <p><?= $post['content'] ?></p>
    </div>
</article>
```

### ✅ Checkpoint 2

- Header dan footer muncul di semua halaman
- Navigation dengan active state berfungsi
- Styling dari CSS external terload

---

## 📦 Lab 3: Template Library

### Langkah 3.1: Buat Template Library

Buat file: `application/libraries/Template.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Template {
    
    protected $CI;
    
    public function __construct() {
        $this->CI =& get_instance();
    }
    
    /**
     * Load view dengan template standar
     */
    public function load($view, $data = []) {
        $this->CI->load->view('templates/header', $data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('templates/footer');
    }
    
    /**
     * Load view dengan sidebar
     */
    public function with_sidebar($view, $data = []) {
        $this->CI->load->view('templates/header', $data);
        echo '<div class="with-sidebar">';
        echo '<aside class="sidebar">';
        $this->CI->load->view('templates/sidebar', $data);
        echo '</aside>';
        echo '<div class="content">';
        $this->CI->load->view($view, $data);
        echo '</div>';
        echo '</div>';
        $this->CI->load->view('templates/footer');
    }
}
```

### Langkah 3.2: Buat Home Controller dengan Template Library

Buat file: `application/controllers/Home.php`

```php
<?php
defined('BASEPATH') OR exit('No direct script access allowed');

class Home extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->library('template');
        $this->load->helper('url');
    }
    
    public function index() {
        $data['title'] = 'Home';
        $data['active'] = 'home';
        $data['welcome_message'] = 'Selamat datang di website kami!';
        $data['features'] = [
            ['icon' => '🚀', 'title' => 'Fast', 'desc' => 'Website super cepat'],
            ['icon' => '🔒', 'title' => 'Secure', 'desc' => 'Keamanan terjamin'],
            ['icon' => '📱', 'title' => 'Responsive', 'desc' => 'Tampil baik di semua device'],
        ];
        
        // Satu baris saja!
        $this->template->load('home', $data);
    }
}
```

### Langkah 3.3: Buat Home View

Buat file: `application/views/home.php`

```php
<div style="text-align: center; padding: 40px 0;">
    <h1>👋 <?= $welcome_message ?></h1>
    <p>Website ini dibuat dengan CodeIgniter 3</p>
</div>

<div class="grid">
    <?php foreach ($features as $feature): ?>
        <div class="card" style="text-align: center;">
            <div style="font-size: 3em;"><?= $feature['icon'] ?></div>
            <h2><?= $feature['title'] ?></h2>
            <p><?= $feature['desc'] ?></p>
        </div>
    <?php endforeach; ?>
</div>
```

### Langkah 3.4: Update Routes

Edit `application/config/routes.php`:

```php
$route['default_controller'] = 'home';
```

### ✅ Checkpoint 3

- Akses `/` menampilkan Home page
- Template library memudahkan load views
- Hanya satu baris untuk load template

---

## 🎨 Lab 4: Conditional dan Empty State

### Langkah 4.1: Update Blog untuk Empty State

Edit `application/views/blog/list.php`:

```php
<div class="page-header">
    <h1>📝 Blog</h1>
    <a href="<?= base_url('blog/create') ?>" class="btn btn-success">+ Tulis Artikel</a>
</div>

<?php if (!empty($posts)): ?>
    <div class="grid">
        <?php foreach ($posts as $post): ?>
            <div class="card">
                <h2><?= $post['title'] ?></h2>
                <p><?= $post['excerpt'] ?></p>
                <p><small>📅 <?= date('d M Y', strtotime($post['date'])) ?></small></p>
                <br>
                <a href="<?= base_url('blog/detail/' . $post['id']) ?>" class="btn">Baca Selengkapnya</a>
            </div>
        <?php endforeach; ?>
    </div>
<?php else: ?>
    <div class="card" style="text-align: center; padding: 60px;">
        <div style="font-size: 4em;">📭</div>
        <h2>Belum Ada Artikel</h2>
        <p>Mulai menulis artikel pertama Anda!</p>
        <br>
        <a href="<?= base_url('blog/create') ?>" class="btn btn-success">+ Tulis Artikel</a>
    </div>
<?php endif; ?>
```

### ✅ Checkpoint 4

- Empty state muncul jika tidak ada data
- Conditional rendering bekerja dengan benar

---

## 🏆 Challenge

<details>
<summary>💪 Challenge 1: Buat About dan Contact Page</summary>

Buat controller `Pages.php` dengan methods:
- `about()` - Halaman about dengan info perusahaan
- `contact()` - Halaman contact dengan form

Gunakan template library!

</details>

<details>
<summary>💪 Challenge 2: Buat Partial View untuk Card</summary>

Buat partial view `views/partials/post_card.php` dan gunakan di blog list:

```php
<?php foreach ($posts as $post): ?>
    <?php $this->load->view('partials/post_card', ['post' => $post]); ?>
<?php endforeach; ?>
```

</details>

<details>
<summary>💪 Challenge 3: Tambahkan Extra CSS/JS Support</summary>

Modifikasi template untuk mendukung:
- `$extra_css` - Array CSS tambahan per halaman
- `$extra_js` - Array JS tambahan per halaman

</details>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="template-system.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Template System
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
