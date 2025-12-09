# 🐘 PHP Fundamentals

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan memahami:
- ✅ Syntax dan struktur dasar PHP
- ✅ Variables, data types, dan operators
- ✅ Control structures (if, loop, switch)
- ✅ Functions dan scope
- ✅ Arrays dan manipulasinya
- ✅ Object-Oriented Programming basics

---

## 🤔 Apa itu PHP?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**PHP** (Hypertext Preprocessor) adalah bahasa pemrograman yang berjalan di **sisi server** (server-side). Artinya, kode PHP dieksekusi di server, bukan di browser pengguna.

</div>

### � Analogi Sederhana

Bayangkan Anda memesan makanan di restoran:

| Komponen | Analogi | Penjelasan |
|----------|---------|------------|
| **Browser** | Pelanggan | Mengirim pesanan (request) |
| **PHP** | Koki di dapur | Memproses pesanan, memasak |
| **HTML** | Makanan yang disajikan | Hasil yang dilihat pelanggan |
| **Database** | Lemari bahan makanan | Tempat mengambil data |

> 🎯 **Ingat:** PHP bekerja "di belakang layar" - pengguna tidak pernah melihat kode PHP, hanya hasilnya (HTML).

---

## � PHP Basics

### 🔤 PHP Syntax

#### Apa yang Perlu Diketahui?

Setiap kode PHP harus ditulis di antara **tag pembuka dan penutup** khusus. Ini memberitahu server: "Hei, ini kode PHP, tolong eksekusi!"

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Tag PHP Standar</h4>

```php
<?php
    // Kode PHP di sini
?>
```

Ini adalah cara **paling aman** dan **selalu berfungsi**.
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>⚠️ Short Tag (Tidak Direkomendasikan)</h4>

```php
<?
    // Kode PHP
?>
```

Bisa tidak berfungsi di beberapa server!
</div>

</div>

#### Contoh PHP dalam HTML

```php
<!DOCTYPE html>
<html>
<body>
    <h1>Selamat Datang</h1>
    
    <?php
        // Ini kode PHP - dieksekusi di server
        $nama = "John";
        echo "Halo, $nama!";  // Output: Halo, John!
    ?>
    
    <p>Ini HTML biasa</p>
</body>
</html>
```

**Apa yang terjadi?**
1. Server membaca file
2. Menemukan `<?php` → mulai eksekusi PHP
3. Variabel `$nama` diisi "John"
4. `echo` menampilkan teks
5. `?>` → kembali ke HTML
6. Browser menerima HTML murni (tanpa kode PHP)

#### 📝 Aturan Dasar Syntax

<div style="background: #FFF3E0; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Aturan | Contoh | Penjelasan |
|--------|--------|------------|
| **Semicolon** | `echo "Hi";` | Setiap perintah HARUS diakhiri `;` |
| **Case-sensitive (variabel)** | `$Name` ≠ `$name` | Huruf besar/kecil berbeda |
| **Case-insensitive (fungsi)** | `ECHO` = `echo` | Fungsi tidak peduli huruf |
| **Komentar satu baris** | `// ini komentar` | Diabaikan saat eksekusi |
| **Komentar multi-baris** | `/* komentar */` | Untuk penjelasan panjang |

</div>

---

### 📦 Variables (Variabel)

#### Apa itu Variabel?

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Variabel** adalah seperti **kotak penyimpanan** yang diberi nama. Anda bisa menyimpan data di dalamnya dan mengambilnya kapan saja.

🎁 Bayangkan variabel seperti **kotak berlabel** di gudang:
- Kotak berlabel "nama" berisi "John"
- Kotak berlabel "umur" berisi angka 25
- Kotak berlabel "sudah_menikah" berisi true/false

</div>

#### Cara Membuat Variabel

Di PHP, variabel **SELALU** dimulai dengan tanda dollar (`$`):

```php
<?php
// Membuat variabel dan mengisinya
$nama = "John Doe";      // Menyimpan teks
$umur = 25;              // Menyimpan angka
$tinggi = 175.5;         // Menyimpan angka desimal
$sudahMenikah = false;   // Menyimpan benar/salah
```

**Penjelasan baris per baris:**
- `$nama = "John Doe";` → Buat kotak "nama", isi dengan teks "John Doe"
- `$umur = 25;` → Buat kotak "umur", isi dengan angka 25
- Tanda `=` artinya "isi dengan" atau "simpan nilai"

#### 📋 Aturan Penamaan Variabel

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Nama Valid</h4>

```php
$nama           // huruf saja
$nama_lengkap   // dengan underscore
$namaLengkap    // camelCase
$nama123        // huruf diikuti angka
$_private       // dimulai underscore
```
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Nama Tidak Valid</h4>

```php
$123nama        // dimulai angka
$nama-lengkap   // mengandung dash
$nama lengkap   // mengandung spasi
nama            // tanpa $ (ERROR!)
```
</div>

</div>

---

### 🎨 Data Types (Tipe Data)

#### Mengapa Tipe Data Penting?

Komputer perlu tahu **jenis data** yang disimpan agar bisa memprosesnya dengan benar. Misalnya:
- Angka bisa dijumlahkan: `5 + 3 = 8`
- Teks digabung: `"Hello" + "World" = "HelloWorld"`

