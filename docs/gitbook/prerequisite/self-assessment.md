# ✅ Self-Assessment Quiz

## 🎯 Tentang Quiz Ini

Quiz ini dirancang untuk menguji kesiapan Anda sebelum memulai pembelajaran CodeIgniter 3. Terdapat **20 pertanyaan** yang mencakup semua prerequisite.

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

### 📊 Cara Penilaian

| Skor | Status | Rekomendasi |
|------|--------|-------------|
| **16-20** | 🟢 Siap | Langsung lanjut ke Phase 0! |
| **11-15** | 🟡 Perlu Review | Review materi sambil belajar CI3 |
| **6-10** | 🟠 Perlu Belajar | Pelajari prerequisite lebih dalam |
| **0-5** | 🔴 Belum Siap | Kuasai prerequisite dulu |

</div>

---

## 🌐 Bagian 1: Web Basics (4 Pertanyaan)

### Pertanyaan 1
**HTTP Method mana yang digunakan browser saat Anda membuka halaman web?**

- [ ] A. POST
- [ ] B. GET
- [ ] C. PUT
- [ ] D. DELETE

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. GET**

GET digunakan untuk **mengambil/membaca** data. Saat Anda mengetik URL dan menekan Enter, browser mengirim GET request untuk meminta halaman tersebut.

</details>

---

### Pertanyaan 2
**Status code HTTP yang menunjukkan "halaman tidak ditemukan" adalah?**

- [ ] A. 200
- [ ] B. 301
- [ ] C. 404
- [ ] D. 500

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. 404**

- 200 = OK (berhasil)
- 301 = Redirect permanent
- 404 = Not Found (tidak ditemukan)
- 500 = Internal Server Error

</details>

---

### Pertanyaan 3
**Dalam arsitektur web, browser termasuk kategori?**

- [ ] A. Server
- [ ] B. Client
- [ ] C. Database
- [ ] D. Middleware

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. Client**

Browser adalah **client** yang mengirim request ke server dan menampilkan response. Server adalah yang memproses request dan mengirim data kembali.

</details>

---

### Pertanyaan 4
**Bagian URL `?category=shoes&size=42` disebut?**

- [ ] A. Path
- [ ] B. Fragment
- [ ] C. Query String
- [ ] D. Protocol

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. Query String**

Query string adalah bagian setelah `?` yang berisi parameter dalam format `key=value`, dipisahkan dengan `&`.

</details>

---

## 🐘 Bagian 2: PHP Basics (6 Pertanyaan)

### Pertanyaan 5
**Cara yang benar untuk mendeklarasikan variabel di PHP adalah?**

- [ ] A. `var name = "John";`
- [ ] B. `$name = "John";`
- [ ] C. `let name = "John";`
- [ ] D. `name := "John";`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. `$name = "John";`**

Di PHP, variabel selalu dimulai dengan tanda dollar (`$`). Contoh:
```php
$name = "John";    // String
$age = 25;         // Integer
$price = 99.99;    // Float
```

</details>

---

### Pertanyaan 6
**Apa output dari kode berikut?**
```php
$arr = ["a", "b", "c"];
echo $arr[1];
```

- [ ] A. a
- [ ] B. b
- [ ] C. c
- [ ] D. Error

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. b**

Array di PHP dimulai dari index 0:
- `$arr[0]` = "a"
- `$arr[1]` = "b" ← ini yang dipilih
- `$arr[2]` = "c"

</details>

---

### Pertanyaan 7
**Perbedaan antara `==` dan `===` di PHP adalah?**

- [ ] A. Tidak ada perbedaan
- [ ] B. `===` lebih cepat
- [ ] C. `==` membandingkan nilai, `===` membandingkan nilai DAN tipe
- [ ] D. `===` hanya untuk string

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. `==` membandingkan nilai, `===` membandingkan nilai DAN tipe**

```php
$a = 10;
$b = "10";

$a == $b   // true  (nilai sama)
$a === $b  // false (tipe berbeda: int vs string)
```

</details>

---

### Pertanyaan 8
**Fungsi untuk menghitung jumlah elemen dalam array adalah?**

- [ ] A. `length($arr)`
- [ ] B. `size($arr)`
- [ ] C. `count($arr)`
- [ ] D. `total($arr)`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. `count($arr)`**

```php
$fruits = ["Apple", "Banana", "Orange"];
echo count($fruits);  // Output: 3
```

Alternatif: `sizeof($arr)` juga bisa, tapi `count()` lebih umum digunakan.

</details>

---

### Pertanyaan 9
**Apa output dari kode berikut?**
```php
function add($a, $b = 5) {
    return $a + $b;
}
echo add(10);
```

- [ ] A. 10
- [ ] B. 15
- [ ] C. 5
- [ ] D. Error

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. 15**

