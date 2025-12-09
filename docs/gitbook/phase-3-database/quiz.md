# ❓ Quiz: Phase 3 - Database & Model

## 🎯 Tentang Quiz Ini

Quiz ini menguji pemahaman Anda tentang Database dan Model di CodeIgniter 3. Terdapat **15 pertanyaan**.

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### 📊 Passing Score

| Skor | Status |
|------|--------|
| **12-15** | 🟢 Excellent - Lanjut ke Phase 4! |
| **9-11** | 🟡 Good - Review materi yang kurang |
| **6-8** | 🟠 Need Review |
| **0-5** | 🔴 Ulangi Phase 3 |

</div>

---

## 🔧 Bagian 1: Database Configuration (5 Pertanyaan)

### Pertanyaan 1
**Di file mana konfigurasi database CI3 berada?**

- [ ] A. `application/config/config.php`
- [ ] B. `application/config/database.php`
- [ ] C. `system/database/config.php`
- [ ] D. `application/database.php`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `application/config/database.php`**

**Penjelasan:**
Semua konfigurasi database ada di `application/config/database.php`:
- hostname
- username
- password
- database name
- driver, dll

</details>

---

### Pertanyaan 2
**Mengapa `db_debug` harus FALSE di production?**

- [ ] A. Agar website lebih cepat
- [ ] B. Agar error database tidak ditampilkan ke user (keamanan)
- [ ] C. Agar database tidak bisa di-hack
- [ ] D. Tidak ada alasan khusus

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Agar error database tidak ditampilkan ke user (keamanan)**

**Penjelasan:**
Jika `db_debug = TRUE`:
- Error SQL ditampilkan lengkap dengan nama tabel, kolom
- Informasi sensitif bisa terekspos
- Memudahkan attacker untuk SQL Injection

```php
// Production
'db_debug' => (ENVIRONMENT !== 'production'),
```

</details>

---

### Pertanyaan 3
**Driver database mana yang paling umum digunakan untuk MySQL?**

- [ ] A. `mysql`
- [ ] B. `mysqli`
- [ ] C. `pdo`
- [ ] D. `mariadb`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `mysqli`**

**Penjelasan:**
```php
'dbdriver' => 'mysqli',
```

`mysqli` adalah driver yang direkomendasikan untuk MySQL/MariaDB karena:
- Lebih modern dari `mysql` (deprecated)
- Native support untuk MySQL
- Prepared statements

</details>

---

### Pertanyaan 4
**Bagaimana cara menggunakan multiple database di CI3?**

- [ ] A. Tidak bisa, hanya satu database
- [ ] B. Buat config terpisah dan load dengan `$this->load->database('nama', TRUE)`
- [ ] C. Edit file system
- [ ] D. Install plugin tambahan

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Buat config terpisah dan load dengan `$this->load->database('nama', TRUE)`**

**Penjelasan:**
```php
// database.php
$db['default'] = [...];
$db['analytics'] = [...];

// Di model
$this->db_analytics = $this->load->database('analytics', TRUE);
$this->db_analytics->get('table');
```

Parameter `TRUE` artinya return database object.

</details>

---

### Pertanyaan 5
**Apa fungsi `$autoload['libraries'] = array('database')` di autoload.php?**

- [ ] A. Install database
- [ ] B. Load database library otomatis di setiap request
- [ ] C. Backup database
- [ ] D. Optimize database

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Load database library otomatis di setiap request**

**Penjelasan:**
Dengan autoload database:
- Tidak perlu `$this->load->database()` manual
- `$this->db` langsung tersedia di semua controller dan model

</details>

---

## 📊 Bagian 2: Model & Query Builder (5 Pertanyaan)

### Pertanyaan 6
**Dalam MVC, apa tugas utama Model?**

- [ ] A. Menampilkan HTML
- [ ] B. Menerima request user
- [ ] C. Mengelola data (database operations)
- [ ] D. Routing URL

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Mengelola data (database operations)**

**Penjelasan:**
Model bertanggung jawab untuk:
- Query database (SELECT, INSERT, UPDATE, DELETE)
- Business logic terkait data
- Validasi data

Controller hanya memanggil model, tidak query langsung.

</details>

---

### Pertanyaan 7
**Apa perbedaan `result()` dan `row()` di CI3?**

