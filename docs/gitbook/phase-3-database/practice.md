# 💻 Practice Lab: Database & Model

## 🎯 Tujuan

Lab ini akan memandu Anda membuat aplikasi CRUD lengkap dengan database, model, dan relationships.

---

## 📋 Persiapan

Pastikan Anda sudah:
- [ ] MySQL/MariaDB running
- [ ] Database `belajar_ci3` sudah dibuat
- [ ] Konfigurasi database.php sudah benar
- [ ] Database library di-autoload

---

## 🚀 Lab 1: Setup Database

### Langkah 1.1: Buat Database

```sql
-- Jalankan di phpMyAdmin atau MySQL CLI
CREATE DATABASE IF NOT EXISTS belajar_ci3;
USE belajar_ci3;
```

### Langkah 1.2: Buat Tabel Categories

```sql
CREATE TABLE categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100) NOT NULL,
    slug VARCHAR(100) NOT NULL,
    description TEXT,
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT NULL,
    deleted_at DATETIME DEFAULT NULL
);

-- Insert sample data
INSERT INTO categories (name, slug, description) VALUES
('Electronics', 'electronics', 'Electronic devices and gadgets'),
('Fashion', 'fashion', 'Clothing and accessories'),
('Books', 'books', 'Books and publications'),
('Sports', 'sports', 'Sports equipment and gear');
```

### Langkah 1.3: Buat Tabel Products

```sql
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    category_id INT,
    name VARCHAR(200) NOT NULL,
    slug VARCHAR(200) NOT NULL,
    description TEXT,
    price DECIMAL(12,2) NOT NULL DEFAULT 0,
    stock INT NOT NULL DEFAULT 0,
    image VARCHAR(255),
    status ENUM('active', 'inactive') DEFAULT 'active',
    created_at DATETIME DEFAULT CURRENT_TIMESTAMP,
    updated_at DATETIME DEFAULT NULL,
    deleted_at DATETIME DEFAULT NULL,
    FOREIGN KEY (category_id) REFERENCES categories(id)
);

-- Insert sample data
INSERT INTO products (category_id, name, slug, price, stock) VALUES
(1, 'Laptop Gaming ASUS', 'laptop-gaming-asus', 15000000, 10),
(1, 'Wireless Mouse Logitech', 'wireless-mouse-logitech', 350000, 50),
(1, 'Mechanical Keyboard', 'mechanical-keyboard', 1200000, 25),
(2, 'T-Shirt Premium', 't-shirt-premium', 150000, 100),
(2, 'Jeans Slim Fit', 'jeans-slim-fit', 350000, 40),
(3, 'Buku Pemrograman PHP', 'buku-pemrograman-php', 120000, 30),
(4, 'Sepatu Running Nike', 'sepatu-running-nike', 1500000, 20);
```

### Langkah 1.4: Konfigurasi Database

Edit `application/config/database.php`:

```php
<?php
$db['default'] = array(
    'hostname' => 'localhost',
    'username' => 'root',
    'password' => '',  // Sesuaikan dengan password Anda
    'database' => 'belajar_ci3',
    'dbdriver' => 'mysqli',
    'dbprefix' => '',
    'pconnect' => FALSE,
    'db_debug' => (ENVIRONMENT !== 'production'),
    'cache_on' => FALSE,
    'cachedir' => '',
    'char_set' => 'utf8',
    'dbcollat' => 'utf8_general_ci',
    'swap_pre' => '',
    'encrypt'  => FALSE,
    'compress' => FALSE,
    'stricton' => FALSE,
    'failover' => array(),
    'save_queries' => TRUE
);
```

### Langkah 1.5: Autoload Database

Edit `application/config/autoload.php`:

```php
$autoload['libraries'] = array('database', 'session');
```

### ✅ Checkpoint 1

Test koneksi dengan membuat controller sederhana:

```php
<?php
// application/controllers/Test.php

class Test extends CI_Controller {
    public function db() {
        $query = $this->db->query("SELECT * FROM categories");
        echo "<pre>";
        print_r($query->result());
        echo "</pre>";
    }
}
```

Akses: `/test/db` - harus menampilkan list categories.

---

