# ❓ Quiz: Phase 1 - Controller & Routing

## 🎯 Tentang Quiz Ini

Quiz ini menguji pemahaman Anda tentang Controller dan Routing di CodeIgniter 3. Terdapat **15 pertanyaan**.

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### 📊 Passing Score

| Skor | Status |
|------|--------|
| **12-15** | 🟢 Excellent - Lanjut ke Phase 2! |
| **9-11** | 🟡 Good - Review materi yang kurang |
| **6-8** | 🟠 Need Review |
| **0-5** | 🔴 Ulangi Phase 1 |

</div>

---

## 🎮 Bagian 1: Controller Basics (5 Pertanyaan)

### Pertanyaan 1
**Dalam arsitektur MVC, apa peran utama Controller?**

- [ ] A. Menyimpan data ke database
- [ ] B. Menampilkan HTML ke browser
- [ ] C. Menerima request dan mengkoordinasikan Model dan View
- [ ] D. Membuat koneksi database

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Menerima request dan mengkoordinasikan Model dan View**

**Penjelasan:**
Controller adalah "otak" yang:
- Menerima request dari user
- Memanggil Model untuk data (jika perlu)
- Memilih View yang tepat
- Mengirim data ke View

Model = Data, View = Tampilan, Controller = Koordinator

</details>

---

### Pertanyaan 2
**Baris kode apa yang WAJIB ada di setiap file controller CI3?**

- [ ] A. `<?php echo "Hello"; ?>`
- [ ] B. `defined('BASEPATH') OR exit('No direct script access allowed');`
- [ ] C. `include 'system/core/Controller.php';`
- [ ] D. `namespace App\Controllers;`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `defined('BASEPATH') OR exit('No direct script access allowed');`**

**Penjelasan:**
Security line ini mencegah akses langsung ke file PHP. Tanpa ini, file controller bisa diakses langsung via URL:
```
http://site.com/application/controllers/User.php  ❌ Berbahaya!
```

Dengan security line, akses langsung akan ditolak.

</details>

---

### Pertanyaan 3
**Apa yang harus dipanggil di dalam constructor controller?**

- [ ] A. `$this->load->model()`
- [ ] B. `parent::__construct()`
- [ ] C. `CI_Controller::init()`
- [ ] D. `require_once('system/core/Controller.php')`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `parent::__construct()`**

**Penjelasan:**
```php
public function __construct() {
    parent::__construct();  // WAJIB!
    
    // Inisialisasi lainnya...
}
```

`parent::__construct()` memanggil constructor dari `CI_Controller` yang mengaktifkan semua fitur CI3 seperti `$this->load`, `$this->input`, dll.

</details>

---

### Pertanyaan 4
**Jika URL adalah `/product/detail/5`, controller mana yang dipanggil?**

- [ ] A. `Detail.php`
- [ ] B. `Product.php`
- [ ] C. `5.php`
- [ ] D. `Product_detail.php`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `Product.php`**

**Penjelasan:**
Pola URL CI3: `/[controller]/[method]/[param]`
- Segment 1: `product` → Controller `Product.php`
- Segment 2: `detail` → Method `detail()`
- Segment 3: `5` → Parameter `$id = 5`

```php
// Product.php
public function detail($id) {  // $id = 5
    // ...
}
```

</details>

---

### Pertanyaan 5
**Method mana yang dipanggil jika URL hanya `/user`?**

- [ ] A. `user()`
- [ ] B. `default()`
- [ ] C. `index()`
- [ ] D. `home()`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `index()`**

**Penjelasan:**
Jika URL tidak menyebutkan method, CI3 memanggil `index()` sebagai method default.

| URL | Controller | Method |
|-----|------------|--------|
| `/user` | User | `index()` |
| `/user/index` | User | `index()` |
| `/product` | Product | `index()` |

</details>

---

## 🔧 Bagian 2: Methods & Parameters (5 Pertanyaan)

### Pertanyaan 6
**Untuk URL `/blog/post/2024/01/hello-world`, bagaimana method yang benar?**

- [ ] A. `public function post($data) { }`
- [ ] B. `public function post($year, $month, $slug) { }`
- [ ] C. `public function post() { }`
- [ ] D. `public function 2024($month, $slug) { }`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `public function post($year, $month, $slug) { }`**

**Penjelasan:**
Setiap segment setelah method menjadi parameter:
```
/blog/post/2024/01/hello-world
      │     │    │    │
      │     │    │    └── $slug = 'hello-world'
      │     │    └── $month = '01'
      │     └── $year = '2024'
      └── method = post()
```

