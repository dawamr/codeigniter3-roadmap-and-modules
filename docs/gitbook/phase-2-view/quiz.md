# ❓ Quiz: Phase 2 - View & Template

## 🎯 Tentang Quiz Ini

Quiz ini menguji pemahaman Anda tentang View dan Template di CodeIgniter 3. Terdapat **15 pertanyaan**.

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### 📊 Passing Score

| Skor | Status |
|------|--------|
| **12-15** | 🟢 Excellent - Lanjut ke Phase 3! |
| **9-11** | 🟡 Good - Review materi yang kurang |
| **6-8** | 🟠 Need Review |
| **0-5** | 🔴 Ulangi Phase 2 |

</div>

---

## 🎨 Bagian 1: View Basics (5 Pertanyaan)

### Pertanyaan 1
**Dalam arsitektur MVC, apa tugas utama View?**

- [ ] A. Mengambil data dari database
- [ ] B. Memproses business logic
- [ ] C. Menampilkan data dalam format HTML
- [ ] D. Menerima dan memproses request

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Menampilkan data dalam format HTML**

**Penjelasan:**
- **Model** = Data (database)
- **View** = Presentation (tampilan HTML)
- **Controller** = Logic & koordinasi

View hanya fokus pada **bagaimana menampilkan data**, bukan dari mana data berasal atau bagaimana data diproses.

</details>

---

### Pertanyaan 2
**Di folder mana file view disimpan?**

- [ ] A. `system/views/`
- [ ] B. `application/views/`
- [ ] C. `public/views/`
- [ ] D. `views/`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `application/views/`**

**Penjelasan:**
Semua file view disimpan di `application/views/`. Strukturnya:

```
application/views/
├── home.php
├── templates/
│   ├── header.php
│   └── footer.php
└── user/
    └── profile.php
```

</details>

---

### Pertanyaan 3
**Syntax yang benar untuk memuat view adalah?**

- [ ] A. `$this->view('home');`
- [ ] B. `$this->load->view('home');`
- [ ] C. `load_view('home');`
- [ ] D. `include('views/home.php');`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$this->load->view('home');`**

**Penjelasan:**
CI3 menggunakan Loader class untuk memuat resources:

```php
// Load view
$this->load->view('home');

// Load view dengan data
$this->load->view('home', $data);

// Load view dari subfolder
$this->load->view('user/profile', $data);
```

</details>

---

### Pertanyaan 4
**Jika data dikirim dengan `$data['title'] = 'Hello'`, bagaimana mengaksesnya di view?**

- [ ] A. `<?= $data['title'] ?>`
- [ ] B. `<?= $title ?>`
- [ ] C. `<?= $this->data->title ?>`
- [ ] D. `<?= get('title') ?>`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `<?= $title ?>`**

**Penjelasan:**
CI3 otomatis meng-extract array `$data` menjadi variabel terpisah:

```php
// Controller
$data['title'] = 'Hello';
$data['user'] = $userObject;
$this->load->view('home', $data);

// View
<?= $title ?>  // 'Hello'
<?= $user->name ?>  // property dari object
```

</details>

---

### Pertanyaan 5
**Apa keuntungan short echo tag `<?= ?>` dibanding `<?php echo ?>`?**

- [ ] A. Lebih cepat diproses
- [ ] B. Lebih aman
- [ ] C. Lebih singkat dan mudah dibaca
- [ ] D. Tidak ada perbedaan

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Lebih singkat dan mudah dibaca**

**Penjelasan:**
```php
<!-- Short echo (recommended) -->
<p><?= $name ?></p>

<!-- Regular echo (lebih panjang) -->
<p><?php echo $name; ?></p>
```

Short echo tag membuat kode view lebih bersih dan mudah dibaca, terutama saat banyak output.

</details>

---

## 📊 Bagian 2: Passing Data & PHP in Views (5 Pertanyaan)

### Pertanyaan 6
**Bagaimana cara mengirim array dari controller ke view?**

- [ ] A. Tidak bisa mengirim array
- [ ] B. `$data['products'] = $products_array;`
- [ ] C. `$this->set('products', $products_array);`
- [ ] D. `$this->view->products = $products_array;`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$data['products'] = $products_array;`**

**Penjelasan:**
```php
// Controller
$data['products'] = [
    ['name' => 'Laptop', 'price' => 15000000],
    ['name' => 'Mouse', 'price' => 250000]
];
$this->load->view('products', $data);

// View
<?php foreach ($products as $product): ?>
    <p><?= $product['name'] ?> - Rp <?= $product['price'] ?></p>
<?php endforeach; ?>
```

</details>

---

### Pertanyaan 7
**Mana syntax alternative yang RECOMMENDED untuk foreach di view?**