#### 📊 7 Tipe Data Utama di PHP

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Tipe | Deskripsi | Contoh | Kapan Digunakan |
|------|-----------|--------|-----------------|
| **String** | Teks/karakter | `"Hello"`, `'World'` | Nama, alamat, pesan |
| **Integer** | Bilangan bulat | `42`, `-17`, `0` | Umur, quantity, ID |
| **Float** | Bilangan desimal | `3.14`, `99.99` | Harga, tinggi, berat |
| **Boolean** | Benar/Salah | `true`, `false` | Status, flag, kondisi |
| **Array** | Kumpulan data | `[1, 2, 3]` | List item, data tabel |
| **Object** | Instance class | `new User()` | OOP, data kompleks |
| **NULL** | Tidak ada nilai | `null` | Variabel kosong |

</div>

#### 1️⃣ String (Teks)

String adalah **kumpulan karakter** yang diapit tanda kutip.

```php
<?php
// Dua cara menulis string:
$nama1 = "John Doe";    // Double quotes (kutip ganda)
$nama2 = 'Jane Doe';    // Single quotes (kutip tunggal)

// Perbedaan penting!
$nama = "John";
$salam1 = "Halo, $nama!";    // Output: Halo, John! ✅
$salam2 = 'Halo, $nama!';    // Output: Halo, $nama! ❌ (literal)
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Tips:** Gunakan **double quotes** jika ingin memasukkan variabel ke dalam string. Gunakan **single quotes** untuk teks biasa (sedikit lebih cepat).

</div>

#### 2️⃣ Integer (Bilangan Bulat)

```php
<?php
$umur = 25;          // Positif
$suhu = -5;          // Negatif
$stok = 0;           // Nol
$jumlah = 1000000;   // Besar

// Operasi matematika
$total = $umur + 5;  // 30
```

#### 3️⃣ Float (Bilangan Desimal)

```php
<?php
$harga = 99.99;
$pi = 3.14159;
$diskon = 0.15;      // 15%

// Perhitungan
$total = $harga * (1 - $diskon);  // 84.9915
```

#### 4️⃣ Boolean (Benar/Salah)

Boolean hanya punya **2 nilai**: `true` atau `false`. Sangat berguna untuk kondisi.

```php
<?php
$sudahLogin = true;
$isAdmin = false;
$aktif = TRUE;       // Case-insensitive

// Digunakan dalam kondisi
if ($sudahLogin) {
    echo "Selamat datang!";
}
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Kapan pakai Boolean?**
- Status: `$aktif`, `$terverifikasi`
- Flag: `$sudahBayar`, `$emailTerkirim`
- Kondisi: `$bolehEdit`, `$isAdmin`

</div>

---

### ➕ Operators (Operator)

Operator adalah simbol yang melakukan **operasi** pada nilai/variabel.

#### 🔢 Arithmetic Operators (Operator Matematika)

Digunakan untuk perhitungan matematika:

```php
<?php
$a = 10;
$b = 3;
```

| Operator | Nama | Contoh | Hasil | Penjelasan |
|----------|------|--------|-------|------------|
| `+` | Addition | `$a + $b` | `13` | Penjumlahan |
| `-` | Subtraction | `$a - $b` | `7` | Pengurangan |
| `*` | Multiplication | `$a * $b` | `30` | Perkalian |
| `/` | Division | `$a / $b` | `3.33` | Pembagian |
| `%` | Modulus | `$a % $b` | `1` | Sisa bagi (10÷3 = 3 sisa **1**) |
| `**` | Exponent | `$a ** $b` | `1000` | Pangkat (10³) |

#### 📝 Assignment Operators (Operator Penugasan)

Cara cepat untuk mengubah nilai variabel:

```php
<?php
$x = 10;        // x = 10

$x += 5;        // x = x + 5  → x = 15
$x -= 3;        // x = x - 3  → x = 12
$x *= 2;        // x = x * 2  → x = 24
$x /= 4;        // x = x / 4  → x = 6
```

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Tips:** `$x += 5` adalah **shorthand** (singkatan) dari `$x = $x + 5`. Lebih ringkas dan umum digunakan!

</div>

#### ⚖️ Comparison Operators (Operator Perbandingan)

Membandingkan dua nilai dan menghasilkan `true` atau `false`:

```php
<?php
$a = 10;
$b = "10";      // String "10"
```

| Operator | Nama | Contoh | Hasil | Penjelasan |
|----------|------|--------|-------|------------|
| `==` | Equal | `$a == $b` | `true` | Nilai sama (abaikan tipe) |
| `===` | Identical | `$a === $b` | `false` | Nilai DAN tipe harus sama |
| `!=` | Not equal | `$a != $b` | `false` | Nilai tidak sama |
| `!==` | Not identical | `$a !== $b` | `true` | Nilai ATAU tipe berbeda |
| `>` | Greater than | `$a > 5` | `true` | Lebih besar |
| `<` | Less than | `$a < 20` | `true` | Lebih kecil |
| `>=` | Greater or equal | `$a >= 10` | `true` | Lebih besar atau sama |
| `<=` | Less or equal | `$a <= 10` | `true` | Lebih kecil atau sama |

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Penting!** Perbedaan `==` vs `===`:
- `10 == "10"` → `true` (nilai sama, tipe diabaikan)
- `10 === "10"` → `false` (tipe berbeda: integer vs string)

