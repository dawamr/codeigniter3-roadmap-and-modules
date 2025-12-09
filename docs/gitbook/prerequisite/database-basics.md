# 💾 Database & SQL Basics

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan memahami:
- ✅ Konsep database dan fungsinya
- ✅ Struktur tabel (row, column, primary key)
- ✅ Perintah SQL dasar (SELECT, INSERT, UPDATE, DELETE)
- ✅ Relasi antar tabel
- ✅ Penggunaan phpMyAdmin

---

## 🤔 Apa itu Database?

Bayangkan database seperti **lemari arsip digital** yang terorganisir:

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Database = Tempat menyimpan data secara terstruktur**

- 📁 **Database** = Lemari arsip (berisi banyak tabel)
- 📋 **Table** = Map folder (berisi data sejenis)
- 📝 **Row/Record** = Satu lembar dokumen (satu data)
- 📊 **Column/Field** = Kategori informasi (nama, alamat, dll)

</div>

### 💡 Contoh Nyata

```
🏢 Database: toko_online
    │
    ├── 📋 Table: users
    │       ├── id | nama      | email           | password
    │       ├── 1  | John Doe  | john@mail.com   | ********
    │       └── 2  | Jane Doe  | jane@mail.com   | ********
    │
    ├── 📋 Table: products
    │       ├── id | nama_produk | harga   | stok
    │       ├── 1  | Laptop      | 8000000 | 10
    │       └── 2  | Mouse       | 150000  | 50
    │
    └── 📋 Table: orders
            ├── id | user_id | product_id | quantity | tanggal
            ├── 1  | 1       | 2          | 2        | 2024-01-15
            └── 2  | 2       | 1          | 1        | 2024-01-16
```

---

## 📊 Struktur Tabel

### 🔑 Komponen Penting Tabel

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; border-left: 4px solid #FF9800;">
<h4>🔑 Primary Key (PK)</h4>
<p><strong>Fungsi:</strong> Identitas unik setiap row</p>
<p><strong>Contoh:</strong> id, user_id, product_id</p>
<p><strong>Sifat:</strong> Unik, tidak boleh NULL</p>
</div>

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; border-left: 4px solid #2196F3;">
<h4>🔗 Foreign Key (FK)</h4>
<p><strong>Fungsi:</strong> Referensi ke tabel lain</p>
<p><strong>Contoh:</strong> user_id di tabel orders</p>
<p><strong>Sifat:</strong> Menghubungkan tabel</p>
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; border-left: 4px solid #4CAF50;">
<h4>📝 Column/Field</h4>
<p><strong>Fungsi:</strong> Kategori data</p>
<p><strong>Contoh:</strong> nama, email, harga</p>
<p><strong>Sifat:</strong> Punya tipe data tertentu</p>
</div>

<div style="background: #FCE4EC; padding: 15px; border-radius: 8px; border-left: 4px solid #E91E63;">
<h4>📄 Row/Record</h4>
<p><strong>Fungsi:</strong> Satu entri data lengkap</p>
<p><strong>Contoh:</strong> Data satu user</p>
<p><strong>Sifat:</strong> Horizontal, satu baris</p>
</div>

</div>

### 📋 Tipe Data Umum

| Tipe | Keterangan | Contoh |
|------|------------|--------|
| `INT` | Bilangan bulat | 1, 42, 1000 |
| `VARCHAR(n)` | Teks dengan panjang max n | "John", "Hello" |
| `TEXT` | Teks panjang | Deskripsi, artikel |
| `DECIMAL(p,s)` | Angka desimal | 99.99, 1500.50 |
| `DATE` | Tanggal | 2024-01-15 |
| `DATETIME` | Tanggal dan waktu | 2024-01-15 10:30:00 |
| `BOOLEAN` | True/False | 1 atau 0 |

---

## 📝 SQL Commands

SQL (Structured Query Language) adalah bahasa untuk berkomunikasi dengan database.

### 🟢 SELECT - Mengambil Data

