# 🌐 Web Development Basics

## 🎯 Learning Objectives

Setelah mempelajari bagian ini, Anda akan memahami:
- ✅ Bagaimana website bekerja (dari browser ke server)
- ✅ Protokol HTTP dan komponennya
- ✅ Arsitektur Client-Server
- ✅ Struktur URL dan fungsinya
- ✅ Perbedaan Frontend vs Backend

---

## 🤔 Bagaimana Website Bekerja?

Bayangkan Anda sedang **memesan makanan di restoran**:

```
🧑 Anda (Browser)     →    📝 Pesanan (Request)      →    👨‍🍳 Dapur (Server)
                      ←    🍽️ Makanan (Response)     ←
```

### 🔄 Alur Sederhana

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white; margin: 20px 0;">

**Langkah-langkah saat Anda membuka website:**

1. **🖱️ Anda ketik URL** → `www.example.com`
2. **🔍 Browser mencari alamat** → DNS mengubah domain menjadi IP address
3. **📨 Browser mengirim request** → "Halo server, saya mau halaman ini!"
4. **⚙️ Server memproses** → Server mencari/membuat halaman yang diminta
5. **📬 Server mengirim response** → HTML, CSS, JS, gambar, dll
6. **🖥️ Browser menampilkan** → Render menjadi halaman yang Anda lihat

</div>

### 💡 Analogi Lebih Detail

| Komponen Web | Analogi Restoran | Fungsi |
|--------------|------------------|--------|
| **Browser** | Pelanggan | Meminta dan menampilkan informasi |
| **URL** | Alamat Restoran | Lokasi tujuan yang ingin dikunjungi |
| **HTTP Request** | Pesanan Makanan | Permintaan spesifik ke server |
| **Server** | Dapur + Koki | Memproses permintaan, menyiapkan data |
| **HTTP Response** | Makanan yang Disajikan | Data yang dikembalikan ke browser |
| **HTML/CSS/JS** | Penyajian & Rasa | Tampilan dan interaksi halaman |

---

## 📡 HTTP Protocol

**HTTP** (HyperText Transfer Protocol) adalah "bahasa" yang digunakan browser dan server untuk berkomunikasi.

### 🔵 HTTP Request

Ketika browser meminta sesuatu ke server:

```http
GET /products/123 HTTP/1.1
Host: www.toko-online.com
User-Agent: Chrome/91.0
Accept: text/html
```

**Komponen Request:**

| Bagian | Contoh | Penjelasan |
|--------|--------|------------|
| **Method** | `GET` | Jenis aksi yang diminta |
| **Path** | `/products/123` | Lokasi resource di server |
| **Host** | `www.toko-online.com` | Alamat server tujuan |
| **Headers** | `User-Agent: Chrome` | Informasi tambahan |

### 📋 HTTP Methods (Paling Penting!)

<div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 15px; margin: 20px 0;">

<div style="background: #E3F2FD; padding: 15px; border-radius: 8px; border-left: 4px solid #2196F3;">
<h4>🔵 GET</h4>
<p><strong>Fungsi:</strong> Mengambil/membaca data</p>
<p><strong>Contoh:</strong> Buka halaman, lihat produk</p>
<p><strong>Ciri:</strong> Data terlihat di URL</p>
</div>

<div style="background: #E8F5E9; padding: 15px; border-radius: 8px; border-left: 4px solid #4CAF50;">
<h4>🟢 POST</h4>
<p><strong>Fungsi:</strong> Mengirim/membuat data baru</p>
<p><strong>Contoh:</strong> Submit form, login</p>
<p><strong>Ciri:</strong> Data tersembunyi di body</p>
</div>

<div style="background: #FFF3E0; padding: 15px; border-radius: 8px; border-left: 4px solid #FF9800;">
<h4>🟠 PUT/PATCH</h4>
<p><strong>Fungsi:</strong> Mengupdate data yang ada</p>
<p><strong>Contoh:</strong> Edit profil, update artikel</p>
<p><strong>Ciri:</strong> Butuh ID data yang diupdate</p>
</div>

<div style="background: #FFEBEE; padding: 15px; border-radius: 8px; border-left: 4px solid #F44336;">
<h4>🔴 DELETE</h4>
<p><strong>Fungsi:</strong> Menghapus data</p>
<p><strong>Contoh:</strong> Hapus komentar, hapus file</p>
<p><strong>Ciri:</strong> Butuh ID data yang dihapus</p>
</div>

</div>

### 🟣 HTTP Response

Server merespons dengan:

```http
HTTP/1.1 200 OK
Content-Type: text/html
Content-Length: 1234

<!DOCTYPE html>
<html>
  <body>Hello World!</body>
</html>
```

### 📊 HTTP Status Codes

<div style="background: #f8f9fa; padding: 20px; border-radius: 10px; margin: 20px 0;">

| Kode | Kategori | Arti | Contoh |
|------|----------|------|--------|
| **2xx** | ✅ Success | Berhasil | `200 OK`, `201 Created` |
| **3xx** | ↪️ Redirect | Dialihkan | `301 Moved`, `302 Found` |
| **4xx** | ❌ Client Error | Kesalahan user | `404 Not Found`, `403 Forbidden` |
| **5xx** | 💥 Server Error | Kesalahan server | `500 Internal Error` |

