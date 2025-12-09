# 🔨 Query Builder

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Menggunakan Query Builder untuk operasi database
- ✅ Membuat query SELECT, INSERT, UPDATE, DELETE
- ✅ Menggunakan WHERE, ORDER BY, LIMIT, JOIN
- ✅ Memahami keamanan Query Builder

---

## 🤔 Apa itu Query Builder?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Query Builder** adalah cara **aman dan mudah** untuk membuat query SQL tanpa menulis SQL mentah.

**Keuntungan:**
- ✅ Otomatis escape data (aman dari SQL Injection)
- ✅ Syntax lebih mudah dibaca
- ✅ Database-agnostic (bisa pindah MySQL ke PostgreSQL)
- ✅ Method chaining yang elegan

</div>

---

## 📊 Perbandingan: Raw SQL vs Query Builder

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Raw SQL (Berbahaya)</h4>

```php
// SQL Injection vulnerability!
$name = $_POST['name'];
$sql = "SELECT * FROM users 
        WHERE name = '$name'";
$this->db->query($sql);
```

Input berbahaya:
```
' OR '1'='1
```
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Query Builder (Aman)</h4>

```php
$name = $this->input->post('name');
$this->db
    ->where('name', $name)
    ->get('users');
```

Data otomatis di-escape!
</div>

</div>

---

## 📖 SELECT Queries

### Get All Data

```php
<?php
// SELECT * FROM users
$query = $this->db->get('users');
$users = $query->result();
```

### Select Kolom Tertentu

```php
<?php
// SELECT id, name, email FROM users
$this->db->select('id, name, email');
$query = $this->db->get('users');

// Atau dengan array
$this->db->select(['id', 'name', 'email']);
```

### Select dengan Alias

```php
<?php
// SELECT name, email AS user_email FROM users
$this->db->select('name, email AS user_email');
```

### Select dengan Function

```php
<?php
// SELECT COUNT(*) as total FROM users
$this->db->select('COUNT(*) as total', FALSE);  // FALSE = jangan escape

// SELECT MAX(price) as max_price FROM products
$this->db->select_max('price', 'max_price');

// SELECT MIN(price) as min_price FROM products
$this->db->select_min('price', 'min_price');

// SELECT AVG(price) as avg_price FROM products
$this->db->select_avg('price', 'avg_price');

// SELECT SUM(quantity) as total_qty FROM order_items
$this->db->select_sum('quantity', 'total_qty');
```

---

## 🔍 WHERE Clauses

### Basic WHERE

```php
<?php
// WHERE status = 'active'
$this->db->where('status', 'active');

// WHERE id = 5
$this->db->where('id', 5);

// Multiple WHERE (AND)
$this->db->where('status', 'active');
$this->db->where('role', 'admin');
// WHERE status = 'active' AND role = 'admin'
```

### WHERE dengan Operator

```php
<?php
// WHERE price > 1000
$this->db->where('price >', 1000);

// WHERE price >= 1000
$this->db->where('price >=', 1000);

// WHERE price < 5000
$this->db->where('price <', 5000);

// WHERE price != 0
$this->db->where('price !=', 0);
```

### WHERE dengan Array

```php
<?php
// Multiple conditions
$where = [
    'status' => 'active',
    'role' => 'user',
    'age >' => 18
];
$this->db->where($where);
// WHERE status = 'active' AND role = 'user' AND age > 18
```

### OR WHERE

```php
<?php
$this->db->where('role', 'admin');
$this->db->or_where('role', 'moderator');
// WHERE role = 'admin' OR role = 'moderator'
```

### WHERE IN

```php
<?php
// WHERE id IN (1, 3, 5, 7)
$ids = [1, 3, 5, 7];
$this->db->where_in('id', $ids);

// WHERE status NOT IN ('deleted', 'banned')
$statuses = ['deleted', 'banned'];
$this->db->where_not_in('status', $statuses);
```

### LIKE (Pencarian)

```php
<?php
// WHERE name LIKE '%john%'
$this->db->like('name', 'john');

// WHERE name LIKE 'john%' (dimulai dengan)
$this->db->like('name', 'john', 'after');

// WHERE name LIKE '%john' (diakhiri dengan)
$this->db->like('name', 'john', 'before');

// OR LIKE
$this->db->like('name', 'john');
$this->db->or_like('email', 'john');

// NOT LIKE
$this->db->not_like('name', 'admin');
```

### WHERE NULL

```php
<?php
// WHERE deleted_at IS NULL
$this->db->where('deleted_at IS NULL');

// WHERE deleted_at IS NOT NULL
$this->db->where('deleted_at IS NOT NULL');
```

---

## 📊 ORDER BY, LIMIT, OFFSET

### ORDER BY

```php
<?php
// ORDER BY created_at DESC
$this->db->order_by('created_at', 'DESC');

// ORDER BY name ASC
$this->db->order_by('name', 'ASC');

// Multiple ORDER BY
$this->db->order_by('status', 'ASC');
$this->db->order_by('created_at', 'DESC');

// Random order
$this->db->order_by('id', 'RANDOM');
```

### LIMIT dan OFFSET

