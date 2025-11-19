<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>PresenceParse - Rekap Absensi Universal</title>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.18.5/xlsx.full.min.js"></script>
    <style>
        :root {
            --primary: #4361ee;
            --primary-light: #4895ef;
            --secondary: #3f37c9;
            --success: #4cc9f0;
            --warning: #f72585;
            --light: #f8f9fa;
            --dark: #212529;
            --gray: #6c757d;
            --border-radius: 8px;
            --box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            -webkit-tap-highlight-color: transparent;
            -webkit-text-size-adjust: 100%;
        }
        
        html {
            scroll-behavior: smooth;
        }
        
        body {
            background-color: #f5f7fb;
            color: var(--dark);
            line-height: 1.6;
            overflow-x: hidden;
            min-height: 100vh;
            position: relative;
        }
        
        .container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 0 15px;
            width: 100%;
        }
        
        header {
            background: linear-gradient(135deg, var(--primary), var(--secondary));
            color: white;
            padding: 1rem 0;
            box-shadow: var(--box-shadow);
            position: sticky;
            top: 0;
            z-index: 1000;
            width: 100%;
        }
        
        .header-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        
        .logo {
            display: flex;
            align-items: center;
            gap: 10px;
            flex-shrink: 0;
        }
        
        .logo-icon {
            font-size: 1.8rem;
        }
        
        .logo h1 {
            font-size: 1.5rem;
            font-weight: 700;
            white-space: nowrap;
        }
        
        nav {
            width: 100%;
            margin-top: 1rem;
            display: none;
        }
        
        nav.active {
            display: block;
        }
        
        nav ul {
            display: flex;
            list-style: none;
            gap: 10px;
            flex-direction: column;
            background: rgba(255, 255, 255, 0.1);
            border-radius: var(--border-radius);
            padding: 10px;
        }
        
        nav a {
            color: white;
            text-decoration: none;
            font-weight: 500;
            padding: 8px 12px;
            border-radius: var(--border-radius);
            transition: background-color 0.3s;
            display: block;
        }
        
        nav a:hover {
            background-color: rgba(255, 255, 255, 0.2);
        }
        
        .mobile-menu-btn {
            display: block;
            background: none;
            border: none;
            color: white;
            font-size: 1.5rem;
            cursor: pointer;
            padding: 5px;
        }
        
        .hero {
            padding: 2rem 0;
            text-align: center;
            max-width: 800px;
            margin: 0 auto;
        }
        
        .hero h2 {
            font-size: 1.8rem;
            margin-bottom: 1rem;
            color: var(--primary);
            line-height: 1.3;
        }
        
        .hero p {
            font-size: 1rem;
            color: var(--gray);
            margin-bottom: 1.5rem;
        }
        
        .features {
            display: grid;
            grid-template-columns: 1fr;
            gap: 1.5rem;
            margin: 2rem 0;
        }
        
        .feature-card {
            background: white;
            border-radius: var(--border-radius);
            padding: 1.5rem;
            box-shadow: var(--box-shadow);
            transition: transform 0.3s;
        }
        
        .feature-card:hover {
            transform: translateY(-3px);
        }
        
        .feature-icon {
            font-size: 2rem;
            color: var(--primary);
            margin-bottom: 1rem;
        }
        
        .feature-card h3 {
            margin-bottom: 0.8rem;
            color: var(--primary);
            font-size: 1.2rem;
        }
        
        .main-content {
            background: white;
            border-radius: var(--border-radius);
            padding: 1.5rem;
            box-shadow: var(--box-shadow);
            margin: 1.5rem 0;
        }
        
        .feature-selection {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 1rem;
            margin-bottom: 2rem;
        }
        
        .feature-option {
            border: 2px solid #ddd;
            border-radius: var(--border-radius);
            padding: 1.5rem;
            text-align: center;
            cursor: pointer;
            transition: all 0.3s;
        }
        
        .feature-option:hover {
            border-color: var(--primary);
            transform: translateY(-2px);
        }
        
        .feature-option.active {
            border-color: var(--primary);
            background: #f0f4ff;
        }
        
        .feature-option-icon {
            font-size: 2.5rem;
            margin-bottom: 1rem;
        }
        
        .feature-option h3 {
            margin-bottom: 0.5rem;
            color: var(--primary);
        }
        
        .tab-container {
            margin-bottom: 1.5rem;
        }
        
        .tabs {
            display: flex;
            border-bottom: 1px solid #ddd;
            margin-bottom: 1rem;
            flex-wrap: wrap;
        }
        
        .tab {
            padding: 10px 15px;
            cursor: pointer;
            border-bottom: 3px solid transparent;
            transition: all 0.3s;
            font-size: 0.9rem;
            flex: 1;
            min-width: 120px;
            text-align: center;
        }
        
        .tab.active {
            border-bottom: 3px solid var(--primary);
            color: var(--primary);
            font-weight: 600;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .input-area {
            width: 100%;
            min-height: 150px;
            padding: 1rem;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-family: monospace;
            margin-bottom: 1rem;
            resize: vertical;
            font-size: 16px;
        }
        
        .file-upload {
            border: 2px dashed #ddd;
            border-radius: var(--border-radius);
            padding: 1.5rem;
            text-align: center;
            margin-bottom: 1rem;
            cursor: pointer;
            transition: border-color 0.3s;
        }
        
        .file-upload:hover {
            border-color: var(--primary);
        }
        
        .file-upload i {
            font-size: 2.5rem;
            color: var(--gray);
            margin-bottom: 1rem;
        }
        
        .btn {
            background-color: var(--primary);
            color: white;
            border: none;
            padding: 12px 20px;
            border-radius: var(--border-radius);
            cursor: pointer;
            font-weight: 600;
            transition: background-color 0.3s;
            display: inline-flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            font-size: 1rem;
            width: 100%;
            max-width: 300px;
            margin: 0 auto;
        }
        
        .btn:hover {
            background-color: var(--secondary);
        }
        
        .btn:disabled {
            background-color: var(--gray);
            cursor: not-allowed;
        }
        
        .btn-outline {
            background-color: transparent;
            border: 2px solid var(--primary);
            color: var(--primary);
        }
        
        .btn-outline:hover {
            background-color: var(--primary);
            color: white;
        }
        
        .btn-success {
            background-color: #4CAF50;
        }
        
        .btn-success:hover {
            background-color: #45a049;
        }
        
        .btn-warning {
            background-color: #FF9800;
        }
        
        .btn-warning:hover {
            background-color: #F57C00;
        }
        
        .results {
            margin-top: 1.5rem;
            display: none;
        }
        
        .results.active {
            display: block;
        }
        
        .summary-cards {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 1rem;
            margin-bottom: 1.5rem;
        }
        
        .summary-card {
            background: white;
            border-radius: var(--border-radius);
            padding: 1rem;
            box-shadow: var(--box-shadow);
            text-align: center;
        }
        
        .summary-card h3 {
            font-size: 1.5rem;
            margin-bottom: 0.5rem;
        }
        
        .summary-card.present {
            border-top: 4px solid #4CAF50;
        }
        
        .summary-card.absent {
            border-top: 4px solid #F44336;
        }
        
        .summary-card.late {
            border-top: 4px solid #FF9800;
        }
        
        .summary-card.permission {
            border-top: 4px solid #2196F3;
        }
        
        .summary-card.sick {
            border-top: 4px solid #9C27B0;
        }
        
        .summary-card.early {
            border-top: 4px solid #4CAF50;
        }
        
        .summary-card.ontime {
            border-top: 4px solid #2196F3;
        }
        
        .chart-container {
            margin: 1.5rem 0;
            height: 250px;
            position: relative;
        }
        
        table {
            width: 100%;
            border-collapse: collapse;
            margin: 1rem 0;
            font-size: 0.9rem;
        }
        
        th, td {
            padding: 10px 8px;
            text-align: left;
            border-bottom: 1px solid #ddd;
        }
        
        th {
            background-color: #f8f9fa;
            font-weight: 600;
        }
        
        tr:hover {
            background-color: #f5f5f5;
        }
        
        .status-present {
            color: #4CAF50;
            font-weight: 600;
        }
        
        .status-absent {
            color: #F44336;
            font-weight: 600;
        }
        
        .status-late {
            color: #FF9800;
            font-weight: 600;
        }
        
        .status-permission {
            color: #2196F3;
            font-weight: 600;
        }
        
        .status-sick {
            color: #9C27B0;
            font-weight: 600;
        }
        
        .status-early {
            color: #4CAF50;
            font-weight: 600;
        }
        
        .status-ontime {
            color: #2196F3;
            font-weight: 600;
        }
        
        .export-options {
            display: flex;
            gap: 1rem;
            margin-top: 1rem;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .export-options .btn {
            width: auto;
            min-width: 150px;
        }
        
        footer {
            background-color: var(--dark);
            color: white;
            padding: 1.5rem 0;
            margin-top: 2rem;
        }
        
        .footer-content {
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
        }
        
        .loading {
            display: none;
            text-align: center;
            padding: 1.5rem;
        }
        
        .loading.spinner {
            border: 4px solid #f3f3f3;
            border-top: 4px solid var(--primary);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            animation: spin 2s linear infinite;
            margin: 0 auto 1rem;
        }
        
        @keyframes spin {
            0% { transform: rotate(0deg); }
            100% { transform: rotate(360deg); }
        }
        
        /* NEW STYLES FOR TIME-BASED FEATURE */
        .time-configuration {
            background: #f8f9fa;
            border-radius: var(--border-radius);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            border-left: 4px solid var(--primary);
            display: none;
        }
        
        .time-configuration.active {
            display: block;
        }
        
        .time-input-group {
            display: flex;
            gap: 1rem;
            align-items: center;
            flex-wrap: wrap;
            margin-bottom: 1rem;
        }
        
        .time-input {
            padding: 10px 12px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-size: 1rem;
            width: 120px;
        }
        
        .time-example {
            background: #e7f3ff;
            border-radius: var(--border-radius);
            padding: 1rem;
            margin-top: 1rem;
            font-size: 0.9rem;
        }
        
        .time-example code {
            background: #ffffff;
            padding: 2px 6px;
            border-radius: 4px;
            font-family: monospace;
        }
        
        .time-detail {
            font-size: 0.85rem;
            color: var(--gray);
            margin-top: 4px;
        }
        
        /* NEW STYLES FOR CUSTOM CATEGORIES */
        .custom-categories {
            background: #f8f9fa;
            border-radius: var(--border-radius);
            padding: 1.5rem;
            margin-bottom: 1.5rem;
            border-left: 4px solid #9C27B0;
        }
        
        .category-management {
            margin-bottom: 1rem;
        }
        
        .category-input-group {
            display: flex;
            gap: 1rem;
            margin-bottom: 1rem;
            flex-wrap: wrap;
            align-items: center;
        }
        
        .category-input {
            padding: 10px 12px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            font-size: 1rem;
            flex: 1;
            min-width: 200px;
        }
        
        .color-picker {
            width: 60px;
            height: 40px;
            border: 1px solid #ddd;
            border-radius: var(--border-radius);
            cursor: pointer;
        }
        
        .categories-list {
            display: flex;
            flex-wrap: wrap;
            gap: 0.5rem;
            margin-top: 1rem;
        }
        
        .category-tag {
            display: inline-flex;
            align-items: center;
            gap: 0.5rem;
            padding: 6px 12px;
            border-radius: 20px;
            font-size: 0.8rem;
            font-weight: 500;
            color: white;
        }
        
        .category-tag .remove-btn {
            background: rgba(255, 255, 255, 0.3);
            border: none;
            border-radius: 50%;
            width: 18px;
            height: 18px;
            color: white;
            cursor: pointer;
            font-size: 0.7rem;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .category-tag .remove-btn:hover {
            background: rgba(255, 255, 255, 0.5);
        }
        
        .custom-status {
            display: inline-block;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-left: 8px;
            color: white;
        }
        
        /* Media queries untuk tablet */
        @media (min-width: 768px) {
            .container {
                padding: 0 20px;
            }
            
            header {
                padding: 1.5rem 0;
            }
            
            .logo h1 {
                font-size: 1.8rem;
            }
            
            .mobile-menu-btn {
                display: none;
            }
            
            nav {
                display: block;
                width: auto;
                margin-top: 0;
            }
            
            nav ul {
                flex-direction: row;
                background: transparent;
                padding: 0;
            }
            
            .hero {
                padding: 3rem 0;
            }
            
            .hero h2 {
                font-size: 2.2rem;
            }
            
            .hero p {
                font-size: 1.1rem;
            }
            
            .features {
                grid-template-columns: repeat(2, 1fr);
                gap: 2rem;
                margin: 3rem 0;
            }
            
            .main-content {
                padding: 2rem;
            }
            
            .summary-cards {
                grid-template-columns: repeat(5, 1fr);
            }
            
            .btn {
                width: auto;
                margin: 0;
            }
        }
        
        /* Media queries untuk desktop */
        @media (min-width: 1024px) {
            .features {
                grid-template-columns: repeat(3, 1fr);
            }
            
            .hero h2 {
                font-size: 2.5rem;
            }
        }
        
        /* Perbaikan khusus untuk iOS */
        @supports (-webkit-touch-callout: none) {
            .input-area {
                font-size: 16px;
            }
            
            .btn {
                -webkit-appearance: none;
            }
        }
        
        /* Perbaikan untuk browser lama */
        @media all and (-ms-high-contrast: none), (-ms-high-contrast: active) {
            .features {
                display: flex;
                flex-wrap: wrap;
            }
            
            .feature-card {
                flex: 1 1 300px;
                margin: 10px;
            }
        }
    </style>
</head>
<body>
    <header>
        <div class="container">
            <div class="header-content">
                <div class="logo">
                    <div class="logo-icon">📊</div>
                    <h1>PresenceParse</h1>
                </div>
                <button class="mobile-menu-btn" id="mobile-menu-btn">☰</button>
                <nav id="main-nav">
                    <ul>
                        <li><a href="#home" class="nav-link">Beranda</a></li>
                        <li><a href="#features" class="nav-link">Fitur</a></li>
                        <li><a href="#process" class="nav-link">Proses Data</a></li>
                        <li><a href="#results" class="nav-link">Hasil</a></li>
                    </ul>
                </nav>
            </div>
        </div>
    </header>

    <section class="hero" id="home">
        <div class="container">
            <h2>PresenceParse dengan Kategori Kustom</h2>
            <p>Sekarang dengan fitur kategori kustom! Tambahkan kategori sendiri sesuai kebutuhan Anda.</p>
            <button class="btn" onclick="scrollToSection('process')">
                <span>Coba Sekarang</span>
            </button>
        </div>
    </section>

    <section id="features">
        <div class="container">
            <h2 style="text-align: center; margin-bottom: 2rem;">Fitur Unggulan</h2>
            <div class="features">
                <div class="feature-card">
                    <div class="feature-icon">📋</div>
                    <h3>Rekap Status Kehadiran</h3>
                    <p>Analisis data kehadiran berdasarkan status: Hadir, Izin, Sakit, Absen, dan Terlambat.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">⏰</div>
                    <h3>Analisis Ketepatan Waktu</h3>
                    <p>Deteksi otomatis status Early, Tepat Waktu, atau Terlambat berdasarkan waktu kedatangan.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">🎨</div>
                    <h3>Kategori Kustom</h3>
                    <p>Tambahkan kategori sendiri dengan warna kustom untuk kebutuhan spesifik Anda.</p>
                </div>
            </div>
        </div>
    </section>

    <section id="process">
        <div class="container">
            <div class="main-content">
                <h2 style="margin-bottom: 1.5rem;">Proses Data Kehadiran</h2>
                
                <!-- FEATURE SELECTION -->
                <div class="feature-selection">
                    <div class="feature-option active" data-feature="attendance">
                        <div class="feature-option-icon">📋</div>
                        <h3>Rekap Status Kehadiran</h3>
                        <p>Analisis berdasarkan status: Hadir, Izin, Sakit, Absen</p>
                    </div>
                    <div class="feature-option" data-feature="time">
                        <div class="feature-option-icon">⏰</div>
                        <h3>Analisis Ketepatan Waktu</h3>
                        <p>Analisis berdasarkan waktu: Early, Tepat Waktu, Terlambat</p>
                    </div>
                </div>
                
                <!-- CUSTOM CATEGORIES SECTION -->
                <div class="custom-categories">
                    <h3>🎨 Kategori Kustom</h3>
                    <div class="category-management">
                        <div class="category-input-group">
                            <input type="text" class="category-input" id="custom-category-name" placeholder="Nama kategori (contoh: Dinas Luar, Cuti, dll)">
                            <input type="color" class="color-picker" id="custom-category-color" value="#FF6B6B">
                            <button class="btn btn-warning" id="add-category-btn">
                                <span>➕ Tambah Kategori</span>
                            </button>
                        </div>
                        <div class="categories-list" id="categories-list">
                            <!-- Kategori default -->
                            <div class="category-tag" style="background-color: #4CAF50;">Hadir</div>
                            <div class="category-tag" style="background-color: #F44336;">Absen</div>
                            <div class="category-tag" style="background-color: #FF9800;">Terlambat</div>
                            <div class="category-tag" style="background-color: #2196F3;">Izin</div>
                            <div class="category-tag" style="background-color: #9C27B0;">Sakit</div>
                        </div>
                    </div>
                    <p><small>💡 Kategori kustom akan otomatis terdeteksi dalam data Anda. Contoh: <code>Nama - Dinas Luar</code></small></p>
                </div>
                
                <!-- TIME CONFIGURATION (HIDDEN BY DEFAULT) -->
                <div class="time-configuration" id="time-configuration">
                    <h3>⚙️ Konfigurasi Waktu</h3>
                    <div class="time-input-group">
                        <label><strong>Batas Waktu Tepat Waktu:</strong></label>
                        <input type="time" class="time-input" id="on-time-limit" value="07:00">
                        <span>(Contoh: 07:00)</span>
                    </div>
                    <div class="time-input-group">
                        <label><strong>Batas Waktu Terlambat:</strong></label>
                        <input type="time" class="time-input" id="late-time-limit" value="07:15">
                        <span>(Contoh: 07:15)</span>
                    </div>
                    <p><small>💡 <strong>Early:</strong> Sebelum batas tepat waktu | <strong>Tepat Waktu:</strong> Sesuai batas | <strong>Terlambat:</strong> Setelah batas terlambat</small></p>
                    
                    <div class="time-example">
                        <strong>Contoh Format Data Waktu:</strong><br>
                        <code>Nama (HH:MM)</code> atau <code>Nama - HH.MM</code><br>
                        Contoh: <code>Yessika (06:35)</code> atau <code>risa alveria (06.16)</code>
                    </div>
                </div>
                
                <div class="tab-container">
                    <div class="tabs">
                        <div class="tab active" data-tab="paste">Copy-Paste Teks</div>
                        <div class="tab" data-tab="upload">Unggah File</div>
                        <div class="tab" data-tab="ocr">OCR dari Gambar</div>
                    </div>
                    
                    <!-- ATTENDANCE STATUS CONTENT -->
                    <div class="tab-content active" id="paste-content">
                        <p id="paste-description">Tempel data kehadiran dengan format status:</p>
                        <textarea class="input-area" id="paste-input" placeholder="Contoh:
Andi - Hadir
Budi - Izin (Sakit)
Citra - Terlambat
Dewi - Hadir
Eko - Sakit
Fajar - Dinas Luar
...">Andi - Hadir
Budi - Izin (Sakit)
Citra - Terlambat
Dewi - Hadir
Eko - Sakit
Fajar - Dinas Luar
Gita - Izin (Keluarga)
Hana - Terlambat
Ivan - Hadir
Joko - Cuti Tahunan</textarea>
                        <button class="btn" id="process-paste">
                            <span>Proses Data Kehadiran</span>
                        </button>
                    </div>
                    
                    <div class="tab-content" id="upload-content">
                        <p id="upload-description">Unggah file teks atau CSV yang berisi data kehadiran:</p>
                        <div class="file-upload" id="file-upload-area">
                            <i>📁</i>
                            <p>Klik untuk memilih file atau seret file ke sini</p>
                            <input type="file" id="file-input" style="display: none;" accept=".txt,.csv">
                        </div>
                        <button class="btn" id="process-file" disabled>
                            <span>Proses File</span>
                        </button>
                    </div>
                    
                    <div class="tab-content" id="ocr-content">
                        <p id="ocr-description">Unggah tangkapan layar atau gambar yang berisi daftar kehadiran:</p>
                        <div class="file-upload" id="ocr-upload-area">
                            <i>🖼️</i>
                            <p>Klik untuk memilih gambar atau seret gambar ke sini</p>
                            <input type="file" id="ocr-input" style="display: none;" accept="image/*">
                        </div>
                        <button class="btn" id="process-ocr" disabled>
                            <span>Ekstrak Teks dan Proses</span>
                        </button>
                    </div>
                </div>
                
                <div class="loading" id="loading">
                    <div class="spinner"></div>
                    <p>Memproses data kehadiran...</p>
                </div>
                
                <!-- RESULTS SECTION -->
                <div class="results" id="results-section">
                    <h2 id="results-title">Hasil Rekap Kehadiran</h2>
                    
                    <!-- SUMMARY CARDS WILL BE DYNAMICALLY GENERATED -->
                    <div class="summary-cards" id="summary-cards-container">
                        <!-- Cards will be generated by JavaScript -->
                    </div>
                    
                    <div class="chart-container">
                        <canvas id="attendance-chart"></canvas>
                    </div>
                    
                    <div class="quick-summary">
                        <h3>Ringkasan Cepat</h3>
                        <p id="quick-summary-text">Total 0 peserta</p>
                        <p id="time-config-summary" style="font-size: 0.9rem; color: var(--gray); margin-top: 0.5rem;"></p>
                        <button class="btn btn-outline" id="copy-summary">
                            <span>Salin Ringkasan</span>
                        </button>
                    </div>
                    
                    <h3 style="margin-top: 2rem;">Detail Kehadiran</h3>
                    <div style="overflow-x: auto;">
                        <table id="attendance-table">
                            <thead>
                                <tr>
                                    <th>No</th>
                                    <th>Nama</th>
                                    <th id="time-column-header">Status</th>
                                    <th>Keterangan</th>
                                </tr>
                            </thead>
                            <tbody id="attendance-table-body">
                                <!-- Data akan diisi oleh JavaScript -->
                            </tbody>
                        </table>
                    </div>
                    
                    <div class="export-options">
                        <button class="btn btn-success" id="export-csv">
                            <span>📥 Ekspor ke CSV</span>
                        </button>
                        <button class="btn btn-success" id="export-excel">
                            <span>📊 Ekspor ke Excel</span>
                        </button>
                        <button class="btn btn-outline" id="reset-data">
                            <span>🔄 Proses Data Baru</span>
                        </button>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <footer>
        <div class="container">
            <div class="footer-content">
                <div class="logo">
                    <div class="logo-icon">📊</div>
                    <h1>PresenceParse</h1>
                </div>
                <p>&copy; 2023 PresenceParse. All rights reserved.</p>
            </div>
        </div>
    </footer>

    <script>
        // Inisialisasi chart global
        let attendanceChart = null;
        let currentProcessedData = null;
        let currentFeature = 'attendance'; // 'attendance' or 'time'
        
        // Custom categories storage
        let customCategories = [
            { name: 'Hadir', color: '#4CAF50' },
            { name: 'Absen', color: '#F44336' },
            { name: 'Terlambat', color: '#FF9800' },
            { name: 'Izin', color: '#2196F3' },
            { name: 'Sakit', color: '#9C27B0' }
        ];

        // Mobile menu functionality
        const mobileMenuBtn = document.getElementById('mobile-menu-btn');
        const mainNav = document.getElementById('main-nav');
        
        mobileMenuBtn.addEventListener('click', () => {
            mainNav.classList.toggle('active');
        });

        // Feature selection
        document.querySelectorAll('.feature-option').forEach(option => {
            option.addEventListener('click', () => {
                // Remove active class from all options
                document.querySelectorAll('.feature-option').forEach(opt => opt.classList.remove('active'));
                // Add active class to clicked option
                option.classList.add('active');
                
                // Update current feature
                currentFeature = option.getAttribute('data-feature');
                
                // Update UI based on selected feature
                updateFeatureUI();
            });
        });

        // Custom categories functionality
        document.getElementById('add-category-btn').addEventListener('click', addCustomCategory);
        
        function addCustomCategory() {
            const nameInput = document.getElementById('custom-category-name');
            const colorInput = document.getElementById('custom-category-color');
            const name = nameInput.value.trim();
            const color = colorInput.value;
            
            if (!name) {
                alert('Silakan masukkan nama kategori');
                return;
            }
            
            // Check if category already exists
            if (customCategories.some(cat => cat.name.toLowerCase() === name.toLowerCase())) {
                alert('Kategori sudah ada!');
                return;
            }
            
            // Add to categories array
            customCategories.push({ name, color });
            
            // Update UI
            updateCategoriesList();
            
            // Clear input
            nameInput.value = '';
            nameInput.focus();
        }
        
        function updateCategoriesList() {
            const categoriesList = document.getElementById('categories-list');
            categoriesList.innerHTML = '';
            
            customCategories.forEach((category, index) => {
                const tag = document.createElement('div');
                tag.className = 'category-tag';
                tag.style.backgroundColor = category.color;
                tag.innerHTML = `
                    ${category.name}
                    ${index >= 5 ? `<button class="remove-btn" data-index="${index}">×</button>` : ''}
                `;
                categoriesList.appendChild(tag);
            });
            
            // Add event listeners to remove buttons
            document.querySelectorAll('.remove-btn').forEach(btn => {
                btn.addEventListener('click', (e) => {
                    e.stopPropagation();
                    const index = parseInt(btn.getAttribute('data-index'));
                    removeCustomCategory(index);
                });
            });
        }
        
        function removeCustomCategory(index) {
            if (index >= 5) { // Only allow removal of custom categories (not default ones)
                customCategories.splice(index, 1);
                updateCategoriesList();
            }
        }

        function updateFeatureUI() {
            const timeConfig = document.getElementById('time-configuration');
            const resultsTitle = document.getElementById('results-title');
            const timeColumnHeader = document.getElementById('time-column-header');
            const pasteDescription = document.getElementById('paste-description');
            const uploadDescription = document.getElementById('upload-description');
            const ocrDescription = document.getElementById('ocr-description');
            const processButton = document.getElementById('process-paste');
            
            if (currentFeature === 'time') {
                // Time analysis feature
                timeConfig.classList.add('active');
                resultsTitle.textContent = 'Hasil Analisis Ketepatan Waktu';
                timeColumnHeader.textContent = 'Waktu';
                pasteDescription.textContent = 'Tempel data kehadiran dengan format waktu:';
                uploadDescription.textContent = 'Unggah file teks atau CSV yang berisi data kehadiran dengan waktu:';
                ocrDescription.textContent = 'Unggah tangkapan layar atau gambar yang berisi daftar kehadiran dengan waktu:';
                processButton.querySelector('span').textContent = 'Analisis Ketepatan Waktu';
                
                // Update placeholder with time example
                document.getElementById('paste-input').placeholder = `Contoh:
List piket PMR 17 April 2025
1. Yessika (06:35)
2. risa alveria (06.16) 
3. nadatul aiysah (06.27)
4. Riska auliya (6:51)
...`;
            } else {
                // Attendance status feature
                timeConfig.classList.remove('active');
                resultsTitle.textContent = 'Hasil Rekap Kehadiran';
                timeColumnHeader.textContent = 'Status';
                pasteDescription.textContent = 'Tempel data kehadiran dengan format status:';
                uploadDescription.textContent = 'Unggah file teks atau CSV yang berisi data kehadiran:';
                ocrDescription.textContent = 'Unggah tangkapan layar atau gambar yang berisi daftar kehadiran:';
                processButton.querySelector('span').textContent = 'Proses Data Kehadiran';
                
                // Update placeholder with status example
                document.getElementById('paste-input').placeholder = `Contoh:
Andi - Hadir
Budi - Izin (Sakit)
Citra - Terlambat
Dewi - Hadir
Eko - Sakit
Fajar - Dinas Luar
...`;
            }
            
            // Reset results when switching features
            document.getElementById('results-section').classList.remove('active');
            currentProcessedData = null;
        }

        // [ALL THE PREVIOUS NAVIGATION, TAB, FILE UPLOAD, AND PROCESSING CODE REMAINS THE SAME]
        // ... (saya potong untuk hemat space, tapi semua kode sebelumnya tetap ada)

        // MODIFIED PARSING FUNCTION TO SUPPORT CUSTOM CATEGORIES
        function parseStatusBasedData(rawData) {
            console.log("Memproses data status kehadiran dengan kategori kustom:", rawData);
            
            const lines = rawData.split('\n').filter(line => line.trim());
            const attendanceData = [];
            
            // Initialize counters for all categories
            const categoryCounts = {};
            customCategories.forEach(cat => {
                categoryCounts[cat.name] = 0;
            });
            
            let totalCount = 0;
            
            lines.forEach((line, index) => {
                let name = '';
                let status = '';
                let notes = '';
                
                // Clean the line
                const cleanLine = line.trim();
                
                // Skip header lines or empty lines
                if (cleanLine.toLowerCase().includes('list') || 
                    cleanLine.toLowerCase().includes('piket') ||
                    cleanLine.match(/^\d+\.\s*$/) ||
                    !cleanLine) {
                    return;
                }
                
                // Check for custom categories first
                let categoryFound = false;
                for (const category of customCategories) {
                    if (cleanLine.toLowerCase().includes(category.name.toLowerCase())) {
                        status = category.name;
                        categoryCounts[category.name]++;
                        categoryFound = true;
                        
                        // Extract notes if available
                        const notesMatch = line.match(new RegExp(`${category.name}\\s*\\(([^)]+)\\)`, 'i'));
                        if (notesMatch) {
                            notes = notesMatch[1];
                        }
                        break;
                    }
                }
                
                // If no custom category found, use default parsing
                if (!categoryFound) {
                    if (cleanLine.toLowerCase().includes('hadir')) {
                        status = 'Hadir';
                        categoryCounts['Hadir']++;
                    } else if (cleanLine.toLowerCase().includes('absen') || cleanLine.toLowerCase().includes('tidak hadir')) {
                        status = 'Absen';
                        categoryCounts['Absen']++;
                    } else if (cleanLine.toLowerCase().includes('terlambat')) {
                        status = 'Terlambat';
                        categoryCounts['Terlambat']++;
                    } else if (cleanLine.toLowerCase().includes('izin') && !cleanLine.toLowerCase().includes('sakit')) {
                        status = 'Izin';
                        categoryCounts['Izin']++;
                        
                        // Extract reason if available
                        const reasonMatch = line.match(/izin\s*\(([^)]+)\)/i);
                        if (reasonMatch) {
                            notes = reasonMatch[1];
                        }
                    } else if (cleanLine.toLowerCase().includes('sakit')) {
                        status = 'Sakit';
                        categoryCounts['Sakit']++;
                        
                        // Extract reason if available
                        const reasonMatch = line.match(/sakit\s*\(([^)]+)\)/i);
                        if (reasonMatch) {
                            notes = reasonMatch[1];
                        }
                    } else {
                        // Default to present if no status detected
                        status = 'Hadir';
                        categoryCounts['Hadir']++;
                    }
                }
                
                // Extract name
                const statusIndex = cleanLine.toLowerCase().indexOf(status.toLowerCase());
                if (statusIndex > 0) {
                    name = cleanLine.substring(0, statusIndex).trim().replace(/[-:]/g, '').trim();
                } else {
                    name = cleanLine.replace(/[-:]/g, '').trim();
                }
                
                // If name is empty, use a generic name
                if (!name) {
                    name = `Peserta ${index + 1}`;
                }
                
                attendanceData.push({
                    id: index + 1,
                    name: name,
                    status: status,
                    notes: notes,
                    categoryColor: getCategoryColor(status)
                });
                
                totalCount++;
            });
            
            return {
                data: attendanceData,
                summary: {
                    categories: categoryCounts,
                    total: totalCount
                }
            };
        }
        
        function getCategoryColor(status) {
            const category = customCategories.find(cat => cat.name === status);
            return category ? category.color : '#6c757d'; // Default gray color
        }

        // MODIFIED DISPLAY FUNCTION TO SUPPORT DYNAMIC CATEGORIES
        function displayResults(processedData) {
            console.log("Menampilkan hasil dengan kategori dinamis:", processedData);
            currentProcessedData = processedData;
            
            if (currentFeature === 'time') {
                // [Time analysis display code remains the same]
                // ... (potong untuk hemat space)
            } else {
                // Update summary cards dynamically based on categories
                updateSummaryCards(processedData.summary.categories);
                
                // Update quick summary
                const summaryText = generateQuickSummary(processedData.summary.categories, processedData.summary.total);
                document.getElementById('quick-summary-text').textContent = summaryText;
                document.getElementById('time-config-summary').textContent = '';
                
                // Create chart for attendance status with dynamic categories
                createDynamicChart(processedData.summary.categories);
            }
            
            // Update table
            const tableBody = document.getElementById('attendance-table-body');
            tableBody.innerHTML = '';
            
            processedData.data.forEach(item => {
                const row = document.createElement('tr');
                
                const statusClass = `status-${item.status.toLowerCase().replace(' ', '-')}`;
                const statusStyle = `color: ${item.categoryColor}; font-weight: 600;`;
                
                let timeColumn = item.status;
                if (currentFeature === 'time' && item.time) {
                    timeColumn = item.time;
                }
                
                row.innerHTML = `
                    <td>${item.id}</td>
                    <td>${item.name}</td>
                    <td style="${statusStyle}">${timeColumn}</td>
                    <td>${item.notes}</td>
                `;
                
                tableBody.appendChild(row);
            });
            
            // Show results section
            document.getElementById('results-section').classList.add('active');
            
            // Scroll to results
            setTimeout(() => {
                document.getElementById('results-section').scrollIntoView({ behavior: 'smooth' });
            }, 100);
        }
        
        function updateSummaryCards(categoryCounts) {
            const container = document.getElementById('summary-cards-container');
            container.innerHTML = '';
            
            // Create cards only for categories that have counts > 0
            Object.entries(categoryCounts).forEach(([categoryName, count]) => {
                if (count > 0) {
                    const category = customCategories.find(cat => cat.name === categoryName);
                    const card = document.createElement('div');
                    card.className = 'summary-card';
                    card.style.borderTopColor = category ? category.color : '#6c757d';
                    
                    card.innerHTML = `
                        <h3>${count}</h3>
                        <p>${categoryName}</p>
                    `;
                    
                    container.appendChild(card);
                }
            });
        }
        
        function generateQuickSummary(categoryCounts, total) {
            const parts = [];
            Object.entries(categoryCounts).forEach(([categoryName, count]) => {
                if (count > 0) {
                    parts.push(`${count} ${categoryName}`);
                }
            });
            
            return `Total ${total} peserta, ${parts.join(', ')}`;
        }
        
        function createDynamicChart(categoryCounts) {
            const ctx = document.getElementById('attendance-chart').getContext('2d');
            
            // If a chart already exists, destroy it
            if (attendanceChart) {
                attendanceChart.destroy();
            }
            
            const labels = [];
            const data = [];
            const backgroundColors = [];
            const borderColors = [];
            
            // Only include categories that have counts > 0
            Object.entries(categoryCounts).forEach(([categoryName, count]) => {
                if (count > 0) {
                    const category = customCategories.find(cat => cat.name === categoryName);
                    labels.push(categoryName);
                    data.push(count);
                    backgroundColors.push(category ? category.color : '#6c757d');
                    borderColors.push(category ? darkenColor(category.color, 20) : '#495057');
                }
            });
            
            attendanceChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: labels,
                    datasets: [{
                        label: 'Jumlah Peserta',
                        data: data,
                        backgroundColor: backgroundColors,
                        borderColor: borderColors,
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        y: {
                            beginAtZero: true,
                            ticks: {
                                stepSize: 1
                            }
                        }
                    }
                }
            });
        }
        
        function darkenColor(color, percent) {
            // Simple function to darken a color
            const num = parseInt(color.replace("#", ""), 16);
            const amt = Math.round(2.55 * percent);
            const R = (num >> 16) - amt;
            const G = (num >> 8 & 0x00FF) - amt;
            const B = (num & 0x0000FF) - amt;
            return "#" + (0x1000000 + (R < 255 ? R < 1 ? 0 : R : 255) * 0x10000 +
                (G < 255 ? G < 1 ? 0 : G : 255) * 0x100 +
                (B < 255 ? B < 1 ? 0 : B : 255)).toString(16).slice(1);
        }

        // Initialize categories list on load
        document.addEventListener('DOMContentLoaded', () => {
            updateCategoriesList();
        });

        // [ALL OTHER FUNCTIONS REMAIN THE SAME - Copy, Export, Reset, etc.]
        // ... (saya potong untuk hemat space)

    </script>
</body>
</html>
