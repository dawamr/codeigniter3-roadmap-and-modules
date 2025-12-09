# 💼 CRUD Operations

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Melakukan operasi Create (INSERT)
- ✅ Melakukan operasi Read (SELECT)
- ✅ Melakukan operasi Update (UPDATE)
- ✅ Melakukan operasi Delete (DELETE)
- ✅ Membuat CRUD lengkap dengan Model

---

## 🤔 Apa itu CRUD?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**CRUD** adalah 4 operasi dasar dalam pengelolaan data:

| Letter | Operation | SQL | Deskripsi |
|--------|-----------|-----|-----------|
| **C** | Create | INSERT | Membuat data baru |
| **R** | Read | SELECT | Membaca/mengambil data |
| **U** | Update | UPDATE | Mengubah data yang ada |
| **D** | Delete | DELETE | Menghapus data |

Hampir semua aplikasi membutuhkan CRUD: user management, product catalog, blog posts, dll.

</div>

---

## ➕ CREATE (INSERT)

### Insert Single Row

```php
<?php
// Cara 1: Array data
$data = [
    'name' => 'John Doe',
    'email' => 'john@example.com',
    'password' => password_hash('secret', PASSWORD_DEFAULT),
    'created_at' => date('Y-m-d H:i:s')
];

$this->db->insert('users', $data);

// Cara 2: Set individual fields
$this->db->set('name', 'John Doe');
$this->db->set('email', 'john@example.com');
$this->db->set('created_at', date('Y-m-d H:i:s'));
$this->db->insert('users');
```

### Get Insert ID

```php
<?php
$data = [
    'name' => 'John Doe',
    'email' => 'john@example.com'
];

$this->db->insert('users', $data);

// Dapatkan ID yang baru saja di-insert
$new_id = $this->db->insert_id();
echo "User baru dengan ID: " . $new_id;
```

### Insert Batch (Multiple Rows)

```php
<?php
$data = [
    [
        'name' => 'John Doe',
        'email' => 'john@example.com'
    ],
    [
        'name' => 'Jane Doe',
        'email' => 'jane@example.com'
    ],
    [
        'name' => 'Bob Smith',
        'email' => 'bob@example.com'
    ]
];

// Insert semua sekaligus (lebih efisien!)
$this->db->insert_batch('users', $data);

// Dapatkan jumlah rows yang ter-insert
$affected = $this->db->affected_rows();
echo "Inserted: " . $affected . " rows";
```

### Model Method untuk Create

```php
<?php
// User_model.php
public function create($data) {
    // Tambah timestamp
    $data['created_at'] = date('Y-m-d H:i:s');
    
    // Hash password jika ada
    if (isset($data['password'])) {
        $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
    }
    
    $this->db->insert('users', $data);
    return $this->db->insert_id();
}

// Penggunaan di Controller
$user_id = $this->user_model->create([
    'name' => $this->input->post('name'),
    'email' => $this->input->post('email'),
    'password' => $this->input->post('password')
]);
```

---

## 📖 READ (SELECT)

### Get All Records

```php
<?php
// SELECT * FROM users
$users = $this->db->get('users')->result();
```

### Get Single Record

```php
<?php
// SELECT * FROM users WHERE id = 5
$user = $this->db->get_where('users', ['id' => 5])->row();

// Atau
$user = $this->db
    ->where('id', 5)
    ->get('users')
    ->row();
```

### Get dengan Kondisi

```php
<?php
// Users yang aktif, diurutkan berdasarkan nama
$users = $this->db
    ->where('status', 'active')
    ->order_by('name', 'ASC')
    ->get('users')
    ->result();
```

### Get dengan Pagination

```php
<?php
$page = 2;
$per_page = 10;
$offset = ($page - 1) * $per_page;

$users = $this->db
    ->order_by('created_at', 'DESC')
    ->limit($per_page, $offset)
    ->get('users')
    ->result();

$total = $this->db->count_all('users');
```

### Model Methods untuk Read

```php
<?php
// User_model.php

public function get_all() {
    return $this->db
        ->order_by('created_at', 'DESC')
        ->get('users')
        ->result();
}

public function get_by_id($id) {
    return $this->db
        ->get_where('users', ['id' => $id])
        ->row();
}

public function get_by_email($email) {
    return $this->db
        ->get_where('users', ['email' => $email])
        ->row();
}

public function get_active() {
    return $this->db
        ->where('status', 'active')
        ->where('deleted_at IS NULL')
        ->order_by('name', 'ASC')
        ->get('users')
        ->result();
}

public function search($keyword) {
    return $this->db
        ->like('name', $keyword)
        ->or_like('email', $keyword)
        ->get('users')
        ->result();
}

public function get_paginated($page = 1, $per_page = 10) {
    $offset = ($page - 1) * $per_page;
    
    $data['users'] = $this->db
        ->order_by('created_at', 'DESC')
        ->limit($per_page, $offset)
        ->get('users')
        ->result();
    
    $data['total'] = $this->db->count_all('users');
    $data['pages'] = ceil($data['total'] / $per_page);
    
    return $data;
}
```

