# 🏗️ Controller Anatomy

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami struktur dasar Controller CI3
- ✅ Mengenal constructor dan fungsinya
- ✅ Membuat controller pertama Anda
- ✅ Memahami naming conventions

---

## 📋 Struktur Dasar Controller

```php
<?php
// application/controllers/NamaController.php

defined('BASEPATH') OR exit('No direct script access allowed');

class NamaController extends CI_Controller {
    
    /**
     * Constructor - dipanggil saat controller diload
     */
    public function __construct() {
        parent::__construct();
        // Load model, library, helper yang dibutuhkan
    }
    
    /**
     * Method default - dipanggil jika tidak ada method di URL
     */
    public function index() {
        // Kode untuk halaman utama controller ini
    }
    
    /**
     * Method lainnya
     */
    public function nama_method() {
        // Kode untuk method ini
    }
}
```

---

## 🔍 Penjelasan Setiap Bagian

### 1️⃣ Security Line

```php
defined('BASEPATH') OR exit('No direct script access allowed');
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **WAJIB ada di baris pertama setiap file PHP CI3!**

Fungsinya: Mencegah akses langsung ke file PHP. File hanya bisa diakses melalui `index.php` (front controller).

**Tanpa security line:**
```
http://site.com/application/controllers/User.php  ❌ Bisa diakses langsung!
```

**Dengan security line:**
```
http://site.com/application/controllers/User.php  ✅ Access denied
http://site.com/user  ✅ Berfungsi normal
```

</div>

---

### 2️⃣ Class Declaration

```php
class User extends CI_Controller {
```

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Penjelasan:**
- `class User` → Nama class (harus sama dengan nama file)
- `extends CI_Controller` → Mewarisi semua fitur dari CI_Controller

**Dengan extends, Anda mendapat akses ke:**
- `$this->load` → Load model, view, library
- `$this->input` → Akses input (GET, POST)
- `$this->session` → Manage session
- `$this->db` → Query database (jika autoload)
- Dan banyak lagi...

</div>

---

### 3️⃣ Constructor

```php
public function __construct() {
    parent::__construct();  // WAJIB dipanggil!
    
    // Inisialisasi yang dibutuhkan
    $this->load->model('user_model');
    $this->load->helper('form');
}
```

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi Constructor:**
- Dipanggil **otomatis** setiap controller diakses
- Tempat untuk load model/library yang sering dipakai
- Tempat untuk cek authentication

**`parent::__construct()` WAJIB dipanggil!**  
Tanpa ini, fitur CI3 tidak akan berfungsi.

</div>

**Contoh penggunaan constructor:**

```php
<?php
class Admin extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        
        // Load model yang sering dipakai
        $this->load->model('admin_model');
        
        // Cek apakah user sudah login sebagai admin
        if (!$this->session->userdata('is_admin')) {
            redirect('auth/login');
        }
    }
    
    public function dashboard() {
        // User pasti sudah login (dicek di constructor)
        $data['stats'] = $this->admin_model->get_stats();
        $this->load->view('admin/dashboard', $data);
    }
}
```

---

### 4️⃣ Method index()

```php
public function index() {
    // Method default
}
```

<div style="background: #FCE4EC; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Method `index()` adalah method DEFAULT.**

Dipanggil ketika URL hanya menyebut controller tanpa method:

| URL | Method yang dipanggil |
|-----|----------------------|
| `/user` | `User->index()` |
| `/user/index` | `User->index()` |
| `/product` | `Product->index()` |

</div>

---

### 5️⃣ Methods Lainnya

```php
public function profile() {
    // Dipanggil: /user/profile
}

public function edit($id) {
    // Dipanggil: /user/edit/5
}

private function _helper_method() {
    // Tidak bisa diakses via URL (private)
}
```

---

## 📏 Naming Conventions

### 📁 Nama File

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Benar</h4>

```
User.php
Product.php
User_profile.php
Admin_dashboard.php
```

**Aturan:**
- Huruf pertama KAPITAL
- Underscore untuk multi-word
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Salah</h4>

```
user.php         (huruf kecil)
userProfile.php  (camelCase)
user-profile.php (dash)
```

</div>

</div>

### 📝 Nama Class

```php
// File: User.php
class User extends CI_Controller { }

// File: User_profile.php
class User_profile extends CI_Controller { }

// File: Admin_dashboard.php
class Admin_dashboard extends CI_Controller { }
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **PENTING:** Nama class HARUS sama dengan nama file!

```php
// File: Product.php
class Product extends CI_Controller { }  // ✅ Benar

// File: Product.php
class Products extends CI_Controller { } // ❌ Salah - nama berbeda
```

</div>

### 🔧 Nama Method

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Benar</h4>

```php
public function index() { }
public function profile() { }
public function edit_profile() { }
public function get_user_data() { }
```

