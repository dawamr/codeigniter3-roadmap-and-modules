# 🔒 Phase 5 - Input & Security Dasar

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Memahami Input Class di CI3
- ✅ Menerapkan XSS filtering
- ✅ Mengimplementasi CSRF protection
- ✅ Mencegah SQL injection
- ✅ Melakukan output escaping
- ✅ Mengamankan password dengan hashing

---

## 📋 Overview

Security adalah **prioritas utama** dalam web development. Phase ini fokus pada teknik-teknik fundamental untuk mengamankan aplikasi CI3 dari serangan umum.

> 💡 **Analogi Sederhana:**  
> Security adalah **sistem keamanan rumah** 🏠. Input filtering = pintu depan 🚪, CSRF = alarm 🚨, Password hashing = brankas 🔐!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #FFEBEE; border-left: 4px solid #D32F2F; padding: 15px;">
    <h4>📥 Input Class</h4>
    <small>Safe input handling</small>
  </div>
  
  <div style="background: #E8F5E9; border-left: 4px solid #388E3C; padding: 15px;">
    <h4>🛡️ XSS Protection</h4>
    <small>Cross-site scripting prevention</small>
  </div>
  
  <div style="background: #E3F2FD; border-left: 4px solid #1976D2; padding: 15px;">
    <h4>🔐 CSRF Protection</h4>
    <small>Request forgery prevention</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #F57C00; padding: 15px;">
    <h4>💉 SQL Injection</h4>
    <small>Database security</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #7B1FA2; padding: 15px;">
    <h4>🔑 Password Security</h4>
    <small>Hashing & verification</small>
  </div>
  
  <div style="background: #E0F7FA; border-left: 4px solid #0097A7; padding: 15px;">
    <h4>📤 Output Escaping</h4>
    <small>Safe data display</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[📥 Input Class Mastery](input-class.md)**
   - POST, GET, COOKIE, SERVER
   - Input filtering methods
   - IP address & user agent

2. **[🛡️ XSS Filtering](xss-protection.md)**
   - Global vs per-input filtering
   - XSS clean function
   - Best practices

3. **[🔐 CSRF Protection](csrf-protection.md)**
   - Token generation
   - Form integration
   - AJAX handling

4. **[💉 SQL Injection Prevention](sql-injection.md)**
   - Query binding
   - Escaping methods
   - Query Builder safety

5. **[🔑 Password Management](password-security.md)**
   - password_hash()
   - password_verify()
   - Salt & algorithms

### Advanced Topics
6. **[📤 Output Security](output-security.md)**
   - HTML escaping
   - URL encoding
   - JavaScript escaping

7. **[🔒 Encryption](encryption.md)**
   - CI3 Encryption library
   - Two-way encryption
   - Key management

8. **[🚫 Security Headers](security-headers.md)**
   - Content Security Policy
   - X-Frame-Options
   - HTTPS enforcement

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Secure form implementation
   - Attack simulations

10. **[❓ Quiz](quiz.md)**
    - Security knowledge test
    - 10 questions

---

## 🎯 Key Concepts Preview

### Input Handling
```php
// Safe input retrieval
$username = $this->input->post('username', TRUE); // XSS filtered
$email = $this->input->post('email', TRUE);

// Check request method
if ($this->input->method() === 'post') {
    // Process POST request
}

// Get user IP
$ip_address = $this->input->ip_address();
```

### CSRF Implementation
```php
// In config.php
$config['csrf_protection'] = TRUE;
$config['csrf_token_name'] = 'csrf_token';
$config['csrf_cookie_name'] = 'csrf_cookie';

// In form
<?= form_open('user/update') ?> <!-- Auto includes CSRF -->
```

### Password Security
```php
// Registration - Hash password
$hash = password_hash($password, PASSWORD_DEFAULT);

// Login - Verify password
if (password_verify($input_password, $stored_hash)) {
    // Password correct
}
```

---

## 🚨 Common Security Threats

### XSS (Cross-Site Scripting)
- Injecting malicious scripts
- Stealing cookies/sessions
- **Prevention:** Input filtering, output escaping

### CSRF (Cross-Site Request Forgery)
- Unauthorized actions
- State-changing operations
- **Prevention:** CSRF tokens

### SQL Injection
- Database manipulation
- Data theft
- **Prevention:** Query binding, escaping

### Password Attacks
- Brute force
- Rainbow tables
- **Prevention:** Strong hashing, rate limiting

---

## ✅ Success Criteria

- [ ] Use Input Class properly
- [ ] Implement XSS filtering
- [ ] Enable CSRF protection
- [ ] Prevent SQL injection
- [ ] Hash passwords correctly
- [ ] Escape output properly
- [ ] Understand security threats
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Secure User Portal"**
- Secure registration/login
- Protected user area
- File upload security
- Activity logging
- Two-factor authentication (basic)

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-4-form/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 4: Form & Validation
      </button>
    </a>
  </div>
  <div>
    <a href="input-class.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Input Class →
      </button>
    </a>
  </div>
</div>
