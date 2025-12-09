# 🔧 Methods & Parameters

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Membuat berbagai jenis method di Controller
- ✅ Menerima parameter dari URL
- ✅ Mengakses data GET dan POST
- ✅ Mengembalikan response (view, json, redirect)

---

## 📋 Jenis-jenis Methods

### 1️⃣ Method Tanpa Parameter

```php
<?php
class Page extends CI_Controller {
    
    // URL: /page/about
    public function about() {
        $data['title'] = 'Tentang Kami';
        $this->load->view('page/about', $data);
    }
    
    // URL: /page/contact
    public function contact() {
        $data['title'] = 'Hubungi Kami';
        $this->load->view('page/contact', $data);
    }
}
```

---

### 2️⃣ Method dengan Parameter URL

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Parameter dari URL langsung menjadi parameter method!**

```
URL: /product/detail/5
                    └── Parameter $id = 5
```

</div>

```php
<?php
class Product extends CI_Controller {
    
    // URL: /product/detail/5
    public function detail($id) {
        echo "Product ID: " . $id;  // Output: Product ID: 5
    }
    
    // URL: /product/compare/5/10
    public function compare($id1, $id2) {
        echo "Comparing: $id1 vs $id2";  // Output: Comparing: 5 vs 10
    }
    
    // URL: /product/list/electronics/laptop/1
    public function list($category, $subcategory, $page = 1) {
        echo "Category: $category, Sub: $subcategory, Page: $page";
        // Output: Category: electronics, Sub: laptop, Page: 1
    }
}
```

### 📊 URL to Parameter Mapping

| URL | Method | Parameters |
|-----|--------|------------|
| `/user/profile/5` | `profile($id)` | `$id = 5` |
| `/product/detail/100` | `detail($id)` | `$id = 100` |
| `/order/track/ORD001` | `track($order_id)` | `$order_id = 'ORD001'` |
| `/blog/post/2024/01/hello` | `post($year, $month, $slug)` | `$year=2024, $month=01, $slug='hello'` |

---

### 3️⃣ Parameter dengan Default Value

```php
<?php
class Blog extends CI_Controller {
    
    // URL: /blog          → $page = 1 (default)
    // URL: /blog/page/3   → $page = 3
    public function index($page = 1) {
        $per_page = 10;
        $offset = ($page - 1) * $per_page;
        
        $data['posts'] = $this->blog_model->get_posts($per_page, $offset);
        $data['current_page'] = $page;
        
        $this->load->view('blog/index', $data);
    }
    
    // URL: /blog/category/tech        → $page = 1
    // URL: /blog/category/tech/2      → $page = 2
    public function category($slug, $page = 1) {
        $data['category'] = $slug;
        $data['page'] = $page;
        $this->load->view('blog/category', $data);
    }
}
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Best Practice:**  
Parameter dengan default value harus di posisi **terakhir**!

```php
// ✅ Benar
public function search($keyword, $page = 1, $limit = 10) { }

// ❌ Salah - default di tengah
public function search($keyword, $page = 1, $limit) { }
```

</div>

---

## 📥 Mengakses Input Data

### 🔵 GET Data (dari URL query string)

```php
<?php
// URL: /product/search?keyword=laptop&category=electronics&page=2

class Product extends CI_Controller {
    
    public function search() {
        // Cara 1: Input class (RECOMMENDED - lebih aman)
        $keyword = $this->input->get('keyword');      // 'laptop'
        $category = $this->input->get('category');    // 'electronics'
        $page = $this->input->get('page');            // '2'
        
        // Dengan default value
        $sort = $this->input->get('sort') ?? 'newest';
        
        // Dengan XSS filtering
        $keyword = $this->input->get('keyword', TRUE);
        
        // Cara 2: $_GET langsung (TIDAK recommended)
        $keyword = $_GET['keyword'];  // ❌ Tidak aman
    }
}
```

---

### 🟢 POST Data (dari form)

```php
<?php
class User extends CI_Controller {
    
    public function login() {
        // Hanya proses jika method POST
        if ($this->input->method() === 'post') {
            
            // Ambil data POST
            $username = $this->input->post('username');
            $password = $this->input->post('password');
            $remember = $this->input->post('remember');  // checkbox
            
            // Dengan XSS filtering
            $username = $this->input->post('username', TRUE);
            
            // Proses login...
        }
        
        $this->load->view('auth/login');
    }
    
    public function register() {
        if ($this->input->method() === 'post') {
            // Ambil semua data POST sekaligus
            $user_data = $this->input->post();
            
            // $user_data = [
            //     'name' => 'John',
            //     'email' => 'john@mail.com',
            //     'password' => '123456'
            // ]
            
            $this->user_model->create($user_data);
        }
    }
}
```

**Form HTML:**
```html
<form action="<?= base_url('user/login') ?>" method="POST">
    <input type="text" name="username">
    <input type="password" name="password">
    <input type="checkbox" name="remember" value="1">
    <button type="submit">Login</button>
</form>
```

---

### 🟡 Request Method Check

```php
<?php
class Api extends CI_Controller {
    
