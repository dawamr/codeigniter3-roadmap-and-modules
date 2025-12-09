# 🔄 Request Flow

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami bagaimana CI3 memproses setiap request
- ✅ Mengetahui peran setiap komponen dalam flow
- ✅ Bisa men-debug masalah dengan lebih efektif
- ✅ Memahami MVC pattern dalam konteks CI3

---

## 🤔 Mengapa Perlu Memahami Request Flow?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Analogi:** Request flow adalah seperti **alur produksi di pabrik** 🏭

Ketika ada pesanan (request), produk harus melewati beberapa stasiun kerja sebelum sampai ke pelanggan. Memahami alur ini membantu Anda:

- 🔍 **Debug lebih cepat** - Tahu di mana masalah terjadi
- ⚡ **Optimasi performa** - Tahu bagian mana yang bisa dipercepat
- 🧩 **Arsitektur lebih baik** - Tahu di mana meletakkan kode

</div>

---

## 📊 Request Flow Diagram

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                           USER BROWSER                                       │
│                     http://localhost/ci3/user/profile/5                      │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           1️⃣ index.php                                      │
│                      (Front Controller / Pintu Masuk)                        │
│                                                                              │
│   • Set ENVIRONMENT (development/production)                                 │
│   • Define system & application paths                                        │
│   • Load CodeIgniter core                                                    │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           2️⃣ ROUTING                                        │
│                    (system/core/Router.php)                                  │
│                                                                              │
│   • Parse URI: "user/profile/5"                                              │
│   • Check routes.php for custom routes                                       │
│   • Determine: Controller = User, Method = profile, Param = 5               │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          3️⃣ SECURITY                                        │
│                                                                              │
│   • XSS Filtering (jika aktif)                                               │
│   • CSRF Validation (jika aktif)                                             │
│   • URI Security check                                                       │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                         4️⃣ CONTROLLER                                       │
│                  (application/controllers/User.php)                          │
│                                                                              │
│   class User extends CI_Controller {                                         │
│       public function profile($id) {                                         │
│           // Load model, get data, load view                                 │
│       }                                                                      │
│   }                                                                          │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                          ┌─────────┴─────────┐
                          │                   │
                          ▼                   ▼
┌────────────────────────────────┐  ┌────────────────────────────────┐
│         5️⃣ MODEL               │  │    6️⃣ LIBRARIES/HELPERS        │
│  (application/models/)         │  │                                │
│                                │  │  • Session                     │
│  • Query database              │  │  • Form Validation             │
│  • Return data                 │  │  • Email                       │
│                                │  │  • Custom libraries            │
└────────────────────────────────┘  └────────────────────────────────┘
                          │                   │
                          └─────────┬─────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                            7️⃣ VIEW                                          │
│                    (application/views/)                                      │
│                                                                              │
│   • Receive data from controller                                             │
│   • Render HTML                                                              │
│   • Output to browser                                                        │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                          8️⃣ OUTPUT                                          │
│                                                                              │
│   • Apply output filters                                                     │
│   • Send HTTP headers                                                        │
│   • Return HTML to browser                                                   │
└─────────────────────────────────────────────────────────────────────────────┘
                                    │
                                    ▼
┌─────────────────────────────────────────────────────────────────────────────┐
│                           USER BROWSER                                       │
│                        Menampilkan halaman                                   │
└─────────────────────────────────────────────────────────────────────────────┘
```

---

## 🔍 Penjelasan Setiap Tahap

### 1️⃣ index.php (Front Controller)

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Pintu masuk SEMUA request ke aplikasi.

Setiap URL (`/user`, `/product`, `/api/data`) semuanya masuk melalui `index.php` dulu. Inilah yang disebut **Front Controller Pattern**.

</div>

```php
<?php
// index.php - Apa yang terjadi

// 1. Set environment
define('ENVIRONMENT', 'development');

// 2. Set error reporting berdasarkan environment
switch (ENVIRONMENT) {
    case 'development':
        error_reporting(E_ALL);
        break;
    case 'production':
        error_reporting(0);  // Sembunyikan error
        break;
}

