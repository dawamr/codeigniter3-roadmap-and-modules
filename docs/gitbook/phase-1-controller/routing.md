# 🛣️ Routing System

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Memahami routing default CI3
- ✅ Membuat custom routes
- ✅ Menggunakan wildcards dan regex
- ✅ Routing untuk REST API

---

## 🤔 Apa itu Routing?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Routing** adalah proses **menerjemahkan URL** menjadi Controller dan Method yang tepat.

**Analogi:** Seperti **GPS navigasi** 🗺️
- Input: Alamat tujuan (URL)
- Output: Rute terbaik (Controller/Method)

</div>

---

## 📍 Default Routing

CI3 secara default menggunakan pola URL:

```
http://domain.com/[controller]/[method]/[param1]/[param2]/...
```

### Contoh Default Routing

| URL | Controller | Method | Parameter |
|-----|------------|--------|-----------|
| `/` | Welcome | index() | - |
| `/user` | User | index() | - |
| `/user/profile` | User | profile() | - |
| `/user/profile/5` | User | profile() | 5 |
| `/product/detail/100` | Product | detail() | 100 |

---

## ⚙️ Konfigurasi Routes

File: `application/config/routes.php`

### Default Controller

```php
<?php
// Controller yang dipanggil saat akses root URL (/)
$route['default_controller'] = 'welcome';

// Jika ingin halaman home di controller lain:
$route['default_controller'] = 'home';
$route['default_controller'] = 'pages/home';  // Dengan sub-folder
```

### 404 Override

```php
<?php
// Controller untuk handle halaman 404
$route['404_override'] = '';  // Default CI3 404

// Custom 404 page:
$route['404_override'] = 'errors/not_found';
```

### Translate URI Dashes

```php
<?php
// Ubah dash (-) di URL menjadi underscore (_) di method
$route['translate_uri_dashes'] = TRUE;

// Dengan TRUE:
// URL: /user/edit-profile → Method: edit_profile()
```

---

## 🔧 Custom Routes

### Route Sederhana

```php
<?php
// URL /about → controller Pages, method about
$route['about'] = 'pages/about';

// URL /contact → controller Pages, method contact
$route['contact'] = 'pages/contact';

// URL /faq → controller Help, method faq
$route['faq'] = 'help/faq';
```

### Route dengan Parameter

```php
<?php
// URL: /product/123 → Product controller, view method, param 123
$route['product/(:num)'] = 'product/view/$1';

// URL: /blog/my-first-post → Blog controller, post method
$route['blog/(:any)'] = 'blog/post/$1';

// URL: /user/john/profile → User controller, profile method
$route['user/(:any)/profile'] = 'user/profile/$1';
```

---

## 🎯 Wildcards

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Wildcard | Match | Contoh |
|----------|-------|--------|
| `(:num)` | Angka saja | `123`, `456` |
| `(:any)` | Semua karakter | `hello`, `my-post`, `123` |
| `(:segment)` | Semua kecuali `/` | Mirip `(:any)` |

</div>

```php
<?php
// (:num) - hanya angka
$route['product/(:num)'] = 'catalog/product_lookup/$1';
// /product/123 ✅
// /product/abc ❌ (tidak match, jadi cari controller 'product')

// (:any) - semua karakter
$route['blog/(:any)'] = 'blog/view/$1';
// /blog/hello-world ✅
// /blog/123 ✅
// /blog/test/something ✅ (any termasuk slash!)

// Multiple wildcards
$route['blog/(:num)/(:any)'] = 'blog/read/$1/$2';
// /blog/2024/my-first-post → blog/read(2024, 'my-first-post')
```

---

## 🔣 Regular Expression Routes

Untuk pattern lebih kompleks, gunakan regex:

```php
<?php
// Hanya huruf lowercase
$route['user/([a-z]+)'] = 'user/profile/$1';
// /user/john ✅
// /user/John ❌
// /user/123 ❌

// Kombinasi huruf dan angka
$route['product/([a-zA-Z0-9]+)'] = 'product/view/$1';

// Slug pattern (huruf, angka, dash)
$route['blog/([a-z0-9\-]+)'] = 'blog/post/$1';

// Year/Month pattern
$route['archive/(\d{4})/(\d{2})'] = 'archive/view/$1/$2';
// /archive/2024/01 → archive/view(2024, 01)

// Multiple segments
$route['category/([a-z]+)/([a-z]+)/(:num)'] = 'product/category/$1/$2/$3';
// /category/electronics/laptop/2 → product/category('electronics', 'laptop', 2)
```

---

## 🌐 HTTP Method Routes

Untuk REST API, route berdasarkan HTTP method:

```php
<?php
// GET /api/users → list users
$route['api/users']['GET'] = 'api/users/index';

// POST /api/users → create user
$route['api/users']['POST'] = 'api/users/create';

// GET /api/users/5 → get specific user
$route['api/users/(:num)']['GET'] = 'api/users/show/$1';

// PUT /api/users/5 → update user
$route['api/users/(:num)']['PUT'] = 'api/users/update/$1';

// DELETE /api/users/5 → delete user
$route['api/users/(:num)']['DELETE'] = 'api/users/delete/$1';
```