</div>

> 💡 **Tips Mengingat:**
> - **2xx** = "Yay, berhasil!" 🎉
> - **4xx** = "Kamu yang salah!" 👆
> - **5xx** = "Server yang salah!" 💻

---

## 🏗️ Arsitektur Client-Server

### Apa itu Client dan Server?

<div style="display: flex; justify-content: space-around; margin: 30px 0; flex-wrap: wrap;">

<div style="background: #E3F2FD; padding: 20px; border-radius: 10px; text-align: center; min-width: 200px; margin: 10px;">
<h3>👤 CLIENT</h3>
<p>Browser, Mobile App, Desktop App</p>
<hr>
<p><strong>Tugas:</strong></p>
<ul style="text-align: left;">
<li>Mengirim request</li>
<li>Menerima response</li>
<li>Menampilkan data</li>
<li>Interaksi dengan user</li>
</ul>
</div>

<div style="background: #E8F5E9; padding: 20px; border-radius: 10px; text-align: center; min-width: 200px; margin: 10px;">
<h3>🖥️ SERVER</h3>
<p>Apache, Nginx, IIS</p>
<hr>
<p><strong>Tugas:</strong></p>
<ul style="text-align: left;">
<li>Menerima request</li>
<li>Memproses logika</li>
<li>Akses database</li>
<li>Mengirim response</li>
</ul>
</div>

</div>

### 🔄 Flow Diagram

```
┌─────────────┐         HTTP Request          ┌─────────────┐
│             │  ───────────────────────────► │             │
│   CLIENT    │                               │   SERVER    │
│  (Browser)  │  ◄─────────────────────────── │  (Apache)   │
│             │         HTTP Response         │             │
└─────────────┘                               └─────────────┘
       │                                             │
       │                                             │
       ▼                                             ▼
┌─────────────┐                               ┌─────────────┐
│  HTML/CSS   │                               │  PHP/MySQL  │
│ JavaScript  │                               │  Database   │
└─────────────┘                               └─────────────┘
```

---

## 🔗 Struktur URL

URL (Uniform Resource Locator) adalah alamat lengkap untuk mengakses resource di web.

### 📝 Anatomi URL

```
https://www.example.com:8080/products/search?category=electronics&sort=price#results
└─┬──┘ └───────┬───────┘└─┬─┘└──────┬──────┘└────────────┬────────────────┘└───┬───┘
  │            │          │         │                     │                     │
Protocol     Host       Port      Path              Query String            Fragment
```

### 🧩 Penjelasan Setiap Bagian

| Komponen | Contoh | Fungsi |
|----------|--------|--------|
| **Protocol** | `https://` | Cara komunikasi (http/https) |
| **Host/Domain** | `www.example.com` | Alamat server |
| **Port** | `:8080` | Pintu masuk di server (default: 80/443) |
| **Path** | `/products/search` | Lokasi halaman/resource |
| **Query String** | `?category=electronics` | Parameter tambahan |
| **Fragment** | `#results` | Posisi di dalam halaman |

### 💻 Contoh URL di Kehidupan Nyata

```
🛒 E-commerce:
https://tokopedia.com/search?q=laptop&sort=price

📺 Video:
https://youtube.com/watch?v=abc123

📱 Social Media:
https://instagram.com/username/posts/12345
```

---

## 🎨 Frontend vs Backend

### Apa Bedanya?

<div style="display: grid; grid-template-columns: 1fr 1fr; gap: 20px; margin: 20px 0;">

<div style="background: linear-gradient(135deg, #667eea 0%, #764ba2 100%); padding: 20px; border-radius: 10px; color: white;">
<h3>🎨 FRONTEND</h3>
<p><em>"Apa yang dilihat user"</em></p>
<hr style="border-color: rgba(255,255,255,0.3);">
<p><strong>Bahasa:</strong></p>
<ul>
<li>HTML (Struktur)</li>
<li>CSS (Tampilan)</li>
<li>JavaScript (Interaksi)</li>
</ul>
<p><strong>Contoh:</strong></p>
<ul>
<li>Tombol yang bisa diklik</li>
<li>Form input</li>
<li>Animasi</li>
<li>Layout halaman</li>
</ul>
</div>

<div style="background: linear-gradient(135deg, #11998e 0%, #38ef7d 100%); padding: 20px; border-radius: 10px; color: white;">
<h3>⚙️ BACKEND</h3>
<p><em>"Apa yang terjadi di belakang layar"</em></p>
<hr style="border-color: rgba(255,255,255,0.3);">
<p><strong>Bahasa:</strong></p>
<ul>
<li>PHP, Python, Java</li>
<li>Node.js, Ruby</li>
<li>SQL (Database)</li>
</ul>
<p><strong>Contoh:</strong></p>
<ul>
<li>Proses login</li>
<li>Simpan ke database</li>
<li>Kirim email</li>
<li>Hitung total belanja</li>
</ul>
</div>

