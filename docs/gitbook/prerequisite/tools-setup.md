# 🛠️ Development Tools Setup

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan:
- ✅ Menginstall dan mengkonfigurasi local server (XAMPP/Laragon)
- ✅ Setup code editor (VS Code) dengan extensions berguna
- ✅ Menggunakan browser DevTools untuk debugging
- ✅ Memahami struktur direktori development

---

## 🖥️ Local Server

Untuk menjalankan PHP dan MySQL di komputer lokal, Anda butuh **local server stack**.

### 📦 Pilihan Local Server

<div style="display: grid; grid-template-columns: repeat(3, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; border-left: 4px solid #FF9800;">
<h4>🟠 XAMPP</h4>
<p><strong>Platform:</strong> Windows, Mac, Linux</p>
<p><strong>Isi:</strong> Apache, MySQL, PHP, Perl</p>
<p><strong>Cocok untuk:</strong> Pemula</p>
<p><a href="https://www.apachefriends.org/">Download XAMPP</a></p>
</div>

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; border-left: 4px solid #2196F3;">
<h4>🔵 Laragon</h4>
<p><strong>Platform:</strong> Windows</p>
<p><strong>Isi:</strong> Apache/Nginx, MySQL, PHP</p>
<p><strong>Cocok untuk:</strong> Windows user</p>
<p><a href="https://laragon.org/">Download Laragon</a></p>
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; border-left: 4px solid #4CAF50;">
<h4>🟢 MAMP</h4>
<p><strong>Platform:</strong> Mac, Windows</p>
<p><strong>Isi:</strong> Apache, MySQL, PHP</p>
<p><strong>Cocok untuk:</strong> Mac user</p>
<p><a href="https://www.mamp.info/">Download MAMP</a></p>
</div>

</div>

---

## 🟠 Instalasi XAMPP (Recommended)

### 📥 Langkah Instalasi

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