// 3. Define paths
$system_path = 'system';
$application_folder = 'application';

// 4. Load CodeIgniter
require_once BASEPATH.'core/CodeIgniter.php';
```

---

### 2️⃣ Routing

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menentukan controller dan method mana yang akan dipanggil.

</div>

**Contoh URL Parsing:**

| URL | Controller | Method | Parameter |
|-----|------------|--------|-----------|
| `/welcome` | Welcome | index | - |
| `/user/profile` | User | profile | - |
| `/user/profile/5` | User | profile | 5 |
| `/product/detail/123` | Product | detail | 123 |
| `/admin/users/edit/42` | admin/Users | edit | 42 |

**Proses Routing:**
```
URL: /user/profile/5

1. Router menerima URI: "user/profile/5"
2. Cek routes.php → tidak ada custom route
3. Parse:
   - Segment 1 (user) → Controller: User.php
   - Segment 2 (profile) → Method: profile()
   - Segment 3 (5) → Parameter: $id = 5
4. Load controller User.php
5. Panggil User->profile(5)
```

---

### 3️⃣ Security Check

<div style="background: #FFF3E0; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Memfilter dan memvalidasi input untuk keamanan.

</div>

**Yang dicek:**
- **URI Security** - Karakter berbahaya di URL
- **XSS Filter** - Script injection (jika diaktifkan)
- **CSRF Token** - Validasi token form (jika diaktifkan)

```php
// Di config.php
$config['csrf_protection'] = TRUE;  // Aktifkan CSRF
$config['global_xss_filtering'] = TRUE;  // Filter XSS global
```

---

### 4️⃣ Controller

<div style="background: #FCE4EC; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** "Otak" yang mengatur semua logika dan alur.

**Analogi:** Seperti **manajer proyek** yang:
- Menerima tugas (request)
- Mendelegasikan ke tim (model, library)
- Menyusun hasil (view)
- Mengirim ke client

</div>

```php
<?php
// application/controllers/User.php

class User extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        // Load model yang dibutuhkan
        $this->load->model('user_model');
    }
    
    public function profile($id) {
        // 1. Ambil data dari Model
        $data['user'] = $this->user_model->get_by_id($id);
        
        // 2. Cek apakah user ditemukan
        if (!$data['user']) {
            show_404();  // Tampilkan halaman 404
        }
        
        // 3. Kirim data ke View
        $data['title'] = 'User Profile';
        
        // 4. Load view
        $this->load->view('templates/header', $data);
        $this->load->view('user/profile', $data);
        $this->load->view('templates/footer');
    }
}
```

---

### 5️⃣ Model

<div style="background: #E0F7FA; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Berinteraksi dengan database.

**Analogi:** Seperti **staf gudang** yang:
- Mengambil barang (SELECT)
- Menyimpan barang baru (INSERT)
- Mengupdate stok (UPDATE)
- Membuang barang (DELETE)

</div>

```php
<?php
// application/models/User_model.php

class User_model extends CI_Model {
    
    public function get_by_id($id) {
        // Query ke database
        $query = $this->db->get_where('users', ['id' => $id]);
        
        // Return satu baris data
        return $query->row();
    }
    
    public function get_all() {
        // Return semua user
        return $this->db->get('users')->result();
    }
}
```

**Flow Data:**
```
Controller: $this->user_model->get_by_id(5)
    ↓
Model: $this->db->get_where('users', ['id' => 5])
    ↓
Database: SELECT * FROM users WHERE id = 5
    ↓
Model: return $query->row()
    ↓
Controller: $data['user'] = {...}
```

---

### 6️⃣ Libraries & Helpers

<div style="background: #F3E5F5; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menyediakan tools dan fungsi tambahan.

</div>

**Libraries (Class-based):**
```php
// Load library
$this->load->library('session');
$this->load->library('form_validation');

// Gunakan
$this->session->set_userdata('user_id', 123);
$this->form_validation->set_rules('email', 'Email', 'required|valid_email');
```

**Helpers (Function-based):**
```php
// Load helper
$this->load->helper('url');
$this->load->helper('form');