### RESTful Routes Pattern

```php
<?php
// Resource: users
$route['api/users']['GET'] = 'api/users/index';
$route['api/users']['POST'] = 'api/users/store';
$route['api/users/(:num)']['GET'] = 'api/users/show/$1';
$route['api/users/(:num)']['PUT'] = 'api/users/update/$1';
$route['api/users/(:num)']['PATCH'] = 'api/users/update/$1';
$route['api/users/(:num)']['DELETE'] = 'api/users/destroy/$1';

// Resource: products
$route['api/products']['GET'] = 'api/products/index';
$route['api/products']['POST'] = 'api/products/store';
$route['api/products/(:num)']['GET'] = 'api/products/show/$1';
$route['api/products/(:num)']['PUT'] = 'api/products/update/$1';
$route['api/products/(:num)']['DELETE'] = 'api/products/destroy/$1';
```

---

## 📝 Contoh routes.php Lengkap

```php
<?php
// application/config/routes.php

defined('BASEPATH') OR exit('No direct script access allowed');

/*
| -------------------------------------------------------------------------
| Default & Error Routes
| -------------------------------------------------------------------------
*/
$route['default_controller'] = 'home';
$route['404_override'] = 'errors/not_found';
$route['translate_uri_dashes'] = TRUE;

/*
| -------------------------------------------------------------------------
| Static Pages
| -------------------------------------------------------------------------
*/
$route['about'] = 'pages/about';
$route['contact'] = 'pages/contact';
$route['terms'] = 'pages/terms';
$route['privacy'] = 'pages/privacy';

/*
| -------------------------------------------------------------------------
| Blog Routes
| -------------------------------------------------------------------------
*/
$route['blog'] = 'blog/index';
$route['blog/page/(:num)'] = 'blog/index/$1';
$route['blog/category/(:any)'] = 'blog/category/$1';
$route['blog/(:any)'] = 'blog/post/$1';  // Harus di bawah routes spesifik!

/*
| -------------------------------------------------------------------------
| Product Routes
| -------------------------------------------------------------------------
*/
$route['products'] = 'product/index';
$route['products/(:num)'] = 'product/detail/$1';
$route['products/category/(:any)'] = 'product/category/$1';

/*
| -------------------------------------------------------------------------
| User Routes
| -------------------------------------------------------------------------
*/
$route['login'] = 'auth/login';
$route['logout'] = 'auth/logout';
$route['register'] = 'auth/register';
$route['profile'] = 'user/profile';
$route['profile/edit'] = 'user/edit';

/*
| -------------------------------------------------------------------------
| Admin Routes
| -------------------------------------------------------------------------
*/
$route['admin'] = 'admin/dashboard';
$route['admin/users'] = 'admin/users/index';
$route['admin/users/(:num)'] = 'admin/users/edit/$1';

/*
| -------------------------------------------------------------------------
| API Routes
| -------------------------------------------------------------------------
*/
$route['api/users']['GET'] = 'api/users/index';
$route['api/users']['POST'] = 'api/users/store';
$route['api/users/(:num)']['GET'] = 'api/users/show/$1';
$route['api/users/(:num)']['PUT'] = 'api/users/update/$1';
$route['api/users/(:num)']['DELETE'] = 'api/users/destroy/$1';
```

---

## ⚠️ Urutan Routes Penting!

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Routes diproses dari ATAS ke BAWAH. Yang pertama match akan digunakan!**

</div>

```php
<?php
// ❌ SALAH - (:any) terlalu di atas
$route['blog/(:any)'] = 'blog/post/$1';
$route['blog/category/(:any)'] = 'blog/category/$1';  // Tidak pernah tercapai!

// ✅ BENAR - Spesifik dulu, general belakangan
$route['blog/category/(:any)'] = 'blog/category/$1';
$route['blog/page/(:num)'] = 'blog/index/$1';
$route['blog/(:any)'] = 'blog/post/$1';  // Fallback
```

---

## 🔍 Debugging Routes

```php
<?php
// Di controller, cek route yang aktif
class Debug extends CI_Controller {
    
    public function routes() {
        echo '<h2>Current URI:</h2>';
        echo $this->uri->uri_string();
        
        echo '<h2>Segments:</h2>';
        print_r($this->uri->segment_array());
        
        echo '<h2>Class:</h2>';
        echo $this->router->class;
        
        echo '<h2>Method:</h2>';
        echo $this->router->method;
    }
}
```

---

## ✅ Checklist Pemahaman

Pastikan Anda memahami:

- [ ] Pola default routing CI3
- [ ] Cara set default controller
- [ ] Custom routes sederhana
- [ ] Wildcards: `(:num)`, `(:any)`
- [ ] Regular expression routes
- [ ] HTTP method-based routes untuk API
- [ ] Urutan routes (spesifik → general)

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="methods-parameters.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Methods & Parameters
      </button>
    </a>
  </div>
  <div>
    <a href="loading-resources.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Loading Resources →
      </button>
    </a>
  </div>
</div>