    public function users() {
        $method = $this->input->method();  // 'get', 'post', 'put', 'delete'
        
        switch ($method) {
            case 'get':
                $this->_get_users();
                break;
            case 'post':
                $this->_create_user();
                break;
            case 'put':
                $this->_update_user();
                break;
            case 'delete':
                $this->_delete_user();
                break;
            default:
                show_405();  // Method not allowed
        }
    }
}
```

---

## 📤 Response Types

### 1️⃣ Load View

```php
<?php
public function index() {
    $data['title'] = 'Home';
    $data['users'] = $this->user_model->get_all();
    
    // Load single view
    $this->load->view('home', $data);
    
    // Load multiple views (template pattern)
    $this->load->view('templates/header', $data);
    $this->load->view('home', $data);
    $this->load->view('templates/footer');
}
```

---

### 2️⃣ Redirect

```php
<?php
public function logout() {
    $this->session->destroy();
    
    // Redirect ke URL
    redirect('auth/login');
}

public function save() {
    if ($this->user_model->save($data)) {
        // Redirect dengan flash message
        $this->session->set_flashdata('success', 'Data berhasil disimpan!');
        redirect('user');
    } else {
        $this->session->set_flashdata('error', 'Gagal menyimpan data!');
        redirect('user/create');
    }
}

// Redirect variations
redirect('user/profile');                    // Relative
redirect('user/profile', 'refresh');         // With refresh header
redirect('https://google.com', 'location');  // External URL
```

---

### 3️⃣ JSON Response (untuk AJAX/API)

```php
<?php
public function get_user($id) {
    $user = $this->user_model->get_by_id($id);
    
    // Set header JSON
    $this->output
        ->set_content_type('application/json')
        ->set_output(json_encode($user));
}

public function api_response() {
    $response = [
        'status' => 'success',
        'message' => 'Data retrieved successfully',
        'data' => [
            'id' => 1,
            'name' => 'John Doe'
        ]
    ];
    
    $this->output
        ->set_status_header(200)
        ->set_content_type('application/json')
        ->set_output(json_encode($response));
}

// Untuk error response
public function api_error() {
    $response = [
        'status' => 'error',
        'message' => 'User not found'
    ];
    
    $this->output
        ->set_status_header(404)
        ->set_content_type('application/json')
        ->set_output(json_encode($response));
}
```

---

### 4️⃣ Download File

```php
<?php
public function download($filename) {
    $file_path = './uploads/' . $filename;
    
    if (file_exists($file_path)) {
        $this->load->helper('download');
        force_download($file_path, NULL);
    } else {
        show_404();
    }
}

// Download dengan nama custom
public function download_report($id) {
    $report = $this->report_model->get($id);
    
    $this->load->helper('download');
    force_download('Report-' . date('Y-m-d') . '.pdf', $report->content);
}
```

---

## 🔄 Pattern: CRUD Methods

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Pola standar untuk CRUD operations:**

| Method | URL | HTTP | Fungsi |
|--------|-----|------|--------|
| `index()` | `/user` | GET | List semua |
| `show($id)` | `/user/show/5` | GET | Detail satu |
| `create()` | `/user/create` | GET | Form create |
| `store()` | `/user/store` | POST | Simpan baru |
| `edit($id)` | `/user/edit/5` | GET | Form edit |
| `update($id)` | `/user/update/5` | POST | Simpan update |
| `delete($id)` | `/user/delete/5` | POST | Hapus |

</div>

```php
<?php
class User extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->model('user_model');
    }
    
    // GET /user - List all users
    public function index() {
        $data['users'] = $this->user_model->get_all();
        $this->load->view('user/index', $data);
    }
    
    // GET /user/show/5 - Show user detail
    public function show($id) {
        $data['user'] = $this->user_model->get_by_id($id);
        $this->load->view('user/show', $data);
    }
    
    // GET /user/create - Show create form
    public function create() {
        $this->load->view('user/create');
    }
    
    // POST /user/store - Store new user
    public function store() {
        $data = [
            'name' => $this->input->post('name'),
            'email' => $this->input->post('email')
        ];
        
        $this->user_model->create($data);
        redirect('user');
    }
    
    // GET /user/edit/5 - Show edit form
    public function edit($id) {
        $data['user'] = $this->user_model->get_by_id($id);
        $this->load->view('user/edit', $data);
    }
    
    // POST /user/update/5 - Update user
    public function update($id) {
        $data = [
            'name' => $this->input->post('name'),
            'email' => $this->input->post('email')
        ];
        
        $this->user_model->update($id, $data);
        redirect('user');
    }
    
    // POST /user/delete/5 - Delete user
    public function delete($id) {
        $this->user_model->delete($id);
        redirect('user');
    }
}
```

---

## ⚠️ Validasi Parameter

```php
<?php
public function detail($id = NULL) {
    // Validasi parameter ada
    if ($id === NULL) {
        show_404();
    }
    
    // Validasi tipe data
    if (!is_numeric($id)) {
        show_404();
    }
    
    // Validasi data exists
    $product = $this->product_model->get_by_id($id);
    if (!$product) {
        show_404();
    }
    
    // Lanjut proses...
    $data['product'] = $product;
    $this->load->view('product/detail', $data);
}
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Cara membuat method dengan/tanpa parameter
- [ ] Parameter dari URL segment
- [ ] Default value untuk parameter
- [ ] Mengakses GET dan POST data dengan `$this->input`
- [ ] Berbagai jenis response (view, redirect, JSON)
- [ ] Pattern CRUD methods
- [ ] Validasi parameter

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="controller-anatomy.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Controller Anatomy
      </button>
    </a>
  </div>
  <div>
    <a href="routing.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Routing System →
      </button>
    </a>
  </div>
</div>
