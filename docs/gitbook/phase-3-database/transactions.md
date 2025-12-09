# 🔒 Transactions

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami konsep database transaction
- ✅ Menggunakan transaction di CI3
- ✅ Menangani error dengan rollback
- ✅ Menerapkan transaction pada operasi kompleks

---

## 🤔 Apa itu Transaction?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Transaction** adalah sekelompok operasi database yang dijalankan sebagai **satu kesatuan**. 

**Prinsip: "All or Nothing"**
- ✅ Jika SEMUA berhasil → **COMMIT** (simpan semua)
- ❌ Jika ADA yang gagal → **ROLLBACK** (batalkan semua)

**Analogi:** Transfer uang di ATM 🏧
1. Kurangi saldo pengirim
2. Tambah saldo penerima

Jika step 2 gagal, step 1 harus dibatalkan! Tidak boleh uang hilang.

</div>

---

## 🔄 ACID Properties

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h3>A - Atomicity</h3>
<p>Semua operasi berhasil, atau tidak sama sekali.</p>
</div>

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px;">
<h3>C - Consistency</h3>
<p>Database tetap dalam keadaan valid setelah transaction.</p>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px;">
<h3>I - Isolation</h3>
<p>Transaction tidak saling mengganggu.</p>
</div>

<div style="background: #FCE4EC; padding: 15px; border-radius: 8px;">
<h3>D - Durability</h3>
<p>Data yang di-commit tidak akan hilang.</p>
</div>

</div>

---

## 📋 Syntax Transaction di CI3

### Basic Transaction

```php
<?php
// Mulai transaction
$this->db->trans_start();

// Operasi database
$this->db->insert('orders', $order_data);
$this->db->insert('order_items', $items_data);
$this->db->update('products', ['stock' => $new_stock]);

// Selesaikan transaction
$this->db->trans_complete();

// Cek status
if ($this->db->trans_status() === FALSE) {
    // Transaction gagal
    echo "Error occurred!";
} else {
    // Transaction berhasil
    echo "Success!";
}
```

### Manual Transaction

```php
<?php
// Mulai transaction
$this->db->trans_begin();

try {
    $this->db->insert('orders', $order_data);
    $order_id = $this->db->insert_id();
    
    $this->db->insert('order_items', $items_data);
    $this->db->update('products', $stock_update);
    
    // Jika semua berhasil
    $this->db->trans_commit();
    
} catch (Exception $e) {
    // Jika ada error
    $this->db->trans_rollback();
    throw $e;
}
```

---

## 🛒 Contoh: Order Processing

### Tanpa Transaction (Berbahaya!)

```php
<?php
// ❌ BERBAHAYA - Jika salah satu gagal, data tidak konsisten!
public function create_order_bad($user_id, $cart_items) {
    
    // 1. Insert order
    $this->db->insert('orders', [
        'user_id' => $user_id,
        'total' => $this->calculate_total($cart_items),
        'status' => 'pending'
    ]);
    $order_id = $this->db->insert_id();
    
    // 2. Insert order items
    foreach ($cart_items as $item) {
        $this->db->insert('order_items', [
            'order_id' => $order_id,
            'product_id' => $item['product_id'],
            'quantity' => $item['quantity'],
            'price' => $item['price']
        ]);
    }
    // ❌ Jika gagal di sini, order sudah ter-insert tapi items tidak!
    
    // 3. Update product stock
    foreach ($cart_items as $item) {
        $this->db->set('stock', 'stock - ' . $item['quantity'], FALSE);
        $this->db->where('id', $item['product_id']);
        $this->db->update('products');
    }
    // ❌ Jika gagal di sini, items sudah masuk tapi stock tidak berkurang!
    
    return $order_id;
}
```

### Dengan Transaction (Aman!)

```php
<?php
// ✅ AMAN - Semua berhasil atau semua dibatalkan
public function create_order($user_id, $cart_items) {
    
    // Mulai transaction
    $this->db->trans_start();
    
    // 1. Insert order
    $order_data = [
        'user_id' => $user_id,
        'total' => $this->calculate_total($cart_items),
        'status' => 'pending',
        'created_at' => date('Y-m-d H:i:s')
    ];
    $this->db->insert('orders', $order_data);
    $order_id = $this->db->insert_id();
    
    // 2. Insert order items
    foreach ($cart_items as $item) {
        $this->db->insert('order_items', [
            'order_id' => $order_id,
            'product_id' => $item['product_id'],
            'quantity' => $item['quantity'],
            'price' => $item['price']
        ]);
    }
    
    // 3. Update product stock
    foreach ($cart_items as $item) {
        $this->db->set('stock', 'stock - ' . $item['quantity'], FALSE);
        $this->db->where('id', $item['product_id']);
        $this->db->update('products');
    }
    
    // 4. Clear cart
    $this->db->where('user_id', $user_id)->delete('cart');
    
    // Selesaikan transaction
    $this->db->trans_complete();
    
    // Return hasil
    if ($this->db->trans_status() === FALSE) {
        return FALSE;  // Gagal, semua di-rollback
    }
    
    return $order_id;  // Sukses
}
```

---

## 💳 Contoh: Transfer Uang