```php
<?php
// LIMIT 10
$this->db->limit(10);

// LIMIT 10 OFFSET 20 (untuk pagination)
$this->db->limit(10, 20);

// Atau dengan method terpisah
$this->db->limit(10);
$this->db->offset(20);
```

### Pagination Pattern

```php
<?php
public function get_paginated($page = 1, $per_page = 10) {
    $offset = ($page - 1) * $per_page;
    
    return $this->db
        ->order_by('created_at', 'DESC')
        ->limit($per_page, $offset)
        ->get('products')
        ->result();
}
```

---

## 🔗 JOIN Tables

### Basic JOIN (INNER JOIN)

```php
<?php
// SELECT * FROM orders
// JOIN users ON users.id = orders.user_id
$this->db->select('orders.*, users.name as user_name');
$this->db->from('orders');
$this->db->join('users', 'users.id = orders.user_id');
$query = $this->db->get();
```

### LEFT JOIN

```php
<?php
// LEFT JOIN - semua data dari tabel kiri
$this->db->select('products.*, categories.name as category_name');
$this->db->from('products');
$this->db->join('categories', 'categories.id = products.category_id', 'left');
```

### Multiple JOINs

```php
<?php
$this->db->select('
    orders.id,
    orders.total,
    users.name as user_name,
    products.name as product_name
');
$this->db->from('orders');
$this->db->join('users', 'users.id = orders.user_id');
$this->db->join('order_items', 'order_items.order_id = orders.id');
$this->db->join('products', 'products.id = order_items.product_id');
```

---

## 📊 GROUP BY dan HAVING

```php
<?php
// GROUP BY category_id
$this->db->select('category_id, COUNT(*) as total');
$this->db->from('products');
$this->db->group_by('category_id');

// HAVING - filter setelah GROUP BY
$this->db->select('category_id, COUNT(*) as total');
$this->db->from('products');
$this->db->group_by('category_id');
$this->db->having('total >', 5);
```

---

## 🔧 Method Chaining

Query Builder mendukung **method chaining** untuk syntax yang lebih clean:

```php
<?php
// Semua dalam satu chain
$products = $this->db
    ->select('products.*, categories.name as category_name')
    ->from('products')
    ->join('categories', 'categories.id = products.category_id', 'left')
    ->where('products.status', 'active')
    ->where('products.price >', 1000)
    ->like('products.name', $keyword)
    ->order_by('products.created_at', 'DESC')
    ->limit(10)
    ->get()
    ->result();
```

---

## 📝 Shortcut Methods

### get_where()

```php
<?php
// Shortcut untuk get dengan where
$query = $this->db->get_where('users', ['id' => 5]);
$user = $query->row();

// Sama dengan:
$this->db->where('id', 5);
$query = $this->db->get('users');
```

### count_all()

```php
<?php
// COUNT(*) semua baris
$total = $this->db->count_all('users');
```

### count_all_results()

```php
<?php
// COUNT(*) dengan kondisi
$active_users = $this->db
    ->where('status', 'active')
    ->count_all_results('users');
```

---

## 🔍 Debugging Query

### Lihat Query yang Dihasilkan

```php
<?php
// Sebelum get()
$this->db->select('*');
$this->db->from('users');
$this->db->where('status', 'active');

// Lihat query string
echo $this->db->get_compiled_select();
// Output: SELECT * FROM users WHERE status = 'active'

// Jalankan query
$query = $this->db->get();
```

### Last Query

```php
<?php
$this->db->get('users');

// Lihat query terakhir yang dijalankan
echo $this->db->last_query();
```

### Profiling

```php
<?php
// Enable profiler di controller
$this->output->enable_profiler(TRUE);
// Akan menampilkan semua query di bawah halaman
```

---

## 📊 Query Builder Summary

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Method | Kegunaan | Contoh |
|--------|----------|--------|
| `select()` | Pilih kolom | `select('name, email')` |
| `from()` | Tentukan tabel | `from('users')` |
| `where()` | Kondisi AND | `where('status', 'active')` |
| `or_where()` | Kondisi OR | `or_where('role', 'admin')` |
| `where_in()` | WHERE IN | `where_in('id', [1,2,3])` |
| `like()` | Pencarian LIKE | `like('name', 'john')` |
| `join()` | Join tabel | `join('categories', 'cat.id = prod.cat_id')` |
| `group_by()` | Group hasil | `group_by('category_id')` |
| `order_by()` | Urutkan | `order_by('created_at', 'DESC')` |
| `limit()` | Batasi hasil | `limit(10, 0)` |
| `get()` | Eksekusi SELECT | `get('users')` |
| `get_where()` | SELECT + WHERE | `get_where('users', ['id' => 5])` |

</div>

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Keuntungan Query Builder vs Raw SQL
- [ ] SELECT dengan berbagai opsi (select, select_max, dll)
- [ ] WHERE clauses (where, or_where, where_in, like)
- [ ] ORDER BY, LIMIT, OFFSET
- [ ] JOIN tables
- [ ] Method chaining
- [ ] Debugging dengan get_compiled_select() dan last_query()

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="model-basics.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Model Fundamentals
      </button>
    </a>
  </div>
  <div>
    <a href="crud-operations.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        CRUD Operations →
      </button>
    </a>
  </div>
</div>
