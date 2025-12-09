# 📤 Phase 8 - File Upload

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Menguasai Upload Library CI3
- ✅ Validasi file (type, size, dimensions)
- ✅ Handle multiple file uploads
- ✅ Image manipulation (resize, crop, watermark)
- ✅ Secure file upload practices
- ✅ Organize uploaded files properly

---

## 📋 Overview

File upload adalah fitur essential dalam aplikasi modern. Phase ini fokus pada implementasi upload yang **secure**, **efficient**, dan **user-friendly**.

> 💡 **Analogi Sederhana:**  
> File upload adalah **pos pengiriman** 📮. Validation adalah **petugas inspeksi** 🔍 yang memastikan paket aman!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8F5E9; border-left: 4px solid #4CAF50; padding: 15px;">
    <h4>📁 Upload Basics</h4>
    <small>Configuration & setup</small>
  </div>
  
  <div style="background: #E3F2FD; border-left: 4px solid #2196F3; padding: 15px;">
    <h4>✅ File Validation</h4>
    <small>Type, size, dimensions</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #FF9800; padding: 15px;">
    <h4>📸 Image Processing</h4>
    <small>Resize, crop, watermark</small>
  </div>
  
  <div style="background: #FCE4EC; border-left: 4px solid #E91E63; padding: 15px;">
    <h4>📚 Multiple Uploads</h4>
    <small>Batch file handling</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #9C27B0; padding: 15px;">
    <h4>🔒 Security</h4>
    <small>Safe upload practices</small>
  </div>
  
  <div style="background: #E0F7FA; border-left: 4px solid #00BCD4; padding: 15px;">
    <h4>📂 File Management</h4>
    <small>Organize & retrieve</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[📁 Upload Configuration](upload-config.md)**
   - Upload library setup
   - Configuration options
   - Directory permissions

2. **[📤 Basic Upload](basic-upload.md)**
   - Single file upload
   - Form setup
   - Error handling

3. **[✅ File Validation](file-validation.md)**
   - Allowed types
   - Size limits
   - Custom validation

4. **[📸 Image Manipulation](image-manipulation.md)**
   - Image library
   - Resize & thumbnails
   - Watermarking

5. **[📚 Multiple Files](multiple-uploads.md)**
   - Array handling
   - Batch processing
   - Progress feedback

### Advanced Topics
6. **[🔒 Upload Security](upload-security.md)**
   - File type verification
   - Malware prevention
   - Directory traversal

7. **[📂 File Organization](file-organization.md)**
   - Directory structure
   - Naming conventions
   - Database storage

8. **[⚡ Ajax Upload](ajax-upload.md)**
   - Asynchronous upload
   - Progress bar
   - Drag & drop

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Complete upload system
   - Real scenarios

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### Upload Configuration
```php
$config['upload_path'] = './uploads/';
$config['allowed_types'] = 'jpg|jpeg|png|pdf';
$config['max_size'] = 2048; // 2MB
$config['max_width'] = 1920;
$config['max_height'] = 1080;
$config['encrypt_name'] = TRUE; // Random filename

$this->load->library('upload', $config);
```

### Upload Process
```php
if (!$this->upload->do_upload('userfile')) {
    $error = $this->upload->display_errors();
    $this->session->set_flashdata('error', $error);
} else {
    $data = $this->upload->data();
    // Save to database
    $this->file_model->save([
        'filename' => $data['file_name'],
        'path' => $data['full_path'],
        'size' => $data['file_size'],
        'type' => $data['file_type']
    ]);
}
```

### Image Manipulation
```php
$config['image_library'] = 'gd2';
$config['source_image'] = './uploads/photo.jpg';
$config['create_thumb'] = TRUE;
$config['maintain_ratio'] = TRUE;
$config['width'] = 150;
$config['height'] = 150;

$this->load->library('image_lib', $config);
$this->image_lib->resize();
```

---

## 📊 Common Upload Scenarios

### Profile Picture
- Single image
- Size validation
- Auto-resize
- Replace old file

### Document Upload
- Multiple file types
- PDF, DOC, XLS
- Size limits
- Virus scanning

### Gallery System
- Multiple images
- Thumbnail generation
- Watermarking
- Batch processing

---

## 🔒 Security Checklist

- [ ] Validate MIME type
- [ ] Check file extension
- [ ] Limit file size
- [ ] Use random filenames
- [ ] Store outside web root
- [ ] Scan for malware
- [ ] Validate image headers
- [ ] Set proper permissions

---

## ✅ Success Criteria

- [ ] Configure upload library
- [ ] Handle single uploads
- [ ] Process multiple files
- [ ] Validate files properly
- [ ] Manipulate images
- [ ] Implement security measures
- [ ] Organize files effectively
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Media Manager"**
- File upload interface
- Image gallery
- Document management
- Thumbnail generation
- File categorization
- Download tracking

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-7-helper/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 7: Helper & Library
      </button>
    </a>
  </div>
  <div>
    <a href="upload-config.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Upload Configuration →
      </button>
    </a>
  </div>
</div>
