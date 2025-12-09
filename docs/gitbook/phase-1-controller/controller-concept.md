# 🎯 Understanding Controllers

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami konsep Controller dalam MVC
- ✅ Mengetahui peran dan tanggung jawab Controller
- ✅ Membandingkan Controller dengan PHP Native
- ✅ Memahami bagaimana Controller menghubungkan Model dan View

---

## 🤔 Apa itu Controller?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Controller** adalah **otak** dari aplikasi yang:
- 📥 Menerima request dari user
- 🧠 Memproses logika bisnis
- 💾 Berinteraksi dengan Model (database)
- 📤 Mengirim data ke View (tampilan)

**Analogi:** Controller seperti **pelayan restoran** 🍽️
- Menerima pesanan dari pelanggan (request)
- Menyampaikan ke dapur (model)
- Mengambil makanan dari dapur
- Menyajikan ke pelanggan (view)

</div>

---

## 🔄 Controller dalam Arsitektur MVC

```
┌─────────────────────────────────────────────────────────────────┐
│                         USER REQUEST                             │
│                   http://site.com/product/detail/5               │
└─────────────────────────────────────────────────────────────────┘
                                │
                                ▼
┌─────────────────────────────────────────────────────────────────┐
│                        🎮 CONTROLLER                             │
│                    (Product.php → detail)                        │
│                                                                  │
│   1. Terima request dengan parameter id=5                        │
│   2. Load Model untuk ambil data produk                          │
│   3. Proses logika (cek stok, hitung diskon, dll)               │
│   4. Kirim data ke View untuk ditampilkan                        │
└─────────────────────────────────────────────────────────────────┘
                    │                       │
                    ▼                       ▼
    ┌───────────────────────┐   ┌───────────────────────┐
    │      💾 MODEL         │   │       👁️ VIEW         │
    │                       │   │                       │
    │ • Query database      │   │ • Terima data         │
    │ • Return data produk  │   │ • Render HTML         │
    │                       │   │ • Output ke browser   │
    └───────────────────────┘   └───────────────────────┘
```

---

## 📊 Perbandingan: PHP Native vs CI3 Controller

### 🔴 PHP Native (Tanpa Framework)

```php
<?php
// product_detail.php

// 1. Koneksi database manual
$conn = mysqli_connect("localhost", "root", "", "toko");

// 2. Ambil parameter dari URL
$id = $_GET['id'] ?? 0;

// 3. Query database
$query = "SELECT * FROM products WHERE id = $id";  // ⚠️ SQL Injection!
$result = mysqli_query($conn, $query);
$product = mysqli_fetch_assoc($result);

// 4. Tampilkan HTML (tercampur dengan logic)
?>
<!DOCTYPE html>
<html>
<body>
    <h1><?= $product['name'] ?></h1>
    <p>Harga: Rp <?= $product['price'] ?></p>
</body>
</html>
```

**Masalah PHP Native:**
- ❌ Kode tercampur (logic + HTML)
- ❌ Koneksi database berulang
- ❌ Rentan SQL Injection
- ❌ Sulit di-maintain saat project besar

---

### 🟢 CI3 Controller (Dengan Framework)

```php
<?php
// application/controllers/Product.php

class Product extends CI_Controller {
    
    public function __construct() {
        parent::__construct();
        $this->load->model('product_model');
    }
    
    public function detail($id) {
        // 1. Ambil data dari Model (aman dari SQL Injection)
        $data['product'] = $this->product_model->get_by_id($id);
        
        // 2. Cek apakah produk ada
        if (!$data['product']) {
            show_404();
        }
        
        // 3. Kirim ke View
        $data['title'] = $data['product']->name;
        $this->load->view('templates/header', $data);
        $this->load->view('product/detail', $data);
        $this->load->view('templates/footer');
    }
}
```

**Keunggulan CI3:**
- ✅ Kode terstruktur (MVC)
- ✅ Query Builder aman dari SQL Injection
- ✅ Reusable components
- ✅ Mudah di-maintain

