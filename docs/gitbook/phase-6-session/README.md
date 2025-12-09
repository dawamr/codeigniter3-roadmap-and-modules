# 🔑 Phase 6 - Session & Authentication

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Memahami session management di CI3
- ✅ Membuat sistem authentication lengkap
- ✅ Menggunakan flashdata untuk messaging
- ✅ Implementasi remember me functionality
- ✅ Membuat middleware pattern untuk auth
- ✅ Mengelola user roles dan permissions

---

## 📋 Overview

Session dan Authentication adalah **backbone** dari aplikasi web modern. Phase ini akan mengajarkan cara membuat sistem auth yang secure dan scalable.

> 💡 **Analogi Sederhana:**  
> Session adalah **kartu identitas** 🆔 yang dibawa user. Authentication adalah **satpam** 👮 yang memeriksa kartu tersebut!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8EAF6; border-left: 4px solid #5C6BC0; padding: 15px;">
    <h4>🍪 Session Basics</h4>
    <small>Store user state</small>
  </div>
  
  <div style="background: #E0F2F1; border-left: 4px solid #26A69A; padding: 15px;">
    <h4>⚡ Flashdata</h4>
    <small>Temporary messages</small>
  </div>
  
  <div style="background: #FFF9C4; border-left: 4px solid #FDD835; padding: 15px;">
    <h4>🔐 Login System</h4>
    <small>Authentication flow</small>
  </div>
  
  <div style="background: #FFEBEE; border-left: 4px solid #EF5350; padding: 15px;">
    <h4>🚪 Logout & Security</h4>
    <small>Session destruction</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #AB47BC; padding: 15px;">
    <h4>🛡️ Auth Middleware</h4>
    <small>Route protection</small>
  </div>
  
  <div style="background: #E1F5FE; border-left: 4px solid #29B6F6; padding: 15px;">
    <h4>👥 User Roles</h4>
    <small>Permission system</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[🍪 Session Management](session-basics.md)**
   - Session configuration
   - Session drivers
   - Session data operations

2. **[⚡ Flashdata & Tempdata](flashdata.md)**
   - One-time messages
   - Temporary storage
   - Success/error notifications

3. **[🔐 Authentication System](authentication.md)**
   - Login implementation
   - Password verification
   - Session creation

4. **[🚪 Logout & Security](logout-security.md)**
   - Session destruction
   - Security best practices
   - Session hijacking prevention

5. **[🍪 Remember Me](remember-me.md)**
   - Cookie implementation
   - Auto-login
   - Security considerations

### Advanced Topics
6. **[🛡️ Auth Middleware](middleware.md)**
   - MY_Controller pattern
   - Route protection
   - Redirect handling

7. **[👥 Roles & Permissions](roles-permissions.md)**
   - User roles
   - Permission checking
   - Access control

8. **[🔄 Session Database](session-database.md)**
   - Database driver
   - Session table
   - Performance optimization

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Complete auth system
   - Real-world scenarios

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### Session Operations
```php
// Set session data
$this->session->set_userdata([
    'user_id' => $user->id,
    'username' => $user->username,
    'logged_in' => TRUE
]);

// Get session data
$user_id = $this->session->userdata('user_id');

// Flash message
$this->session->set_flashdata('success', 'Login successful!');
```

### Authentication Flow
```php
class Auth extends CI_Controller {
    public function login() {
        if ($this->form_validation->run()) {
            $user = $this->user_model->verify($email, $password);
            if ($user) {
                $this->session->set_userdata('user', $user);
                redirect('dashboard');
            }
        }
    }
    
    public function logout() {
        $this->session->sess_destroy();
        redirect('login');
    }
}
```

---

## ✅ Success Criteria

- [ ] Configure session properly
- [ ] Implement login/logout
- [ ] Use flashdata for messages
- [ ] Create auth middleware
- [ ] Handle remember me
- [ ] Implement user roles
- [ ] Secure session handling
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Multi-User Blog Platform"**
- User registration/login
- Role-based access (Admin, Author, Reader)
- Protected admin panel
- Remember me functionality
- Activity logging

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-5-security/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 5: Input & Security
      </button>
    </a>
  </div>
  <div>
    <a href="session-basics.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Session Management →
      </button>
    </a>
  </div>
</div>