**Rekomendasi:** Gunakan `===` untuk keamanan lebih baik!

</div>

---

## 🔄 Control Structures

Control structures mengontrol **alur eksekusi** program - bagian mana yang dijalankan berdasarkan kondisi.

### 🔀 If-Else Statements

#### Apa itu If-Else?

Bayangkan if-else seperti **persimpangan jalan** dengan rambu:
- JIKA kondisi terpenuhi → belok kiri
- JIKA TIDAK → belok kanan

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Struktur Dasar:**
```
JIKA (kondisi benar) {
    lakukan ini
} JIKA TIDAK {
    lakukan yang lain
}
```

</div>

#### Contoh Praktis

```php
<?php
$nilai = 85;

// Struktur if-else
if ($nilai >= 90) {
    echo "Grade A - Excellent!";
} elseif ($nilai >= 80) {
    echo "Grade B - Good!";
} elseif ($nilai >= 70) {
    echo "Grade C - Average";
} elseif ($nilai >= 60) {
    echo "Grade D - Below Average";
} else {
    echo "Grade F - Failed";
}

// Output: Grade B - Good!
```

**Cara kerjanya:**
1. Cek `$nilai >= 90`? → 85 ≥ 90? **Tidak** ❌
2. Cek `$nilai >= 80`? → 85 ≥ 80? **Ya** ✅ → Output "Grade B"
3. Kondisi lain tidak dicek lagi (sudah ketemu)

#### ⚡ Ternary Operator (If-Else Singkat)

Untuk kondisi sederhana, gunakan ternary operator:

```php
<?php
$nilai = 75;

// Cara panjang
if ($nilai >= 60) {
    $status = "Lulus";
} else {
    $status = "Tidak Lulus";
}

// Cara singkat dengan ternary
$status = ($nilai >= 60) ? "Lulus" : "Tidak Lulus";

// Format: (kondisi) ? nilai_jika_true : nilai_jika_false
```

---

### 🔄 Switch Statement

Switch lebih cocok ketika Anda membandingkan **satu variabel** dengan **banyak nilai tetap**.

```php
<?php
$hari = "Senin";

switch ($hari) {
    case "Senin":
        echo "Awal minggu, semangat!";
        break;
    case "Selasa":
    case "Rabu":
    case "Kamis":
        echo "Pertengahan minggu";
        break;
    case "Jumat":
        echo "Hampir weekend!";
        break;
    case "Sabtu":
    case "Minggu":
        echo "Weekend! Waktunya istirahat";
        break;
    default:
        echo "Hari tidak valid";
}
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Jangan lupa `break;`!** Tanpa break, eksekusi akan "jatuh" ke case berikutnya.

</div>

---

### 🔁 Loops (Perulangan)

Loop menjalankan kode **berulang-ulang** sampai kondisi tertentu.

#### 1️⃣ For Loop

Digunakan ketika **tahu berapa kali** harus mengulang:

```php
<?php
// Cetak angka 0 sampai 4
for ($i = 0; $i < 5; $i++) {
    echo $i . " ";
}
// Output: 0 1 2 3 4
```

**Anatomi For Loop:**
```
for (inisialisasi; kondisi; increment) {
    // kode yang diulang
}
```
- `$i = 0` → Mulai dari 0
- `$i < 5` → Lanjut selama i kurang dari 5
- `$i++` → Tambah 1 setiap iterasi

#### 2️⃣ While Loop

Digunakan ketika **tidak tahu** berapa kali mengulang:

```php
<?php
$counter = 0;

while ($counter < 5) {
    echo $counter . " ";
    $counter++;  // Jangan lupa increment!
}
// Output: 0 1 2 3 4
```

#### 3️⃣ Foreach Loop (Untuk Array)

Cara **paling nyaman** untuk mengulang array:

```php
<?php
$buah = ["Apel", "Jeruk", "Mangga"];

// Cara 1: Hanya nilai
foreach ($buah as $item) {
    echo $item . " ";
}
// Output: Apel Jeruk Mangga

// Cara 2: Index dan nilai
foreach ($buah as $index => $item) {
    echo "$index: $item, ";
}
// Output: 0: Apel, 1: Jeruk, 2: Mangga,
```

#### 🛑 Break dan Continue

```php
<?php
for ($i = 0; $i < 10; $i++) {
    if ($i == 3) {
        continue;  // Skip angka 3, lanjut ke iterasi berikutnya
    }
    if ($i == 7) {
        break;     // Stop total saat mencapai 7
    }
    echo $i . " ";
}
// Output: 0 1 2 4 5 6
```

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Ringkasan:**
- `break` = **STOP** loop sepenuhnya
- `continue` = **SKIP** iterasi ini, lanjut ke berikutnya

</div>

---

## 📚 Arrays

Array adalah **wadah** yang bisa menyimpan **banyak nilai** dalam satu variabel.

### 🤔 Mengapa Butuh Array?

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

Bayangkan Anda punya daftar 100 nama siswa. Tanpa array:
```php
$siswa1 = "Andi";
$siswa2 = "Budi";
$siswa3 = "Citra";
// ... sampai $siswa100 😱
```

Dengan array:
```php
$siswa = ["Andi", "Budi", "Citra", ...]; // Semua dalam 1 variabel! 🎉
```

</div>

### 📊 Jenis-jenis Array

#### 1️⃣ Indexed Array (Array Berindex)

Elemen diakses menggunakan **angka** (dimulai dari 0):

```php
<?php
// Membuat indexed array
$buah = ["Apel", "Jeruk", "Mangga"];