## 📊 Lab 2: Membuat Model

### Langkah 2.1: Buat Category Model

```php
<?php
// application/models/Category_model.php

defined('BASEPATH') OR exit('No direct script access allowed');

class Category_model extends CI_Model {
    
    protected $table = 'categories';
    
    public function __construct() {
        parent::__construct();
    }
    
    // Get all categories
    public function get_all() {
        return $this->db
            ->where('deleted_at IS NULL')
            ->order_by('name', 'ASC')
            ->get($this->table)
            ->result();
    }
    
    // Get single category by ID
    public function get_by_id($id) {
        return $this->db
            ->where('id', $id)
            ->where('deleted_at IS NULL')
            ->get($this->table)
            ->row();
    }
    
    // Get category by slug
    public function get_by_slug($slug) {
        return $this->db
            ->where('slug', $slug)
            ->where('deleted_at IS NULL')
            ->get($this->table)
            ->row();
    }
    
    // Count all
    public function count_all() {
        return $this->db
            ->where('deleted_at IS NULL')
            ->count_all_results($this->table);
    }
    
    // Create
    public function create($data) {
        $data['created_at'] = date('Y-m-d H:i:s');
        $this->db->insert($this->table, $data);
        return $this->db->insert_id();
    }
    
    // Update
    public function update($id, $data) {
        $data['updated_at'] = date('Y-m-d H:i:s');
        return $this->db
            ->where('id', $id)
            ->update($this->table, $data);
    }
    
    // Soft delete
    public function delete($id) {
        return $this->db
            ->where('id', $id)
            ->update($this->table, ['deleted_at' => date('Y-m-d H:i:s')]);
    }
    
    // Get with product count
    public function get_with_product_count() {
        return $this->db
            ->select('categories.*, COUNT(products.id) as product_count')
            ->from('categories')
            ->join('products', 'products.category_id = categories.id AND products.deleted_at IS NULL', 'left')
            ->where('categories.deleted_at IS NULL')
            ->group_by('categories.id')
            ->order_by('categories.name', 'ASC')
            ->get()
            ->result();
    }
}
```

### Langkah 2.2: Buat Product Model

```php
<?php
// application/models/Product_model.php

defined('BASEPATH') OR exit('No direct script access allowed');

class Product_model extends CI_Model {
    
    protected $table = 'products';
    
    public function __construct() {
        parent::__construct();
    }
    
    // Get all with category
    public function get_all() {
        return $this->db
            ->select('products.*, categories.name as category_name')
            ->from('products')
            ->join('categories', 'categories.id = products.category_id', 'left')
            ->where('products.deleted_at IS NULL')
            ->order_by('products.created_at', 'DESC')
            ->get()
            ->result();
    }
    
    // Get by ID with category
    public function get_by_id($id) {
        return $this->db
            ->select('products.*, categories.name as category_name')
            ->from('products')
            ->join('categories', 'categories.id = products.category_id', 'left')
            ->where('products.id', $id)
            ->where('products.deleted_at IS NULL')
            ->get()
            ->row();
    }
    
    // Get by category
    public function get_by_category($category_id) {
        return $this->db
            ->where('category_id', $category_id)
            ->where('deleted_at IS NULL')
            ->order_by('name', 'ASC')
            ->get($this->table)
            ->result();
    }
    
    // Search products
    public function search($keyword) {
        return $this->db
            ->select('products.*, categories.name as category_name')
            ->from('products')
            ->join('categories', 'categories.id = products.category_id', 'left')
            ->where('products.deleted_at IS NULL')
            ->group_start()
                ->like('products.name', $keyword)
                ->or_like('products.description', $keyword)
            ->group_end()
            ->get()
            ->result();
    }
    
    // Count all
    public function count_all() {
        return $this->db
            ->where('deleted_at IS NULL')
            ->count_all_results($this->table);
    }
    
    // Create
    public function create($data) {
        $data['created_at'] = date('Y-m-d H:i:s');
        $data['slug'] = url_title($data['name'], 'dash', TRUE);
        $this->db->insert($this->table, $data);
        return $this->db->insert_id();
    }
    
    // Update
    public function update($id, $data) {
        $data['updated_at'] = date('Y-m-d H:i:s');
        if (isset($data['name'])) {
            $data['slug'] = url_title($data['name'], 'dash', TRUE);
        }
        return $this->db
            ->where('id', $id)
            ->update($this->table, $data);
    }
    
    // Soft delete
    public function delete($id) {
        return $this->db
            ->where('id', $id)
            ->update($this->table, ['deleted_at' => date('Y-m-d H:i:s')]);
    }
    
    // Update stock
    public function update_stock($id, $quantity) {
        $this->db->set('stock', 'stock + (' . (int)$quantity . ')', FALSE);
        $this->db->where('id', $id);
        return $this->db->update($this->table);
    }
}
```