---

## ✏️ UPDATE

### Update Single Record

```php
<?php
$data = [
    'name' => 'John Updated',
    'email' => 'john.updated@example.com',
    'updated_at' => date('Y-m-d H:i:s')
];

// UPDATE users SET ... WHERE id = 5
$this->db->where('id', 5);
$this->db->update('users', $data);

// Atau dalam satu baris
$this->db->where('id', 5)->update('users', $data);
```

### Update dengan Set

```php
<?php
$this->db->set('name', 'New Name');
$this->db->set('updated_at', date('Y-m-d H:i:s'));
$this->db->where('id', 5);
$this->db->update('users');
```

### Update dengan Increment/Decrement

```php
<?php
// Increment view count
$this->db->set('views', 'views + 1', FALSE);  // FALSE = jangan escape
$this->db->where('id', $article_id);
$this->db->update('articles');

// Decrement stock
$this->db->set('stock', 'stock - ' . $quantity, FALSE);
$this->db->where('id', $product_id);
$this->db->update('products');
```

### Update Batch

```php
<?php
$data = [
    [
        'id' => 1,
        'name' => 'Name 1 Updated'
    ],
    [
        'id' => 2,
        'name' => 'Name 2 Updated'
    ],
    [
        'id' => 3,
        'name' => 'Name 3 Updated'
    ]
];

// Update berdasarkan 'id'
$this->db->update_batch('users', $data, 'id');
```

### Model Method untuk Update

```php
<?php
// User_model.php

public function update($id, $data) {
    // Tambah timestamp
    $data['updated_at'] = date('Y-m-d H:i:s');
    
    // Hash password jika diupdate
    if (isset($data['password']) && !empty($data['password'])) {
        $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
    } else {
        unset($data['password']);  // Jangan update jika kosong
    }
    
    return $this->db
        ->where('id', $id)
        ->update('users', $data);
}

// Penggunaan
$this->user_model->update(5, [
    'name' => 'Updated Name',
    'email' => 'updated@example.com'
]);
```

---

## 🗑️ DELETE

### Delete Single Record

```php
<?php
// DELETE FROM users WHERE id = 5
$this->db->where('id', 5);
$this->db->delete('users');

// Atau shorthand
$this->db->delete('users', ['id' => 5]);
```

### Delete Multiple Records

```php
<?php
// DELETE FROM users WHERE status = 'inactive'
$this->db->where('status', 'inactive');
$this->db->delete('users');

// DELETE dengan IN
$this->db->where_in('id', [1, 3, 5, 7]);
$this->db->delete('users');
```

### Soft Delete (Recommended)

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Soft Delete** = Tidak benar-benar menghapus, hanya menandai sebagai deleted.

**Keuntungan:**
- ✅ Data bisa di-restore
- ✅ Audit trail tetap ada
- ✅ Referensi dari tabel lain tidak rusak

</div>

```php
<?php
// Soft delete - set deleted_at timestamp
public function soft_delete($id) {
    return $this->db
        ->where('id', $id)
        ->update('users', [
            'deleted_at' => date('Y-m-d H:i:s')
        ]);
}

// Get hanya yang tidak di-delete
public function get_all() {
    return $this->db
        ->where('deleted_at IS NULL')
        ->get('users')
        ->result();
}

// Get termasuk yang di-delete
public function get_all_with_trashed() {
    return $this->db->get('users')->result();
}

// Restore
public function restore($id) {
    return $this->db
        ->where('id', $id)
        ->update('users', [
            'deleted_at' => NULL
        ]);
}

// Permanent delete
public function force_delete($id) {
    return $this->db
        ->where('id', $id)
        ->delete('users');
}
```

---

## 📝 Complete CRUD Model