</div>

### 🎭 Analogi Teater

| Aspek | Frontend | Backend |
|-------|----------|---------|
| **Lokasi** | Panggung | Belakang panggung |
| **Pelaku** | Aktor | Kru, sutradara |
| **Fokus** | Penampilan | Persiapan & logistik |
| **Terlihat?** | Ya, oleh penonton | Tidak terlihat |

---

## ❓ Quiz: Web Development Basics

Uji pemahaman Anda dengan menjawab pertanyaan berikut:

### Pertanyaan 1
**Ketika Anda mengetik URL di browser dan menekan Enter, apa yang pertama kali terjadi?**

- [ ] A. Browser langsung menampilkan halaman
- [ ] B. Server mengirim response
- [ ] C. Browser mengirim HTTP request ke server
- [ ] D. Database menyimpan data

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: C. Browser mengirim HTTP request ke server**

**Penjelasan:**
Langkah pertama adalah browser mengirim HTTP request ke server. Ini seperti Anda memesan makanan di restoran - Anda harus pesan dulu (request) sebelum makanan disajikan (response).

Urutan lengkapnya:
1. Browser resolve DNS (cari IP dari domain)
2. **Browser kirim HTTP request** ← Langkah pertama yang terlihat
3. Server terima dan proses request
4. Server kirim HTTP response
5. Browser render halaman

</details>

---

### Pertanyaan 2
**HTTP Method mana yang digunakan untuk mengirim data form login?**

- [ ] A. GET
- [ ] B. POST
- [ ] C. PUT
- [ ] D. DELETE

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. POST**

**Penjelasan:**
POST digunakan untuk mengirim data sensitif seperti password karena:
- Data **tidak terlihat di URL** (berbeda dengan GET)
- Data dikirim di **body request**, bukan di URL
- Lebih **aman** untuk data sensitif

Contoh perbedaan:
```
GET:  /login?username=john&password=secret  ❌ Password terlihat!
POST: /login (data di body, tidak terlihat)  ✅ Lebih aman
```

</details>

---

### Pertanyaan 3
**Apa arti HTTP Status Code 404?**

- [ ] A. Server error
- [ ] B. Redirect ke halaman lain
- [ ] C. Request berhasil
- [ ] D. Halaman tidak ditemukan

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: D. Halaman tidak ditemukan**

**Penjelasan:**
- **4xx** = Client Error (kesalahan dari sisi user/browser)
- **404** = "Not Found" - Resource yang diminta tidak ada di server

Contoh penyebab 404:
- URL salah ketik
- Halaman sudah dihapus
- Link rusak (broken link)

Status code lainnya:
- 200 = OK (berhasil)
- 500 = Server Error (masalah di server)
- 301 = Redirect permanent

</details>

---

### Pertanyaan 4
**Bagian URL mana yang berisi parameter untuk filtering data?**

Contoh: `https://shop.com/products?category=shoes&size=42`

- [ ] A. Protocol
- [ ] B. Host
- [ ] C. Path
- [ ] D. Query String

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: D. Query String**

**Penjelasan:**
Query String adalah bagian setelah tanda `?` yang berisi parameter dalam format `key=value`:

```
https://shop.com/products?category=shoes&size=42
                         └──────────────────────┘
                              Query String
                         
Berisi:
- category = shoes
- size = 42
```

Digunakan untuk:
- Filter data
- Search query
- Pagination (`?page=2`)
- Sorting (`?sort=price`)

</details>

---

### Pertanyaan 5
**Mana yang merupakan tugas BACKEND?**

- [ ] A. Menampilkan animasi loading
- [ ] B. Memvalidasi data dan menyimpan ke database
- [ ] C. Mengatur layout halaman dengan CSS
- [ ] D. Menangani klik tombol dengan JavaScript

<details>
<summary>🔍 Lihat Jawaban & Penjelasan</summary>

**✅ Jawaban: B. Memvalidasi data dan menyimpan ke database**

**Penjelasan:**
Backend menangani proses yang terjadi di **server**, termasuk:
- ✅ Validasi data di server
- ✅ Operasi database (CRUD)
- ✅ Authentication & authorization
- ✅ Business logic

Frontend menangani yang terjadi di **browser**:
- Animasi (JavaScript/CSS)
- Layout (HTML/CSS)
- Interaksi user (JavaScript)

> ⚠️ **Penting:** Validasi sebaiknya dilakukan di KEDUA sisi - Frontend untuk UX, Backend untuk keamanan!

</details>

---

## ✅ Checklist Pemahaman

Pastikan Anda sudah memahami:

- [ ] Alur request-response antara browser dan server
- [ ] Perbedaan HTTP methods (GET, POST, PUT, DELETE)
- [ ] Arti HTTP status codes (2xx, 4xx, 5xx)
- [ ] Komponen-komponen URL
- [ ] Perbedaan frontend dan backend

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="README.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        ← Prerequisite Overview
      </button>
    </a>
  </div>
  <div>
    <a href="php-fundamentals.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px; cursor: pointer;">
        PHP Fundamentals →
      </button>
    </a>
  </div>
</div>