// Mengakses elemen
echo $buah[0];    // Output: Apel
echo $buah[1];    // Output: Jeruk
echo $buah[2];    // Output: Mangga

// Menambah elemen
$buah[] = "Anggur";        // Otomatis index 3
$buah[4] = "Semangka";     // Manual index 4
```

<div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0;">

**Visualisasi:**
| Index | 0 | 1 | 2 | 3 | 4 |
|-------|---|---|---|---|---|
| Nilai | Apel | Jeruk | Mangga | Anggur | Semangka |

</div>

#### 2️⃣ Associative Array (Array Asosiatif)

Elemen diakses menggunakan **key** (nama kustom):

```php
<?php
// Membuat associative array
$mahasiswa = [
    "nama" => "John Doe",
    "umur" => 20,
    "jurusan" => "Informatika",
    "ipk" => 3.75
];

// Mengakses elemen
echo $mahasiswa["nama"];      // Output: John Doe
echo $mahasiswa["ipk"];       // Output: 3.75

// Menambah/mengubah elemen
$mahasiswa["email"] = "john@univ.ac.id";  // Tambah baru
$mahasiswa["ipk"] = 3.80;                  // Update nilai
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Kapan pakai Associative Array?**
- Data yang punya **label/nama**: profil user, konfigurasi, dll
- Lebih mudah dibaca: `$user["email"]` vs `$user[3]`

</div>

#### 3️⃣ Multidimensional Array (Array Bersarang)

Array di dalam array - untuk data **tabel/kompleks**:

```php
<?php
// Data siswa seperti tabel
$siswa = [
    ["nama" => "Alice", "nilai" => 85, "kelas" => "A"],
    ["nama" => "Bob", "nilai" => 90, "kelas" => "B"],
    ["nama" => "Charlie", "nilai" => 78, "kelas" => "A"]
];

// Mengakses: $siswa[baris][kolom]
echo $siswa[0]["nama"];    // Output: Alice
echo $siswa[1]["nilai"];   // Output: 90

// Mengulang semua data
foreach ($siswa as $s) {
    echo $s["nama"] . " dapat nilai " . $s["nilai"] . "\n";
}
```

### 🛠️ Fungsi Array yang Sering Digunakan

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Fungsi | Kegunaan | Contoh |
|--------|----------|--------|
| `count($arr)` | Hitung jumlah elemen | `count([1,2,3])` → `3` |
| `in_array($val, $arr)` | Cek nilai ada | `in_array("a", ["a","b"])` → `true` |
| `array_push($arr, $val)` | Tambah di akhir | Menambah elemen baru |
| `array_pop($arr)` | Hapus dari akhir | Hapus elemen terakhir |
| `sort($arr)` | Urutkan ascending | A-Z atau 0-9 |
| `rsort($arr)` | Urutkan descending | Z-A atau 9-0 |
| `array_merge($a, $b)` | Gabung array | Menggabungkan 2 array |
| `array_keys($arr)` | Ambil semua key | Daftar key saja |
| `array_values($arr)` | Ambil semua nilai | Daftar nilai saja |

</div>

```php
<?php
$angka = [5, 2, 8, 1, 9];

// Hitung jumlah
echo count($angka);  // Output: 5

// Cek apakah nilai ada
if (in_array(8, $angka)) {
    echo "Angka 8 ada!";
}

// Urutkan
sort($angka);
print_r($angka);  // [1, 2, 5, 8, 9]
```

---

## 🔧 Functions (Fungsi)

Function adalah **blok kode** yang bisa dipanggil berkali-kali. Seperti **resep masakan** - tulis sekali, masak berkali-kali!

### 🤔 Mengapa Butuh Function?

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Tanpa function:**
```php
// Menghitung diskon di banyak tempat
$harga1 = 100000;
$diskon1 = $harga1 * 0.1;
$total1 = $harga1 - $diskon1;

$harga2 = 200000;
$diskon2 = $harga2 * 0.1;
$total2 = $harga2 - $diskon2;
// Copy-paste terus... 😫
```

**Dengan function:**
```php
function hitungDiskon($harga) {
    return $harga - ($harga * 0.1);
}

$total1 = hitungDiskon(100000);  // 90000
$total2 = hitungDiskon(200000);  // 180000
// Bersih dan reusable! 🎉
```

</div>

### 📝 Cara Membuat Function

#### Struktur Dasar

```php
<?php
// Membuat function
function namaFunction($parameter1, $parameter2) {
    // Kode yang dijalankan
    return $hasil;  // Mengembalikan nilai
}

// Memanggil function
$hasil = namaFunction("nilai1", "nilai2");
```