```sql
-- Mengambil semua kolom
SELECT * FROM users;

-- Mengambil kolom tertentu
SELECT nama, email FROM users;

-- Dengan kondisi WHERE
SELECT * FROM users WHERE id = 1;

-- Dengan multiple kondisi
SELECT * FROM products 
WHERE harga < 1000000 
AND stok > 0;

-- Mengurutkan hasil
SELECT * FROM products ORDER BY harga ASC;  -- Termurah dulu
SELECT * FROM products ORDER BY harga DESC; -- Termahal dulu

-- Membatasi hasil
SELECT * FROM products LIMIT 10;            -- 10 data pertama
SELECT * FROM products LIMIT 10 OFFSET 20;  -- Skip 20, ambil 10
```

#### 🎯 Contoh Interaktif SELECT

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Tabel `products`:**
| id | nama | harga | kategori | stok |
|----|------|-------|----------|------|
| 1 | Laptop | 8000000 | Elektronik | 10 |
| 2 | Mouse | 150000 | Aksesoris | 50 |
| 3 | Keyboard | 300000 | Aksesoris | 30 |
| 4 | Monitor | 2500000 | Elektronik | 15 |

**Query:** `SELECT nama, harga FROM products WHERE kategori = 'Aksesoris'`

**Hasil:**
| nama | harga |
|------|-------|
| Mouse | 150000 |
| Keyboard | 300000 |

</div>

---

### 🟢 INSERT - Menambah Data

```sql
-- Insert satu baris
INSERT INTO users (nama, email, password) 
VALUES ('John Doe', 'john@mail.com', 'hashed_password');

-- Insert multiple baris
INSERT INTO users (nama, email, password) VALUES 
('Alice', 'alice@mail.com', 'pass1'),
('Bob', 'bob@mail.com', 'pass2'),
('Charlie', 'charlie@mail.com', 'pass3');
```

#### 💡 Tips INSERT

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

1. **Urutan kolom** harus sama dengan urutan values
2. **Primary Key (id)** biasanya auto-increment, tidak perlu diisi
3. **Tipe data** harus sesuai (string pakai quotes, angka tidak)
4. **NULL** diisi jika kolom mengizinkan dan tidak ada nilai

</div>

---

### 🟡 UPDATE - Mengubah Data

```sql
-- Update satu kolom
UPDATE users 
SET email = 'newemail@mail.com' 
WHERE id = 1;

-- Update multiple kolom
UPDATE users 
SET nama = 'John Smith', email = 'john.smith@mail.com' 
WHERE id = 1;

-- Update dengan kondisi
UPDATE products 
SET stok = stok - 1 
WHERE id = 5;
```

> ⚠️ **PERINGATAN PENTING!**
> 
> **SELALU gunakan WHERE** pada UPDATE! Tanpa WHERE, SEMUA data akan terupdate!
> ```sql
> -- ❌ BAHAYA! Semua user akan terupdate!
> UPDATE users SET password = '123456';
> 
> -- ✅ AMAN dengan WHERE
> UPDATE users SET password = '123456' WHERE id = 1;
> ```

---

### 🔴 DELETE - Menghapus Data

```sql
-- Hapus satu baris
DELETE FROM users WHERE id = 5;

-- Hapus dengan kondisi
DELETE FROM products WHERE stok = 0;

-- Hapus semua (HATI-HATI!)
DELETE FROM logs WHERE created_at < '2023-01-01';
```

> ⚠️ **PERINGATAN PENTING!**
> 
> **SELALU gunakan WHERE** pada DELETE! Tanpa WHERE, SEMUA data akan terhapus!
> ```sql
> -- ❌ BAHAYA! Semua user akan terhapus!
> DELETE FROM users;
> 
> -- ✅ AMAN dengan WHERE
> DELETE FROM users WHERE id = 1;
> ```

---

## 🔗 Relasi Antar Tabel

### 📊 Jenis Relasi

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; text-align: center;">
<h4>1️⃣ One-to-One</h4>
<p>1 user = 1 profil</p>
<p><small>Jarang digunakan</small></p>
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; text-align: center;">
<h4>1️⃣ One-to-Many</h4>
<p>1 user = banyak orders</p>
<p><small>Paling umum</small></p>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; text-align: center;">
<h4>🔄 Many-to-Many</h4>
<p>banyak user = banyak products</p>
<p><small>Butuh tabel perantara</small></p>
</div>

