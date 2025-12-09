# 📊 Phase 10 - Pagination, Search & Filter

## 🎯 Learning Objectives

Setelah menyelesaikan phase ini, Anda akan:
- ✅ Implementasi pagination dengan Pagination Library
- ✅ Membuat search functionality yang efisien
- ✅ Implementasi multi-criteria filtering
- ✅ Kombinasi pagination, search, dan filter
- ✅ Optimasi query untuk performa
- ✅ Create user-friendly data navigation

---

## 📋 Overview

Handling large datasets requires smart data presentation. Phase ini fokus pada teknik-teknik untuk menampilkan data dalam chunks yang manageable dan searchable.

> 💡 **Analogi Sederhana:**  
> Pagination adalah **buku dengan halaman** 📖. Search adalah **index buku** 🔍. Filter adalah **kategori rak buku** 📚!

---

## 🗺️ What We'll Learn

<div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin: 20px 0;">
  
  <div style="background: #E8F5E9; border-left: 4px solid #4CAF50; padding: 15px;">
    <h4>📄 Pagination</h4>
    <small>Data chunking</small>
  </div>
  
  <div style="background: #E3F2FD; border-left: 4px solid #2196F3; padding: 15px;">
    <h4>🔍 Search</h4>
    <small>Keyword searching</small>
  </div>
  
  <div style="background: #FFF3E0; border-left: 4px solid #FF9800; padding: 15px;">
    <h4>🎯 Filtering</h4>
    <small>Data refinement</small>
  </div>
  
  <div style="background: #FCE4EC; border-left: 4px solid #E91E63; padding: 15px;">
    <h4>🔗 Combination</h4>
    <small>Search + Filter + Page</small>
  </div>
  
  <div style="background: #F3E5F5; border-left: 4px solid #9C27B0; padding: 15px;">
    <h4>⚡ Performance</h4>
    <small>Query optimization</small>
  </div>
  
  <div style="background: #E0F7FA; border-left: 4px solid #00BCD4; padding: 15px;">
    <h4>🎨 UI/UX</h4>
    <small>User experience</small>
  </div>
  
</div>

---

## 📚 Phase Contents

### Core Topics
1. **[📄 Pagination Basics](pagination-basics.md)**
   - Pagination library setup
   - Configuration options
   - Basic implementation

2. **[🔍 Search Implementation](search.md)**
   - LIKE queries
   - Full-text search
   - Search optimization

3. **[🎯 Filter System](filtering.md)**
   - Single filters
   - Multiple criteria
   - Dynamic filters

4. **[🔗 Combined Features](combination.md)**
   - Search with pagination
   - Filter with pagination
   - All three together

5. **[🎨 UI Components](ui-components.md)**
   - Pagination links
   - Search forms
   - Filter interfaces

### Advanced Topics
6. **[⚡ Performance Optimization](optimization.md)**
   - Query optimization
   - Indexing strategies
   - Caching results

7. **[📱 Ajax Implementation](ajax-pagination.md)**
   - Async pagination
   - Live search
   - Dynamic filtering

8. **[📊 Advanced Patterns](advanced-patterns.md)**
   - Infinite scroll
   - Load more button
   - Virtual scrolling

### Practice & Assessment
9. **[💻 Practice Lab](practice.md)**
   - Complete data table
   - Real scenarios

10. **[❓ Quiz](quiz.md)**
    - Test your knowledge
    - 10 questions

---

## 🎯 Key Concepts Preview

### Pagination Setup
```php
$this->load->library('pagination');

$config['base_url'] = site_url('products/index');
$config['total_rows'] = $this->product_model->count_all();
$config['per_page'] = 10;
$config['uri_segment'] = 3;

// Bootstrap 4 styling
$config['full_tag_open'] = '<nav><ul class="pagination">';
$config['full_tag_close'] = '</ul></nav>';
$config['first_link'] = 'First';
$config['last_link'] = 'Last';
$config['next_link'] = 'Next';
$config['prev_link'] = 'Previous';

$this->pagination->initialize($config);
```

### Search with Pagination
```php
public function search() {
    $keyword = $this->input->get('q');
    $page = $this->uri->segment(3, 0);
    
    // Count filtered results
    $total_rows = $this->product_model->count_search($keyword);
    
    // Pagination config
    $config['base_url'] = site_url('products/search');
    $config['total_rows'] = $total_rows;
    $config['per_page'] = 10;
    $config['reuse_query_string'] = TRUE; // Keep search param
    
    $this->pagination->initialize($config);
    
    // Get paginated results
    $data['products'] = $this->product_model->search($keyword, $config['per_page'], $page);
    $data['pagination'] = $this->pagination->create_links();
    
    $this->load->view('products/list', $data);
}
```

### Advanced Filtering
```php
public function filter() {
    $filters = [
        'category' => $this->input->get('category'),
        'min_price' => $this->input->get('min_price'),
        'max_price' => $this->input->get('max_price'),
        'status' => $this->input->get('status')
    ];
    
    // Remove empty filters
    $filters = array_filter($filters);
    
    // Apply filters in model
    $this->db->where('1=1'); // Start condition
    
    if (isset($filters['category'])) {
        $this->db->where('category_id', $filters['category']);
    }
    
    if (isset($filters['min_price'])) {
        $this->db->where('price >=', $filters['min_price']);
    }
    
    if (isset($filters['max_price'])) {
        $this->db->where('price <=', $filters['max_price']);
    }
}
```

---

## 📊 Common Patterns

### E-commerce Product List
- Category filter
- Price range
- Brand filter
- Sort options
- Grid/List view

### Admin Data Table
- Column sorting
- Quick search
- Status filter
- Bulk actions
- Export options

### Blog Archive
- Category filter
- Tag cloud
- Date archive
- Author filter
- Popular posts

---

## ⚡ Performance Tips

1. **Use Indexes**
   - Index search columns
   - Composite indexes for filters

2. **Limit Results**
   - Don't fetch all columns
   - Use SELECT specific fields

3. **Cache Counts**
   - Cache total row counts
   - Update on data changes

4. **Optimize Queries**
   - Avoid N+1 problems
   - Use JOIN wisely

---

## ✅ Success Criteria

- [ ] Implement pagination
- [ ] Create search functionality
- [ ] Build filter system
- [ ] Combine all features
- [ ] Style pagination links
- [ ] Optimize performance
- [ ] Handle edge cases
- [ ] Score ≥ 80% on quiz

---

## 🚀 Mini Project Preview

**"Product Catalog System"**
- Product listing with pagination
- Multi-criteria search
- Category & price filters
- Sort options
- Grid/List view toggle
- Ajax-powered updates

---

<div style="display: flex; justify-content: space-between; margin-top: 40px;">
  <div>
    <a href="../phase-9-response/quiz.md">
      <button style="background: #6c757d; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        ← Phase 9: Response Handling
      </button>
    </a>
  </div>
  <div>
    <a href="pagination-basics.md">
      <button style="background: #4CAF50; color: white; padding: 10px 20px; border: none; border-radius: 5px;">
        Start: Pagination Basics →
      </button>
    </a>
  </div>
</div>
