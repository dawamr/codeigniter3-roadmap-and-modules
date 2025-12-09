# 📝 Phase 4 - Form Handling & Validation

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Membuat form dengan Form Helper
- ✅ Menguasai Form Validation Library
- ✅ Menangani berbagai input types
- ✅ Membuat custom validation rules
- ✅ Menampilkan error messages yang user-friendly
- ✅ Repopulate form setelah error

---

## 📋 Overview

Form handling adalah aspek krusial dalam web development. CI3 menyediakan tools powerful untuk membuat, validasi, dan process form dengan aman dan efisien.

> 💡 **Analogi Sederhana:**  
> Form adalah **pintu masuk data** 🚪. Validation adalah **security guard** 👮 yang memastikan hanya data valid yang masuk!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8EAF6; border-left: 4px solid #3F51B5; padding: 15px;">
    <h4>📋 Form Helper</h4>
    <small>Generate form elements</small>
  </div>
  
  <div style="background: #FFF9C4; border-left: 4px solid #FBC02D; padding: 15px;">
    <h4>✔️ Validation Library</h4>
    <small>Rules & error handling</small>
  </div>
  
  <div style="background: #FFEBEE; border-left: 4px solid #F44336; padding: 15px;">
    <h4>🎯 Validation Rules</h4>
    <small>Built-in & custom rules</small>
  </div>
  
  <div style="background: #E0F7FA; border-left: 4px solid #00BCD4; padding: 15px;">
    <h4>💬 Error Messages</h4>
    <small>Display & customize</small>
  </div>
  
  <div style="background: #F1F8E9; border-left: 4px solid #689F38; padding: 15px;">
    <h4>🔄 Repopulation</h4>
    <small>Maintain user input</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #FF6F00; padding: 15px;">
    <h4>🛡️ Security</h4>
    <small>CSRF & XSS protection</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[📋 Form Helper Basics](form-helper.md)**
   - Form elements generation
   - Form attributes
   - Multipart forms

2. **[✔️ Validation Library](validation-library.md)**
   - Loading & configuration
   - Setting rules
   - Running validation

3. **[🎯 Validation Rules](validation-rules.md)**
   - Built-in rules reference
   - Combining rules
   - Conditional validation

4. **[💬 Error Handling](error-handling.md)**
   - Display errors
   - Custom error messages
   - Error delimiters

5. **[🔄 Form Repopulation](repopulation.md)**
   - set_value()
   - set_select()
   - set_checkbox()
   - set_radio()

### Advanced Topics
6. **[🎨 Custom Validation](custom-validation.md)**
   - Callback functions
   - Custom rule libraries
   - Complex validations

7. **[📤 File Upload Forms](upload-forms.md)**
   - File validation
   - Multiple uploads
   - Image validation

8. **[🔐 Form Security](form-security.md)**
   - CSRF tokens
   - XSS prevention
   - Input sanitization

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Complete form examples
   - Real-world scenarios

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### Form Generation
```php
<?= form_open('user/register') ?>
    <?= form_input([
        'name' => 'username',
        'class' => 'form-control',
        'placeholder' => 'Enter username',
        'value' => set_value('username')
    ]) ?>
    
    <?= form_dropdown('country', $countries, set_value('country'), [
        'class' => 'form-control'
    ]) ?>
    
    <?= form_submit('submit', 'Register', ['class' => 'btn btn-primary']) ?>
<?= form_close() ?>
```

### Validation Example
```php
$this->form_validation->set_rules('username', 'Username', 'required|min_length[5]|is_unique[users.username]');
$this->form_validation->set_rules('email', 'Email', 'required|valid_email');
$this->form_validation->set_rules('password', 'Password', 'required|min_length[8]');
$this->form_validation->set_rules('passconf', 'Password Confirmation', 'required|matches[password]');

if ($this->form_validation->run() === FALSE) {
    $this->load->view('register_form');
} else {
    // Process valid data
}
```

---

## 📊 Common Form Patterns

### Registration Form
- Username validation
- Email uniqueness check
- Password strength
- Terms acceptance

### Contact Form
- Email validation
- Message length limits
- Captcha integration
- Success messaging

### Search Form
- Input sanitization
- Query validation
- Advanced filters
- Results pagination

---

## ✅ Success Criteria

- [ ] Create forms using Form Helper
- [ ] Implement comprehensive validation
- [ ] Display user-friendly errors
- [ ] Repopulate forms after errors
- [ ] Create custom validation rules
- [ ] Handle file uploads
- [ ] Implement CSRF protection
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"User Registration System"**
- Multi-step registration
- Real-time validation
- Email verification
- Profile image upload
- Terms & conditions

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-3-database/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 3: Database & Model
      </button>
    </a>
  </div>
  <div>
    <a href="form-helper.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Form Helper →
      </button>
    </a>
  </div>
</div>