// Gunakan
echo base_url('assets/css/style.css');
echo form_open('user/save');
```

---

### 7️⃣ View

<div style="background: #FFF9C4; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Menampilkan data dalam format HTML.

**Analogi:** Seperti **template surat** yang tinggal diisi data.

</div>

```php
<!-- application/views/user/profile.php -->
<!DOCTYPE html>
<html>
<head>
    <title><?= $title ?></title>
</head>
<body>
    <h1>Profil User</h1>
    
    <div class="profile-card">
        <p><strong>Nama:</strong> <?= $user->nama ?></p>
        <p><strong>Email:</strong> <?= $user->email ?></p>
        <p><strong>Bergabung:</strong> <?= $user->created_at ?></p>
    </div>
    
    <a href="<?= base_url('user/edit/' . $user->id) ?>">
        Edit Profile
    </a>
</body>
</html>
```

**Flow Data ke View:**
```php
// Controller
$data['user'] = $this->user_model->get_by_id(5);
$data['title'] = 'User Profile';
$this->load->view('user/profile', $data);

// Di View, $data menjadi variabel terpisah:
// $user = object user
// $title = 'User Profile'
```

---

### 8️⃣ Output

<div style="background: #DCEDC8; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Fungsi:** Mengirim response final ke browser.

</div>

**Yang terjadi:**
1. Semua output dari view dikumpulkan
2. Output filters diterapkan (jika ada)
3. HTTP headers dikirim
4. HTML dikirim ke browser
5. Browser merender halaman

---

## 🎯 MVC Pattern di CI3

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 20px; margin: 20px 0;">

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; text-align: center;">
<h3>📊 MODEL</h3>
<p><strong>Data Layer</strong></p>
<hr>
<ul style="text-align: left;">
<li>Query database</li>
<li>Business logic</li>
<li>Data validation</li>
</ul>
<p><code>application/models/</code></p>
</div>

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; text-align: center;">
<h3>👁️ VIEW</h3>
<p><strong>Presentation Layer</strong></p>
<hr>
<ul style="text-align: left;">
<li>HTML output</li>
<li>Display data</li>
<li>User interface</li>
</ul>
<p><code>application/views/</code></p>
</div>

<div style="background: #FCE4EC; padding: 20px; border-radius: 10px; text-align: center;">
<h3>🎮 CONTROLLER</h3>
<p><strong>Logic Layer</strong></p>
<hr>
<ul style="text-align: left;">
<li>Handle request</li>
<li>Coordinate M & V</li>
<li>Response control</li>
</ul>
<p><code>application/controllers/</code></p>
</div>

</div>

---

## 🔧 Contoh Flow Lengkap

**Skenario:** User mengakses halaman profil

```
1. USER: Buka http://localhost/ci3/user/profile/5

2. INDEX.PHP:
   - Load CI core
   - Set environment = development

3. ROUTER:
   - Parse: controller=User, method=profile, param=5

4. CONTROLLER (User.php):
   public function profile($id) {  // $id = 5
       $this->load->model('user_model');
       $data['user'] = $this->user_model->get_by_id($id);
       $this->load->view('user/profile', $data);
   }

5. MODEL (User_model.php):
   public function get_by_id($id) {
       return $this->db->get_where('users', ['id' => $id])->row();
   }
   // Return: {id:5, nama:'John', email:'john@mail.com'}

6. VIEW (user/profile.php):
   <h1><?= $user->nama ?></h1>
   <p><?= $user->email ?></p>
   // Render HTML

7. OUTPUT:
   - Kirim HTML ke browser

8. BROWSER:
   - Tampilkan halaman profil John
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Semua request masuk melalui index.php
- [ ] Router menentukan controller dan method
- [ ] Controller mengatur alur (load model, view)
- [ ] Model berinteraksi dengan database
- [ ] View menampilkan output HTML
- [ ] Data mengalir: Request → Controller → Model → View → Response

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="configuration.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Configuration
      </button>
    </a>
  </div>
  <div>
    <a href="environment.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Environment Settings →
      </button>
    </a>
  </div>
</div>