- [ ] A. Tidak ada perbedaan
- [ ] B. `result()` return array of objects, `row()` return single object
- [ ] C. `result()` lebih cepat
- [ ] D. `row()` untuk INSERT, `result()` untuk SELECT

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `result()` return array of objects, `row()` return single object**

**Penjelasan:**
```php
// result() - untuk list data
$users = $this->db->get('users')->result();
// Return: Array of objects

// row() - untuk single data
$user = $this->db->get_where('users', ['id' => 5])->row();
// Return: Single object atau NULL
```

</details>

---

### Pertanyaan 8
**Keuntungan utama Query Builder dibanding raw SQL adalah?**

- [ ] A. Lebih cepat
- [ ] B. Otomatis escape data (aman dari SQL Injection)
- [ ] C. Syntax lebih pendek
- [ ] D. Bisa multi-threading

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Otomatis escape data (aman dari SQL Injection)**

**Penjelasan:**
```php
// ❌ Raw SQL - BERBAHAYA
$sql = "SELECT * FROM users WHERE name = '$input'";

// ✅ Query Builder - AMAN
$this->db->where('name', $input)->get('users');
// Data otomatis di-escape
```

</details>

---

### Pertanyaan 9
**Bagaimana cara membuat query LIKE untuk search?**

- [ ] A. `$this->db->where('name', '%keyword%')`
- [ ] B. `$this->db->like('name', 'keyword')`
- [ ] C. `$this->db->search('name', 'keyword')`
- [ ] D. `$this->db->find('name', 'keyword')`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$this->db->like('name', 'keyword')`**

**Penjelasan:**
```php
// WHERE name LIKE '%keyword%'
$this->db->like('name', 'keyword');

// WHERE name LIKE 'keyword%' (starts with)
$this->db->like('name', 'keyword', 'after');

// WHERE name LIKE '%keyword' (ends with)
$this->db->like('name', 'keyword', 'before');
```

</details>

---

### Pertanyaan 10
**Bagaimana cara mendapatkan ID dari data yang baru di-INSERT?**

- [ ] A. `$this->db->last_id()`
- [ ] B. `$this->db->insert_id()`
- [ ] C. `$this->db->get_id()`
- [ ] D. `$this->db->new_id()`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$this->db->insert_id()`**

**Penjelasan:**
```php
$this->db->insert('users', $data);
$new_id = $this->db->insert_id();
echo "User baru dengan ID: " . $new_id;
```

Method ini mengembalikan auto-increment ID dari row yang baru di-insert.

</details>

---

## 🔗 Bagian 3: Relationships & Transactions (5 Pertanyaan)

### Pertanyaan 11
**Relasi "1 Category memiliki banyak Products" adalah jenis?**

- [ ] A. One-to-One
- [ ] B. One-to-Many
- [ ] C. Many-to-Many
- [ ] D. Self-referencing

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. One-to-Many**

**Penjelasan:**
```
Category (One)          Products (Many)
+----+-----------+      +----+-------------+
| id | name      |      | id | category_id |
+----+-----------+      +----+-------------+
| 1  | Electronics| ───> | 1  | 1           |
|    |           | ───> | 2  | 1           |
|    |           | ───> | 3  | 1           |
+----+-----------+      +----+-------------+
```

Foreign key `category_id` ada di tabel products.

</details>

---

### Pertanyaan 12
**Untuk relasi Many-to-Many (Products ↔ Tags), apa yang dibutuhkan?**

- [ ] A. Foreign key di kedua tabel
- [ ] B. Pivot table (tabel penghubung)
- [ ] C. JSON column
- [ ] D. Tidak perlu tambahan

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Pivot table (tabel penghubung)**

**Penjelasan:**
```
products          product_tags         tags
+----+------+     +-----------+------+ +----+------+
| id | name |     |product_id |tag_id| | id | name |
+----+------+     +-----------+------+ +----+------+
| 1  | Laptop| ──>| 1         | 1    |<──| 1  | Sale |
|    |      | ──>| 1         | 2    |<──| 2  | New  |
+----+------+     +-----------+------+ +----+------+
```

Pivot table `product_tags` menghubungkan kedua tabel.

</details>

---

### Pertanyaan 13
**Apa itu "Soft Delete"?**

