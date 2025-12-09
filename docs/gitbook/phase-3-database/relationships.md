# 🔗 Database Relationships

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami jenis-jenis relasi database
- ✅ Mengimplementasikan One-to-Many
- ✅ Mengimplementasikan Many-to-Many
- ✅ Menggunakan JOIN untuk mengambil data berelasi

---

## 🤔 Apa itu Database Relationships?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Database Relationships** adalah cara menghubungkan data antar tabel menggunakan **foreign key**.

**Analogi:** Seperti **kartu keluarga** 👨‍👩‍👧‍👦
- 1 Kepala Keluarga → Punya banyak Anggota (One-to-Many)
- 1 Siswa → Bisa ikut banyak Ekstrakurikuler, dan sebaliknya (Many-to-Many)

</div>

---

## 📊 Jenis-jenis Relasi

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; text-align: center;">
<h3>One-to-One</h3>
<p>1 User → 1 Profile</p>
<pre>
User ─── Profile
</pre>
</div>

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; text-align: center;">
<h3>One-to-Many</h3>
<p>1 Category → Many Products</p>
<pre>
Category ─┬─ Product
          ├─ Product
          └─ Product
</pre>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; text-align: center;">
<h3>Many-to-Many</h3>
<p>Products ↔ Tags</p>
<pre>
Product ─┬─ Tag
Product ─┼─ Tag
Product ─┴─ Tag
</pre>
</div>

</div>

---

## 1️⃣ One-to-One

### Schema

```sql
-- users table
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,
    email VARCHAR(100),
    password VARCHAR(255)
);

-- profiles table (1 user = 1 profile)
CREATE TABLE profiles (
    id INT PRIMARY KEY AUTO_INCREMENT,
    user_id INT UNIQUE,  -- UNIQUE ensures 1-to-1
    full_name VARCHAR(100),
    phone VARCHAR(20),
    address TEXT,
    FOREIGN KEY (user_id) REFERENCES users(id)
);
```

### Model Implementation

```php
<?php
// User_model.php

public function get_with_profile($id) {
    return $this->db
        ->select('users.*, profiles.full_name, profiles.phone, profiles.address')
        ->from('users')
        ->join('profiles', 'profiles.user_id = users.id', 'left')
        ->where('users.id', $id)
        ->get()
        ->row();
}
```

---

## 2️⃣ One-to-Many (Paling Umum)

### Schema

```sql
-- categories table (One)
CREATE TABLE categories (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(100),
    slug VARCHAR(100)
);

-- products table (Many)
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    category_id INT,  -- Foreign Key
    name VARCHAR(200),
    price DECIMAL(10,2),
    FOREIGN KEY (category_id) REFERENCES categories(id)
);
```

### Visualisasi

```
categories                 products
+----+-----------+         +----+-------------+-------+--------+
| id | name      |         | id | category_id | name  | price  |
+----+-----------+         +----+-------------+-------+--------+
| 1  | Electronics| ───────>| 1  | 1           | Laptop| 15000  |
|    |           | ───────>| 2  | 1           | Mouse | 250    |
+----+-----------+         +----+-------------+-------+--------+
| 2  | Fashion   | ───────>| 3  | 2           | Shirt | 150    |
+----+-----------+         +----+-------------+-------+--------+
```

### Model: Category (Parent)

```php
<?php
// Category_model.php

class Category_model extends CI_Model {
    
    protected $table = 'categories';
    
    public function get_all() {
        return $this->db->get($this->table)->result();
    }
    
    public function get_by_id($id) {
        return $this->db
            ->get_where($this->table, ['id' => $id])
            ->row();
    }
    
    /**
     * Get category dengan semua products-nya
     */
    public function get_with_products($id) {
        $category = $this->get_by_id($id);
        
        if ($category) {
            $category->products = $this->db
                ->where('category_id', $id)
                ->get('products')
                ->result();
        }
        
        return $category;
    }
    
    /**
     * Get all categories dengan jumlah products
     */
    public function get_with_product_count() {
        return $this->db
            ->select('categories.*, COUNT(products.id) as product_count')
            ->from('categories')
            ->join('products', 'products.category_id = categories.id', 'left')
            ->group_by('categories.id')
            ->get()
            ->result();
    }
}
```

