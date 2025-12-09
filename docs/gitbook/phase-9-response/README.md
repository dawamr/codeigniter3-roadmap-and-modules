# 🔄 Phase 9 - Response Handling (Web & API)

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Membuat dual response (HTML & JSON)
- ✅ Membangun RESTful API dengan CI3
- ✅ Menerapkan HTTP status codes yang tepat
- ✅ Membuat response format yang konsisten
- ✅ Handle CORS untuk API
- ✅ Implementasi API authentication

---

## 📋 Overview

Modern aplikasi perlu serve multiple clients - web browsers, mobile apps, third-party services. Phase ini mengajarkan cara membuat aplikasi yang bisa respond dalam berbagai format.

> 💡 **Analogi Sederhana:**  
> Response handling adalah **multilingual speaker** 🗣️ - bisa berbicara HTML untuk browser, JSON untuk mobile app!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8F5E9; border-left: 4px solid #4CAF50; padding: 15px;">
    <h4>🌐 Web Response</h4>
    <small>HTML output</small>
  </div>
  
  <div style="background: #E3F2FD; border-left: 4px solid #2196F3; padding: 15px;">
    <h4>📡 API Response</h4>
    <small>JSON output</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #FF9800; padding: 15px;">
    <h4>🔢 Status Codes</h4>
    <small>HTTP responses</small>
  </div>
  
  <div style="background: #FCE4EC; border-left: 4px solid #E91E63; padding: 15px;">
    <h4>🏗️ RESTful API</h4>
    <small>REST principles</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #9C27B0; padding: 15px;">
    <h4>🔐 API Auth</h4>
    <small>Token & keys</small>
  </div>
  
  <div style="background: #E0F7FA; border-left: 4px solid #00BCD4; padding: 15px;">
    <h4>🌍 CORS</h4>
    <small>Cross-origin</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[🔄 Response Basics](response-basics.md)**
   - Output class
   - Content types
   - Headers management

2. **[📡 JSON Response](json-response.md)**
   - Creating JSON output
   - Response structure
   - Error formatting

3. **[🔢 HTTP Status Codes](status-codes.md)**
   - Common codes
   - When to use
   - Custom messages

4. **[🏗️ RESTful Design](restful-design.md)**
   - REST principles
   - Resource routing
   - CRUD operations

5. **[🎯 Dual Controller](dual-controller.md)**
   - Web vs API methods
   - Content negotiation
   - Shared logic

### Advanced Topics
6. **[🔐 API Authentication](api-auth.md)**
   - API keys
   - Token-based auth
   - JWT implementation

7. **[🌍 CORS Handling](cors.md)**
   - Cross-origin requests
   - Preflight handling
   - Security considerations

8. **[📊 API Versioning](versioning.md)**
   - Version strategies
   - Backward compatibility
   - Migration paths

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Build complete API
   - Real implementations

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### JSON Response Format
```php
class Api_Controller extends CI_Controller {
    
    protected function json_response($data, $status = 200) {
        $this->output
            ->set_content_type('application/json')
            ->set_status_header($status)
            ->set_output(json_encode([
                'status' => $status < 400,
                'data' => $data,
                'timestamp' => time()
            ]));
    }
    
    public function users() {
        $users = $this->user_model->get_all();
        $this->json_response($users);
    }
}
```

### RESTful Routes
```php
// config/routes.php
$route['api/users']['GET'] = 'api/users/index';
$route['api/users/(:num)']['GET'] = 'api/users/show/$1';
$route['api/users']['POST'] = 'api/users/create';
$route['api/users/(:num)']['PUT'] = 'api/users/update/$1';
$route['api/users/(:num)']['DELETE'] = 'api/users/delete/$1';
```

### Dual Response Pattern
```php
class Products extends CI_Controller {
    
    public function index() {
        $products = $this->product_model->get_all();
        
        if ($this->input->is_ajax_request() || 
            $this->input->get('format') === 'json') {
            // Return JSON
            $this->output
                ->set_content_type('application/json')
                ->set_output(json_encode($products));
        } else {
            // Return HTML view
            $this->load->view('products/index', ['products' => $products]);
        }
    }
}
```

---

## 📊 Response Types Comparison

| Type | Use Case | Format | Client |
|------|----------|--------|--------|
| **HTML** | Web pages | HTML/CSS/JS | Browser |
| **JSON** | API data | JSON | Mobile/SPA |
| **XML** | Legacy API | XML | Enterprise |
| **CSV** | Data export | CSV | Excel |
| **PDF** | Documents | PDF | Download |

---

## 🔢 Common HTTP Status Codes

### Success (2xx)
- **200** OK
- **201** Created
- **204** No Content

### Client Error (4xx)
- **400** Bad Request
- **401** Unauthorized
- **403** Forbidden
- **404** Not Found
- **422** Unprocessable Entity

### Server Error (5xx)
- **500** Internal Server Error
- **503** Service Unavailable

---

## ✅ Success Criteria

- [ ] Create JSON responses
- [ ] Implement RESTful API
- [ ] Use proper status codes
- [ ] Handle CORS requests
- [ ] Implement API auth
- [ ] Version your API
- [ ] Document endpoints
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Product API Service"**
- RESTful product endpoints
- Authentication with API keys
- Rate limiting
- API documentation
- Web & mobile clients
- Webhook notifications

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-8-upload/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 8: File Upload
      </button>
    </a>
  </div>
  <div>
    <a href="response-basics.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Response Basics →
      </button>
    </a>
  </div>
</div>