```php
<?php
// application/models/User_model.php

defined('BASEPATH') OR exit('No direct script access allowed');

class User_model extends CI_Model {
    
    protected $table = 'users';
    
    // ==================== CREATE ====================
    
    public function create($data) {
        $data['created_at'] = date('Y-m-d H:i:s');
        
        if (isset($data['password'])) {
            $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
        }
        
        $this->db->insert($this->table, $data);
        return $this->db->insert_id();
    }
    
    // ==================== READ ====================
    
    public function get_all() {
        return $this->db
            ->where('deleted_at IS NULL')
            ->order_by('created_at', 'DESC')
            ->get($this->table)
            ->result();
    }
    
    public function get_by_id($id) {
        return $this->db
            ->where('id', $id)
            ->where('deleted_at IS NULL')
            ->get($this->table)
            ->row();
    }
    
    public function get_by_email($email) {
        return $this->db
            ->where('email', $email)
            ->where('deleted_at IS NULL')
            ->get($this->table)
            ->row();
    }
    
    public function count_all() {
        return $this->db
            ->where('deleted_at IS NULL')
            ->count_all_results($this->table);
    }
    
    // ==================== UPDATE ====================
    
    public function update($id, $data) {
        $data['updated_at'] = date('Y-m-d H:i:s');
        
        if (isset($data['password']) && !empty($data['password'])) {
            $data['password'] = password_hash($data['password'], PASSWORD_DEFAULT);
        } else {
            unset($data['password']);
        }
        
        return $this->db
            ->where('id', $id)
            ->update($this->table, $data);
    }
    
    // ==================== DELETE ====================
    
    public function delete($id) {
        return $this->db
            ->where('id', $id)
            ->update($this->table, [
                'deleted_at' => date('Y-m-d H:i:s')
            ]);
    }
    
    public function restore($id) {
        return $this->db
            ->where('id', $id)
            ->update($this->table, [
                'deleted_at' => NULL
            ]);
    }
    
    public function force_delete($id) {
        return $this->db
            ->where('id', $id)
            ->delete($this->table);
    }
    
    // ==================== VALIDATION ====================
    
    public function is_email_exists($email, $exclude_id = NULL) {
        $this->db->where('email', $email);
        
        if ($exclude_id) {
            $this->db->where('id !=', $exclude_id);
        }
        
        return $this->db->count_all_results($this->table) > 0;
    }
}
```

---

## 🎮 Complete CRUD Controller

```php
<?php
// application/controllers/User.php

class User extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->model('user_model');
        $this->load->library('template');
    }
    
    // List all users
    public function index() {
        $data['title'] = 'Users';
        $data['users'] = $this->user_model->get_all();
        $this->template->load('user/index', $data);
    }
    
    // Show single user
    public function show($id) {
        $user = $this->user_model->get_by_id($id);
        
        if (!$user) {
            show_404();
        }
        
        $data['title'] = $user->name;
        $data['user'] = $user;
        $this->template->load('user/show', $data);
    }
    
    // Create form
    public function create() {
        $data['title'] = 'Add User';
        $this->template->load('user/create', $data);
    }
    
    // Store new user
    public function store() {
        // Validate
        $this->load->library('form_validation');
        $this->form_validation->set_rules('name', 'Name', 'required');
        $this->form_validation->set_rules('email', 'Email', 'required|valid_email|is_unique[users.email]');
        $this->form_validation->set_rules('password', 'Password', 'required|min_length[6]');
        
        if ($this->form_validation->run() === FALSE) {
            $this->create();
            return;
        }
        
        // Insert
        $user_id = $this->user_model->create([
            'name' => $this->input->post('name'),
            'email' => $this->input->post('email'),
            'password' => $this->input->post('password')
        ]);
        
        $this->session->set_flashdata('success', 'User created successfully!');
        redirect('user');
    }
    
    // Edit form
    public function edit($id) {
        $user = $this->user_model->get_by_id($id);
        
        if (!$user) {
            show_404();
        }
        
        $data['title'] = 'Edit User';
        $data['user'] = $user;
        $this->template->load('user/edit', $data);
    }
    
    // Update user
    public function update($id) {
        $user = $this->user_model->get_by_id($id);
        
        if (!$user) {
            show_404();
        }
        
        // Validate
        $this->load->library('form_validation');
        $this->form_validation->set_rules('name', 'Name', 'required');
        $this->form_validation->set_rules('email', 'Email', 'required|valid_email');
        
        if ($this->form_validation->run() === FALSE) {
            $this->edit($id);
            return;
        }
        
        // Update
        $this->user_model->update($id, [
            'name' => $this->input->post('name'),
            'email' => $this->input->post('email'),
            'password' => $this->input->post('password')
        ]);
        
        $this->session->set_flashdata('success', 'User updated successfully!');
        redirect('user');
    }
    
    // Delete user
    public function delete($id) {
        $user = $this->user_model->get_by_id($id);
        
        if (!$user) {
            show_404();
        }
        
        $this->user_model->delete($id);
        
        $this->session->set_flashdata('success', 'User deleted successfully!');
        redirect('user');
    }
}
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] INSERT dengan insert() dan insert_batch()
- [ ] Mendapatkan insert_id() setelah insert
- [ ] SELECT dengan berbagai kondisi
- [ ] UPDATE dengan where clause
- [ ] Perbedaan hard delete dan soft delete
- [ ] Complete CRUD pattern dengan Model dan Controller

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="query-builder.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Query Builder
      </button>
    </a>
  </div>
  <div>
    <a href="relationships.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Database Relationships →
      </button>
    </a>
  </div>
</div>