**Aturan:**
- Huruf kecil semua
- Underscore untuk multi-word
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Salah</h4>

```php
public function Profile() { }     // Kapital
public function editProfile() { } // camelCase
public function edit-profile() { }// Dash
```

</div>

</div>

---

## 🔒 Method Visibility

### Public Methods (Bisa diakses via URL)

```php
public function profile() {
    // URL: /user/profile ✅
}

public function edit($id) {
    // URL: /user/edit/5 ✅
}
```

### Private/Protected Methods (TIDAK bisa diakses via URL)

```php
private function _validate_user($id) {
    // URL: /user/_validate_user/5 ❌ 404!
    // Hanya bisa dipanggil dari dalam class
}

protected function _helper() {
    // Sama seperti private, tapi bisa diakses child class
}
```

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Best Practice:**  
Gunakan prefix `_` (underscore) untuk private methods agar mudah dibedakan.

```php
public function save() {
    if ($this->_validate_input()) {  // Private helper
        $this->user_model->save($data);
    }
}

private function _validate_input() {
    // Validasi logic
    return true;
}
```

</div>

---

## 📝 Contoh Controller Lengkap

```php
<?php
// application/controllers/Product.php

defined('BASEPATH') OR exit('No direct script access allowed');

class Product extends CI_Controller {
    
    /**
     * Constructor
     */
    public function __construct() {
        parent::__construct();
        $this->load->model('product_model');
        $this->load->helper(['url', 'form']);
    }
    
    /**
     * Halaman daftar produk
     * URL: /product atau /product/index
     */
    public function index() {
        $data['title'] = 'Daftar Produk';
        $data['products'] = $this->product_model->get_all();
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/index', $data);
        $this->load->view('templates/footer');
    }
    
    /**
     * Detail produk
     * URL: /product/detail/5
     */
    public function detail($id) {
        $data['product'] = $this->product_model->get_by_id($id);
        
        if (!$data['product']) {
            show_404();
        }
        
        $data['title'] = $data['product']->name;
        $data['related'] = $this->product_model->get_related($id);
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/detail', $data);
        $this->load->view('templates/footer');
    }
    
    /**
     * Form tambah produk
     * URL: /product/create
     */
    public function create() {
        $data['title'] = 'Tambah Produk';
        $data['categories'] = $this->product_model->get_categories();
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/create', $data);
        $this->load->view('templates/footer');
    }
    
    /**
     * Proses simpan produk
     * URL: /product/store (POST)
     */
    public function store() {
        // Validasi
        if (!$this->_validate_product()) {
            $this->create();  // Kembali ke form
            return;
        }
        
        // Simpan
        $product_data = [
            'name' => $this->input->post('name'),
            'price' => $this->input->post('price'),
            'category_id' => $this->input->post('category_id')
        ];
        
        if ($this->product_model->create($product_data)) {
            $this->session->set_flashdata('success', 'Produk berhasil ditambah');
            redirect('product');
        } else {
            $this->session->set_flashdata('error', 'Gagal menambah produk');
            redirect('product/create');
        }
    }
    
    /**
     * Private method untuk validasi
     * TIDAK bisa diakses via URL
     */
    private function _validate_product() {
        $this->load->library('form_validation');
        
        $this->form_validation->set_rules('name', 'Nama', 'required|min_length[3]');
        $this->form_validation->set_rules('price', 'Harga', 'required|numeric');
        
        return $this->form_validation->run();
    }
}
```

---

## 📁 Sub-folder Controllers

Untuk organisasi lebih baik, controller bisa diletakkan di sub-folder:

```
application/controllers/
├── Welcome.php
├── User.php
│
├── admin/                    # Sub-folder admin
│   ├── Dashboard.php
│   ├── Users.php
│   └── Products.php
│
└── api/                      # Sub-folder API
    ├── Users.php
    └── Products.php
```

**URL Mapping dengan Sub-folder:**

| URL | File |
|-----|------|
| `/admin/dashboard` | `admin/Dashboard.php` → `index()` |
| `/admin/users` | `admin/Users.php` → `index()` |
| `/admin/users/edit/5` | `admin/Users.php` → `edit(5)` |
| `/api/users` | `api/Users.php` → `index()` |

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Struktur dasar controller CI3
- [ ] Fungsi security line `defined('BASEPATH')`
- [ ] Cara kerja constructor dan `parent::__construct()`
- [ ] Method `index()` sebagai default
- [ ] Naming conventions (file, class, method)
- [ ] Perbedaan public dan private methods
- [ ] Cara menggunakan sub-folder

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="controller-concept.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Understanding Controllers
      </button>
    </a>
  </div>
  <div>
    <a href="methods-parameters.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Methods & Parameters →
      </button>
    </a>
  </div>
</div>
