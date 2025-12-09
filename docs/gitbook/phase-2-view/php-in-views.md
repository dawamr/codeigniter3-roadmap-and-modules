# 🔄 PHP in Views

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Menggunakan PHP syntax yang tepat di views
- ✅ Membuat conditional display
- ✅ Menggunakan loops untuk list data
- ✅ Format dan escape output dengan aman

---

## 📋 PHP Syntax di Views

### Short Echo Tag

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Gunakan `<?= ?>` untuk output** - lebih pendek dan mudah dibaca!

</div>

```php
<!-- ✅ Recommended: Short echo tag -->
<p><?= $name ?></p>
<p><?= $user->email ?></p>
<p><?= $product['price'] ?></p>

<!-- ❌ Lebih panjang -->
<p><?php echo $name; ?></p>
```

### Alternative Syntax

CI3 merekomendasikan **alternative syntax** untuk control structures di views:

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Alternative Syntax (Recommended)</h4>

```php
<?php if ($condition): ?>
    <p>True</p>
<?php endif; ?>

<?php foreach ($items as $item): ?>
    <p><?= $item ?></p>
<?php endforeach; ?>
```

Lebih mudah dibaca dalam HTML!
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px;">
<h4>⚠️ Regular Syntax</h4>

```php
<?php if ($condition) { ?>
    <p>True</p>
<?php } ?>

<?php foreach ($items as $item) { ?>
    <p><?= $item ?></p>
<?php } ?>
```

Bisa digunakan, tapi kurang rapi.
</div>

</div>

---

## 🔀 Conditional Display

### If-Else

```php
<?php if ($is_logged_in): ?>
    <p>Selamat datang, <?= $username ?>!</p>
    <a href="<?= base_url('logout') ?>">Logout</a>
<?php else: ?>
    <a href="<?= base_url('login') ?>">Login</a>
    <a href="<?= base_url('register') ?>">Register</a>
<?php endif; ?>
```

### If-Elseif-Else

```php
<?php if ($user->role === 'admin'): ?>
    <span class="badge badge-red">Admin</span>
<?php elseif ($user->role === 'moderator'): ?>
    <span class="badge badge-blue">Moderator</span>
<?php else: ?>
    <span class="badge badge-gray">Member</span>
<?php endif; ?>
```

### Ternary Operator (Inline)

```php
<!-- Simple ternary -->
<p class="<?= $is_active ? 'active' : 'inactive' ?>">Status</p>

<!-- Null coalescing -->
<p><?= $user->nickname ?? $user->name ?></p>

<!-- Dengan default value -->
<img src="<?= $user->avatar ?? base_url('images/default-avatar.png') ?>">
```

### Multiple Conditions

```php
<?php if ($stock > 10): ?>
    <span class="text-green">Tersedia</span>
<?php elseif ($stock > 0): ?>
    <span class="text-orange">Stok Terbatas (<?= $stock ?>)</span>
<?php else: ?>
    <span class="text-red">Habis</span>
<?php endif; ?>
```

---

## 🔁 Loops

### Foreach (Paling Sering Digunakan)

```php
<!-- Array sederhana -->
<ul>
<?php foreach ($colors as $color): ?>
    <li><?= $color ?></li>
<?php endforeach; ?>
</ul>

<!-- Array dengan key -->
<dl>
<?php foreach ($user as $key => $value): ?>
    <dt><?= $key ?></dt>
    <dd><?= $value ?></dd>
<?php endforeach; ?>
</dl>

<!-- Array of objects -->
<div class="product-grid">
<?php foreach ($products as $product): ?>
    <div class="product-card">
        <h3><?= $product->name ?></h3>
        <p>Rp <?= number_format($product->price) ?></p>
    </div>
<?php endforeach; ?>
</div>
```

### Foreach dengan Index

```php
<table>
<?php foreach ($users as $index => $user): ?>
    <tr class="<?= $index % 2 == 0 ? 'even' : 'odd' ?>">
        <td><?= $index + 1 ?></td>
        <td><?= $user->name ?></td>
    </tr>
<?php endforeach; ?>
</table>
```

### For Loop

```php
<!-- Pagination numbers -->
<nav>
<?php for ($i = 1; $i <= $total_pages; $i++): ?>
    <a href="?page=<?= $i ?>" class="<?= $i == $current_page ? 'active' : '' ?>">
        <?= $i ?>
    </a>
<?php endfor; ?>
</nav>
```

### While Loop (Jarang di Views)

```php
<?php while ($row = $query->unbuffered_row()): ?>
    <p><?= $row->name ?></p>
<?php endwhile; ?>
```

---

## 🛡️ Output Escaping (Keamanan)

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; margin: 20px 0;">

⚠️ **PENTING: Selalu escape output dari user input!**

Tanpa escaping, aplikasi rentan terhadap **XSS (Cross-Site Scripting)** attack.

</div>

### Mengapa Perlu Escape?

```php
// Jika user input: <script>alert('hacked!')</script>

<!-- ❌ BERBAHAYA -->
<p><?= $user_input ?></p>
<!-- Output: <script>alert('hacked!')</script> (DIEKSEKUSI!) -->

<!-- ✅ AMAN -->
<p><?= htmlspecialchars($user_input) ?></p>
<!-- Output: &lt;script&gt;alert('hacked!')&lt;/script&gt; (Ditampilkan sebagai teks) -->
```

### Cara Escape di CI3

