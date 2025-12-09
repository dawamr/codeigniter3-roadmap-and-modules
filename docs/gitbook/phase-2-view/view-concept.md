# 🎨 Understanding Views

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami konsep View dalam MVC
- ✅ Mengetahui peran dan tanggung jawab View
- ✅ Membandingkan View CI3 dengan PHP Native
- ✅ Memahami separation of concerns

---

## 🤔 Apa itu View?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**View** adalah komponen yang bertanggung jawab untuk **menampilkan data** ke user dalam bentuk HTML.

**Analogi:** View seperti **layar TV** 📺
- TV tidak tahu dari mana siaran berasal (controller/model)
- TV hanya menampilkan apa yang diberikan kepadanya
- TV fokus pada **presentasi** yang baik

</div>

---

## 🔄 View dalam Arsitektur MVC

```
┌─────────────────────────────────────────────────────────────────┐
│                       🎮 CONTROLLER                              │
│                                                                  │
│   $data['title'] = 'User Profile';                               │
│   $data['user'] = $this->user_model->get(5);                    │
│   $this->load->view('user/profile', $data);                     │
│                            │                                     │
└────────────────────────────│─────────────────────────────────────┘
                             │
                             ▼ Data dikirim ke View
┌─────────────────────────────────────────────────────────────────┐
│                         👁️ VIEW                                  │
│                    (user/profile.php)                            │
│                                                                  │
│   <h1><?= $title ?></h1>                                        │
│   <p>Nama: <?= $user->name ?></p>                               │
│   <p>Email: <?= $user->email ?></p>                             │
│                                                                  │
│   🎨 Fokus: Tampilan, layout, styling                           │
└─────────────────────────────────────────────────────────────────┘
                             │
                             ▼ HTML dikirim ke browser
┌─────────────────────────────────────────────────────────────────┐
│                        🌐 BROWSER                                │
│                                                                  │
│   <h1>User Profile</h1>                                         │
│   <p>Nama: John Doe</p>                                         │
│   <p>Email: john@mail.com</p>                                   │
└─────────────────────────────────────────────────────────────────┘
```

---

## 📊 Perbandingan: PHP Native vs CI3 View

### 🔴 PHP Native (Kode Tercampur)

```php
<?php
// user_profile.php - SEMUA tercampur dalam 1 file!

// Koneksi database
$conn = mysqli_connect("localhost", "root", "", "myapp");

// Query data
$id = $_GET['id'];
$query = "SELECT * FROM users WHERE id = $id";
$result = mysqli_query($conn, $query);
$user = mysqli_fetch_assoc($result);
?>
<!DOCTYPE html>
<html>
<head>
    <title>Profile</title>
</head>
<body>
    <h1><?= $user['name'] ?></h1>
    <p>Email: <?= $user['email'] ?></p>
</body>
</html>
```

**Masalah:**
- ❌ Logic dan tampilan tercampur
- ❌ Sulit di-maintain
- ❌ Tidak bisa reuse
- ❌ Designer sulit edit (harus paham PHP)

---

### 🟢 CI3 View (Terpisah & Bersih)

**Controller:**
```php
<?php
// controllers/User.php
public function profile($id) {
    $data['user'] = $this->user_model->get_by_id($id);
    $data['title'] = 'User Profile';
    
    $this->load->view('user/profile', $data);
}
```

**View:**
```php
<!-- views/user/profile.php -->
<!DOCTYPE html>
<html>
<head>
    <title><?= $title ?></title>
</head>
<body>
    <h1><?= $user->name ?></h1>
    <p>Email: <?= $user->email ?></p>
</body>
</html>
```

**Keunggulan:**
- ✅ Kode terorganisir
- ✅ Mudah di-maintain
- ✅ View bisa di-reuse
- ✅ Designer bisa edit tanpa merusak logic

---

## 🎯 Tanggung Jawab View

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px;">
<h3>✅ Yang Harus Dilakukan View</h3>
<ul>
<li>Menampilkan data dari Controller</li>
<li>Membuat struktur HTML</li>
<li>Menerapkan CSS styling</li>
<li>Loop untuk menampilkan list</li>
<li>Conditional display (if ada data)</li>
<li>Format output (tanggal, angka)</li>
</ul>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px;">
<h3>❌ Yang TIDAK Boleh di View</h3>
<ul>
<li>Query database</li>
<li>Business logic kompleks</li>
<li>Session manipulation</li>
<li>Redirect</li>
<li>Validasi data</li>
<li>Koneksi API</li>
</ul>
</div>