</div>

### 🔗 Contoh Relasi One-to-Many

```
📋 users                    📋 orders
┌────┬──────────┐           ┌────┬─────────┬─────────┬──────────┐
│ id │ nama     │           │ id │ user_id │ total   │ tanggal  │
├────┼──────────┤           ├────┼─────────┼─────────┼──────────┤
│ 1  │ John     │◄────────┐ │ 1  │ 1       │ 500000  │ 2024-01  │
│ 2  │ Jane     │         ├─│ 2  │ 1       │ 750000  │ 2024-02  │
└────┴──────────┘         │ │ 3  │ 2       │ 300000  │ 2024-02  │
                          └─│ 4  │ 1       │ 200000  │ 2024-03  │
                            └────┴─────────┴─────────┴──────────┘
                            
John (id=1) memiliki 3 orders
Jane (id=2) memiliki 1 order
```

### 🔗 JOIN - Menggabungkan Tabel

```sql
-- Mengambil data dari 2 tabel yang berelasi
SELECT 
    users.nama,
    orders.total,
    orders.tanggal
FROM users
JOIN orders ON users.id = orders.user_id;

-- Hasil:
-- | nama | total  | tanggal  |
-- |------|--------|----------|
-- | John | 500000 | 2024-01  |
-- | John | 750000 | 2024-02  |
-- | Jane | 300000 | 2024-02  |
```

---

## 🛠️ phpMyAdmin

phpMyAdmin adalah **antarmuka web** untuk mengelola database MySQL.

### 🔧 Cara Akses

1. Jalankan XAMPP/Laragon
2. Buka browser
3. Ketik `http://localhost/phpmyadmin`

### 📋 Fitur Utama

| Tab | Fungsi |
|-----|--------|
| **Structure** | Lihat/edit struktur tabel |
| **Browse** | Lihat data dalam tabel |
| **SQL** | Jalankan query manual |
| **Insert** | Tambah data baru |
| **Export** | Backup database |
| **Import** | Restore database |

### 💡 Tips phpMyAdmin

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

1. **Backup rutin** - Export database sebelum modifikasi besar
2. **Gunakan SQL tab** - Untuk query kompleks
3. **Perhatikan collation** - Gunakan `utf8mb4_general_ci` untuk support emoji
4. **Foreign key** - Aktifkan di InnoDB engine untuk relasi

</div>

---

## ❓ Quiz: Database & SQL

Uji pemahaman Anda:

### Pertanyaan 1
**Apa fungsi PRIMARY KEY dalam sebuah tabel?**

- [ ] A. Menyimpan password
- [ ] B. Menghubungkan ke tabel lain
- [ ] C. Identitas unik setiap baris data
- [ ] D. Menyimpan data sementara

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Identitas unik setiap baris data**

**Penjelasan:**
Primary Key adalah kolom yang nilainya **unik** untuk setiap baris, berfungsi sebagai identitas data.

Karakteristik Primary Key:
- 🔹 Nilai harus **unik** (tidak boleh sama)
- 🔹 Tidak boleh **NULL** (harus ada nilainya)
- 🔹 Satu tabel hanya punya **satu** Primary Key
- 🔹 Biasanya kolom `id` dengan auto-increment

```sql
CREATE TABLE users (
    id INT PRIMARY KEY AUTO_INCREMENT,  -- ← Primary Key
    nama VARCHAR(100),
    email VARCHAR(100)
);
```

</details>

---

### Pertanyaan 2
**Query mana yang benar untuk mengambil produk dengan harga di bawah 100.000?**

- [ ] A. `SELECT * FROM products WHERE harga = 100000`
- [ ] B. `SELECT * FROM products WHERE harga < 100000`
- [ ] C. `SELECT * FROM products IF harga < 100000`
- [ ] D. `GET * FROM products WHERE harga < 100000`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `SELECT * FROM products WHERE harga < 100000`**