</details>

---

### Pertanyaan 7
**Cara yang AMAN untuk mengambil data POST adalah?**

- [ ] A. `$_POST['username']`
- [ ] B. `$this->post->get('username')`
- [ ] C. `$this->input->post('username')`
- [ ] D. `post('username')`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `$this->input->post('username')`**

**Penjelasan:**
`$this->input->post()` lebih aman karena:
- Mengembalikan NULL jika tidak ada (bukan error)
- Bisa tambah XSS filter: `$this->input->post('username', TRUE)`
- Konsisten dengan coding style CI3

```php
// ✅ Recommended
$username = $this->input->post('username');

// ❌ Tidak recommended
$username = $_POST['username'];  // Bisa error jika tidak ada
```

</details>

---

### Pertanyaan 8
**Bagaimana cara redirect ke halaman lain di CI3?**

- [ ] A. `header('Location: /user')`
- [ ] B. `$this->redirect('user')`
- [ ] C. `redirect('user')`
- [ ] D. `$this->load->redirect('user')`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `redirect('user')`**

**Penjelasan:**
`redirect()` adalah fungsi dari URL helper:

```php
$this->load->helper('url');  // Atau autoload

redirect('user');                 // ke /user
redirect('user/profile');         // ke /user/profile
redirect('https://google.com');   // ke external URL
```

</details>

---

### Pertanyaan 9
**Apa output dari kode ini jika URL adalah `/test/greet`?**
```php
public function greet($name = 'Guest') {
    echo "Hello, $name!";
}
```

- [ ] A. Error: Missing argument
- [ ] B. Hello, !
- [ ] C. Hello, Guest!
- [ ] D. Hello, null!

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Hello, Guest!**

**Penjelasan:**
Parameter dengan default value akan menggunakan nilai default jika tidak disediakan di URL.

| URL | Output |
|-----|--------|
| `/test/greet` | Hello, Guest! |
| `/test/greet/John` | Hello, John! |

</details>

---

### Pertanyaan 10
**Bagaimana cara return JSON response untuk AJAX/API?**

- [ ] A. `echo json_encode($data);`
- [ ] B. `return json_encode($data);`
- [ ] C. `$this->output->set_content_type('application/json')->set_output(json_encode($data));`
- [ ] D. `$this->json($data);`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `$this->output->set_content_type('application/json')->set_output(json_encode($data));`**

**Penjelasan:**
Cara yang benar untuk API response:

```php
$response = ['status' => 'success', 'data' => $users];

$this->output
    ->set_status_header(200)
    ->set_content_type('application/json')
    ->set_output(json_encode($response));
```

Ini memastikan:
- Content-Type header benar
- Status code bisa diset
- Output terformat dengan benar

</details>

---

## 🛣️ Bagian 3: Routing (5 Pertanyaan)

### Pertanyaan 11
**Di file mana konfigurasi routing berada?**

- [ ] A. `application/config/config.php`
- [ ] B. `application/config/routes.php`
- [ ] C. `application/routes.php`
- [ ] D. `system/config/routes.php`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `application/config/routes.php`**

**Penjelasan:**
File `routes.php` berisi:
- Default controller
- 404 override
- Custom routes

```php
// application/config/routes.php
$route['default_controller'] = 'home';
$route['404_override'] = 'errors/not_found';
$route['about'] = 'pages/about';
```

</details>

---

### Pertanyaan 12
**Apa arti route `$route['product/(:num)'] = 'catalog/view/$1';`?**

- [ ] A. URL `/product/abc` → `catalog/view/abc`
- [ ] B. URL `/product/123` → `catalog/view/123`
- [ ] C. URL `/catalog/view/123` → `product/123`
- [ ] D. URL `/product/` → `catalog/view/`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. URL `/product/123` → `catalog/view/123`**

**Penjelasan:**
- `(:num)` = hanya cocok dengan angka
- `$1` = nilai yang ditangkap oleh wildcard pertama

```php
$route['product/(:num)'] = 'catalog/view/$1';

// /product/123 → catalog/view(123) ✅
// /product/abc → TIDAK match (bukan angka) ❌
```

</details>

---

### Pertanyaan 13
**Perbedaan `(:num)` dan `(:any)` adalah?**