</div>

### 💡 Prinsip: "Dumb View"

View seharusnya **"bodoh"** - hanya tahu cara menampilkan data, tidak tahu logika bisnis.

```php
<!-- ❌ BAD: Logic di View -->
<?php
$discount = 0;
if ($user->is_member) {
    if ($user->membership_years > 5) {
        $discount = 0.2;
    } else if ($user->total_purchase > 1000000) {
        $discount = 0.15;
    } else {
        $discount = 0.1;
    }
}
$final_price = $product->price * (1 - $discount);
?>
<p>Harga: <?= $final_price ?></p>

<!-- ✅ GOOD: Logic di Controller/Model, View hanya tampilkan -->
<p>Harga: <?= $final_price ?></p>
```

---

## 📁 Lokasi File View

```
application/
└── views/
    ├── welcome_message.php      # View default
    │
    ├── templates/               # Template layout
    │   ├── header.php
    │   ├── footer.php
    │   └── sidebar.php
    │
    ├── user/                    # Views untuk user
    │   ├── index.php            # List users
    │   ├── profile.php          # User detail
    │   └── edit.php             # Edit form
    │
    ├── product/                 # Views untuk product
    │   ├── index.php
    │   ├── detail.php
    │   └── cart.php
    │
    └── errors/                  # Error pages
        ├── 404.php
        └── 500.php
```

---

## 🔧 Jenis-jenis View

### 1️⃣ Page View

View untuk satu halaman lengkap:

```php
<!-- views/home.php -->
<!DOCTYPE html>
<html>
<head>
    <title>Home</title>
</head>
<body>
    <h1>Welcome</h1>
    <p>Selamat datang di website kami.</p>
</body>
</html>
```

### 2️⃣ Partial View

View untuk bagian halaman (reusable):

```php
<!-- views/partials/user_card.php -->
<div class="user-card">
    <img src="<?= $user->avatar ?>">
    <h3><?= $user->name ?></h3>
    <p><?= $user->email ?></p>
</div>
```

### 3️⃣ Template View

View untuk layout/kerangka:

```php
<!-- views/templates/header.php -->
<!DOCTYPE html>
<html>
<head>
    <title><?= $title ?></title>
    <link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
</head>
<body>
    <nav><!-- Navigation --></nav>
    <main>
```

```php
<!-- views/templates/footer.php -->
    </main>
    <footer><!-- Footer --></footer>
    <script src="<?= base_url('assets/js/script.js') ?>"></script>
</body>
</html>
```

---

## 💡 Best Practices

### 1. Gunakan Short Echo Tag

```php
<!-- ✅ Recommended -->
<p><?= $name ?></p>
<p><?= $user->email ?></p>

<!-- ❌ Lebih panjang -->
<p><?php echo $name; ?></p>
```

### 2. Escape Output untuk Keamanan

```php
<!-- ✅ Aman dari XSS -->
<p><?= htmlspecialchars($user_input) ?></p>
<p><?= html_escape($user_input) ?></p>

<!-- ❌ Berbahaya jika data dari user -->
<p><?= $user_input ?></p>
```

### 3. Pisahkan Logic dan Presentation

```php
<!-- ✅ Minimal logic -->
<?php if ($users): ?>
    <?php foreach ($users as $user): ?>
        <p><?= $user->name ?></p>
    <?php endforeach; ?>
<?php else: ?>
    <p>Tidak ada data.</p>
<?php endif; ?>
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] View hanya untuk menampilkan data
- [ ] Perbedaan PHP Native dan CI3 View
- [ ] Prinsip "Dumb View"
- [ ] Lokasi dan struktur folder views
- [ ] Jenis-jenis view (page, partial, template)
- [ ] Best practices (short tag, escape output)

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
    <a href="loading-views.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Loading Views →
      </button>
    </a>
  </div>
</div>
