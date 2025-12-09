# 💾 Phase 3 - Database & Model

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Memahami konsep Model di MVC pattern
- ✅ Menguasai Query Builder (Active Record) CI3
- ✅ Melakukan operasi CRUD dengan benar
- ✅ Memahami database relationships
- ✅ Menggunakan transactions untuk data integrity
- ✅ Optimasi query untuk performa

---

## 📋 Overview

Model adalah **data layer** di CodeIgniter 3 - jembatan antara aplikasi dan database. Model menangani semua interaksi dengan database, menjaga business logic tetap terpisah dari presentation.

> 💡 **Analogi Sederhana:**  
> Jika Controller adalah **manager** 👔 dan View adalah **display** 🖼️,  
> maka Model adalah **gudang data** 📦 yang menyimpan dan mengambil informasi!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8F5E9; border-left: 4px solid #4CAF50; padding: 15px;">
    <h4>🔧 Database Config</h4>
    <small>Setup koneksi database</small>
  </div>
  
  <div style="background: #E3F2FD; border-left: 4px solid #2196F3; padding: 15px;">
    <h4>📊 Model Basics</h4>
    <small>Struktur dan konsep Model</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #FF9800; padding: 15px;">
    <h4>🔨 Query Builder</h4>
    <small>Active Record pattern</small>
  </div>
  
  <div style="background: #FCE4EC; border-left: 4px solid #E91E63; padding: 15px;">
    <h4>💼 CRUD Operations</h4>
    <small>Create, Read, Update, Delete</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #9C27B0; padding: 15px;">
    <h4>🔗 Relationships</h4>
    <small>Join tables & relations</small>
  </div>
  
  <div style="background: #E0F2F1; border-left: 4px solid #009688; padding: 15px;">
    <h4>🔒 Transactions</h4>
    <small>Data integrity & rollback</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[🔧 Database Configuration](database-config.md)**
   - Connection settings
   - Multiple databases
   - Environment-based config

2. **[📊 Model Fundamentals](model-basics.md)**
   - Creating models
   - Naming conventions
   - Model structure

3. **[🔨 Query Builder](query-builder.md)**
   - SELECT queries
   - INSERT, UPDATE, DELETE
   - WHERE conditions
   - Chaining methods

4. **[💼 CRUD Operations](crud-operations.md)**
   - Complete CRUD example
   - Best practices
   - Error handling

5. **[🔗 Database Relationships](relationships.md)**
   - One-to-One
   - One-to-Many
   - Many-to-Many
   - JOIN operations

### Advanced Topics
6. **[🔒 Transactions](transactions.md)**
   - Transaction basics
   - Rollback on error
   - Nested transactions

7. **[⚡ Query Optimization](optimization.md)**
   - Query profiling
   - Indexing strategies
   - Caching results

8. **[🛡️ Security](security.md)**
   - SQL injection prevention
   - Query binding
   - Escaping data

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Build complete data layer
   - Real-world scenarios

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### Query Builder Example
```php
// SELECT with conditions
$this->db->select('id, name, price')
         ->from('products')
         ->where('category_id', 5)
         ->where('price <', 100)
         ->order_by('name', 'ASC')
         ->limit(10);
$query = $this->db->get();
```

### Model Structure
```php
class Product_model extends CI_Model {
    private $table = 'products';
    
    public function get_all() {
        return $this->db->get($this->table)->result();
    }
    
    public function get_by_id($id) {
        return $this->db->get_where($this->table, ['id' => $id])->row();
    }
}
```

---

## ✅ Success Criteria

- [ ] Configure database connection
- [ ] Create models following conventions
- [ ] Perform CRUD operations
- [ ] Use Query Builder effectively
- [ ] Handle relationships between tables
- [ ] Implement transactions
- [ ] Prevent SQL injection
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Product Inventory System"**
- Product CRUD with categories
- Stock management
- Transaction history
- Search and filter
- Reports generation

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-2-view/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 2: View & Template
      </button>
    </a>
  </div>
  <div>
    <a href="database-config.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Database Configuration →
      </button>
    </a>
  </div>
</div>