**Penjelasan:**
- `SELECT` adalah perintah untuk mengambil data
- `*` artinya semua kolom
- `FROM products` menentukan tabel sumber
- `WHERE harga < 100000` adalah kondisi filter

Operator perbandingan SQL:
- `=` sama dengan
- `<` kurang dari
- `>` lebih dari
- `<=` kurang dari atau sama dengan
- `>=` lebih dari atau sama dengan
- `<>` atau `!=` tidak sama dengan

</details>

---

### Pertanyaan 3
**Apa yang terjadi jika menjalankan `DELETE FROM users` tanpa WHERE?**

- [ ] A. Error karena syntax salah
- [ ] B. Tidak terjadi apa-apa
- [ ] C. Menghapus user pertama saja
- [ ] D. Menghapus SEMUA data di tabel users

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: D. Menghapus SEMUA data di tabel users**

**Penjelasan:**
**SANGAT BERBAHAYA!** Tanpa WHERE clause, perintah DELETE akan menghapus **seluruh baris** dalam tabel.

```sql
-- ❌ BERBAHAYA - hapus semua!
DELETE FROM users;

-- ✅ AMAN - hanya hapus id tertentu
DELETE FROM users WHERE id = 5;
```

> 💡 **Best Practice:**
> 1. Selalu backup sebelum DELETE massal
> 2. Test dengan SELECT dulu: `SELECT * FROM users WHERE ...`
> 3. Baru ganti SELECT dengan DELETE

</details>

---

### Pertanyaan 4
**Untuk menambah data baru ke tabel, perintah SQL yang digunakan adalah?**

- [ ] A. ADD
- [ ] B. CREATE
- [ ] C. INSERT
- [ ] D. PUT

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. INSERT**

**Penjelasan:**
`INSERT INTO` adalah perintah SQL untuk menambahkan data baru ke tabel.

```sql
INSERT INTO users (nama, email, password) 
VALUES ('John Doe', 'john@mail.com', 'secret123');
```

Perintah SQL lainnya:
- `SELECT` = mengambil data
- `UPDATE` = mengubah data yang ada
- `DELETE` = menghapus data
- `CREATE` = membuat tabel/database baru

</details>

---

### Pertanyaan 5
**Query untuk menggabungkan data dari tabel `users` dan `orders` berdasarkan relasi adalah?**

- [ ] A. `SELECT * FROM users, orders`
- [ ] B. `SELECT * FROM users MERGE orders`
- [ ] C. `SELECT * FROM users JOIN orders ON users.id = orders.user_id`
- [ ] D. `SELECT * FROM users + orders`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `SELECT * FROM users JOIN orders ON users.id = orders.user_id`**

**Penjelasan:**
`JOIN` digunakan untuk menggabungkan data dari dua tabel berdasarkan relasi (foreign key).

```sql
SELECT 
    users.nama,
    orders.total,
    orders.tanggal
FROM users
JOIN orders ON users.id = orders.user_id;
```

Bagian `ON` menentukan **kondisi penggabungan** - kolom mana yang berelasi.

Jenis JOIN:
- `INNER JOIN` (atau `JOIN`) = hanya data yang match
- `LEFT JOIN` = semua dari tabel kiri + match
- `RIGHT JOIN` = semua dari tabel kanan + match

</details>

---

## ✅ Checklist Pemahaman

Pastikan Anda sudah memahami:

- [ ] Konsep database, tabel, row, dan column
- [ ] Fungsi Primary Key dan Foreign Key
- [ ] Query SELECT dengan WHERE, ORDER BY, LIMIT
- [ ] Query INSERT untuk menambah data
- [ ] Query UPDATE dengan WHERE clause
- [ ] Query DELETE dengan WHERE clause
- [ ] Konsep relasi dan JOIN
- [ ] Cara menggunakan phpMyAdmin

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="php-fundamentals.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← PHP Fundamentals
      </button>
    </a>
  </div>
  <div>
    <a href="tools-setup.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Development Tools →
      </button>
    </a>
  </div>
</div>