#### Contoh Praktis

```php
<?php
// Function sederhana tanpa parameter
function sayHello() {
    echo "Halo Dunia!";
}
sayHello();  // Output: Halo Dunia!

// Function dengan parameter
function sapa($nama) {
    echo "Halo, $nama!";
}
sapa("John");  // Output: Halo, John!

// Function dengan return value
function tambah($a, $b) {
    return $a + $b;
}
$hasil = tambah(5, 3);  // $hasil = 8
```

### 📋 Parameter Default

Parameter bisa punya **nilai default** jika tidak diisi:

```php
<?php
function salam($nama, $waktu = "pagi") {
    return "Selamat $waktu, $nama!";
}

echo salam("John");           // Selamat pagi, John!
echo salam("John", "malam");  // Selamat malam, John!
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Aturan:** Parameter dengan default value harus diletakkan **setelah** parameter tanpa default!

```php
// ✅ Benar
function contoh($wajib, $opsional = "default") {}

// ❌ Salah
function contoh($opsional = "default", $wajib) {}
```

</div>

### 🔒 Variable Scope

Variabel di dalam function **tidak bisa diakses** dari luar, dan sebaliknya:

```php
<?php
$globalVar = "Saya global";

function testScope() {
    $localVar = "Saya lokal";
    
    // Mengakses global variable - cara 1
    global $globalVar;
    echo $globalVar;  // ✅ Berhasil
    
    // Mengakses global variable - cara 2
    echo $GLOBALS['globalVar'];  // ✅ Berhasil
}

testScope();
echo $localVar;  // ❌ Error! Tidak dikenal di luar function
```

<div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0;">

| Jenis | Bisa Diakses | Contoh |
|-------|--------------|--------|
| **Local** | Hanya di dalam function | `$localVar` dalam function |
| **Global** | Di luar function | Variabel biasa di script |
| **Static** | Tetap ada setelah function selesai | `static $counter = 0;` |

</div>

---

## 🎯 Object-Oriented Programming (OOP) Basics

OOP adalah paradigma pemrograman yang mengorganisir kode menjadi **objek** - seperti di dunia nyata!

### 🤔 Apa itu OOP?

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Analogi:** Bayangkan membuat mobil 🚗

- **Class** = Blueprint/cetakan mobil (definisi)
- **Object** = Mobil yang sudah jadi (instance)
- **Property** = Karakteristik mobil (warna, merek)
- **Method** = Kemampuan mobil (jalan, rem, klakson)

Satu blueprint bisa menghasilkan **banyak mobil** dengan karakteristik berbeda!

</div>

### 📦 Class dan Object

#### Membuat Class

Class adalah **cetakan** untuk membuat object:

```php
<?php
class Mobil {
    // PROPERTIES (karakteristik)
    public $merek;
    public $warna;
    public $kecepatan = 0;
    
    // CONSTRUCTOR (dipanggil saat object dibuat)
    public function __construct($merek, $warna) {
        $this->merek = $merek;
        $this->warna = $warna;
        echo "Mobil $merek baru dibuat!\n";
    }
    
    // METHODS (kemampuan)
    public function gas() {
        $this->kecepatan += 10;
        echo "Mobil melaju... Kecepatan: {$this->kecepatan} km/jam\n";
    }
    
    public function rem() {
        $this->kecepatan = 0;
        echo "Mobil berhenti.\n";
    }
    
    public function klakson() {
        return "BEEP BEEP!";
    }
    
    public function info() {
        return "Ini {$this->merek} warna {$this->warna}";
    }
}
```

#### Membuat Object

```php
<?php
// Membuat object dari class Mobil
$mobil1 = new Mobil("Toyota", "Merah");
$mobil2 = new Mobil("Honda", "Biru");

// Mengakses property
echo $mobil1->merek;    // Output: Toyota
echo $mobil2->warna;    // Output: Biru

// Memanggil method
$mobil1->gas();         // Mobil melaju... Kecepatan: 10 km/jam
$mobil1->gas();         // Mobil melaju... Kecepatan: 20 km/jam
echo $mobil1->klakson();  // BEEP BEEP!
echo $mobil1->info();     // Ini Toyota warna Merah
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Poin Penting:**
- `$this` → mengacu pada **object saat ini**
- `->` → untuk mengakses property/method
- `new` → membuat object baru dari class

</div>

### 🔒 Visibility (Akses Kontrol)

Mengontrol **siapa yang boleh mengakses** property/method:

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; text-align: center;">
<h4>🟢 Public</h4>
<p>Bisa diakses dari <strong>mana saja</strong></p>
<small>Seperti pintu depan rumah</small>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; text-align: center;">
<h4>🟡 Protected</h4>
<p>Class ini + <strong>anak class</strong></p>
<small>Seperti ruang keluarga</small>
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px; text-align: center;">
<h4>🔴 Private</h4>
<p>Hanya <strong>class ini</strong></p>
<small>Seperti brankas pribadi</small>
</div>

</div>