```php
<!-- Cara 1: htmlspecialchars() -->
<p><?= htmlspecialchars($comment) ?></p>

<!-- Cara 2: html_escape() - CI3 helper -->
<p><?= html_escape($comment) ?></p>

<!-- Cara 3: Di form value -->
<input type="text" value="<?= html_escape($name) ?>">

<!-- Cara 4: Dengan set_value() untuk form -->
<input type="text" name="name" value="<?= set_value('name') ?>">
```

### Kapan Perlu Escape?

| Data Source | Perlu Escape? |
|-------------|---------------|
| User input (form, URL) | ✅ WAJIB |
| Database dari user | ✅ WAJIB |
| Static text dari code | ❌ Tidak perlu |
| Data dari API external | ✅ WAJIB |
| Config/Constants | ❌ Tidak perlu |

---

## 🎨 Formatting Output

### Number Format

```php
<!-- Harga dengan ribuan -->
<p>Rp <?= number_format($price, 0, ',', '.') ?></p>
<!-- Output: Rp 1.500.000 -->

<!-- Dengan desimal -->
<p><?= number_format($value, 2) ?></p>
<!-- Output: 1,500.00 -->
```

### Date Format

```php
<!-- Format tanggal -->
<p><?= date('d M Y', strtotime($created_at)) ?></p>
<!-- Output: 15 Jan 2024 -->

<!-- Format lengkap -->
<p><?= date('l, d F Y H:i', strtotime($updated_at)) ?></p>
<!-- Output: Monday, 15 January 2024 14:30 -->

<!-- Relative time (dengan helper) -->
<p><?= timespan(strtotime($created_at)) ?> ago</p>
```

### Text Truncate

```php
<!-- Dengan PHP -->
<p><?= strlen($content) > 100 ? substr($content, 0, 100) . '...' : $content ?></p>

<!-- Dengan CI3 Text Helper -->
<?php $this->load->helper('text'); ?>
<p><?= word_limiter($content, 25) ?></p>
<p><?= character_limiter($content, 100) ?></p>
```

### Custom Helper untuk Format

```php
<!-- Jika sudah load my_helper -->
<p><?= format_rupiah($price) ?></p>
<p><?= format_tanggal($date) ?></p>
<p><?= time_ago($created_at) ?></p>
```

---

## 📝 Praktik Terbaik

### 1. Minimal Logic

```php
<!-- ✅ GOOD: Simple conditionals dan loops -->
<?php if ($user): ?>
    <p><?= $user->name ?></p>
<?php endif; ?>

<!-- ❌ BAD: Complex logic -->
<?php
$price = $product->price;
if ($user->is_member) {
    $discount = $user->years > 5 ? 0.2 : 0.1;
    $price = $price * (1 - $discount);
}
?>
<p><?= $price ?></p>

<!-- ✅ BETTER: Calculate in controller, display in view -->
<p><?= $final_price ?></p>
```

### 2. Consistent Indentation

```php
<!-- ✅ GOOD: Proper indentation -->
<ul>
    <?php foreach ($items as $item): ?>
        <li>
            <span><?= $item->name ?></span>
            <?php if ($item->is_new): ?>
                <span class="badge">New</span>
            <?php endif; ?>
        </li>
    <?php endforeach; ?>
</ul>
```

### 3. Use Partials untuk Repetitive Code

```php
<!-- Daripada copy-paste -->
<?php foreach ($products as $product): ?>
    <?php $this->load->view('partials/product_card', ['product' => $product]); ?>
<?php endforeach; ?>
```

---

## 🔧 Common Patterns

### Empty State

```php
<?php if (!empty($products)): ?>
    <div class="product-grid">
        <?php foreach ($products as $product): ?>
            <div class="product-card">
                <h3><?= $product->name ?></h3>
            </div>
        <?php endforeach; ?>
    </div>
<?php else: ?>
    <div class="empty-state">
        <img src="<?= base_url('images/empty.svg') ?>">
        <h3>Tidak ada produk</h3>
        <p>Belum ada produk yang ditambahkan.</p>
        <a href="<?= base_url('product/create') ?>" class="btn">Tambah Produk</a>
    </div>
<?php endif; ?>
```

### Flash Messages

```php
<?php if ($this->session->flashdata('success')): ?>
    <div class="alert alert-success">
        <?= $this->session->flashdata('success') ?>
    </div>
<?php endif; ?>

<?php if ($this->session->flashdata('error')): ?>
    <div class="alert alert-error">
        <?= $this->session->flashdata('error') ?>
    </div>
<?php endif; ?>
```

### Active Menu

```php
<nav>
    <a href="/" class="<?= $active_menu == 'home' ? 'active' : '' ?>">Home</a>
    <a href="/products" class="<?= $active_menu == 'products' ? 'active' : '' ?>">Products</a>
    <a href="/about" class="<?= $active_menu == 'about' ? 'active' : '' ?>">About</a>
</nav>
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Short echo tag `<?= ?>`
- [ ] Alternative syntax untuk if/foreach
- [ ] Conditional display (if, ternary)
- [ ] Loops (foreach, for)
- [ ] Output escaping untuk keamanan
- [ ] Formatting (number, date, text)
- [ ] Common patterns (empty state, flash message)

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="passing-data.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Passing Data
      </button>
    </a>
  </div>
  <div>
    <a href="template-system.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Template System →
      </button>
    </a>
  </div>
</div>