- [ ] A. `<?php foreach ($items as $item) { ?> ... <?php } ?>`
- [ ] B. `<?php foreach ($items as $item): ?> ... <?php endforeach; ?>`
- [ ] C. `@foreach ($items as $item) ... @endforeach`
- [ ] D. `{{ foreach $items as $item }} ... {{ /foreach }}`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `<?php foreach ($items as $item): ?> ... <?php endforeach; ?>`**

**Penjelasan:**
Alternative syntax lebih mudah dibaca dalam HTML:

```php
<!-- ✅ Alternative syntax (recommended) -->
<?php foreach ($items as $item): ?>
    <li><?= $item ?></li>
<?php endforeach; ?>

<!-- Juga untuk if -->
<?php if ($condition): ?>
    <p>True</p>
<?php endif; ?>
```

</details>

---

### Pertanyaan 8
**Mengapa perlu escape output dari user input di view?**

- [ ] A. Untuk mempercepat loading
- [ ] B. Untuk mencegah XSS (Cross-Site Scripting) attack
- [ ] C. Untuk kompatibilitas browser
- [ ] D. Tidak perlu escape

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Untuk mencegah XSS (Cross-Site Scripting) attack**

**Penjelasan:**
```php
// Jika user input: <script>alert('hacked!')</script>

<!-- ❌ Berbahaya - script akan dieksekusi! -->
<p><?= $user_comment ?></p>

<!-- ✅ Aman - script ditampilkan sebagai teks -->
<p><?= htmlspecialchars($user_comment) ?></p>
<p><?= html_escape($user_comment) ?></p>
```

XSS attack bisa mencuri cookies, session, atau data sensitif lainnya.

</details>

---

### Pertanyaan 9
**Bagaimana cara menampilkan data jika ada, atau default jika tidak ada?**

- [ ] A. `<?= $name || 'Guest' ?>`
- [ ] B. `<?= $name ?? 'Guest' ?>`
- [ ] C. `<?= $name ? $name : 'Guest' ?>`
- [ ] D. B dan C keduanya benar

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: D. B dan C keduanya benar**

**Penjelasan:**
```php
<!-- Null coalescing operator (PHP 7+) - paling singkat -->
<?= $name ?? 'Guest' ?>

<!-- Ternary operator - juga benar -->
<?= isset($name) ? $name : 'Guest' ?>

<!-- ❌ Ini SALAH syntax -->
<?= $name || 'Guest' ?>
```

Null coalescing (`??`) adalah cara paling singkat dan recommended.

</details>

---

### Pertanyaan 10
**Bagaimana cara menampilkan harga dengan format ribuan?**

- [ ] A. `<?= $price ?>`
- [ ] B. `<?= format($price) ?>`
- [ ] C. `<?= number_format($price) ?>`
- [ ] D. `<?= money_format($price) ?>`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `<?= number_format($price) ?>`**

**Penjelasan:**
```php
$price = 1500000;

// Basic format
<?= number_format($price) ?>
// Output: 1,500,000

// Format Indonesia (dengan titik)
<?= number_format($price, 0, ',', '.') ?>
// Output: 1.500.000

// Dengan prefix
Rp <?= number_format($price, 0, ',', '.') ?>
// Output: Rp 1.500.000
```

</details>

---

## 🏗️ Bagian 3: Template System (5 Pertanyaan)

### Pertanyaan 11
**Apa keuntungan menggunakan template system (header + content + footer)?**

- [ ] A. Website lebih cepat
- [ ] B. Konsistensi layout dan menghindari duplikasi kode
- [ ] C. SEO lebih baik
- [ ] D. Lebih mudah di-debug

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Konsistensi layout dan menghindari duplikasi kode**

