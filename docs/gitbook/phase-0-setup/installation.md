# 📥 Installing CodeIgniter 3

## 🎯 Learning Objectives

Setelah menyelesaikan bagian ini, Anda akan:
- ✅ Berhasil download dan install CodeIgniter 3
- ✅ Menjalankan CI3 di local server
- ✅ Melihat welcome page CI3
- ✅ Memahami perbedaan dengan PHP Native setup

---

## 📋 Installation Overview

CodeIgniter 3 memiliki proses instalasi yang **sangat sederhana** - hanya download dan extract! Tidak perlu composer atau command line kompleks.

> 💡 **Perbandingan dengan PHP Native:**  
> **PHP Native:** Buat folder → mulai coding dari nol  
> **CodeIgniter 3:** Download → Extract → Framework siap pakai!

---

## 🔧 Step-by-Step Installation

### Step 1: Download CodeIgniter 3

#### Option A: Download dari Website Official
1. Buka browser, pergi ke: [https://codeigniter.com/download](https://codeigniter.com/download)
2. Klik **"Download CodeIgniter 3"** (versi terbaru 3.1.13)
3. File `CodeIgniter-3.1.13.zip` akan terdownload (~2MB)

#### Option B: Download dari GitHub
```bash
# Via Git Clone
git clone https://github.com/bcit-ci/CodeIgniter.git

# Atau download ZIP dari:
https://github.com/bcit-ci/CodeIgniter/archive/3.1.13.zip
```

> 📌 **Note:** Pastikan download CodeIgniter **3.x**, bukan CodeIgniter 4!

---

### Step 2: Extract Files

#### Windows:
1. Klik kanan file `CodeIgniter-3.1.13.zip`
2. Pilih **"Extract All"** atau **"Extract Here"**
3. Rename folder hasil extract menjadi nama project Anda
   - Contoh: `ci3-belajar` atau `myapp`

#### Mac/Linux:
```bash
# Extract via terminal
unzip CodeIgniter-3.1.13.zip

# Rename folder
mv CodeIgniter-3.1.13 ci3-belajar
```

---

### Step 3: Move to Web Server Directory

Pindahkan folder project ke directory web server:

#### XAMPP (Windows/Mac/Linux):
```
C:\xampp\htdocs\ci3-belajar\        # Windows
/Applications/XAMPP/htdocs/ci3-belajar/  # Mac
/opt/lampp/htdocs/ci3-belajar/      # Linux
```

#### Laragon (Windows):
```
C:\laragon\www\ci3-belajar\
```

#### MAMP (Mac):
```
/Applications/MAMP/htdocs/ci3-belajar/
```

#### WAMP (Windows):
```
C:\wamp64\www\ci3-belajar\
```

> ⚠️ **Important:** Pastikan web server (Apache) dan MySQL sudah running!

---

### Step 4: Test Installation

1. **Start Web Server:**
   - XAMPP: Klik "Start" pada Apache dan MySQL
   - Laragon: Klik "Start All"
   - MAMP: Klik "Start Servers"

2. **Buka Browser:**
   - Ketik: `http://localhost/ci3-belajar/`
   - Atau: `http://127.0.0.1/ci3-belajar/`

3. **Expected Result:**

<div style="border: 2px solid #4CAF50; border-radius: 10px; padding: 20px; margin: 20px 0; background: #f9f9f9;">
  <h2 style="color: #4CAF50;">Welcome to CodeIgniter!</h2>
  <p>The page you are looking at is being generated dynamically by CodeIgniter.</p>
  <p>If you would like to edit this page you'll find it located at:</p>
  <code style="background: #333; color: #fff; padding: 5px;">application/views/welcome_message.php</code>
  <p>The corresponding controller for this page is found at:</p>
  <code style="background: #333; color: #fff; padding: 5px;">application/controllers/Welcome.php</code>
</div>

✅ **Congratulations!** Jika melihat halaman di atas, CodeIgniter 3 berhasil terinstall!

---

## 🔍 Verify Installation

### Check PHP Version
CodeIgniter 3 requires PHP 5.6+. Verify dengan:

```php
// Buat file: ci3-belajar/phpinfo.php
<?php
phpinfo();
?>
```

Akses: `http://localhost/ci3-belajar/phpinfo.php`

Cari:
- **PHP Version:** Minimal 5.6 (recommended 7.4+)
- **Loaded Modules:** mysqli, mbstring, json

### Check Directory Structure
Pastikan struktur folder lengkap:

```
ci3-belajar/
├── application/
│   ├── cache/
│   ├── config/
│   ├── controllers/
│   ├── core/
│   ├── helpers/
│   ├── hooks/
│   ├── language/
│   ├── libraries/
│   ├── logs/
│   ├── models/
│   ├── third_party/
│   └── views/
├── system/
├── user_guide/
├── composer.json
├── contributing.md
├── index.php
├── license.txt
└── readme.rst
```

---

## 🛠️ Post-Installation Setup

### 1. Remove Unnecessary Files

Untuk production, hapus file yang tidak perlu:

```bash
# Hapus user guide (dokumentasi offline)
rm -rf user_guide/

# Hapus file readme dan license
rm readme.rst
rm contributing.md
rm composer.json  # Jika tidak pakai composer
```

### 2. Set Folder Permissions (Linux/Mac)

```bash
# Berikan write permission untuk cache dan logs
chmod 755 application/cache/
chmod 755 application/logs/
```

### 3. Create .htaccess (Optional)

Untuk remove `index.php` dari URL:

```apache
# Buat file: ci3-belajar/.htaccess
RewriteEngine On
RewriteCond %{REQUEST_FILENAME} !-f
RewriteCond %{REQUEST_FILENAME} !-d
RewriteRule ^(.*)$ index.php/$1 [L]
```

> 📌 **Note:** Pastikan mod_rewrite enabled di Apache!

---

## 🔄 Alternative Installation Methods

### Method 1: Using Composer

Meskipun tidak wajib, CI3 bisa diinstall via Composer:

```bash
# Install via Composer
composer create-project codeigniter/framework ci3-belajar

# Atau dengan version specific
composer create-project codeigniter/framework:3.1.13 ci3-belajar
```

### Method 2: Using Git

Untuk developer yang suka version control:

```bash
# Clone repository
git clone https://github.com/bcit-ci/CodeIgniter.git ci3-belajar

# Masuk ke folder
cd ci3-belajar

# Checkout ke version 3.1.13
git checkout v3.1.13

# Remove git history (optional)
rm -rf .git
```

---

## 🚨 Troubleshooting

### Problem 1: Blank Page / Error 500

**Possible Causes:**
- PHP version terlalu lama (< 5.6)
- Missing PHP extensions
- Permission issues

**Solution:**
```bash
# Check PHP version
php -v

# Check error logs
# XAMPP: xampp/apache/logs/error.log
# Check CI logs: application/logs/
```

### Problem 2: 404 Not Found

**Possible Causes:**
- Wrong URL
- Apache not running
- Wrong folder location

**Solution:**
- Verify URL: `http://localhost/[folder-name]/`
- Check Apache status
- Ensure folder in htdocs/www

### Problem 3: Welcome Page Styling Broken

**Possible Causes:**
- Base URL not configured
- CSS path issues

**Solution:**
Will be fixed in Configuration section

### Problem 4: "Disallowed Key Characters"

**Possible Causes:**
- Special characters in URL
- Cookie issues

**Solution:**
- Clear browser cookies
- Use clean URLs

---

## ✅ Installation Checklist

Pastikan semua step completed:

- [ ] Download CodeIgniter 3.1.13
- [ ] Extract file ZIP
- [ ] Rename folder sesuai nama project
- [ ] Move ke htdocs/www directory
- [ ] Start Apache dan MySQL
- [ ] Test di browser: `http://localhost/[project-name]/`
- [ ] Lihat "Welcome to CodeIgniter" page
- [ ] Verify PHP version (≥ 5.6)
- [ ] Check folder structure lengkap
- [ ] (Optional) Remove unnecessary files
- [ ] (Optional) Set folder permissions
- [ ] (Optional) Create .htaccess

---

## 📊 Comparison Table

| Aspect | PHP Native | CodeIgniter 3 |
|--------|------------|---------------|
| **Setup Time** | Instant (buat folder) | 5 menit (download & extract) |
| **Initial Files** | 0 (blank) | ~200 files (framework) |
| **Structure** | DIY | Pre-organized (MVC) |
| **Features** | None | Router, Database, Session, etc. |
| **File Size** | 0 KB | ~2 MB |
| **Learning Curve** | Low (tapi susah maintain) | Medium (tapi scalable) |

---

## 🎯 What's Next?

Sekarang CI3 sudah terinstall, selanjutnya kita akan:

1. **Explore struktur folder** - Mengenal setiap folder
2. **Setup configuration** - Base URL, database, etc.
3. **Understand request flow** - Bagaimana CI3 bekerja
4. **Create first custom page** - Hello World dengan CI3

---

## 💡 Pro Tips

### Tip 1: Multiple Projects
Anda bisa install multiple CI3 projects:
```
htdocs/
├── project1-ci3/
├── project2-ci3/
└── project3-ci3/
```
Access: `localhost/project1-ci3/`, `localhost/project2-ci3/`, etc.

### Tip 2: Version Control
Immediately init Git after installation:
```bash
cd ci3-belajar
git init
git add .
git commit -m "Initial CI3 installation"
```

### Tip 3: Backup Fresh Install
Zip fresh installation sebagai template:
```bash
cp -r ci3-belajar ci3-template
zip -r ci3-fresh-template.zip ci3-template/
```

---

## 📝 Summary

✅ CodeIgniter 3 installation sangat simple - just download & extract  
✅ Tidak perlu command line atau composer (optional)  
✅ Welcome page = installation success  
✅ Struktur folder sudah organized (MVC ready)  
✅ Siap untuk configuration dan development  

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="README.md" style="text-decoration: none;">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Previous: Phase 0 Overview
      </button>
    </a>
  </div>
  <div>
    <a href="structure.md" style="text-decoration: none;">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        Next: Project Structure →
      </button>
    </a>
  </div>
</div>

---

<p align="center">
  <strong>🎉 Congratulations on installing CodeIgniter 3!</strong><br/>
  <em>You've taken the first step in your CI3 journey</em>
</p>