- [ ] A. Menghapus dengan konfirmasi
- [ ] B. Menandai data sebagai deleted (tidak benar-benar hapus)
- [ ] C. Menghapus secara bertahap
- [ ] D. Menghapus dari cache saja

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Menandai data sebagai deleted (tidak benar-benar hapus)**

**Penjelasan:**
```php
// Soft delete
public function delete($id) {
    return $this->db
        ->where('id', $id)
        ->update('users', ['deleted_at' => date('Y-m-d H:i:s')]);
}

// Get hanya yang tidak deleted
public function get_all() {
    return $this->db
        ->where('deleted_at IS NULL')
        ->get('users')->result();
}
```

Keuntungan: Data bisa di-restore, audit trail tetap ada.

</details>

---

### Pertanyaan 14
**Apa prinsip utama Database Transaction?**

- [ ] A. First In, First Out
- [ ] B. All or Nothing (Atomicity)
- [ ] C. Lazy Loading
- [ ] D. Eager Loading

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. All or Nothing (Atomicity)**

**Penjelasan:**
Transaction memastikan:
- Jika SEMUA operasi berhasil → COMMIT (simpan semua)
- Jika ADA yang gagal → ROLLBACK (batalkan semua)

Contoh: Transfer uang - kurangi saldo A DAN tambah saldo B harus keduanya berhasil.

</details>

---

### Pertanyaan 15
**Bagaimana syntax transaction di CI3?**

- [ ] A. `$this->db->begin()` dan `$this->db->end()`
- [ ] B. `$this->db->trans_start()` dan `$this->db->trans_complete()`
- [ ] C. `$this->db->transaction(function() { })`
- [ ] D. `$this->db->startTransaction()`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$this->db->trans_start()` dan `$this->db->trans_complete()`**

**Penjelasan:**
```php
$this->db->trans_start();

$this->db->insert('orders', $order_data);
$this->db->insert('order_items', $items_data);
$this->db->update('products', $stock_update);

$this->db->trans_complete();

if ($this->db->trans_status() === FALSE) {
    // Gagal, semua di-rollback
} else {
    // Sukses
}
```

</details>

---

## 📊 Hitung Skor Anda

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

### 📝 Scorecard

| Bagian | Benar | Total |
|--------|-------|-------|
| Database Configuration (1-5) | ___ | 5 |
| Model & Query Builder (6-10) | ___ | 5 |
| Relationships & Transactions (11-15) | ___ | 5 |
| **TOTAL** | ___ | **15** |

</div>

---

## 🎯 Hasil Quiz

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; border-left: 4px solid #4CAF50;">
<h3>🟢 12-15 Benar</h3>
<p><strong>Excellent!</strong></p>
<p>Anda menguasai Database & Model dengan baik!</p>
<a href="../phase-4-form/README.md">➡️ Lanjut ke Phase 4</a>
</div>

<div style="background: #FFF9C4; padding: 20px; border-radius: 10px; border-left: 4px solid #FDD835;">
<h3>🟡 9-11 Benar</h3>
<p><strong>Good!</strong></p>
<p>Hampir sempurna. Review bagian yang kurang.</p>
</div>

<div style="background: #FFE0B2; padding: 20px; border-radius: 10px; border-left: 4px solid #FF9800;">
<h3>🟠 6-8 Benar</h3>
<p><strong>Need Review</strong></p>
<a href="database-config.md">📖 Review Database Config</a>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; border-left: 4px solid #F44336;">
<h3>🔴 0-5 Benar</h3>
<p><strong>Try Again</strong></p>
<a href="README.md">🔄 Ulangi Phase 3</a>
</div>

</div>

---

## 📚 Area yang Perlu Diperbaiki

| Jika salah di... | Review |
|------------------|--------|
| Pertanyaan 1-5 | [🔧 Database Configuration](database-config.md) |
| Pertanyaan 6-10 | [📊 Model Basics](model-basics.md) & [🔨 Query Builder](query-builder.md) |
| Pertanyaan 11-15 | [🔗 Relationships](relationships.md) & [🔒 Transactions](transactions.md) |

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="practice.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Practice Lab
      </button>
    </a>
  </div>
  <div>
    <a href="../phase-4-form/README.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Phase 4: Form Handling →
      </button>
    </a>
  </div>
</div>