```php
<?php
class User {
    public $nama;                    // Bisa diakses semua
    protected $email;                // Hanya class & turunan
    private $password;               // Hanya class ini
    
    public function setPassword($pass) {
        // Method public bisa akses private
        $this->password = password_hash($pass, PASSWORD_DEFAULT);
    }
    
    public function getEmail() {
        return $this->email;  // Getter untuk protected
    }
}

$user = new User();
echo $user->nama;           // ✅ OK - public
echo $user->email;          // ❌ Error - protected
echo $user->password;       // ❌ Error - private
```

### 🧬 Inheritance (Pewarisan)

Class bisa **mewarisi** property dan method dari class lain:

```php
<?php
// Parent class (class induk)
class Hewan {
    public $nama;
    
    public function makan() {
        echo "{$this->nama} sedang makan.\n";
    }
    
    public function tidur() {
        echo "{$this->nama} sedang tidur.\n";
    }
}

// Child class (class anak) - mewarisi Hewan
class Kucing extends Hewan {
    public function meong() {
        echo "Meooong!\n";
    }
}

class Anjing extends Hewan {
    public function gonggong() {
        echo "Guk guk!\n";
    }
}

// Penggunaan
$kucing = new Kucing();
$kucing->nama = "Kitty";
$kucing->makan();      // Kitty sedang makan. (dari Hewan)
$kucing->meong();      // Meooong! (dari Kucing)

$anjing = new Anjing();
$anjing->nama = "Buddy";
$anjing->tidur();      // Buddy sedang tidur. (dari Hewan)
$anjing->gonggong();   // Guk guk! (dari Anjing)
```

<div style="background: #f8f9fa; padding: 15px; border-radius: 8px; margin: 15px 0;">

**Alur Inheritance:**
```
      Hewan (Parent)
         │
         ├── makan()
         └── tidur()
         │
    ┌────┴────┐
    │         │
  Kucing    Anjing
    │         │
  meong()  gonggong()
```

</div>

---

## 🔌 Include & Require

Untuk membagi kode ke beberapa file, PHP menyediakan cara untuk **menyertakan** file lain.

### 🤔 Mengapa Memisah File?

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Manfaat memisah file:**
- 📦 **Organisasi** - Kode lebih teratur
- 🔄 **Reusable** - File bisa dipakai di banyak halaman
- 🔧 **Maintainable** - Mudah diperbaiki/diubah
- 👥 **Kolaborasi** - Tim bisa kerja paralel

</div>

### 📝 Perbedaan Include vs Require

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px;">
<h4>📁 include</h4>
<p><strong>Jika file tidak ada:</strong></p>
<p>⚠️ Warning, script <strong>tetap lanjut</strong></p>
<p><strong>Gunakan untuk:</strong> File opsional (sidebar, widget)</p>
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>📁 require</h4>
<p><strong>Jika file tidak ada:</strong></p>
<p>❌ Fatal Error, script <strong>berhenti</strong></p>
<p><strong>Gunakan untuk:</strong> File wajib (config, database)</p>
</div>

</div>

```php
<?php
// File: config.php
define('DB_HOST', 'localhost');
define('DB_USER', 'root');
define('DB_NAME', 'toko_online');

// File: functions.php
function formatRupiah($angka) {
    return "Rp " . number_format($angka, 0, ',', '.');
}

// File: main.php
require_once 'config.php';      // Wajib ada - pakai require
require_once 'functions.php';   // Wajib ada - pakai require
include 'sidebar.php';          // Opsional - pakai include

echo DB_HOST;                   // Output: localhost
echo formatRupiah(50000);       // Output: Rp 50.000
```

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

💡 **Tips:** Gunakan `_once` (include_once, require_once) untuk menghindari file di-include dua kali!

</div>

---

## 🌐 Superglobals

Superglobals adalah variabel **built-in** yang bisa diakses dari **mana saja** - di dalam function, class, maupun di luar.

### 🤔 Apa itu Superglobals?

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; margin: 20px 0;">

PHP menyediakan variabel khusus yang selalu tersedia:
- 📥 `$_GET` - Data dari URL
- 📤 `$_POST` - Data dari form
- 🍪 `$_COOKIE` - Data cookie
- 🔐 `$_SESSION` - Data session
- 📁 `$_FILES` - File yang diupload
- 🖥️ `$_SERVER` - Info server

</div>

### 📊 Superglobals yang Sering Digunakan

#### 📥 $_GET - Data dari URL

Data yang dikirim melalui URL (terlihat di address bar):

```php
<?php
// URL: halaman.php?nama=John&umur=25

$nama = $_GET['nama'];    // "John"
$umur = $_GET['umur'];    // "25" (string!)

// Cara aman (jika parameter tidak ada)
$nama = $_GET['nama'] ?? 'Guest';
```

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; margin: 15px 0;">

⚠️ **Perhatian:** Data $_GET **terlihat di URL** - jangan kirim data sensitif seperti password!

</div>

#### 📤 $_POST - Data dari Form

Data yang dikirim melalui form dengan method POST:

```php
<!-- form.html -->
<form action="proses.php" method="POST">
    <input type="text" name="username">
    <input type="password" name="password">
    <button type="submit">Login</button>
</form>
```

```php
<?php
// proses.php
$username = $_POST['username'] ?? '';
$password = $_POST['password'] ?? '';

if ($username && $password) {
    echo "Login sebagai: $username";
}
```