Parameter `$b = 5` adalah **default value**. Jika tidak diberikan saat pemanggilan, nilainya adalah 5.

```php
add(10);     // $a=10, $b=5 → 15
add(10, 3);  // $a=10, $b=3 → 13
```

</details>

---

### Pertanyaan 10
**Superglobal PHP untuk mengakses data dari form POST adalah?**

- [ ] A. `$_GET`
- [ ] B. `$_POST`
- [ ] C. `$_REQUEST`
- [ ] D. `$_FORM`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. `$_POST`**

- `$_GET` = data dari URL query string
- `$_POST` = data dari form dengan method POST
- `$_REQUEST` = gabungan GET, POST, COOKIE
- `$_FORM` = tidak ada di PHP

</details>

---

## 🎯 Bagian 3: PHP OOP (4 Pertanyaan)

### Pertanyaan 11
**Keyword untuk membuat class di PHP adalah?**

- [ ] A. `object`
- [ ] B. `new`
- [ ] C. `class`
- [ ] D. `create`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. `class`**

```php
class User {
    public $name;
    
    public function greet() {
        return "Hello, " . $this->name;
    }
}

$user = new User();  // 'new' untuk membuat object
```

</details>

---

### Pertanyaan 12
**Visibility mana yang membuat property hanya bisa diakses dari dalam class itu sendiri?**

- [ ] A. public
- [ ] B. protected
- [ ] C. private
- [ ] D. internal

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. private**

- `public` = bisa diakses dari mana saja
- `protected` = hanya dari class dan turunannya
- `private` = hanya dari dalam class itu sendiri

```php
class Example {
    public $a;     // Accessible everywhere
    protected $b;  // Class & subclasses only
    private $c;    // This class only
}
```

</details>

---

### Pertanyaan 13
**Method yang otomatis dipanggil saat object dibuat adalah?**

- [ ] A. `__init()`
- [ ] B. `__construct()`
- [ ] C. `__start()`
- [ ] D. `__create()`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. `__construct()`**

Constructor adalah method khusus yang dipanggil otomatis saat `new ClassName()`.

```php
class User {
    public function __construct($name) {
        $this->name = $name;
        echo "User created!";
    }
}

$user = new User("John");  // "User created!" akan muncul
```

</details>

---

### Pertanyaan 14
**Keyword untuk inheritance (pewarisan class) di PHP adalah?**

- [ ] A. `inherits`
- [ ] B. `implements`
- [ ] C. `extends`
- [ ] D. `uses`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. `extends`**

```php
class Animal {
    public function eat() {
        echo "Eating...";
    }
}

class Dog extends Animal {  // Dog mewarisi Animal
    public function bark() {
        echo "Woof!";
    }
}

$dog = new Dog();
$dog->eat();   // Dari parent class
$dog->bark();  // Dari Dog class
```

</details>

---

## 💾 Bagian 4: Database & SQL (4 Pertanyaan)

### Pertanyaan 15
**Perintah SQL untuk mengambil data dari tabel adalah?**

- [ ] A. GET
- [ ] B. FETCH
- [ ] C. SELECT
- [ ] D. READ

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. SELECT**

```sql
SELECT * FROM users;                    -- Semua kolom
SELECT nama, email FROM users;          -- Kolom tertentu
SELECT * FROM users WHERE id = 1;       -- Dengan kondisi
```

</details>

---

### Pertanyaan 16
**Query mana yang benar untuk menambah data baru?**

- [ ] A. `ADD INTO users (nama) VALUES ('John')`
- [ ] B. `INSERT INTO users (nama) VALUES ('John')`
- [ ] C. `CREATE users SET nama = 'John'`
- [ ] D. `PUT INTO users (nama) VALUES ('John')`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. `INSERT INTO users (nama) VALUES ('John')`**

Struktur INSERT:
```sql
INSERT INTO nama_tabel (kolom1, kolom2) 
VALUES (nilai1, nilai2);
```

</details>

---

### Pertanyaan 17
**Apa fungsi PRIMARY KEY dalam tabel database?**

- [ ] A. Menyimpan password
- [ ] B. Identitas unik setiap baris
- [ ] C. Menghubungkan ke tabel lain
- [ ] D. Menyimpan data temporary

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: B. Identitas unik setiap baris**

Primary Key memiliki karakteristik:
- Nilai harus UNIK
- Tidak boleh NULL
- Satu tabel hanya punya satu PK
- Biasanya kolom `id` dengan auto_increment

</details>

---

### Pertanyaan 18
**Apa yang terjadi jika UPDATE tanpa WHERE clause?**

- [ ] A. Error
- [ ] B. Tidak ada yang berubah
- [ ] C. Hanya baris pertama yang terupdate
- [ ] D. SEMUA baris akan terupdate

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: D. SEMUA baris akan terupdate**