```php
<?php
// Order_model.php atau Transaction_model.php

public function transfer($from_user_id, $to_user_id, $amount) {
    
    $this->db->trans_start();
    
    // 1. Cek saldo pengirim
    $sender = $this->db
        ->select('balance')
        ->get_where('wallets', ['user_id' => $from_user_id])
        ->row();
    
    if (!$sender || $sender->balance < $amount) {
        $this->db->trans_rollback();
        return ['success' => false, 'message' => 'Saldo tidak cukup'];
    }
    
    // 2. Kurangi saldo pengirim
    $this->db->set('balance', 'balance - ' . $amount, FALSE);
    $this->db->where('user_id', $from_user_id);
    $this->db->update('wallets');
    
    // 3. Tambah saldo penerima
    $this->db->set('balance', 'balance + ' . $amount, FALSE);
    $this->db->where('user_id', $to_user_id);
    $this->db->update('wallets');
    
    // 4. Catat history transaksi
    $this->db->insert('transactions', [
        'from_user_id' => $from_user_id,
        'to_user_id' => $to_user_id,
        'amount' => $amount,
        'type' => 'transfer',
        'created_at' => date('Y-m-d H:i:s')
    ]);
    
    $this->db->trans_complete();
    
    if ($this->db->trans_status() === FALSE) {
        return ['success' => false, 'message' => 'Transfer gagal'];
    }
    
    return ['success' => true, 'message' => 'Transfer berhasil'];
}
```

---

## 📦 Contoh: Bulk Import

```php
<?php
public function import_products($products) {
    
    $this->db->trans_start();
    
    $success_count = 0;
    $errors = [];
    
    foreach ($products as $index => $product) {
        // Validasi
        if (empty($product['name']) || empty($product['price'])) {
            $errors[] = "Row " . ($index + 1) . ": Missing required fields";
            continue;
        }
        
        // Insert
        $this->db->insert('products', [
            'name' => $product['name'],
            'price' => $product['price'],
            'stock' => $product['stock'] ?? 0,
            'created_at' => date('Y-m-d H:i:s')
        ]);
        
        $success_count++;
    }
    
    // Jika ada error, rollback semua
    if (!empty($errors)) {
        $this->db->trans_rollback();
        return [
            'success' => false,
            'message' => 'Import dibatalkan karena ada error',
            'errors' => $errors
        ];
    }
    
    $this->db->trans_complete();
    
    return [
        'success' => true,
        'message' => "Berhasil import $success_count produk"
    ];
}
```

---

## ⚙️ Transaction Modes

### Strict Mode (Default)

```php
<?php
// Jika ada query yang gagal, otomatis rollback
$this->db->trans_start();
// ... queries ...
$this->db->trans_complete();
```

### Test Mode

```php
<?php
// Untuk testing - selalu rollback di akhir
$this->db->trans_start(TRUE);  // TRUE = test mode
// ... queries ...
$this->db->trans_complete();
// Semua akan di-rollback, berguna untuk testing
```

### Disable Transaction (Hati-hati!)

```php
<?php
// Nonaktifkan transaction sementara
$this->db->trans_off();
// ... queries tanpa transaction ...

// Aktifkan kembali
$this->db->trans_start();
```

---

## 🔄 Nested Transactions

```php
<?php
public function complex_operation() {
    
    $this->db->trans_start();
    
    // Level 1 operations
    $this->db->insert('table1', $data1);
    
    // Nested transaction (CI3 handles this)
    $this->db->trans_start();
    $this->db->insert('table2', $data2);
    $this->db->trans_complete();
    
    // More Level 1 operations
    $this->db->insert('table3', $data3);
    
    $this->db->trans_complete();
    
    return $this->db->trans_status();
}
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Note:** CI3 menggunakan "savepoints" untuk nested transactions. Jika inner transaction gagal, hanya inner yang di-rollback (tergantung database driver).

</div>

---

## 📊 Kapan Menggunakan Transaction?

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px;">
<h3>✅ Gunakan Transaction</h3>
<ul>
<li>Order processing (order + items + stock)</li>
<li>Transfer uang/balance</li>
<li>Registrasi user (user + profile + settings)</li>
<li>Bulk import data</li>
<li>Update related tables</li>
<li>Operasi yang mempengaruhi banyak tabel</li>
</ul>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px;">
<h3>❌ Tidak Perlu Transaction</h3>
<ul>
<li>Single INSERT/UPDATE/DELETE</li>
<li>Read-only operations (SELECT)</li>
<li>Operasi yang bisa partial success</li>
<li>Logging (boleh gagal)</li>
</ul>
</div>

</div>

---

## 🐛 Debugging Transaction

```php
<?php
public function debug_transaction() {
    
    $this->db->trans_start();
    
    $this->db->insert('products', ['name' => 'Test']);
    echo "After insert: " . $this->db->last_query() . "<br>";
    
    $this->db->update('products', ['name' => 'Updated'], ['id' => 999]);
    echo "After update: " . $this->db->last_query() . "<br>";
    
    $this->db->trans_complete();
    
    echo "Transaction status: " . ($this->db->trans_status() ? 'SUCCESS' : 'FAILED');
    
    // Lihat error jika ada
    $error = $this->db->error();
    if ($error['code'] != 0) {
        echo "<br>Error: " . $error['message'];
    }
}
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Konsep "All or Nothing" dalam transaction
- [ ] ACID properties
- [ ] Syntax trans_start(), trans_complete(), trans_status()
- [ ] Perbedaan commit dan rollback
- [ ] Kapan menggunakan transaction
- [ ] Contoh use case (order, transfer, bulk import)

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="relationships.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Database Relationships
      </button>
    </a>
  </div>
  <div>
    <a href="practice.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Practice Lab →
      </button>
    </a>
  </div>
</div>