### ✅ Checkpoint 2

Test model:

```php
<?php
// Di Test controller
public function model() {
    $this->load->model('product_model');
    
    $products = $this->product_model->get_all();
    echo "<h2>All Products</h2>";
    echo "<pre>";
    print_r($products);
    echo "</pre>";
}
```

---

## 🎮 Lab 3: CRUD Controller

### Langkah 3.1: Buat Product Controller

```php
<?php
// application/controllers/Product.php

defined('BASEPATH') OR exit('No direct script access allowed');

class Product extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->model('product_model');
        $this->load->model('category_model');
        $this->load->helper(['url', 'form']);
        $this->load->library('form_validation');
    }
    
    // List all products
    public function index() {
        $data['title'] = 'Products';
        $data['products'] = $this->product_model->get_all();
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/index', $data);
        $this->load->view('templates/footer');
    }
    
    // Show single product
    public function show($id) {
        $product = $this->product_model->get_by_id($id);
        
        if (!$product) {
            show_404();
        }
        
        $data['title'] = $product->name;
        $data['product'] = $product;
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/show', $data);
        $this->load->view('templates/footer');
    }
    
    // Create form
    public function create() {
        $data['title'] = 'Add Product';
        $data['categories'] = $this->category_model->get_all();
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/create', $data);
        $this->load->view('templates/footer');
    }
    
    // Store new product
    public function store() {
        // Validation rules
        $this->form_validation->set_rules('name', 'Name', 'required|min_length[3]');
        $this->form_validation->set_rules('category_id', 'Category', 'required');
        $this->form_validation->set_rules('price', 'Price', 'required|numeric');
        $this->form_validation->set_rules('stock', 'Stock', 'required|integer');
        
        if ($this->form_validation->run() === FALSE) {
            $this->create();
            return;
        }
        
        // Insert data
        $product_id = $this->product_model->create([
            'name' => $this->input->post('name'),
            'category_id' => $this->input->post('category_id'),
            'description' => $this->input->post('description'),
            'price' => $this->input->post('price'),
            'stock' => $this->input->post('stock'),
            'status' => $this->input->post('status') ?: 'active'
        ]);
        
        $this->session->set_flashdata('success', 'Product created successfully!');
        redirect('product');
    }
    
    // Edit form
    public function edit($id) {
        $product = $this->product_model->get_by_id($id);
        
        if (!$product) {
            show_404();
        }
        
        $data['title'] = 'Edit Product';
        $data['product'] = $product;
        $data['categories'] = $this->category_model->get_all();
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/edit', $data);
        $this->load->view('templates/footer');
    }
    
    // Update product
    public function update($id) {
        $product = $this->product_model->get_by_id($id);
        
        if (!$product) {
            show_404();
        }
        
        // Validation
        $this->form_validation->set_rules('name', 'Name', 'required|min_length[3]');
        $this->form_validation->set_rules('category_id', 'Category', 'required');
        $this->form_validation->set_rules('price', 'Price', 'required|numeric');
        $this->form_validation->set_rules('stock', 'Stock', 'required|integer');
        
        if ($this->form_validation->run() === FALSE) {
            $this->edit($id);
            return;
        }
        
        // Update
        $this->product_model->update($id, [
            'name' => $this->input->post('name'),
            'category_id' => $this->input->post('category_id'),
            'description' => $this->input->post('description'),
            'price' => $this->input->post('price'),
            'stock' => $this->input->post('stock'),
            'status' => $this->input->post('status')
        ]);
        
        $this->session->set_flashdata('success', 'Product updated successfully!');
        redirect('product');
    }
    
    // Delete product
    public function delete($id) {
        $product = $this->product_model->get_by_id($id);
        
        if (!$product) {
            show_404();
        }
        
        $this->product_model->delete($id);
        
        $this->session->set_flashdata('success', 'Product deleted successfully!');
        redirect('product');
    }
    
    // Search
    public function search() {
        $keyword = $this->input->get('q');
        
        $data['title'] = 'Search: ' . $keyword;
        $data['keyword'] = $keyword;
        $data['products'] = $this->product_model->search($keyword);
        
        $this->load->view('templates/header', $data);
        $this->load->view('product/index', $data);
        $this->load->view('templates/footer');
    }
}
```