```sql
-- BAHAYA! Semua user password-nya jadi '123'
UPDATE users SET password = '123';

-- AMAN - hanya id tertentu
UPDATE users SET password = '123' WHERE id = 1;
```

⚠️ Selalu gunakan WHERE pada UPDATE dan DELETE!

</details>

---

## 🛠️ Bagian 5: Development Tools (2 Pertanyaan)

### Pertanyaan 19
**Folder tempat meletakkan project PHP di XAMPP adalah?**

- [ ] A. `C:\xampp\apache\`
- [ ] B. `C:\xampp\php\`
- [ ] C. `C:\xampp\htdocs\`
- [ ] D. `C:\xampp\mysql\`

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. `C:\xampp\htdocs\`**

`htdocs` adalah document root - folder yang bisa diakses via `http://localhost/`

</details>

---

### Pertanyaan 20
**Shortcut untuk membuka Developer Tools di browser adalah?**

- [ ] A. F1
- [ ] B. F5
- [ ] C. F12
- [ ] D. Ctrl + F

<details>
<summary>🔍 Lihat Jawaban</summary>

**✅ Jawaban: C. F12**

F12 membuka DevTools di Chrome, Firefox, Edge. Alternatif: `Ctrl + Shift + I`

- F1 = Help
- F5 = Refresh
- Ctrl + F = Find

</details>

---

## 📊 Hitung Skor Anda

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

### 📝 Scorecard

| Bagian | Benar | Total |
|--------|-------|-------|
| Web Basics | ___ | 4 |
| PHP Basics | ___ | 6 |
| PHP OOP | ___ | 4 |
| Database & SQL | ___ | 4 |
| Development Tools | ___ | 2 |
| **TOTAL** | ___ | **20** |

</div>

---

## 🎯 Hasil Assessment

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; border-left: 4px solid #4CAF50;">
<h3>🟢 16-20 Benar</h3>
<p><strong>Status: SIAP!</strong></p>
<p>Selamat! Anda sudah siap untuk memulai pembelajaran CodeIgniter 3. Lanjutkan ke Phase 0!</p>
<a href="../phase-0-setup/README.md">➡️ Mulai Phase 0</a>
</div>

<div style="background: #FFF9C4; padding: 20px; border-radius: 10px; border-left: 4px solid #FDD835;">
<h3>🟡 11-15 Benar</h3>
<p><strong>Status: Perlu Review</strong></p>
<p>Anda bisa lanjut, tapi review materi yang masih kurang saat belajar CI3.</p>
<a href="../phase-0-setup/README.md">➡️ Lanjut dengan Review</a>
</div>

<div style="background: #FFE0B2; padding: 20px; border-radius: 10px; border-left: 4px solid #FF9800;">
<h3>🟠 6-10 Benar</h3>
<p><strong>Status: Perlu Belajar</strong></p>
<p>Sebaiknya pelajari prerequisite lebih dalam sebelum lanjut.</p>
<a href="README.md">📚 Review Prerequisite</a>
</div>

<div style="background: #FFEBEE; padding: 20px; border-radius: 10px; border-left: 4px solid #F44336;">
<h3>🔴 0-5 Benar</h3>
<p><strong>Status: Belum Siap</strong></p>
<p>Pelajari prerequisite dari awal untuk fondasi yang kuat.</p>
<a href="web-basics.md">🌐 Mulai dari Web Basics</a>
</div>

</div>

---

## 📚 Area yang Perlu Diperbaiki

Berdasarkan jawaban Anda, review materi berikut:

| Jika salah di... | Pelajari |
|------------------|----------|
| Web Basics (1-4) | [🌐 Web Development Basics](web-basics.md) |
| PHP Basics (5-10) | [🐘 PHP Fundamentals](php-fundamentals.md) |
| PHP OOP (11-14) | [🎯 PHP OOP Section](php-fundamentals.md#-object-oriented-programming-basics) |
| Database (15-18) | [💾 Database & SQL Basics](database-basics.md) |
| Dev Tools (19-20) | [🛠️ Development Tools](tools-setup.md) |

---

## 🚀 Ready to Start?

<div style="text-align: center; margin: 40px 0;">

Sudah siap? Mari mulai perjalanan CodeIgniter 3!

<a href="../phase-0-setup/README.md">
<button style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); color: white; padding: 15px 40px; border: none; border-radius: 25px; font-size: 18px; cursor: pointer; margin: 10px;">
🚀 Mulai Phase 0: Environment & Setup
</button>
</a>

</div>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="tools-setup.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Development Tools
      </button>
    </a>
  </div>
  <div>
    <a href="../phase-0-setup/README.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Phase 0: Environment & Setup →
      </button>
    </a>
  </div>
</div>
