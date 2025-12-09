# 📊 Model Fundamentals

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami konsep Model dalam MVC
- ✅ Membuat Model pertama
- ✅ Menggunakan Model di Controller
- ✅ Memahami naming conventions

---

## 🤔 Apa itu Model?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Model** adalah komponen yang bertanggung jawab untuk **mengelola data** - mengambil, menyimpan, mengupdate, dan menghapus data dari database.

**Analogi:** Model seperti **staff gudang** 📦
- Tahu di mana barang disimpan
- Bisa mengambil barang yang diminta
- Bisa menyimpan barang baru
- Bisa update stok
- Tidak perlu tahu siapa yang minta (controller) atau bagaimana ditampilkan (view)

</div>

---

## 🔄 Model dalam Arsitektur MVC

```
┌─────────────────────────────────────────────────────────────────┐
│                       🎮 CONTROLLER                              │
│                                                                  │
│   // Minta data ke Model                                         │
│   $users = $this->user_model->get_all();                        │
│                            │                                     │
└────────────────────────────│─────────────────────────────────────┘
                             │ Request data
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                        💾 MODEL                                  │
│                    (User_model.php)                              │
│                                                                  │
│   public function get_all() {                                   │
│       return $this->db->get('users')->result();                 │
│   }                        │                                     │
│                            │                                     │
└────────────────────────────│─────────────────────────────────────┘
                             │ Query
                             ▼
┌─────────────────────────────────────────────────────────────────┐
│                       🗄️ DATABASE                                │
│                                                                  │
│   SELECT * FROM users                                            │
│                                                                  │
│   +----+----------+------------------+                           │
│   | id | name     | email            |                           │
│   +----+----------+------------------+                           │
│   | 1  | John     | john@mail.com    |                           │
│   | 2  | Jane     | jane@mail.com    |                           │
│   +----+----------+------------------+                           │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📁 Lokasi File Model

```
application/
└── models/
    ├── User_model.php        # Model untuk users
    ├── Product_model.php     # Model untuk products
    ├── Order_model.php       # Model untuk orders
    │
    └── admin/                # Sub-folder untuk admin models
        ├── Report_model.php
        └── Setting_model.php
```

---

## 📋 Struktur Dasar Model

```php
<?php
// application/models/User_model.php

defined('BASEPATH') OR exit('No direct script access allowed');

class User_model extends CI_Model {
    
    // Nama tabel (opsional, tapi recommended)
    protected $table = 'users';
    
    /**
     * Constructor
     */
    public function __construct() {
        parent::__construct();
        // Database otomatis ter-load jika di-autoload
        // atau load manual: $this->load->database();
    }
    
    /**
     * Get all users
     */
    public function get_all() {
        return $this->db->get($this->table)->result();
    }
    
    /**
     * Get user by ID
     */
    public function get_by_id($id) {
        return $this->db->get_where($this->table, ['id' => $id])->row();
    }
    
    /**
     * Create new user
     */
    public function create($data) {
        return $this->db->insert($this->table, $data);
    }
    
    /**
     * Update user
     */
    public function update($id, $data) {
        return $this->db->where('id', $id)->update($this->table, $data);
    }
    
    /**
     * Delete user
     */
    public function delete($id) {
        return $this->db->where('id', $id)->delete($this->table);
    }
}
```

---

## 📏 Naming Conventions

### Nama File

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Benar</h4>

```
User_model.php
Product_model.php
Order_item_model.php
```

- Huruf pertama KAPITAL
- Diakhiri dengan `_model`
- Underscore untuk multi-word
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Salah</h4>

```
user_model.php     (huruf kecil)
UserModel.php      (tanpa underscore)
users.php          (tanpa _model)
```

</div>

</div>

### Nama Class

```php
// File: User_model.php
class User_model extends CI_Model { }

// File: Product_model.php
class Product_model extends CI_Model { }

// File: Order_item_model.php
class Order_item_model extends CI_Model { }
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **PENTING:** Nama class HARUS sama dengan nama file!

</div>

---

## 🔌 Menggunakan Model

### Load Model di Controller

```php
<?php
class User extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        // Load model
        $this->load->model('user_model');
    }
    
    public function index() {
        // Gunakan model
        $data['users'] = $this->user_model->get_all();
        $this->load->view('user/index', $data);
    }
    
    public function profile($id) {
        $data['user'] = $this->user_model->get_by_id($id);
        $this->load->view('user/profile', $data);
    }
}
```

### Load Model dengan Alias

```php
<?php
// Load dengan alias
$this->load->model('user_model', 'users');

// Gunakan dengan alias
$data = $this->users->get_all();
```

### Load dari Sub-folder

```php
<?php
// Model di: models/admin/Report_model.php
$this->load->model('admin/report_model');

// Gunakan
$reports = $this->report_model->get_monthly();
```

### Autoload Model

```php
<?php
// application/config/autoload.php

$autoload['model'] = array('user_model', 'product_model');
```

---

## 📊 Return Types

### result() - Array of Objects

```php
<?php
public function get_all() {
    $query = $this->db->get('users');
    return $query->result();  // Array of objects
}

// Penggunaan di controller
$users = $this->user_model->get_all();
foreach ($users as $user) {
    echo $user->name;   // Akses dengan ->
    echo $user->email;
}
```