### ✅ Checkpoint 3

Controller siap digunakan.

---

## 👁️ Lab 4: Views

### Langkah 4.1: Product List View

```php
<!-- application/views/product/index.php -->
<div class="page-header">
    <h1>📦 <?= $title ?></h1>
    <a href="<?= base_url('product/create') ?>" class="btn btn-success">+ Add Product</a>
</div>

<!-- Search -->
<form action="<?= base_url('product/search') ?>" method="GET" style="margin-bottom: 20px;">
    <input type="text" name="q" placeholder="Search products..." value="<?= $keyword ?? '' ?>" style="padding: 10px; width: 300px;">
    <button type="submit" class="btn">Search</button>
</form>

<?php if (!empty($products)): ?>
    <table style="width: 100%; border-collapse: collapse;">
        <thead>
            <tr style="background: #f5f5f5;">
                <th style="padding: 10px; text-align: left;">Name</th>
                <th style="padding: 10px; text-align: left;">Category</th>
                <th style="padding: 10px; text-align: right;">Price</th>
                <th style="padding: 10px; text-align: center;">Stock</th>
                <th style="padding: 10px; text-align: center;">Status</th>
                <th style="padding: 10px; text-align: center;">Actions</th>
            </tr>
        </thead>
        <tbody>
            <?php foreach ($products as $product): ?>
                <tr style="border-bottom: 1px solid #ddd;">
                    <td style="padding: 10px;">
                        <a href="<?= base_url('product/show/' . $product->id) ?>">
                            <?= $product->name ?>
                        </a>
                    </td>
                    <td style="padding: 10px;"><?= $product->category_name ?></td>
                    <td style="padding: 10px; text-align: right;">Rp <?= number_format($product->price) ?></td>
                    <td style="padding: 10px; text-align: center;"><?= $product->stock ?></td>
                    <td style="padding: 10px; text-align: center;">
                        <span class="badge <?= $product->status == 'active' ? 'badge-success' : 'badge-secondary' ?>">
                            <?= $product->status ?>
                        </span>
                    </td>
                    <td style="padding: 10px; text-align: center;">
                        <a href="<?= base_url('product/edit/' . $product->id) ?>" class="btn btn-sm">Edit</a>
                        <a href="<?= base_url('product/delete/' . $product->id) ?>" 
                           class="btn btn-sm btn-danger"
                           onclick="return confirm('Are you sure?')">Delete</a>
                    </td>
                </tr>
            <?php endforeach; ?>
        </tbody>
    </table>
<?php else: ?>
    <div class="empty-state">
        <h3>No products found</h3>
        <p>Start by adding your first product.</p>
        <a href="<?= base_url('product/create') ?>" class="btn btn-success">+ Add Product</a>
    </div>
<?php endif; ?>
```

### Langkah 4.2: Create Form View