### Model: Product (Child)

```php
<?php
// Product_model.php

class Product_model extends CI_Model {
    
    protected $table = 'products';
    
    /**
     * Get all products dengan nama category
     */
    public function get_all_with_category() {
        return $this->db
            ->select('products.*, categories.name as category_name')
            ->from('products')
            ->join('categories', 'categories.id = products.category_id', 'left')
            ->order_by('products.created_at', 'DESC')
            ->get()
            ->result();
    }
    
    /**
     * Get product dengan detail category
     */
    public function get_with_category($id) {
        return $this->db
            ->select('products.*, categories.name as category_name, categories.slug as category_slug')
            ->from('products')
            ->join('categories', 'categories.id = products.category_id', 'left')
            ->where('products.id', $id)
            ->get()
            ->row();
    }
    
    /**
     * Get products by category
     */
    public function get_by_category($category_id) {
        return $this->db
            ->where('category_id', $category_id)
            ->order_by('name', 'ASC')
            ->get($this->table)
            ->result();
    }
}
```

### Penggunaan di Controller

```php
<?php
class Product extends CI_Controller {
    
    public function index() {
        $data['products'] = $this->product_model->get_all_with_category();
        $this->load->view('product/index', $data);
    }
    
    public function detail($id) {
        $data['product'] = $this->product_model->get_with_category($id);
        $this->load->view('product/detail', $data);
    }
    
    public function category($category_id) {
        $data['category'] = $this->category_model->get_with_products($category_id);
        $this->load->view('product/category', $data);
    }
}
```

### View

```php
<!-- product/index.php -->
<?php foreach ($products as $product): ?>
    <div class="product">
        <h3><?= $product->name ?></h3>
        <p>Category: <?= $product->category_name ?></p>
        <p>Price: Rp <?= number_format($product->price) ?></p>
    </div>
<?php endforeach; ?>

<!-- product/category.php -->
<h1><?= $category->name ?></h1>
<p>Total: <?= count($category->products) ?> products</p>

<?php foreach ($category->products as $product): ?>
    <div class="product">
        <h3><?= $product->name ?></h3>
    </div>
<?php endforeach; ?>
```

---

## 3️⃣ Many-to-Many

### Schema

```sql
-- products table
CREATE TABLE products (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(200),
    price DECIMAL(10,2)
);

-- tags table
CREATE TABLE tags (
    id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(50),
    slug VARCHAR(50)
);

-- Pivot table (penghubung)
CREATE TABLE product_tags (
    id INT PRIMARY KEY AUTO_INCREMENT,
    product_id INT,
    tag_id INT,
    FOREIGN KEY (product_id) REFERENCES products(id) ON DELETE CASCADE,
    FOREIGN KEY (tag_id) REFERENCES tags(id) ON DELETE CASCADE,
    UNIQUE KEY unique_product_tag (product_id, tag_id)
);
```

### Visualisasi

```
products          product_tags         tags
+----+--------+   +------------+-----+   +----+--------+
| id | name   |   | product_id |tag_id   | id | name   |
+----+--------+   +------------+-----+   +----+--------+
| 1  | Laptop |──>| 1          | 1   |<──| 1  | Sale   |
|    |        |──>| 1          | 2   |<──| 2  | New    |
+----+--------+   +------------+-----+   +----+--------+
| 2  | Mouse  |──>| 2          | 1   |   | 3  | Popular|
|    |        |──>| 2          | 3   |<──|    |        |
+----+--------+   +------------+-----+   +----+--------+
```

### Model: Product