- [ ] A. Tidak ada perbedaan
- [ ] B. `(:num)` hanya angka, `(:any)` semua karakter
- [ ] C. `(:num)` lebih cepat
- [ ] D. `(:any)` hanya huruf

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `(:num)` hanya angka, `(:any)` semua karakter**

**Penjelasan:**

| Wildcard | Match | Contoh |
|----------|-------|--------|
| `(:num)` | Angka saja | `123`, `456` |
| `(:any)` | Semua karakter | `hello`, `my-post`, `123` |

```php
$route['user/(:num)'] = 'user/profile/$1';
// /user/5 ✅
// /user/john ❌

$route['blog/(:any)'] = 'blog/post/$1';
// /blog/5 ✅
// /blog/hello-world ✅
```

</details>

---

### Pertanyaan 14
**Untuk REST API, bagaimana membuat route yang hanya untuk method POST?**

- [ ] A. `$route['api/users']['POST'] = 'api/users/create';`
- [ ] B. `$route['api/users'] = 'api/users/create';`
- [ ] C. `$route['POST:api/users'] = 'api/users/create';`
- [ ] D. `$route['api/users']->post('api/users/create');`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: A. `$route['api/users']['POST'] = 'api/users/create';`**

**Penjelasan:**
CI3 mendukung HTTP verb-based routing:

```php
$route['api/users']['GET'] = 'api/users/index';     // List
$route['api/users']['POST'] = 'api/users/create';   // Create
$route['api/users/(:num)']['PUT'] = 'api/users/update/$1';    // Update
$route['api/users/(:num)']['DELETE'] = 'api/users/delete/$1'; // Delete
```

</details>

---

### Pertanyaan 15
**Mengapa urutan routes di routes.php penting?**

- [ ] A. Routes harus alphabetical
- [ ] B. Routes diproses dari bawah ke atas
- [ ] C. Routes diproses dari atas ke bawah, yang pertama match digunakan
- [ ] D. Urutan tidak penting

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Routes diproses dari atas ke bawah, yang pertama match digunakan**

**Penjelasan:**
```php
// ❌ SALAH - (:any) terlalu di atas
$route['blog/(:any)'] = 'blog/post/$1';
$route['blog/category/(:any)'] = 'blog/category/$1';  // Tidak pernah tercapai!

// ✅ BENAR - Spesifik dulu
$route['blog/category/(:any)'] = 'blog/category/$1';
$route['blog/(:any)'] = 'blog/post/$1';  // Fallback
```

Route yang lebih spesifik harus di atas, yang general di bawah.

</details>

---

## 📊 Hitung Skor Anda

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

### 📝 Scorecard

| Bagian | Benar | Total |
|--------|-------|-------|
| Controller Basics (1-5) | ___ | 5 |
| Methods & Parameters (6-10) | ___ | 5 |
| Routing (11-15) | ___ | 5 |
| **TOTAL** | ___ | **15** |

</div>

---

## 🎯 Hasil Quiz

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; border-left: 4px solid #4CAF50;">
<h3>🟢 12-15 Benar</h3>
<p><strong>Excellent!</strong></p>
<p>Anda menguasai Controller & Routing dengan baik!</p>
<a href="../phase-2-view/README.md">➡️ Lanjut ke Phase 2</a>
</div>

<div style="background: #FFF9C4; padding: 20px; border-radius: 10px; border-left: 4px solid #FDD835;">
<h3>🟡 9-11 Benar</h3>
<p><strong>Good!</strong></p>
<p>Hampir sempurna. Review bagian yang kurang.</p>
</div>

<div style="background: #FFE0B2; padding: 20px; border-radius: 10px; border-left: 4px solid #FF9800;">
<h3>🟠 6-8 Benar</h3>
<p><strong>Need Review</strong></p>
<a href="controller-concept.md">📖 Review Controller</a>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; border-left: 4px solid #F44336;">
<h3>🔴 0-5 Benar</h3>
<p><strong>Try Again</strong></p>
<a href="README.md">🔄 Ulangi Phase 1</a>
</div>

</div>

---

## 📚 Area yang Perlu Diperbaiki

| Jika salah di... | Review |
|------------------|--------|
| Pertanyaan 1-5 | [🎯 Understanding Controllers](controller-concept.md) |
| Pertanyaan 6-10 | [🔧 Methods & Parameters](methods-parameters.md) |
| Pertanyaan 11-15 | [🛣️ Routing System](routing.md) |

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
    <a href="../phase-2-view/README.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Phase 2: View →
      </button>
    </a>
  </div>
</div>