#### 🔐 $_SESSION - Menyimpan Data User

Session menyimpan data **sementara** selama user aktif:

```php
<?php
// Wajib dipanggil sebelum menggunakan session!
session_start();

// Menyimpan data
$_SESSION['user_id'] = 123;
$_SESSION['username'] = 'john';
$_SESSION['role'] = 'admin';

// Mengambil data
echo $_SESSION['username'];  // "john"

// Menghapus session (logout)
session_destroy();
```

#### 🖥️ $_SERVER - Informasi Server

```php
<?php
// Informasi yang berguna
echo $_SERVER['REQUEST_METHOD'];   // GET atau POST
echo $_SERVER['HTTP_HOST'];        // localhost atau domain
echo $_SERVER['REQUEST_URI'];      // /halaman.php?id=1
echo $_SERVER['SCRIPT_NAME'];      // /folder/file.php
echo $_SERVER['HTTP_USER_AGENT'];  // Info browser
echo $_SERVER['REMOTE_ADDR'];      // IP address user
```

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Superglobal | Kegunaan | Contoh |
|-------------|----------|--------|
| `$_GET` | Data dari URL | Search query, ID halaman |
| `$_POST` | Data dari form | Login, registrasi |
| `$_SESSION` | Data user sementara | Status login, keranjang belanja |
| `$_COOKIE` | Data di browser | Preferensi tema, "remember me" |
| `$_FILES` | Upload file | Gambar profil, dokumen |
| `$_SERVER` | Info request | Method, URL, IP |

</div>

---

## 💡 Best Practices

### 1️⃣ Penamaan Variabel yang Baik

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 15px; margin: 20px 0;">

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✅ Good</h4>

```php
$userName = "John";
$isLoggedIn = true;
$totalPrice = 150000;
$productList = [];
```

**Ciri-ciri:** Deskriptif, jelas tujuannya
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px;">
<h4>❌ Bad</h4>

```php
$n = "John";
$flag = true;
$x = 150000;
$arr = [];
```

**Masalah:** Singkat tapi membingungkan
</div>

</div>

### 2️⃣ Selalu Validasi Input

```php
<?php
// ✅ Cara aman mengakses $_GET/$_POST
$nama = $_POST['nama'] ?? '';          // Default empty string
$umur = $_GET['umur'] ?? 0;            // Default 0
$aktif = $_POST['aktif'] ?? false;     // Default false

// ✅ Cek sebelum menggunakan
if (isset($_POST['email']) && !empty($_POST['email'])) {
    $email = $_POST['email'];
    // Proses email...
}
```

### 3️⃣ Gunakan Type Checking

```php
<?php
$input = $_GET['nilai'];

// Cek tipe data
if (is_numeric($input)) {
    $angka = (int) $input;  // Convert ke integer
    echo "Angka: $angka";
} else {
    echo "Input harus angka!";
}
```

---

## ✅ Skill Check

Pastikan Anda bisa:

- [ ] Membuat variables dengan tipe data berbeda
- [ ] Menggunakan operators (arithmetic, comparison, logical)
- [ ] Membuat conditional statements (if-else, switch)
- [ ] Menggunakan loops (for, while, foreach)
- [ ] Membuat dan memanipulasi arrays
- [ ] Membuat functions dengan parameters dan return values
- [ ] Membuat class sederhana dengan properties dan methods
- [ ] Menggunakan include/require untuk modularisasi
- [ ] Mengakses superglobals ($_GET, $_POST, $_SESSION)

---

## ❓ Quiz: PHP Fundamentals

Uji pemahaman Anda dengan menjawab pertanyaan berikut:

### Pertanyaan 1
**Apa output dari kode berikut?**
```php
$x = 5;
$y = "5";
var_dump($x === $y);
```

- [ ] A. true
- [ ] B. false
- [ ] C. 5
- [ ] D. Error

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. false**

**Penjelasan:**
Operator `===` (identical) membandingkan **nilai DAN tipe data**.
- `$x = 5` adalah **integer**
- `$y = "5"` adalah **string**

Meskipun nilainya sama (5), tipe datanya berbeda, sehingga hasilnya `false`.

Jika menggunakan `==` (equal), hasilnya akan `true` karena hanya membandingkan nilai.

</details>

---

### Pertanyaan 2
**Cara yang benar untuk membuat associative array adalah?**

- [ ] A. `$arr = array[1 => "a", 2 => "b"];`
- [ ] B. `$arr = ["name" => "John", "age" => 25];`
- [ ] C. `$arr = {"name": "John", "age": 25};`
- [ ] D. `$arr = (name => "John", age => 25);`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `$arr = ["name" => "John", "age" => 25];`**

**Penjelasan:**
PHP menggunakan sintaks `=>` untuk mendefinisikan key-value pair dalam associative array.

```php
// Sintaks modern (PHP 5.4+)
$arr = ["name" => "John", "age" => 25];

// Sintaks lama
$arr = array("name" => "John", "age" => 25);

// Akses data
echo $arr["name"];  // Output: John
```

</details>

---

### Pertanyaan 3
**Apa perbedaan `include` dan `require`?**