```php
<!-- application/views/product/create.php -->
<h1><?= $title ?></h1>

<div class="card">
    <form action="<?= base_url('product/store') ?>" method="POST">
        
        <div class="form-group">
            <label>Name *</label>
            <input type="text" name="name" value="<?= set_value('name') ?>" class="form-control" required>
            <?= form_error('name', '<small class="text-danger">', '</small>') ?>
        </div>
        
        <div class="form-group">
            <label>Category *</label>
            <select name="category_id" class="form-control" required>
                <option value="">-- Select Category --</option>
                <?php foreach ($categories as $cat): ?>
                    <option value="<?= $cat->id ?>" <?= set_select('category_id', $cat->id) ?>>
                        <?= $cat->name ?>
                    </option>
                <?php endforeach; ?>
            </select>
            <?= form_error('category_id', '<small class="text-danger">', '</small>') ?>
        </div>
        
        <div class="form-group">
            <label>Description</label>
            <textarea name="description" class="form-control" rows="4"><?= set_value('description') ?></textarea>
        </div>
        
        <div class="form-group">
            <label>Price *</label>
            <input type="number" name="price" value="<?= set_value('price') ?>" class="form-control" required>
            <?= form_error('price', '<small class="text-danger">', '</small>') ?>
        </div>
        
        <div class="form-group">
            <label>Stock *</label>
            <input type="number" name="stock" value="<?= set_value('stock', 0) ?>" class="form-control" required>
            <?= form_error('stock', '<small class="text-danger">', '</small>') ?>
        </div>
        
        <div class="form-group">
            <label>Status</label>
            <select name="status" class="form-control">
                <option value="active" <?= set_select('status', 'active', TRUE) ?>>Active</option>
                <option value="inactive" <?= set_select('status', 'inactive') ?>>Inactive</option>
            </select>
        </div>
        
        <div class="form-group">
            <button type="submit" class="btn btn-success">Save Product</button>
            <a href="<?= base_url('product') ?>" class="btn">Cancel</a>
        </div>
        
    </form>
</div>
```

### Langkah 4.3: Edit Form View

```php
<!-- application/views/product/edit.php -->
<h1><?= $title ?></h1>

<div class="card">
    <form action="<?= base_url('product/update/' . $product->id) ?>" method="POST">
        
        <div class="form-group">
            <label>Name *</label>
            <input type="text" name="name" value="<?= set_value('name', $product->name) ?>" class="form-control" required>
            <?= form_error('name', '<small class="text-danger">', '</small>') ?>
        </div>
        
        <div class="form-group">
            <label>Category *</label>
            <select name="category_id" class="form-control" required>
                <option value="">-- Select Category --</option>
                <?php foreach ($categories as $cat): ?>
                    <option value="<?= $cat->id ?>" <?= set_select('category_id', $cat->id, $cat->id == $product->category_id) ?>>
                        <?= $cat->name ?>
                    </option>
                <?php endforeach; ?>
            </select>
        </div>
        
        <div class="form-group">
            <label>Description</label>
            <textarea name="description" class="form-control" rows="4"><?= set_value('description', $product->description) ?></textarea>
        </div>
        
        <div class="form-group">
            <label>Price *</label>
            <input type="number" name="price" value="<?= set_value('price', $product->price) ?>" class="form-control" required>
        </div>
        
        <div class="form-group">
            <label>Stock *</label>
            <input type="number" name="stock" value="<?= set_value('stock', $product->stock) ?>" class="form-control" required>
        </div>
        
        <div class="form-group">
            <label>Status</label>
            <select name="status" class="form-control">
                <option value="active" <?= $product->status == 'active' ? 'selected' : '' ?>>Active</option>
                <option value="inactive" <?= $product->status == 'inactive' ? 'selected' : '' ?>>Inactive</option>
            </select>
        </div>
        
        <div class="form-group">
            <button type="submit" class="btn btn-success">Update Product</button>
            <a href="<?= base_url('product') ?>" class="btn">Cancel</a>
        </div>
        
    </form>
</div>
```

### ✅ Checkpoint 4

Test CRUD lengkap:
- [ ] List products
- [ ] Create new product
- [ ] Edit product
- [ ] Delete product
- [ ] Search products

---

## 🏆 Challenge

<details>
<summary>💪 Challenge 1: Tambahkan Pagination</summary>

Implementasikan pagination untuk product list dengan 10 items per page.

</details>

<details>
<summary>💪 Challenge 2: CRUD Categories</summary>

Buat CRUD lengkap untuk categories dengan validasi nama unik.

</details>

<details>
<summary>💪 Challenge 3: Product dengan Image Upload</summary>

Tambahkan fitur upload gambar product.

</details>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="transactions.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Transactions
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
