# 📦 Phase 7 - Helper & Library

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Memahami perbedaan Helper dan Library
- ✅ Membuat custom Helper untuk fungsi reusable
- ✅ Membuat custom Library untuk class reusable
- ✅ Menggunakan built-in Helpers dan Libraries
- ✅ Extending core classes (MY_Controller, MY_Model)
- ✅ Implementasi Hooks untuk lifecycle management

---

## 📋 Overview

Helper dan Library adalah tools untuk membuat kode yang **DRY (Don't Repeat Yourself)**. Phase ini fokus pada creating reusable components untuk meningkatkan produktivitas.

> 💡 **Analogi Sederhana:**  
> Helper adalah **toolbox** 🧰 berisi fungsi-fungsi praktis. Library adalah **mesin canggih** ⚙️ dengan fitur kompleks!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8F5E9; border-left: 4px solid #66BB6A; padding: 15px;">
    <h4>🔧 Helper Basics</h4>
    <small>Function collections</small>
  </div>
  
  <div style="background: #E3F2FD; border-left: 4px solid #42A5F5; padding: 15px;">
    <h4>📚 Library Concepts</h4>
    <small>Class-based tools</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #FFA726; padding: 15px;">
    <h4>🏗️ Custom Helpers</h4>
    <small>Create your own</small>
  </div>
  
  <div style="background: #FCE4EC; border-left: 4px solid #EC407A; padding: 15px;">
    <h4>🏭 Custom Libraries</h4>
    <small>Build complex tools</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #BA68C8; padding: 15px;">
    <h4>🔄 Core Extension</h4>
    <small>MY_Controller pattern</small>
  </div>
  
  <div style="background: #E0F7FA; border-left: 4px solid #26C6DA; padding: 15px;">
    <h4>🎣 Hooks</h4>
    <small>Lifecycle intercepts</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[🔧 Helper Fundamentals](helper-basics.md)**
   - What are helpers
   - Loading helpers
   - Built-in helpers overview

2. **[📚 Library Fundamentals](library-basics.md)**
   - Library concepts
   - Loading libraries
   - Built-in libraries overview

3. **[🏗️ Creating Custom Helpers](custom-helpers.md)**
   - Helper structure
   - Naming conventions
   - Practical examples

4. **[🏭 Creating Custom Libraries](custom-libraries.md)**
   - Library structure
   - Constructor & CI instance
   - Complex implementations

5. **[📦 Built-in Tools](builtin-tools.md)**
   - URL Helper
   - Form Helper
   - Email Library
   - Upload Library

### Advanced Topics
6. **[🔄 Extending Core](core-extension.md)**
   - MY_Controller
   - MY_Model
   - MY_Loader

7. **[🎣 Hooks System](hooks.md)**
   - Hook points
   - Creating hooks
   - Use cases

8. **[🔌 Third-party Integration](third-party.md)**
   - Installing packages
   - Composer integration
   - Popular libraries

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Build utility toolkit
   - Real implementations

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### Custom Helper Example
```php
// application/helpers/format_helper.php
if (!function_exists('format_rupiah')) {
    function format_rupiah($amount) {
        return 'Rp ' . number_format($amount, 0, ',', '.');
    }
}

if (!function_exists('format_date_indo')) {
    function format_date_indo($date) {
        $months = ['Jan', 'Feb', 'Mar', 'Apr', 'Mei', 'Jun', 
                   'Jul', 'Agu', 'Sep', 'Okt', 'Nov', 'Des'];
        return date('d', strtotime($date)) . ' ' . 
               $months[date('n', strtotime($date)) - 1] . ' ' . 
               date('Y', strtotime($date));
    }
}
```

### Custom Library Example
```php
// application/libraries/Template.php
class Template {
    protected $CI;
    protected $template_data = [];
    
    public function __construct() {
        $this->CI =& get_instance();
    }
    
    public function set($key, $value) {
        $this->template_data[$key] = $value;
    }
    
    public function render($view, $data = []) {
        $this->CI->load->view('layouts/header', $this->template_data);
        $this->CI->load->view($view, $data);
        $this->CI->load->view('layouts/footer');
    }
}
```

### MY_Controller Pattern
```php
// application/core/MY_Controller.php
class MY_Controller extends CI_Controller {
    protected $data = [];
    
    public function __construct() {
        parent::__construct();
        $this->load->library('template');
        $this->data['app_name'] = 'My Application';
    }
}

class Auth_Controller extends MY_Controller {
    public function __construct() {
        parent::__construct();
        if (!$this->session->userdata('logged_in')) {
            redirect('login');
        }
    }
}
```

---

## 📊 Helper vs Library

| Aspect | Helper | Library |
|--------|--------|---------|
| **Structure** | Functions | Class |
| **State** | Stateless | Can have state |
| **Complexity** | Simple | Complex |
| **Use Case** | Utility functions | Business logic |
| **Example** | format_date() | Email sender |

---

## ✅ Success Criteria

- [ ] Create custom helpers
- [ ] Build custom libraries
- [ ] Use built-in tools effectively
- [ ] Implement MY_Controller
- [ ] Create MY_Model
- [ ] Understand hooks
- [ ] Organize reusable code
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Utility Toolkit"**
- Format helper (date, currency, text)
- API Response library
- Template engine
- Base controllers
- Activity logger

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-6-session/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 6: Session & Auth
      </button>
    </a>
  </div>
  <div>
    <a href="helper-basics.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Helper Fundamentals →
      </button>
    </a>
  </div>
</div>