- [ ] A. Tidak ada perbedaan
- [ ] B. `require` lebih cepat dari `include`
- [ ] C. `include` memberikan warning jika file tidak ada, `require` memberikan fatal error
- [ ] D. `include` hanya untuk PHP, `require` untuk HTML

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. `include` memberikan warning jika file tidak ada, `require` memberikan fatal error**

**Penjelasan:**

| Perintah | Jika file tidak ada |
|----------|---------------------|
| `include` | Warning, script **lanjut** |
| `require` | Fatal error, script **berhenti** |

```php
// Jika config.php tidak ada:
include 'config.php';  // Warning, tapi script lanjut
require 'config.php';  // Fatal error, script berhenti

// Gunakan require untuk file penting (database, core functions)
// Gunakan include untuk file opsional (sidebar, widget)
```

</details>

---

### Pertanyaan 4
**Apa output dari loop berikut?**
```php
for ($i = 0; $i < 5; $i++) {
    if ($i == 2) continue;
    if ($i == 4) break;
    echo $i;
}
```

- [ ] A. 01234
- [ ] B. 0123
- [ ] C. 013
- [ ] D. 01

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. 013**

**Penjelasan:**
- `$i = 0`: echo 0
- `$i = 1`: echo 1
- `$i = 2`: **continue** → skip, tidak echo
- `$i = 3`: echo 3
- `$i = 4`: **break** → keluar loop

Hasil: `013`

`continue` = lompat ke iterasi berikutnya
`break` = keluar dari loop sepenuhnya

</details>

---

### Pertanyaan 5
**Dalam OOP PHP, keyword `$this` digunakan untuk?**

- [ ] A. Membuat object baru
- [ ] B. Memanggil static method
- [ ] C. Mengakses property/method dari object saat ini
- [ ] D. Mendefinisikan constructor

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Mengakses property/method dari object saat ini**

**Penjelasan:**
`$this` adalah referensi ke **object saat ini** (current instance).

```php
class User {
    private $name;
    
    public function setName($name) {
        $this->name = $name;  // Akses property dari object ini
    }
    
    public function greet() {
        return $this->getName();  // Panggil method dari object ini
    }
    
    public function getName() {
        return $this->name;
    }
}

$user = new User();
$user->setName("John");
echo $user->greet();  // Output: John
```

</details>

---

## 🎯 Practice Exercise

Buat file PHP yang:

### 1. Function Calculate Grade
```php
function calculateGrade($score) {
    // Return A, B, C, D, atau F berdasarkan score
    // A: 90-100, B: 80-89, C: 70-79, D: 60-69, F: < 60
}

// Test
echo calculateGrade(85);  // Output: B
```

<details>
<summary>💡 Lihat Contoh Solusi</summary>

```php
function calculateGrade($score) {
    if ($score >= 90) return 'A';
    if ($score >= 80) return 'B';
    if ($score >= 70) return 'C';
    if ($score >= 60) return 'D';
    return 'F';
}
```

</details>

---

### 2. Class Student
```php
class Student {
    // Properties: name, age, grades (array)
    // Methods: addGrade(), getAverage(), getStatus()
}

// Test
$student = new Student("John", 20);
$student->addGrade(85);
$student->addGrade(90);
echo $student->getAverage();  // Output: 87.5
```

<details>
<summary>💡 Lihat Contoh Solusi</summary>

```php
class Student {
    public $name;
    public $age;
    private $grades = [];
    
    public function __construct($name, $age) {
        $this->name = $name;
        $this->age = $age;
    }
    
    public function addGrade($grade) {
        $this->grades[] = $grade;
    }
    
    public function getAverage() {
        if (count($this->grades) === 0) return 0;
        return array_sum($this->grades) / count($this->grades);
    }
    
    public function getStatus() {
        return $this->getAverage() >= 60 ? "PASS" : "FAIL";
    }
}
```

</details>

---

### 3. Array Manipulation
```php
$students = [
    ["name" => "Alice", "score" => 85],
    ["name" => "Bob", "score" => 92],
    ["name" => "Charlie", "score" => 55],
    ["name" => "Diana", "score" => 78],
];

// Tasks:
// 1. Sort by score (descending)
// 2. Filter only passing students (score >= 60)
// 3. Calculate average score of passing students
```

<details>
<summary>💡 Lihat Contoh Solusi</summary>

```php
$students = [
    ["name" => "Alice", "score" => 85],
    ["name" => "Bob", "score" => 92],
    ["name" => "Charlie", "score" => 55],
    ["name" => "Diana", "score" => 78],
];

// 1. Sort by score descending
usort($students, function($a, $b) {
    return $b['score'] - $a['score'];
});

// 2. Filter passing students
$passing = array_filter($students, function($s) {
    return $s['score'] >= 60;
});

// 3. Calculate average
$total = array_sum(array_column($passing, 'score'));
$average = $total / count($passing);

echo "Passing students: " . count($passing) . "\n";
echo "Average: " . $average;
```

</details>

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="README.md" style="text-decoration: none;">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Previous: Prerequisite Overview
      </button>
    </a>
  </div>
  <div>
    <a href="database-basics.md" style="text-decoration: none;">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Next: Database & SQL Basics →
      </button>
    </a>
  </div>
</div>