```php
<?php
// Product_model.php

/**
 * Get product dengan semua tags-nya
 */
public function get_with_tags($id) {
    $product = $this->get_by_id($id);
    
    if ($product) {
        $product->tags = $this->db
            ->select('tags.*')
            ->from('tags')
            ->join('product_tags', 'product_tags.tag_id = tags.id')
            ->where('product_tags.product_id', $id)
            ->get()
            ->result();
    }
    
    return $product;
}

/**
 * Get all products dengan tags
 */
public function get_all_with_tags() {
    $products = $this->get_all();
    
    foreach ($products as &$product) {
        $product->tags = $this->db
            ->select('tags.*')
            ->from('tags')
            ->join('product_tags', 'product_tags.tag_id = tags.id')
            ->where('product_tags.product_id', $product->id)
            ->get()
            ->result();
    }
    
    return $products;
}

/**
 * Attach tags ke product
 */
public function attach_tags($product_id, $tag_ids) {
    // Hapus existing tags
    $this->db->where('product_id', $product_id)->delete('product_tags');
    
    // Insert new tags
    if (!empty($tag_ids)) {
        $data = [];
        foreach ($tag_ids as $tag_id) {
            $data[] = [
                'product_id' => $product_id,
                'tag_id' => $tag_id
            ];
        }
        $this->db->insert_batch('product_tags', $data);
    }
}

/**
 * Detach specific tag dari product
 */
public function detach_tag($product_id, $tag_id) {
    return $this->db
        ->where('product_id', $product_id)
        ->where('tag_id', $tag_id)
        ->delete('product_tags');
}
```

### Model: Tag

```php
<?php
// Tag_model.php

/**
 * Get tag dengan semua products-nya
 */
public function get_with_products($tag_id) {
    $tag = $this->get_by_id($tag_id);
    
    if ($tag) {
        $tag->products = $this->db
            ->select('products.*')
            ->from('products')
            ->join('product_tags', 'product_tags.product_id = products.id')
            ->where('product_tags.tag_id', $tag_id)
            ->get()
            ->result();
    }
    
    return $tag;
}

/**
 * Get all tags dengan jumlah products
 */
public function get_with_product_count() {
    return $this->db
        ->select('tags.*, COUNT(product_tags.product_id) as product_count')
        ->from('tags')
        ->join('product_tags', 'product_tags.tag_id = tags.id', 'left')
        ->group_by('tags.id')
        ->get()
        ->result();
}
```

### Penggunaan

```php
<?php
// Controller
public function create() {
    // Simpan product
    $product_id = $this->product_model->create([
        'name' => $this->input->post('name'),
        'price' => $this->input->post('price')
    ]);
    
    // Attach tags
    $tag_ids = $this->input->post('tags');  // Array dari form checkbox
    $this->product_model->attach_tags($product_id, $tag_ids);
}

// View - Form dengan checkboxes
<?php foreach ($tags as $tag): ?>
    <label>
        <input type="checkbox" name="tags[]" value="<?= $tag->id ?>">
        <?= $tag->name ?>
    </label>
<?php endforeach; ?>
```

---

## 🔧 Eager Loading Pattern

Untuk menghindari N+1 query problem:

```php
<?php
// ❌ N+1 Problem - banyak query
public function get_all_with_category_bad() {
    $products = $this->db->get('products')->result();
    
    foreach ($products as $product) {
        // Query untuk setiap product! (N queries)
        $product->category = $this->db
            ->get_where('categories', ['id' => $product->category_id])
            ->row();
    }
    
    return $products;
}

// ✅ Eager Loading - 1 query dengan JOIN
public function get_all_with_category_good() {
    return $this->db
        ->select('products.*, categories.name as category_name')
        ->from('products')
        ->join('categories', 'categories.id = products.category_id', 'left')
        ->get()
        ->result();
}
```

---

## 📊 Summary Relationships

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Relasi | Contoh | Foreign Key |
|--------|--------|-------------|
| **One-to-One** | User → Profile | profile.user_id (UNIQUE) |
| **One-to-Many** | Category → Products | product.category_id |
| **Many-to-Many** | Products ↔ Tags | Pivot table: product_tags |

</div>

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Perbedaan One-to-One, One-to-Many, Many-to-Many
- [ ] Cara membuat foreign key
- [ ] JOIN untuk mengambil data berelasi
- [ ] Pivot table untuk Many-to-Many
- [ ] Eager loading untuk menghindari N+1 problem

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="crud-operations.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← CRUD Operations
      </button>
    </a>
  </div>
  <div>
    <a href="transactions.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Transactions →
      </button>
    </a>
  </div>
</div>
