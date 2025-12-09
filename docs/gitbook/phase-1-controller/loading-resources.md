# 📦 Loading Resources

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Load views dengan benar
- ✅ Load dan menggunakan models
- ✅ Load libraries (built-in dan custom)
- ✅ Load helpers
- ✅ Memahami kapan menggunakan autoload

---

## 🤔 Apa itu Loader?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Loader** (`$this->load`) adalah class untuk memuat berbagai resources di CI3:
- 👁️ **Views** - File tampilan HTML
- 💾 **Models** - Class untuk database
- 📚 **Libraries** - Class dengan fungsionalitas tertentu
- 🔧 **Helpers** - Kumpulan fungsi bantuan
- ⚙️ **Config** - File konfigurasi tambahan

</div>

---

## 👁️ Loading Views

### Basic View Loading

```php
<?php
class Home extends CI_Controller {
    
    public function index() {
        // Load single view
        $this->load->view('home');
        // Mencari: application/views/home.php
    }
}
```

### View dengan Data

```php
<?php
public function profile() {
    // Siapkan data
    $data = [
        'title' => 'User Profile',
        'username' => 'john_doe',
        'email' => 'john@mail.com'
    ];
    
    // Kirim data ke view
    $this->load->view('user/profile', $data);
}
```

**Di View (user/profile.php):**
```php
<h1><?= $title ?></h1>
<p>Username: <?= $username ?></p>
<p>Email: <?= $email ?></p>
```

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Key dari `$data` menjadi nama variabel di view!**
- `$data['title']` → `$title`
- `$data['username']` → `$username`

</div>

### Multiple Views (Template Pattern)

```php
<?php
public function index() {
    $data['title'] = 'Home Page';
    $data['products'] = $this->product_model->get_all();
    
    // Load views berurutan
    $this->load->view('templates/header', $data);
    $this->load->view('home', $data);
    $this->load->view('templates/footer');
}
```

### Return View as String

```php
<?php
public function email_preview() {
    $data['order'] = $this->order_model->get(123);
    
    // Parameter ketiga TRUE = return as string
    $html = $this->load->view('email/order_confirmation', $data, TRUE);
    
    echo $html;
    // atau kirim via email
    $this->email->message($html);
}
```

### View di Sub-folder

```
views/
├── templates/
│   ├── header.php
│   └── footer.php
├── user/
│   ├── profile.php
│   └── settings.php
└── admin/
    └── dashboard.php
```

```php
<?php
$this->load->view('templates/header');
$this->load->view('user/profile');
$this->load->view('admin/dashboard');
```

---

## 💾 Loading Models

### Basic Model Loading

```php
<?php
public function index() {
    // Load model
    $this->load->model('user_model');
    
    // Gunakan model
    $users = $this->user_model->get_all();
}
```

### Model dengan Alias

```php
<?php
// Load dengan nama alias
$this->load->model('user_model', 'users');

// Gunakan dengan alias
$data = $this->users->get_all();
```

### Load di Constructor

```php
<?php
class User extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        // Load sekali, gunakan di semua method
        $this->load->model('user_model');
        $this->load->model('profile_model');
    }
    
    public function index() {
        $users = $this->user_model->get_all();
    }
    
    public function profile($id) {
        $user = $this->user_model->get_by_id($id);
        $profile = $this->profile_model->get_by_user($id);
    }
}
```

### Model di Sub-folder

```
models/
├── User_model.php
├── admin/
│   ├── Admin_model.php
│   └── Report_model.php
└── api/
    └── Api_user_model.php
```

```php
<?php
$this->load->model('admin/admin_model');
$this->load->model('api/api_user_model', 'api_user');
```

---

## 📚 Loading Libraries

### Built-in Libraries

```php
<?php
// Session
$this->load->library('session');
$this->session->set_userdata('user_id', 123);

// Form Validation
$this->load->library('form_validation');
$this->form_validation->set_rules('email', 'Email', 'required|valid_email');

// Email
$this->load->library('email');
$this->email->to('user@mail.com');
$this->email->send();

// Pagination
$this->load->library('pagination');

// Upload
$this->load->library('upload');

// Image Manipulation
$this->load->library('image_lib');
```

### Multiple Libraries

```php
<?php
// Load beberapa library sekaligus
$this->load->library(['session', 'form_validation', 'email']);
```

### Library dengan Config

```php
<?php
// Upload dengan konfigurasi
$config = [
    'upload_path' => './uploads/',
    'allowed_types' => 'gif|jpg|png',
    'max_size' => 2048
];
$this->load->library('upload', $config);

// Pagination dengan config
$config = [
    'base_url' => base_url('product/index'),
    'total_rows' => 100,
    'per_page' => 10
];
$this->load->library('pagination', $config);
```

