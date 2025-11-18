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
        
        .time-status {
            display: inline-block;
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 0.8rem;
            font-weight: 600;
            margin-left: 8px;
        }
        
        .status-early {
            background: #e8f5e8;
            color: #2e7d32;
        }
        
        .status-ontime {
            background: #e3f2fd;
            color: #1565c0;
        }
        
        .status-late {
            background: #fff3e0;
            color: #ef6c00;
        }
        
        .time-detail {
            font-size: 0.85rem;
            color: var(--gray);
            margin-top: 4px;
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
            <h2>Analisis Kehadiran Cerdas Berbasis Waktu</h2>
            <p>PresenceParse kini dengan fitur analisis waktu otomatis! Tentukan batas waktu, sistem akan secara cerdas mengkategorikan tepat waktu, terlambat, atau early.</p>
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
                    <div class="feature-icon">⏰</div>
                    <h3>Analisis Waktu Cerdas</h3>
                    <p>Otomatis deteksi status tepat waktu, early, atau terlambat berdasarkan batas waktu yang Anda tentukan.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">🧠</div>
                    <h3>Logika Rekap Otomatis</h3>
                    <p>Sistem secara cerdas membersihkan nama, mengidentifikasi status kehadiran, dan menganalisis waktu berdasarkan kata kunci.</p>
                </div>
                <div class="feature-card">
                    <div class="feature-icon">📈</div>
                    <h3>Laporan yang Dapat Disesuaikan</h3>
                    <p>Hasilkan dashboard visual dengan grafik dan opsi ekspor ke berbagai format sesuai kebutuhan Anda.</p>
                </div>
            </div>
        </div>
    </section>

    <section id="process">
        <div class="container">
            <div class="main-content">
                <h2 style="margin-bottom: 1.5rem;">Proses Data Kehadiran</h2>
                
                <!-- NEW TIME CONFIGURATION SECTION -->
                <div class="time-configuration">
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
                        <strong>Contoh Format Data:</strong><br>
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
                    
                    <div class="tab-content active" id="paste-content">
                        <p>Tempel data kehadiran dengan format waktu:</p>
                        <textarea class="input-area" id="paste-input" placeholder="Contoh:
List piket PMR 17 April 2025
1. Yessika (06:35)
2. risa alveria (06.16) 
3. nadatul aiysah (06.27)
4. Riska auliya (6:51)
5. Tara aldiana nahara sakila (6:51)
6. Rachmania (6.34) mengganti hari senin 
7. Salsa nur(6.13)
8. Delfi Anandarista (06.20)
9. Hartopo husna d ( 06.33 )
10. Nurul (06.55)
11. Rehan (06.35)
12. Aisyah Amelia (06.57)
13. Ucik deanita (06.04)">List piket PMR 17 April 2025
1. Yessika (06:35)
2. risa alveria (06.16) 
3. nadatul aiysah (06.27)
4. Riska auliya (6:51)
5. Tara aldiana nahara sakila (6:51)
6. Rachmania (6.34) mengganti hari senin 
7. Salsa nur(6.13)
8. Delfi Anandarista (06.20)
9. Hartopo husna d ( 06.33 )
10. Nurul (06.55)
11. Rehan (06.35)
12. Aisyah Amelia (06.57)
13. Ucik deanita (06.04)</textarea>
                        <button class="btn" id="process-paste">
                            <span>Analisis Data Waktu</span>
                        </button>
                    </div>
                    
                    <div class="tab-content" id="upload-content">
                        <p>Unggah file teks atau CSV yang berisi data kehadiran dengan waktu:</p>
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
                        <p>Unggah tangkapan layar atau gambar yang berisi daftar kehadiran dengan waktu:</p>
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
                    <p>Menganalisis data kehadiran...</p>
                </div>
                
                <div class="results" id="results-section">
                    <h2>Hasil Analisis Kehadiran</h2>
                    
                    <div class="summary-cards">
                        <div class="summary-card present">
                            <h3 id="present-count">0</h3>
                            <p>Tepat Waktu</p>
                        </div>
                        <div class="summary-card absent">
                            <h3 id="absent-count">0</h3>
                            <p>Terlambat</p>
                        </div>
                        <div class="summary-card late">
                            <h3 id="late-count">0</h3>
                            <p>Early</p>
                        </div>
                        <div class="summary-card permission">
                            <h3 id="permission-count">0</h3>
                            <p>Izin</p>
                        </div>
                        <div class="summary-card sick">
                            <h3 id="sick-count">0</h3>
                            <p>Sakit</p>
                        </div>
                    </div>
                    
                    <div class="chart-container">
                        <canvas id="attendance-chart"></canvas>
                    </div>
                    
                    <div class="quick-summary">
                        <h3>Ringkasan Cepat</h3>
                        <p id="quick-summary-text">Total 0 peserta, 0 Tepat Waktu, 0 Terlambat, 0 Early, 0 Izin, 0 Sakit</p>
                        <p id="time-config-summary" style="font-size: 0.9rem; color: var(--gray); margin-top: 0.5rem;"></p>
                        <button class="btn btn-outline" id="copy-summary">
                            <span>Salin Ringkasan</span>
                        </button>
                    </div>
                    
                    <h3 style="margin-top: 2rem;">Detail Analisis Waktu</h3>
                    <div style="overflow-x: auto;">
                        <table id="attendance-table">
                            <thead>
                                <tr>
                                    <th>No</th>
                                    <th>Nama</th>
                                    <th>Waktu</th>
                                    <th>Status</th>
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
                            <span>🔄 Analisis Data Baru</span>
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

        // Mobile menu functionality
        const mobileMenuBtn = document.getElementById('mobile-menu-btn');
        const mainNav = document.getElementById('main-nav');
        
        mobileMenuBtn.addEventListener('click', () => {
            mainNav.classList.toggle('active');
        });

        // Perbaikan navigasi - handle semua link navigasi
        document.querySelectorAll('.nav-link').forEach(link => {
            link.addEventListener('click', (e) => {
                e.preventDefault();
                const targetId = link.getAttribute('href').substring(1);
                const targetSection = document.getElementById(targetId);
                
                if (targetSection) {
                    // Tutup mobile menu jika terbuka
                    mainNav.classList.remove('active');
                    
                    // Scroll ke section target
                    targetSection.scrollIntoView({ 
                        behavior: 'smooth',
                        block: 'start'
                    });
                }
            });
        });

        // Tab functionality
        document.querySelectorAll('.tab').forEach(tab => {
            tab.addEventListener('click', () => {
                // Remove active class from all tabs and contents
                document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                
                // Add active class to clicked tab
                tab.classList.add('active');
                
                // Show corresponding content
                const tabId = tab.getAttribute('data-tab');
                document.getElementById(`${tabId}-content`).classList.add('active');
            });
        });
        
        // File upload functionality
        const fileUploadArea = document.getElementById('file-upload-area');
        const fileInput = document.getElementById('file-input');
        const processFileBtn = document.getElementById('process-file');
        
        fileUploadArea.addEventListener('click', () => {
            fileInput.click();
        });
        
        fileUploadArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            fileUploadArea.style.borderColor = 'var(--primary)';
            fileUploadArea.style.backgroundColor = 'rgba(67, 97, 238, 0.05)';
        });
        
        fileUploadArea.addEventListener('dragleave', () => {
            fileUploadArea.style.borderColor = '#ddd';
            fileUploadArea.style.backgroundColor = 'transparent';
        });
        
        fileUploadArea.addEventListener('drop', (e) => {
            e.preventDefault();
            fileUploadArea.style.borderColor = '#ddd';
            fileUploadArea.style.backgroundColor = 'transparent';
            
            if (e.dataTransfer.files.length) {
                fileInput.files = e.dataTransfer.files;
                processFileBtn.disabled = false;
                fileUploadArea.querySelector('p').textContent = `File dipilih: ${e.dataTransfer.files[0].name}`;
            }
        });
        
        fileInput.addEventListener('change', () => {
            if (fileInput.files.length) {
                processFileBtn.disabled = false;
                fileUploadArea.querySelector('p').textContent = `File dipilih: ${fileInput.files[0].name}`;
            }
        });
        
        // OCR upload functionality
        const ocrUploadArea = document.getElementById('ocr-upload-area');
        const ocrInput = document.getElementById('ocr-input');
        const processOcrBtn = document.getElementById('process-ocr');
        
        ocrUploadArea.addEventListener('click', () => {
            ocrInput.click();
        });
        
        ocrUploadArea.addEventListener('dragover', (e) => {
            e.preventDefault();
            ocrUploadArea.style.borderColor = 'var(--primary)';
            ocrUploadArea.style.backgroundColor = 'rgba(67, 97, 238, 0.05)';
        });
        
        ocrUploadArea.addEventListener('dragleave', () => {
            ocrUploadArea.style.borderColor = '#ddd';
            ocrUploadArea.style.backgroundColor = 'transparent';
        });
        
        ocrUploadArea.addEventListener('drop', (e) => {
            e.preventDefault();
            ocrUploadArea.style.borderColor = '#ddd';
            ocrUploadArea.style.backgroundColor = 'transparent';
            
            if (e.dataTransfer.files.length) {
                ocrInput.files = e.dataTransfer.files;
                processOcrBtn.disabled = false;
                ocrUploadArea.querySelector('p').textContent = `Gambar dipilih: ${e.dataTransfer.files[0].name}`;
            }
        });
        
        ocrInput.addEventListener('change', () => {
            if (ocrInput.files.length) {
                processOcrBtn.disabled = false;
                ocrUploadArea.querySelector('p').textContent = `Gambar dipilih: ${ocrInput.files[0].name}`;
            }
        });
        
        // Process data functions
        document.getElementById('process-paste').addEventListener('click', processPasteData);
        document.getElementById('process-file').addEventListener('click', processFileData);
        document.getElementById('process-ocr').addEventListener('click', processOcrData);
        
        function processPasteData() {
            const inputText = document.getElementById('paste-input').value;
            if (!inputText.trim()) {
                alert('Silakan tempel data kehadiran terlebih dahulu.');
                return;
            }
            
            showLoading();
            setTimeout(() => {
                const processedData = parseAttendanceData(inputText);
                hideLoading();
                displayResults(processedData);
            }, 1000);
        }
        
        function processFileData() {
            const file = fileInput.files[0];
            if (!file) {
                alert('Silakan pilih file terlebih dahulu.');
                return;
            }
            
            showLoading();
            const reader = new FileReader();
            reader.onload = function(e) {
                setTimeout(() => {
                    const processedData = parseAttendanceData(e.target.result);
                    hideLoading();
                    displayResults(processedData);
                }, 1000);
            };
            reader.readAsText(file);
        }
        
        function processOcrData() {
            const file = ocrInput.files[0];
            if (!file) {
                alert('Silakan pilih gambar terlebih dahulu.');
                return;
            }
            
            showLoading();
            // Simulate OCR processing
            setTimeout(() => {
                const sampleText = `List piket PMR 17 April 2025
1. Yessika (06:35)
2. risa alveria (06.16) 
3. nadatul aiysah (06.27)
4. Riska auliya (6:51)
5. Tara aldiana nahara sakila (6:51)
6. Rachmania (6.34) mengganti hari senin 
7. Salsa nur(6.13)
8. Delfi Anandarista (06.20)
9. Hartopo husna d ( 06.33 )
10. Nurul (06.55)
11. Rehan (06.35)
12. Aisyah Amelia (06.57)
13. Ucik deanita (06.04)`;
                const processedData = parseAttendanceData(sampleText);
                hideLoading();
                displayResults(processedData);
                
                setTimeout(() => {
                    alert('Catatan: Ini adalah simulasi OCR. Dalam implementasi nyata, gambar akan diproses dengan teknologi OCR untuk mengekstrak teks.');
                }, 500);
            }, 2000);
        }
        
        function showLoading() {
            document.getElementById('loading').style.display = 'block';
        }
        
        function hideLoading() {
            document.getElementById('loading').style.display = 'none';
        }
        
        // NEW IMPROVED PARSING FUNCTION WITH TIME ANALYSIS
        function parseAttendanceData(rawData) {
            console.log("Memproses data dengan analisis waktu:", rawData);
            
            const lines = rawData.split('\n').filter(line => line.trim());
            const attendanceData = [];
            
            // Get time configuration from user input
            const onTimeLimit = document.getElementById('on-time-limit').value;
            const lateTimeLimit = document.getElementById('late-time-limit').value;
            
            // Convert time strings to minutes for comparison
            const onTimeMinutes = timeToMinutes(onTimeLimit);
            const lateTimeMinutes = timeToMinutes(lateTimeLimit);
            
            let presentCount = 0;
            let absentCount = 0;
            let lateCount = 0;
            let permissionCount = 0;
            let sickCount = 0;
            let earlyCount = 0;
            
            lines.forEach((line, index) => {
                let name = '';
                let status = '';
                let notes = '';
                let time = '';
                let timeMinutes = 0;
                
                // Clean the line
                const cleanLine = line.trim();
                
                // Skip header lines or lines without time data
                if (cleanLine.toLowerCase().includes('list') || 
                    cleanLine.toLowerCase().includes('piket') ||
                    cleanLine.match(/^\d+\.\s*$/)) {
                    return;
                }
                
                // Extract time using regex - supports (HH:MM), (HH.MM), HH:MM, HH.MM
                const timeMatch = cleanLine.match(/(\d{1,2})[\.:](\d{2})/);
                if (timeMatch) {
                    const hours = parseInt(timeMatch[1]);
                    const minutes = parseInt(timeMatch[2]);
                    time = `${hours.toString().padStart(2, '0')}:${minutes.toString().padStart(2, '0')}`;
                    timeMinutes = hours * 60 + minutes;
                    
                    // Determine status based on time comparison
                    if (timeMinutes < onTimeMinutes) {
                        status = 'Early';
                        earlyCount++;
                    } else if (timeMinutes <= lateTimeMinutes) {
                        status = 'Tepat Waktu';
                        presentCount++;
                    } else {
                        status = 'Terlambat';
                        lateCount++;
                    }
                } else {
                    // Fallback to text-based parsing if no time found
                    if (cleanLine.toLowerCase().includes('hadir')) {
                        status = 'Tepat Waktu';
                        presentCount++;
                    } else if (cleanLine.toLowerCase().includes('absen') || cleanLine.toLowerCase().includes('tidak hadir')) {
                        status = 'Terlambat';
                        lateCount++;
                    } else if (cleanLine.toLowerCase().includes('terlambat')) {
                        status = 'Terlambat';
                        lateCount++;
                    } else if (cleanLine.toLowerCase().includes('izin') && !cleanLine.toLowerCase().includes('sakit')) {
                        status = 'Izin';
                        permissionCount++;
                    } else if (cleanLine.toLowerCase().includes('sakit')) {
                        status = 'Sakit';
                        sickCount++;
                    } else {
                        status = 'Tepat Waktu';
                        presentCount++;
                    }
                }
                
                // Extract name (remove numbers, time patterns, and special characters)
                name = cleanLine
                    .replace(/\d+\.\s*/, '') // Remove numbering
                    .replace(/\(.*?\)/g, '')  // Remove parentheses content
                    .replace(/\d{1,2}[\.:]\d{2}/g, '') // Remove time patterns
                    .replace(/[^\w\s]/g, '') // Remove special characters
                    .trim();
                
                // If name is empty, use a generic name
                if (!name) {
                    name = `Peserta ${index + 1}`;
                }
                
                // Extract additional notes
                const notesMatch = cleanLine.match(/\(([^)]+)\)/);
                if (notesMatch && !timeMatch) {
                    notes = notesMatch[1];
                }
                
                attendanceData.push({
                    id: index + 1,
                    name: name,
                    status: status,
                    notes: notes,
                    time: time,
                    timeMinutes: timeMinutes
                });
            });
            
            return {
                data: attendanceData,
                summary: {
                    present: presentCount,
                    absent: lateCount, // Using absent for Terlambat
                    late: earlyCount,  // Using late for Early
                    permission: permissionCount,
                    sick: sickCount,
                    total: attendanceData.length,
                    timeConfig: {
                        onTime: onTimeLimit,
                        lateTime: lateTimeLimit
                    }
                }
            };
        }
        
        // Helper function to convert time string to minutes
        function timeToMinutes(timeStr) {
            const [hours, minutes] = timeStr.split(':').map(Number);
            return hours * 60 + minutes;
        }
        
        // Helper function to format time difference
        function formatTimeDifference(minutes, referenceMinutes) {
            const diff = minutes - referenceMinutes;
            const absDiff = Math.abs(diff);
            const hours = Math.floor(absDiff / 60);
            const mins = absDiff % 60;
            
            if (diff < 0) {
                return `${hours > 0 ? hours + 'j ' : ''}${mins}m lebih awal`;
            } else {
                return `${hours > 0 ? hours + 'j ' : ''}${mins}m terlambat`;
            }
        }
        
        function displayResults(processedData) {
            console.log("Menampilkan hasil dengan analisis waktu:", processedData);
            currentProcessedData = processedData;
            
            // Update summary cards
            document.getElementById('present-count').textContent = processedData.summary.present;
            document.getElementById('absent-count').textContent = processedData.summary.absent;
            document.getElementById('late-count').textContent = processedData.summary.late;
            document.getElementById('permission-count').textContent = processedData.summary.permission;
            document.getElementById('sick-count').textContent = processedData.summary.sick;
            
            // Update quick summary
            const quickSummary = `Total ${processedData.summary.total} peserta, ${processedData.summary.present} Tepat Waktu, ${processedData.summary.absent} Terlambat, ${processedData.summary.late} Early, ${processedData.summary.permission} Izin, ${processedData.summary.sick} Sakit`;
            document.getElementById('quick-summary-text').textContent = quickSummary;
            
            // Update time configuration summary
            const timeSummary = `Batas waktu: Tepat Waktu ≤ ${processedData.summary.timeConfig.onTime}, Terlambat > ${processedData.summary.timeConfig.lateTime}`;
            document.getElementById('time-config-summary').textContent = timeSummary;
            
            // Update table with time analysis
            const tableBody = document.getElementById('attendance-table-body');
            tableBody.innerHTML = '';
            
            const onTimeMinutes = timeToMinutes(processedData.summary.timeConfig.onTime);
            const lateTimeMinutes = timeToMinutes(processedData.summary.timeConfig.lateTime);
            
            processedData.data.forEach(item => {
                const row = document.createElement('tr');
                
                let statusClass = '';
                let statusBadge = '';
                let timeDetail = '';
                
                if (item.status === 'Tepat Waktu') {
                    statusClass = 'status-present';
                    statusBadge = '<span class="time-status status-ontime">TEPAT WAKTU</span>';
                    if (item.time) {
                        timeDetail = `<div class="time-detail">${formatTimeDifference(item.timeMinutes, onTimeMinutes)}</div>`;
                    }
                } else if (item.status === 'Terlambat') {
                    statusClass = 'status-absent';
                    statusBadge = '<span class="time-status status-late">TERLAMBAT</span>';
                    if (item.time) {
                        timeDetail = `<div class="time-detail">${formatTimeDifference(item.timeMinutes, lateTimeMinutes)}</div>`;
                    }
                } else if (item.status === 'Early') {
                    statusClass = 'status-late';
                    statusBadge = '<span class="time-status status-early">EARLY</span>';
                    if (item.time) {
                        timeDetail = `<div class="time-detail">${formatTimeDifference(item.timeMinutes, onTimeMinutes)}</div>`;
                    }
                } else if (item.status === 'Izin') {
                    statusClass = 'status-permission';
                    statusBadge = '<span class="time-status status-permission">IZIN</span>';
                } else if (item.status === 'Sakit') {
                    statusClass = 'status-sick';
                    statusBadge = '<span class="time-status status-sick">SAKIT</span>';
                }
                
                row.innerHTML = `
                    <td>${item.id}</td>
                    <td>${item.name}</td>
                    <td>${item.time || '-'}</td>
                    <td class="${statusClass}">
                        ${item.status}
                        ${statusBadge}
                        ${timeDetail}
                    </td>
                    <td>${item.notes}</td>
                `;
                
                tableBody.appendChild(row);
            });
            
            // Create chart
            createChart(processedData.summary);
            
            // Show results section
            document.getElementById('results-section').classList.add('active');
            
            // Scroll to results
            setTimeout(() => {
                document.getElementById('results-section').scrollIntoView({ behavior: 'smooth' });
            }, 100);
        }
        
        function createChart(summary) {
            const ctx = document.getElementById('attendance-chart').getContext('2d');
            
            // If a chart already exists, destroy it
            if (attendanceChart) {
                attendanceChart.destroy();
            }
            
            attendanceChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Tepat Waktu', 'Terlambat', 'Early', 'Izin', 'Sakit'],
                    datasets: [{
                        label: 'Jumlah Peserta',
                        data: [
                            summary.present, 
                            summary.absent, 
                            summary.late, 
                            summary.permission, 
                            summary.sick
                        ],
                        backgroundColor: [
                            '#4CAF50',  // Hijau untuk Tepat Waktu
                            '#F44336',  // Merah untuk Terlambat
                            '#FF9800',  // Oranye untuk Early
                            '#2196F3',  // Biru untuk Izin
                            '#9C27B0'   // Ungu untuk Sakit
                        ],
                        borderColor: [
                            '#388E3C',
                            '#D32F2F',
                            '#F57C00',
                            '#1976D2',
                            '#7B1FA2'
                        ],
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
        
        // Copy summary functionality
        document.getElementById('copy-summary').addEventListener('click', () => {
            const summaryText = document.getElementById('quick-summary-text').textContent;
            const timeConfigText = document.getElementById('time-config-summary').textContent;
            const fullText = `${summaryText}\n${timeConfigText}`;
            
            navigator.clipboard.writeText(fullText).then(() => {
                const button = document.getElementById('copy-summary');
                const originalText = button.querySelector('span').textContent;
                button.querySelector('span').textContent = 'Tersalin!';
                
                setTimeout(() => {
                    button.querySelector('span').textContent = originalText;
                }, 2000);
            }).catch(err => {
                console.error('Gagal menyalin teks: ', err);
                alert('Gagal menyalin teks. Silakan salin manual.');
            });
        });
        
        // Export to CSV functionality
        document.getElementById('export-csv').addEventListener('click', () => {
            if (!currentProcessedData) {
                alert('Tidak ada data untuk diekspor. Silakan proses data terlebih dahulu.');
                return;
            }
            
            exportToCSV(currentProcessedData);
        });
        
        // Export to Excel functionality
        document.getElementById('export-excel').addEventListener('click', () => {
            if (!currentProcessedData) {
                alert('Tidak ada data untuk diekspor. Silakan proses data terlebih dahulu.');
                return;
            }
            
            exportToExcel(currentProcessedData);
        }
        
        function exportToCSV(processedData) {
            let csvContent = "No,Nama,Waktu,Status,Keterangan\n";
            
            processedData.data.forEach(item => {
                csvContent += `"${item.id}","${item.name}","${item.time || '-'}","${item.status}","${item.notes}"\n`;
            });
            
            // Add summary section
            csvContent += "\n\nSUMMARY\n";
            csvContent += `Total Peserta,${processedData.summary.total}\n`;
            csvContent += `Tepat Waktu,${processedData.summary.present}\n`;
            csvContent += `Terlambat,${processedData.summary.absent}\n`;
            csvContent += `Early,${processedData.summary.late}\n`;
            csvContent += `Izin,${processedData.summary.permission}\n`;
            csvContent += `Sakit,${processedData.summary.sick}\n`;
            csvContent += `Batas Tepat Waktu,${processedData.summary.timeConfig.onTime}\n`;
            csvContent += `Batas Terlambat,${processedData.summary.timeConfig.lateTime}\n`;
            
            const blob = new Blob([csvContent], { type: 'text/csv;charset=utf-8;' });
            const link = document.createElement("a");
            const url = URL.createObjectURL(blob);
            link.setAttribute("href", url);
            link.setAttribute("download", `rekap_kehadiran_waktu_${new Date().toISOString().split('T')[0]}.csv`);
            link.style.visibility = 'hidden';
            document.body.appendChild(link);
            link.click();
            document.body.removeChild(link);
            
            showExportSuccess('CSV');
        }
        
        function exportToExcel(processedData) {
            try {
                // Prepare worksheet data
                const worksheetData = [
                    ["No", "Nama", "Waktu", "Status", "Keterangan"],
                    ...processedData.data.map(item => [item.id, item.name, item.time || '-', item.status, item.notes]),
                    [""],
                    ["SUMMARY"],
                    ["Total Peserta", processedData.summary.total],
                    ["Tepat Waktu", processedData.summary.present],
                    ["Terlambat", processedData.summary.absent],
                    ["Early", processedData.summary.late],
                    ["Izin", processedData.summary.permission],
                    ["Sakit", processedData.summary.sick],
                    ["Batas Tepat Waktu", processedData.summary.timeConfig.onTime],
                    ["Batas Terlambat", processedData.summary.timeConfig.lateTime]
                ];
                
                // Create workbook and worksheet
                const wb = XLSX.utils.book_new();
                const ws = XLSX.utils.aoa_to_sheet(worksheetData);
                
                // Add worksheet to workbook
                XLSX.utils.book_append_sheet(wb, ws, "Rekap Kehadiran");
                
                // Generate Excel file and download
                XLSX.writeFile(wb, `rekap_kehadiran_waktu_${new Date().toISOString().split('T')[0]}.xlsx`);
                
                showExportSuccess('Excel');
            } catch (error) {
                console.error('Error exporting to Excel:', error);
                alert('Terjadi error saat mengekspor ke Excel. Silakan coba lagi.');
            }
        }
        
        function showExportSuccess(format) {
            const button = document.getElementById(`export-${format.toLowerCase()}`);
            const originalText = button.querySelector('span').textContent;
            button.querySelector('span').textContent = `✅ ${format} Terunduh!`;
            button.style.backgroundColor = '#4CAF50';
            
            setTimeout(() => {
                button.querySelector('span').textContent = originalText;
                button.style.backgroundColor = '';
            }, 2000);
        }
        
        // Reset data functionality
        document.getElementById('reset-data').addEventListener('click', () => {
            document.getElementById('results-section').classList.remove('active');
            document.getElementById('paste-input').value = '';
            document.getElementById('file-input').value = '';
            document.getElementById('ocr-input').value = '';
            fileUploadArea.querySelector('p').textContent = 'Klik untuk memilih file atau seret file ke sini';
            ocrUploadArea.querySelector('p').textContent = 'Klik untuk memilih gambar atau seret gambar ke sini';
            processFileBtn.disabled = true;
            processOcrBtn.disabled = true;
            currentProcessedData = null;
            
            // Scroll back to process section
            document.getElementById('process').scrollIntoView({ behavior: 'smooth' });
        });
        
        // Scroll to section function
        function scrollToSection(sectionId) {
            const section = document.getElementById(sectionId);
            if (section) {
                section.scrollIntoView({ 
                    behavior: 'smooth',
                    block: 'start'
                });
            }
        }
        
        // Initialize with sample data for demo
        window.addEventListener('load', () => {
            console.log("Website PresenceParse dengan fitur analisis waktu berhasil dimuat!");
        });
    </script>
</body>
</html>