**Penjelasan:**
Dengan template:
- ✅ Header/footer hanya ditulis sekali
- ✅ Perubahan layout otomatis apply ke semua halaman
- ✅ DRY (Don't Repeat Yourself)
- ✅ Maintenance lebih mudah

```php
// Tanpa template - setiap halaman punya header/footer sendiri
// Dengan template - cukup panggil 3 views
$this->load->view('templates/header', $data);
$this->load->view('content', $data);
$this->load->view('templates/footer');
```

</details>

---

### Pertanyaan 12
**Bagaimana cara mendapatkan instance CI di custom library?**

- [ ] A. `$this->CI = new CI_Controller();`
- [ ] B. `$this->CI =& get_instance();`
- [ ] C. `$this->CI = CI::getInstance();`
- [ ] D. `$this->CI = $this;`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$this->CI =& get_instance();`**

**Penjelasan:**
```php
class Template {
    protected $CI;
    
    public function __construct() {
        // Mendapatkan reference ke CI instance
        $this->CI =& get_instance();
    }
    
    public function load($view, $data = []) {
        // Sekarang bisa akses CI features
        $this->CI->load->view('header', $data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('footer');
    }
}
```

`&` penting untuk mendapatkan reference, bukan copy.

</details>

---

### Pertanyaan 13
**Parameter ketiga TRUE pada `$this->load->view('name', $data, TRUE)` berfungsi untuk?**

- [ ] A. Enable caching
- [ ] B. Return view sebagai string (bukan output)
- [ ] C. Include CSS dan JS
- [ ] D. Enable debug mode

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Return view sebagai string (bukan output)**

**Penjelasan:**
```php
// Normal - langsung output ke browser
$this->load->view('home', $data);

// Return sebagai string
$html = $this->load->view('email/template', $data, TRUE);

// Berguna untuk:
// 1. Email body
$this->email->message($html);

// 2. PDF generation
$pdf->loadHtml($html);

// 3. AJAX response
echo $html;
```

</details>

---

### Pertanyaan 14
**Dimana sebaiknya meletakkan file CSS dan JavaScript?**

- [ ] A. `application/assets/`
- [ ] B. `system/assets/`
- [ ] C. `assets/` di root project (sejajar index.php)
- [ ] D. `application/views/assets/`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `assets/` di root project (sejajar index.php)**

**Penjelasan:**
```
project/
├── application/
├── system/
├── assets/          ← Di sini!
│   ├── css/
│   ├── js/
│   └── images/
└── index.php
```

File di `assets/` bisa diakses langsung oleh browser:
```php
<link rel="stylesheet" href="<?= base_url('assets/css/style.css') ?>">
```

File di `application/` tidak bisa diakses langsung (protected).

</details>

---

### Pertanyaan 15
**Bagaimana cara menampilkan flash message yang diset di controller?**

- [ ] A. `<?= $flash_message ?>`
- [ ] B. `<?= $this->session->flashdata('success') ?>`
- [ ] C. `<?= flash('success') ?>`
- [ ] D. `<?= get_flash('success') ?>`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `<?= $this->session->flashdata('success') ?>`**

**Penjelasan:**
```php
// Controller - set flash message
$this->session->set_flashdata('success', 'Data berhasil disimpan!');
redirect('user');

// View - tampilkan flash message
<?php if ($this->session->flashdata('success')): ?>
    <div class="alert alert-success">
        <?= $this->session->flashdata('success') ?>
    </div>
<?php endif; ?>
```

Flash message hanya tersedia untuk **satu request** berikutnya, lalu otomatis hilang.

</details>

---

## 📊 Hitung Skor Anda

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

### 📝 Scorecard

| Bagian | Benar | Total |
|--------|-------|-------|
| View Basics (1-5) | ___ | 5 |
| Passing Data & PHP (6-10) | ___ | 5 |
| Template System (11-15) | ___ | 5 |
| **TOTAL** | ___ | **15** |

</div>

---

## 🎯 Hasil Quiz

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; border-left: 4px solid #4CAF50;">
<h3>🟢 12-15 Benar</h3>
<p><strong>Excellent!</strong></p>
<p>Anda menguasai View & Template dengan baik!</p>
<a href="../phase-3-database/README.md">➡️ Lanjut ke Phase 3</a>
</div>

<div style="background: #FFF9C4; padding: 20px; border-radius: 10px; border-left: 4px solid #FDD835;">
<h3>🟡 9-11 Benar</h3>
<p><strong>Good!</strong></p>
<p>Hampir sempurna. Review bagian yang kurang.</p>
</div>

<div style="background: #FFE0B2; padding: 20px; border-radius: 10px; border-left: 4px solid #FF9800;">
<h3>🟠 6-8 Benar</h3>
<p><strong>Need Review</strong></p>
<a href="view-concept.md">📖 Review View Concept</a>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; border-left: 4px solid #F44336;">
<h3>🔴 0-5 Benar</h3>
<p><strong>Try Again</strong></p>
<a href="README.md">🔄 Ulangi Phase 2</a>
</div>

</div>

---

## 📚 Area yang Perlu Diperbaiki

| Jika salah di... | Review |
|------------------|--------|
| Pertanyaan 1-5 | [🎨 Understanding Views](view-concept.md) |
| Pertanyaan 6-10 | [📊 Passing Data](passing-data.md) & [🔄 PHP in Views](php-in-views.md) |
| Pertanyaan 11-15 | [🏗️ Template System](template-system.md) |

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
    <a href="../phase-3-database/README.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Phase 3: Database →
      </button>
    </a>
  </div>
</div>