---

## 🎯 Tanggung Jawab Controller

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px;">
<h3>✅ Yang Harus Dilakukan</h3>
<ul>
<li>Menerima dan memvalidasi input</li>
<li>Memanggil Model untuk data</li>
<li>Mengatur flow aplikasi</li>
<li>Memilih View yang tepat</li>
<li>Mengirim data ke View</li>
<li>Handle redirect dan response</li>
</ul>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px;">
<h3>❌ Yang Tidak Boleh Dilakukan</h3>
<ul>
<li>Query database langsung</li>
<li>Menulis HTML di controller</li>
<li>Logika bisnis kompleks</li>
<li>Manipulasi view/tampilan</li>
<li>Terlalu banyak kode (Fat Controller)</li>
</ul>
</div>

</div>

### 🎯 Prinsip: "Thin Controller, Fat Model"

```php
<?php
// ❌ BAD: Fat Controller
class Order extends CI_Controller {
    public function create() {
        // Terlalu banyak logic di controller!
        $total = 0;
        foreach ($items as $item) {
            $total += $item['price'] * $item['qty'];
        }
        $tax = $total * 0.1;
        $discount = $this->calculate_discount($total);
        $grand_total = $total + $tax - $discount;
        // ... 100 baris logic lainnya
    }
}

// ✅ GOOD: Thin Controller
class Order extends CI_Controller {
    public function create() {
        // Delegasikan ke Model
        $order = $this->order_model->create_order($items);
        
        if ($order) {
            redirect('order/success/' . $order->id);
        } else {
            $this->session->set_flashdata('error', 'Gagal membuat order');
            redirect('cart');
        }
    }
}
```

---

## 📍 Lokasi File Controller

```
application/
└── controllers/
    ├── Welcome.php           # Controller default
    ├── User.php              # Controller untuk user
    ├── Product.php           # Controller untuk produk
    ├── Order.php             # Controller untuk order
    │
    └── admin/                # Sub-folder untuk admin
        ├── Dashboard.php
        ├── Users.php
        └── Products.php
```

---

## 🔗 URL to Controller Mapping

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| URL | Controller | File | Method |
|-----|------------|------|--------|
| `/welcome` | Welcome | Welcome.php | index() |
| `/user` | User | User.php | index() |
| `/user/profile` | User | User.php | profile() |
| `/user/profile/5` | User | User.php | profile(5) |
| `/product/detail/123` | Product | Product.php | detail(123) |
| `/admin/dashboard` | admin/Dashboard | admin/Dashboard.php | index() |
| `/admin/users/edit/42` | admin/Users | admin/Users.php | edit(42) |

</div>

**Pola URL CI3:**
```
http://domain.com/[controller]/[method]/[param1]/[param2]/...
```

---

## 💡 Kapan Membuat Controller Baru?

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Buat controller baru ketika:**
- 📦 Mengelola **resource/entitas** berbeda (User, Product, Order)
- 🔐 Butuh **akses kontrol** berbeda (Admin, API, Public)
- 📂 Grouping **fitur terkait** (Auth: login, register, logout)

**Contoh pembagian:**
- `User.php` → CRUD user, profile, settings
- `Product.php` → CRUD produk, kategori
- `Order.php` → Checkout, payment, history
- `Auth.php` → Login, register, forgot password
- `Api.php` → REST API endpoints

</div>

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Controller adalah penghubung antara Model dan View
- [ ] Controller menerima request dan menentukan response
- [ ] Perbedaan PHP Native dan CI3 Controller
- [ ] Prinsip "Thin Controller, Fat Model"
- [ ] URL mapping ke controller dan method
- [ ] Kapan perlu membuat controller baru

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="README.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Overview
      </button>
    </a>
  </div>
  <div>
    <a href="controller-anatomy.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Controller Anatomy →
      </button>
    </a>
  </div>
</div>