### Custom Library

**Buat library: `application/libraries/My_library.php`**
```php
<?php
class My_library {
    
    public function do_something() {
        return 'Hello from my library!';
    }
}
```

**Gunakan di controller:**
```php
<?php
$this->load->library('my_library');
echo $this->my_library->do_something();
```

---

## 🔧 Loading Helpers

### Built-in Helpers

```php
<?php
// URL Helper
$this->load->helper('url');
echo base_url('assets/css/style.css');
echo site_url('user/profile');
redirect('login');

// Form Helper
$this->load->helper('form');
echo form_open('user/save');
echo form_input('name', 'John');
echo form_close();

// Text Helper
$this->load->helper('text');
echo word_limiter($text, 100);
echo character_limiter($text, 200);

// Date Helper
$this->load->helper('date');
echo now();

// Security Helper
$this->load->helper('security');
echo do_hash('password');
```

### Multiple Helpers

```php
<?php
// Array syntax
$this->load->helper(['url', 'form', 'text']);
```

### Custom Helper

**Buat helper: `application/helpers/my_helper.php`**
```php
<?php
if (!function_exists('format_rupiah')) {
    function format_rupiah($number) {
        return 'Rp ' . number_format($number, 0, ',', '.');
    }
}

if (!function_exists('format_date_id')) {
    function format_date_id($date) {
        $months = ['Januari', 'Februari', 'Maret', ...];
        // Format: 15 Januari 2024
    }
}
```

**Gunakan:**
```php
<?php
$this->load->helper('my_helper');
echo format_rupiah(1500000);  // Rp 1.500.000
```

---

## ⚙️ Loading Config

```php
<?php
// Load config file
$this->load->config('custom_config');

// Akses config value
$value = $this->config->item('my_setting');

// Load config ke array tertentu
$this->load->config('custom_config', TRUE);
$value = $this->config->item('my_setting', 'custom_config');
```

**File config: `application/config/custom_config.php`**
```php
<?php
$config['my_setting'] = 'value';
$config['api_key'] = 'abc123';
$config['items_per_page'] = 20;
```

---

## 🔄 Autoload vs Manual Load

### Kapan Autoload?

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px;">
<h4>✅ Gunakan Autoload</h4>
<ul>
<li>Dipakai di hampir semua controller</li>
<li>Database connection</li>
<li>Session library</li>
<li>URL helper</li>
<li>Form helper</li>
</ul>
</div>

<div style="background: #FFF3E0; padding: 20px; border-radius: 10px;">
<h4>⚠️ Manual Load</h4>
<ul>
<li>Hanya dipakai di beberapa controller</li>
<li>Email library</li>
<li>Upload library</li>
<li>PDF generator</li>
<li>Specific model</li>
</ul>
</div>

</div>

### Setting Autoload

**File: `application/config/autoload.php`**
```php
<?php
// Libraries - tersedia di semua controller
$autoload['libraries'] = array('database', 'session');

// Helpers - tersedia di semua controller dan view
$autoload['helper'] = array('url', 'form', 'security');

// Models - jarang di-autoload (lebih baik load sesuai kebutuhan)
$autoload['model'] = array();
```

---

## 📊 Loading Summary

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Resource | Syntax | Lokasi File |
|----------|--------|-------------|
| **View** | `$this->load->view('name')` | `views/name.php` |
| **Model** | `$this->load->model('name')` | `models/Name.php` |
| **Library** | `$this->load->library('name')` | `libraries/Name.php` |
| **Helper** | `$this->load->helper('name')` | `helpers/name_helper.php` |
| **Config** | `$this->load->config('name')` | `config/name.php` |

</div>

---

## 💡 Best Practices

### 1. Load di Constructor untuk Reusability

```php
<?php
public function __construct() {
    parent::__construct();
    
    // Load yang sering dipakai di controller ini
    $this->load->model('product_model');
    $this->load->helper('text');
}
```

### 2. Cek Sebelum Load (untuk Library)

```php
<?php
if (!class_exists('CI_Email')) {
    $this->load->library('email');
}
```

### 3. Load Database Secara Eksplisit (untuk Debugging)

```php
<?php
// Daripada autoload, kadang lebih baik eksplisit
$this->load->database();
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Cara load view dengan data
- [ ] Cara load model dan menggunakannya
- [ ] Perbedaan library dan helper
- [ ] Cara load dengan config/parameter
- [ ] Kapan menggunakan autoload vs manual load
- [ ] Cara membuat custom library/helper

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="routing.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Routing System
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
