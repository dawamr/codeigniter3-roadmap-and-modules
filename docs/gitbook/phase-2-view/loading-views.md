# 📤 Loading Views

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memuat view dari controller
- ✅ Memuat multiple views
- ✅ Memuat view dari sub-folder
- ✅ Mengembalikan view sebagai string

---

## 📋 Syntax Dasar

```php
$this->load->view('nama_view');
$this->load->view('nama_view', $data);
$this->load->view('nama_view', $data, TRUE);  // Return as string
```

| Parameter | Keterangan |
|-----------|------------|
| `nama_view` | Nama file view (tanpa .php) |
| `$data` | Array data yang dikirim ke view |
| `TRUE` | Return sebagai string, bukan output |

---

## 🔧 Cara Memuat View

### 1️⃣ Load View Sederhana

```php
<?php
class Home extends CI_Controller {
    
    public function index() {
        // Load view dari: application/views/home.php
        $this->load->view('home');
    }
}
```

**File View:**
```php
<!-- application/views/home.php -->
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Selamat Datang!</h1>
</body>
</html>
```

---

### 2️⃣ Load View dari Sub-folder

```
views/
├── user/
│   ├── index.php
│   ├── profile.php
│   └── edit.php
├── product/
│   ├── list.php
│   └── detail.php
└── admin/
    └── dashboard.php
```

```php
<?php
// Load views/user/profile.php
$this->load->view('user/profile');

// Load views/product/detail.php
$this->load->view('product/detail');

// Load views/admin/dashboard.php
$this->load->view('admin/dashboard');
```

---

### 3️⃣ Load Multiple Views

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Pattern umum:** Header + Content + Footer

Ini adalah pattern yang paling sering digunakan untuk consistency layout.

</div>

```php
<?php
class Product extends CI_Controller {
    
    public function index() {
        $data['title'] = 'Product List';
        $data['products'] = $this->product_model->get_all();
        
        // Load 3 views berurutan
        $this->load->view('templates/header', $data);
        $this->load->view('product/index', $data);
        $this->load->view('templates/footer');
    }
    
    public function detail($id) {
        $data['title'] = 'Product Detail';
        $data['product'] = $this->product_model->get_by_id($id);
        
        // Pattern yang sama
        $this->load->view('templates/header', $data);
        $this->load->view('product/detail', $data);
        $this->load->view('templates/footer');
    }
}
```

**Output HTML yang dihasilkan:**
```html
<!-- Dari templates/header.php -->
<!DOCTYPE html>
<html>
<head><title>Product List</title></head>
<body>
<nav>...</nav>

<!-- Dari product/index.php -->
<h1>Product List</h1>
<div class="products">...</div>

<!-- Dari templates/footer.php -->
<footer>...</footer>
</body>
</html>
```

---

### 4️⃣ Return View sebagai String

Berguna untuk:
- Email template
- PDF generation
- AJAX partial response
- Caching

```php
<?php
public function send_email($user_id) {
    $data['user'] = $this->user_model->get_by_id($user_id);
    $data['order'] = $this->order_model->get_latest($user_id);
    
    // Parameter ketiga TRUE = return as string
    $email_body = $this->load->view('email/order_confirmation', $data, TRUE);
    
    // Gunakan untuk email
    $this->email->message($email_body);
    $this->email->send();
}

public function ajax_user_card($id) {
    $data['user'] = $this->user_model->get_by_id($id);
    
    // Return partial view untuk AJAX
    $html = $this->load->view('partials/user_card', $data, TRUE);
    
    echo $html;
}
```

---

## 📊 Flow Loading View

```
┌─────────────────────────────────────────────────────────────────┐
│                       CONTROLLER                                 │
│                                                                  │
│   $data['title'] = 'Home';                                      │
│   $data['user'] = ['name' => 'John'];                           │
│                                                                  │
│   $this->load->view('home', $data);                             │
│                      │                                           │
└──────────────────────│───────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                    CI3 LOADER                                    │
│                                                                  │
│   1. Cari file: application/views/home.php                      │
│   2. Extract $data menjadi variabel:                            │
│      - $data['title'] → $title                                  │
│      - $data['user'] → $user                                    │
│   3. Include file view                                           │
│   4. Output ke browser (atau return string)                     │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                    VIEW FILE                                     │
│                 (home.php)                                       │
│                                                                  │
│   <h1><?= $title ?></h1>                                        │
│   <p>Welcome, <?= $user['name'] ?></p>                          │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
                       │
                       ▼
┌─────────────────────────────────────────────────────────────────┐
│                    OUTPUT                                        │
│                                                                  │
│   <h1>Home</h1>                                                  │
│   <p>Welcome, John</p>                                           │
│                                                                  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🔄 View dalam View (Nested Views)

Anda bisa load view dari dalam view lain:

```php
<!-- views/user/profile.php -->
<div class="profile">
    <h1>User Profile</h1>
    
    <!-- Load partial view -->
    <?php $this->load->view('partials/user_info', ['user' => $user]); ?>
    
    <!-- Load another partial -->
    <?php $this->load->view('partials/user_stats', ['stats' => $stats]); ?>
</div>
```

```php
<!-- views/partials/user_info.php -->
<div class="user-info">
    <img src="<?= $user->avatar ?>">
    <h2><?= $user->name ?></h2>
    <p><?= $user->bio ?></p>
</div>
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Catatan:** Meskipun bisa, nested views bisa membuat kode sulit di-debug. Lebih baik load semua views dari controller jika memungkinkan.

</div>

---

## 📁 Naming Conventions

### File View

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Recommended</h4>

```
home.php
user_profile.php
product_list.php
order_confirmation.php
```

- Huruf kecil
- Underscore untuk multi-word
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Hindari</h4>

```
Home.php
userProfile.php
product-list.php
```

- CamelCase
- Dash/hyphen
</div>

</div>

### Folder Structure

```
views/
├── templates/          # Layout components
│   ├── header.php
│   ├── footer.php
│   └── sidebar.php
│
├── partials/           # Reusable components
│   ├── alert.php
│   ├── pagination.php
│   └── user_card.php
│
├── [controller_name]/  # Grouped by controller
│   ├── index.php
│   ├── create.php
│   ├── edit.php
│   └── show.php
│
├── emails/             # Email templates
│   ├── welcome.php
│   └── reset_password.php
│
└── errors/             # Error pages
    ├── 404.php
    └── general.php
```

---

## ⚠️ Common Errors

### Error: Unable to load the requested file

```php
// ❌ File tidak ditemukan
$this->load->view('users/profile');  // File: users/profile.php tidak ada

// ✅ Cek nama file dan folder
$this->load->view('user/profile');   // File: user/profile.php
```

**Troubleshooting:**
1. Cek nama file (case-sensitive di Linux/Mac)
2. Cek lokasi folder
3. Pastikan extension `.php`

### Error: Undefined variable in view

```php
// Controller
$data['title'] = 'Home';
$this->load->view('home', $data);

// View - ❌ Error
<p><?= $username ?></p>  // $username tidak dikirim!

// View - ✅ Benar
<p><?= $title ?></p>     // $title ada di $data
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Syntax `$this->load->view()`
- [ ] Load view dari sub-folder
- [ ] Load multiple views (header + content + footer)
- [ ] Return view sebagai string
- [ ] Naming conventions untuk views
- [ ] Error handling

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="view-concept.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Understanding Views
      </button>
    </a>
  </div>
  <div>
    <a href="passing-data.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Passing Data →
      </button>
    </a>
  </div>
</div>