**Step 1: Download**
1. Buka [apachefriends.org](https://www.apachefriends.org/)
2. Pilih versi sesuai OS (Windows/Mac/Linux)
3. Download installer

**Step 2: Install**
1. Jalankan installer
2. Pilih komponen: ✅ Apache, ✅ MySQL, ✅ PHP, ✅ phpMyAdmin
3. Pilih lokasi instalasi (default: `C:\xampp`)
4. Tunggu proses instalasi selesai

**Step 3: Jalankan**
1. Buka XAMPP Control Panel
2. Klik **Start** pada Apache
3. Klik **Start** pada MySQL
4. Status berubah hijau = berjalan ✅

</div>

### ✅ Verifikasi Instalasi

Setelah XAMPP berjalan:

| Test | URL | Hasil yang Diharapkan |
|------|-----|----------------------|
| Apache | `http://localhost` | Halaman XAMPP Welcome |
| phpMyAdmin | `http://localhost/phpmyadmin` | Dashboard phpMyAdmin |
| PHP Info | Buat file `test.php` | Info PHP version |

**Membuat test.php:**
```php
<?php
// Simpan di: C:\xampp\htdocs\test.php
phpinfo();
```

Buka `http://localhost/test.php` - harus menampilkan informasi PHP.

---

## 📁 Struktur Direktori

### 🗂️ XAMPP Directory Structure

```
C:\xampp\
├── 📂 apache/          # Apache server files
├── 📂 mysql/           # MySQL database files
├── 📂 php/             # PHP installation
├── 📂 phpMyAdmin/      # phpMyAdmin web app
│
└── 📂 htdocs/          ⭐ FOLDER PROJECT ANDA
    ├── 📂 dashboard/   # XAMPP dashboard
    ├── 📄 index.php    # Default page
    │
    └── 📂 my-project/  # Project Anda di sini!
        ├── 📄 index.php
        └── 📂 assets/
```

### 💡 Penempatan Project

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; margin: 15px 0;">

**Semua project PHP harus diletakkan di folder `htdocs`!**

| OS | Lokasi htdocs |
|----|---------------|
| Windows (XAMPP) | `C:\xampp\htdocs\` |
| Windows (Laragon) | `C:\laragon\www\` |
| Mac (MAMP) | `/Applications/MAMP/htdocs/` |
| Linux (XAMPP) | `/opt/lampp/htdocs/` |

**Contoh:**
- Project di `C:\xampp\htdocs\belajar-ci3\`
- Akses via `http://localhost/belajar-ci3/`

</div>

---

## 💻 Visual Studio Code

VS Code adalah **code editor gratis** yang powerful dan ringan.

### 📥 Instalasi

1. Download dari [code.visualstudio.com](https://code.visualstudio.com/)
2. Install seperti aplikasi biasa
3. Jalankan VS Code

### 🧩 Extensions Wajib

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px;">
<h4>🐘 PHP Intelephense</h4>
<p>Autocomplete & error detection untuk PHP</p>
<p><code>bmewburn.vscode-intelephense-client</code></p>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px;">
<h4>🎨 PHP DocBlocker</h4>
<p>Auto-generate documentation comments</p>
<p><code>neilbrayfield.php-docblocker</code></p>
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>✨ Prettier</h4>
<p>Auto-format code agar rapi</p>
<p><code>esbenp.prettier-vscode</code></p>
</div>

<div style="background: #FCE4EC; padding: 15px; border-radius: 8px;">
<h4>🔍 GitLens</h4>
<p>Git integration yang powerful</p>
<p><code>eamodio.gitlens</code></p>
</div>

<div style="background: #F3E5F5; padding: 15px; border-radius: 8px;">
<h4>🌐 Live Server</h4>
<p>Auto-reload browser saat save</p>
<p><code>ritwickdey.LiveServer</code></p>
</div>

<div style="background: #E0F7FA; padding: 15px; border-radius: 8px;">
<h4>📁 Path Intellisense</h4>
<p>Autocomplete untuk file paths</p>
<p><code>christian-kohler.path-intellisense</code></p>
</div>

</div>

### ⚙️ Settings Recommended

Buka Settings (Ctrl+,) dan tambahkan:

```json
{
  "editor.fontSize": 14,
  "editor.tabSize": 4,
  "editor.wordWrap": "on",
  "editor.formatOnSave": true,
  "files.autoSave": "afterDelay",
  "emmet.includeLanguages": {
    "php": "html"
  },
  "php.validate.executablePath": "C:/xampp/php/php.exe"
}
```

### ⌨️ Keyboard Shortcuts Penting

| Shortcut | Fungsi |
|----------|--------|
| `Ctrl + S` | Save file |
| `Ctrl + P` | Quick open file |
| `Ctrl + Shift + P` | Command palette |
| `Ctrl + /` | Toggle comment |
| `Ctrl + D` | Select next occurrence |
| `Alt + ↑/↓` | Move line up/down |
| `Ctrl + Shift + F` | Search in all files |
| `F5` | Start debugging |

---

## 🔍 Browser DevTools

Setiap browser modern punya **Developer Tools** untuk debugging.

### 🚀 Cara Membuka DevTools

| Browser | Shortcut |
|---------|----------|
| Chrome | `F12` atau `Ctrl + Shift + I` |
| Firefox | `F12` atau `Ctrl + Shift + I` |
| Edge | `F12` atau `Ctrl + Shift + I` |
| Safari | `Cmd + Option + I` |

### 📋 Tab-Tab Penting

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px;">
<h4>🔍 Elements</h4>
<p><strong>Fungsi:</strong> Inspect HTML & CSS</p>
<p><strong>Gunakan untuk:</strong></p>
<ul>
<li>Lihat struktur HTML</li>
<li>Edit CSS live</li>
<li>Debug layout</li>
</ul>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px;">
<h4>📝 Console</h4>
<p><strong>Fungsi:</strong> Lihat log & error JavaScript</p>
<p><strong>Gunakan untuk:</strong></p>
<ul>
<li>Debug JavaScript</li>
<li>Lihat error messages</li>
<li>Test code snippets</li>
</ul>
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px;">
<h4>🌐 Network</h4>
<p><strong>Fungsi:</strong> Monitor HTTP requests</p>
<p><strong>Gunakan untuk:</strong></p>
<ul>
<li>Lihat API calls</li>
<li>Check response time</li>
<li>Debug AJAX</li>
</ul>
</div>

<div style="background: #FCE4EC; padding: 15px; border-radius: 8px;">
<h4>💾 Application</h4>
<p><strong>Fungsi:</strong> Manage storage</p>
<p><strong>Gunakan untuk:</strong></p>
<ul>
<li>Lihat cookies</li>
<li>Check localStorage</li>
<li>Manage sessions</li>
</ul>
</div>

</div>

### 🎯 Contoh Penggunaan

**Inspect Element:**
1. Klik kanan pada elemen di halaman
2. Pilih "Inspect"
3. Lihat HTML dan CSS elemen tersebut
4. Edit langsung untuk testing

**Lihat Network Request:**
1. Buka tab Network
2. Refresh halaman
3. Klik request untuk lihat detail
4. Cek Headers, Response, Timing

---

## 🔧 Troubleshooting

### ❌ Common Problems & Solutions

<details>
<summary><strong>🔴 Apache tidak bisa start (Port 80 conflict)</strong></summary>

**Penyebab:** Port 80 digunakan aplikasi lain (Skype, IIS, dll)

**Solusi:**
1. Buka XAMPP Control Panel
2. Klik **Config** → **Apache (httpd.conf)**
3. Cari `Listen 80` dan ubah ke `Listen 8080`
4. Cari `ServerName localhost:80` ubah ke `ServerName localhost:8080`
5. Restart Apache
6. Akses via `http://localhost:8080`

</details>

<details>
<summary><strong>🔴 MySQL tidak bisa start</strong></summary>

**Penyebab:** Port 3306 digunakan atau data corrupt

**Solusi 1 - Ganti Port:**
1. Klik **Config** → **my.ini**
2. Ubah `port=3306` ke `port=3307`
3. Restart MySQL

**Solusi 2 - Reset Data:**
1. Stop MySQL
2. Backup folder `C:\xampp\mysql\data`
3. Delete isi folder data (kecuali folder mysql, performance_schema, phpmyadmin)
4. Start MySQL

</details>

<details>
<summary><strong>🔴 phpMyAdmin Access Denied</strong></summary>

**Penyebab:** Password MySQL berubah

**Solusi:**
1. Buka `C:\xampp\phpMyAdmin\config.inc.php`
2. Cari baris `$cfg['Servers'][$i]['password']`
3. Isi dengan password MySQL Anda
4. Save dan refresh phpMyAdmin

</details>

<details>
<summary><strong>🔴 PHP file ter-download bukan dijalankan</strong></summary>

**Penyebab:** PHP module tidak aktif

**Solusi:**
1. Pastikan Apache sudah start
2. File harus di folder htdocs
3. Akses via `http://localhost/file.php` bukan `file://`

</details>

---

## ❓ Quiz: Development Tools

### Pertanyaan 1
**Di mana Anda harus meletakkan project PHP di XAMPP?**

- [ ] A. `C:\xampp\apache\`
- [ ] B. `C:\xampp\htdocs\`
- [ ] C. `C:\xampp\php\`
- [ ] D. `C:\xampp\mysql\`

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. `C:\xampp\htdocs\`**

**Penjelasan:**
`htdocs` adalah **document root** - folder yang bisa diakses via browser.

- Project di `C:\xampp\htdocs\myapp\`
- Diakses via `http://localhost/myapp/`

Folder lain bukan untuk project:
- `apache/` = file server Apache
- `php/` = instalasi PHP
- `mysql/` = data MySQL

</details>

---

### Pertanyaan 2
**Extension VS Code mana yang memberikan autocomplete untuk PHP?**

- [ ] A. Prettier
- [ ] B. GitLens
- [ ] C. PHP Intelephense
- [ ] D. Live Server

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. PHP Intelephense**

**Penjelasan:**
PHP Intelephense menyediakan:
- ✅ Autocomplete untuk PHP functions & classes
- ✅ Error detection
- ✅ Go to definition
- ✅ Parameter hints
- ✅ Code formatting

Extension lainnya:
- Prettier = format code (HTML, CSS, JS)
- GitLens = Git integration
- Live Server = auto-reload untuk HTML

</details>

---

### Pertanyaan 3
**Shortcut untuk membuka DevTools di browser adalah?**

- [ ] A. Ctrl + D
- [ ] B. Ctrl + T
- [ ] C. F12
- [ ] D. F5

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. F12**

**Penjelasan:**
`F12` membuka Developer Tools di hampir semua browser (Chrome, Firefox, Edge).

Alternatif lain:
- `Ctrl + Shift + I` (Windows/Linux)
- `Cmd + Option + I` (Mac)
- Klik kanan → Inspect

Shortcut lain:
- F5 = Refresh halaman
- Ctrl + T = Tab baru
- Ctrl + D = Bookmark

</details>

---

### Pertanyaan 4
**Jika Apache tidak bisa start karena port conflict, apa solusinya?**

- [ ] A. Uninstall XAMPP
- [ ] B. Ganti port dari 80 ke port lain seperti 8080
- [ ] C. Install ulang Windows
- [ ] D. Hapus folder htdocs

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Ganti port dari 80 ke port lain seperti 8080**

**Penjelasan:**
Port 80 adalah default HTTP port, yang mungkin digunakan aplikasi lain seperti:
- Skype
- IIS (Internet Information Services)
- VMware

**Cara mengubah:**
1. Edit `httpd.conf`
2. Ubah `Listen 80` → `Listen 8080`
3. Akses via `http://localhost:8080`

Port alternatif yang umum: 8080, 8000, 3000

</details>

---

### Pertanyaan 5
**Tab DevTools mana yang digunakan untuk melihat HTTP request ke server?**

- [ ] A. Elements
- [ ] B. Console
- [ ] C. Network
- [ ] D. Sources

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Network**

**Penjelasan:**
Tab Network menampilkan semua HTTP request:

**Informasi yang ditampilkan:**
- 🔹 URL yang direquest
- 🔹 HTTP Method (GET, POST, dll)
- 🔹 Status Code (200, 404, dll)
- 🔹 Response time
- 🔹 Response body

**Berguna untuk:**
- Debug AJAX calls
- Check API responses
- Analyze page load performance
- Verify form submissions

</details>

---

## ✅ Checklist Setup

Pastikan semua sudah terinstall dan berjalan:

- [ ] XAMPP/Laragon terinstall
- [ ] Apache bisa distart (hijau)
- [ ] MySQL bisa distart (hijau)
- [ ] `http://localhost` menampilkan halaman
- [ ] phpMyAdmin bisa diakses
- [ ] VS Code terinstall
- [ ] PHP Intelephense extension aktif
- [ ] Browser DevTools bisa dibuka (F12)
- [ ] Bisa membuat dan menjalankan file PHP test

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="database-basics.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Database & SQL Basics
      </button>
    </a>
  </div>
  <div>
    <a href="self-assessment.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Self-Assessment →
      </button>
    </a>
  </div>
</div>