### result_array() - Array of Arrays

```php
<?php
public function get_all_array() {
    $query = $this->db->get('users');
    return $query->result_array();  // Array of arrays
}

// Penggunaan
$users = $this->user_model->get_all_array();
foreach ($users as $user) {
    echo $user['name'];   // Akses dengan ['key']
    echo $user['email'];
}
```

### row() - Single Object

```php
<?php
public function get_by_id($id) {
    $query = $this->db->get_where('users', ['id' => $id]);
    return $query->row();  // Single object atau NULL
}

// Penggunaan
$user = $this->user_model->get_by_id(5);
if ($user) {
    echo $user->name;
}
```

### row_array() - Single Array

```php
<?php
public function get_by_id_array($id) {
    $query = $this->db->get_where('users', ['id' => $id]);
    return $query->row_array();  // Single array atau NULL
}

// Penggunaan
$user = $this->user_model->get_by_id_array(5);
if ($user) {
    echo $user['name'];
}
```

---

## 🎯 Model Best Practices

### 1. Satu Model = Satu Tabel (Umumnya)

```php
<?php
// User_model.php → tabel 'users'
class User_model extends CI_Model {
    protected $table = 'users';
}

// Product_model.php → tabel 'products'
class Product_model extends CI_Model {
    protected $table = 'products';
}
```

### 2. Fat Model, Thin Controller

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Bad: Fat Controller</h4>

```php
// Controller - terlalu banyak logic
public function report() {
    $users = $this->db->get('users')->result();
    $total = 0;
    $active = 0;
    foreach ($users as $u) {
        $total++;
        if ($u->status == 'active') {
            $active++;
        }
    }
    $data['stats'] = [
        'total' => $total,
        'active' => $active
    ];
}
```
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Good: Fat Model</h4>

```php
// Controller - bersih
public function report() {
    $data['stats'] = $this->user_model->get_stats();
}

// Model - berisi logic
public function get_stats() {
    return [
        'total' => $this->count_all(),
        'active' => $this->count_active()
    ];
}
```
</div>

</div>

### 3. Method Naming yang Jelas

```php
<?php
class User_model extends CI_Model {
    
    // Get methods
    public function get_all() { }
    public function get_by_id($id) { }
    public function get_by_email($email) { }
    public function get_active_users() { }
    
    // Count methods
    public function count_all() { }
    public function count_active() { }
    
    // Check methods
    public function is_email_exists($email) { }
    public function is_username_taken($username) { }
    
    // Action methods
    public function create($data) { }
    public function update($id, $data) { }
    public function delete($id) { }
    public function soft_delete($id) { }
    public function restore($id) { }
}
```

---

## 📝 Contoh Model Lengkap

```php
<?php
// application/models/Product_model.php

defined('BASEPATH') OR exit('No direct script access allowed');

class Product_model extends CI_Model {
    
    protected $table = 'products';
    
    public function __construct() {
        parent::__construct();
    }
    
    // ==================== GET ====================
    
    public function get_all() {
        return $this->db
            ->order_by('created_at', 'DESC')
            ->get($this->table)
            ->result();
    }
    
    public function get_by_id($id) {
        return $this->db
            ->get_where($this->table, ['id' => $id])
            ->row();
    }
    
    public function get_by_category($category_id, $limit = 10) {
        return $this->db
            ->where('category_id', $category_id)
            ->where('status', 'active')
            ->limit($limit)
            ->get($this->table)
            ->result();
    }
    
    public function search($keyword) {
        return $this->db
            ->like('name', $keyword)
            ->or_like('description', $keyword)
            ->get($this->table)
            ->result();
    }
    
    // ==================== COUNT ====================
    
    public function count_all() {
        return $this->db->count_all($this->table);
    }
    
    public function count_by_category($category_id) {
        return $this->db
            ->where('category_id', $category_id)
            ->count_all_results($this->table);
    }
    
    // ==================== CREATE ====================
    
    public function create($data) {
        $data['created_at'] = date('Y-m-d H:i:s');
        $this->db->insert($this->table, $data);
        return $this->db->insert_id();
    }
    
    // ==================== UPDATE ====================
    
    public function update($id, $data) {
        $data['updated_at'] = date('Y-m-d H:i:s');
        return $this->db
            ->where('id', $id)
            ->update($this->table, $data);
    }
    
    // ==================== DELETE ====================
    
    public function delete($id) {
        return $this->db
            ->where('id', $id)
            ->delete($this->table);
    }
    
    public function soft_delete($id) {
        return $this->update($id, ['deleted_at' => date('Y-m-d H:i:s')]);
    }
}
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Peran Model dalam MVC
- [ ] Naming conventions (file dan class)
- [ ] Cara load model di controller
- [ ] Perbedaan result(), row(), result_array(), row_array()
- [ ] Prinsip "Fat Model, Thin Controller"
- [ ] Method naming yang baik

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="database-config.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Database Configuration
      </button>
    </a>
  </div>
  <div>
    <a href="query-builder.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Query Builder →
      </button>
    </a>
  </div>
</div>
