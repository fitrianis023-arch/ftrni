<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>🔬 Simulasi Gas Ideal SMA - Lebih Unggul dari PhET</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
        }
        
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f0f23 0%, #1a1a2e 50%, #16213e 100%);
            color: #e0e6ed;
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* Navigation */
        .navbar {
            position: fixed;
            top: 0;
            width: 100%;
            background: rgba(15, 15, 35, 0.95);
            backdrop-filter: blur(20px);
            padding: 1rem 2rem;
            z-index: 1000;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .nav-container {
            max-width: 1400px;
            margin: 0 auto;
            display: flex;
            justify-content: space-between;
            align-items: center;
        }
        
        .logo {
            font-size: 1.8rem;
            font-weight: 700;
            background: linear-gradient(45deg, #ffd700, #ff6b35);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
        }
        
        .progress-bar {
            flex: 1;
            margin: 0 2rem;
            height: 4px;
            background: rgba(255,255,255,0.1);
            border-radius: 2px;
            overflow: hidden;
        }
        
        .progress-fill {
            height: 100%;
            background: linear-gradient(90deg, #4facfe 0%, #00f2fe 100%);
            width: 0%;
            transition: width 0.5s ease;
            border-radius: 2px;
        }

        /* Page Container */
        .page-container {
            min-height: 100vh;
            padding-top: 80px;
            opacity: 0;
            transform: translateY(30px);
            transition: all 0.8s cubic-bezier(0.25, 0.46, 0.45, 0.94);
        }
        
        .page-container.active {
            opacity: 1;
            transform: translateY(0);
        }

        /* Page 1: Landing */
        .landing {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            text-align: center;
            padding: 2rem;
            max-width: 1400px;
            margin: 0 auto;
        }
        
        .hero-title {
            font-size: clamp(2.5rem, 5vw, 4rem);
            margin-bottom: 1rem;
            background: linear-gradient(45deg, #ffd700, #ff6b35, #4facfe);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            background-clip: text;
            text-shadow: 0 0 30px rgba(255,215,0,0.3);
        }
        
        .hero-subtitle {
            font-size: 1.3rem;
            margin-bottom: 3rem;
            color: #b0b3c1;
            max-width: 600px;
            line-height: 1.6;
        }
        
        .cta-button {
            background: linear-gradient(45deg, #ffd700, #ff6b35);
            color: #1a1a2e;
            border: none;
            padding: 1.2rem 3rem;
            font-size: 1.2rem;
            font-weight: 700;
            border-radius: 50px;
            cursor: pointer;
            box-shadow: 0 10px 30px rgba(255,215,0,0.4);
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .cta-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 15px 40px rgba(255,215,0,0.6);
        }

        /* Page 2: Materi SMA */
        .materi-page {
            max-width: 1200px;
            margin: 0 auto;
            padding: 3rem 2rem;
        }
        
        .section-title {
            font-size: 2.5rem;
            text-align: center;
            margin-bottom: 3rem;
            background: linear-gradient(45deg, #4facfe, #00f2fe);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .materi-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(350px, 1fr));
            gap: 2rem;
            margin-bottom: 3rem;
        }
        
        .materi-card {
            background: rgba(255,255,255,0.05);
            backdrop-filter: blur(15px);
            border-radius: 20px;
            padding: 2rem;
            border: 1px solid rgba(255,255,255,0.1);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .materi-card::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 4px;
            background: linear-gradient(90deg, #ffd700, #ff6b35);
        }
        
        .materi-card:hover {
            transform: translateY(-10px);
            box-shadow: 0 20px 40px rgba(0,0,0,0.3);
        }
        
        .materi-icon {
            font-size: 3rem;
            margin-bottom: 1rem;
            display: block;
        }
        
        .materi-card h3 {
            font-size: 1.5rem;
            margin-bottom: 1rem;
            color: #ffd700;
        }
        
        .formula {
            background: rgba(0,0,0,0.3);
            padding: 1rem;
            border-radius: 10px;
            margin: 1rem 0;
            font-family: monospace;
            font-size: 1.2rem;
            text-align: center;
            border-left: 5px solid #ffd700;
        }

        /* Page 3: Simulasi */
        .simulasi-container {
            max-width: 1200px;
            margin: 0 auto;
            padding: 2rem;
        }
        
        .simulasi-header {
            text-align: center;
            margin-bottom: 2rem;
        }
        
        .process-selector {
            display: flex;
            gap: 1rem;
            justify-content: center;
            margin-bottom: 2rem;
            flex-wrap: wrap;
        }
        
        .process-btn {
            background: rgba(255,255,255,0.1);
            border: 2px solid #4facfe;
            color: #e0e6ed;
            padding: 1rem 1.5rem;
            border-radius: 25px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            font-size: 1rem;
        }
        
        .process-btn.active {
            background: linear-gradient(45deg, #4facfe, #00f2fe);
            color: #1a1a2e;
            box-shadow: 0 5px 15px rgba(79,172,254,0.4);
            transform: scale(1.05);
        }
        
        .canvas-container {
            background: #0b1421;
            border-radius: 32px;
            padding: 20px;
            box-shadow: inset 0 -3px 8px #00000099, 0 12px 25px black;
            border: 2px solid #6a7ca0;
            margin-bottom: 2rem;
        }
        
        canvas {
            display: block;
            width: 100%;
            height: auto;
            background: radial-gradient(circle at 30% 30%, #25344f, #101624);
            border-radius: 24px;
            cursor: pointer;
            border: 2px solid #5f73a1;
            transition: all 0.2s;
        }

        /* LKPD */
        .lkpd-container {
            max-width: 900px;
            margin: 0 auto;
            padding: 3rem 2rem;
        }
        
        .lkpd-form {
            background: rgba(255,255,255,0.05);
            backdrop-filter: blur(15px);
            border-radius: 20px;
            padding: 2.5rem;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .form-section {
            margin-bottom: 2rem;
            padding-bottom: 1.5rem;
            border-bottom: 1px solid rgba(255,255,255,0.1);
        }
        
        .form-section h3 {
            color: #ffd700;
            margin-bottom: 1rem;
        }
        
        .form-group {
            margin-bottom: 1.5rem;
        }
        
        .form-group label {
            display: block;
            margin-bottom: 0.5rem;
            font-weight: 600;
            color: #e0e6ed;
        }
        
        .form-group input, .form-group textarea, .form-group select {
            width: 100%;
            padding: 1rem;
            border: 2px solid rgba(255,255,255,0.2);
            border-radius: 12px;
            background: rgba(255,255,255,0.05);
            color: #e0e6ed;
            font-size: 1rem;
            transition: all 0.3s ease;
            font-family: inherit;
        }
        
        .form-group input:focus, .form-group textarea:focus, .form-group select:focus {
            outline: none;
            border-color: #4facfe;
            box-shadow: 0 0 15px rgba(79,172,254,0.3);
            background: rgba(255,255,255,0.1);
        }
        
        .submit-btn {
            width: 100%;
            background: linear-gradient(45deg, #4facfe, #00f2fe);
            color: #1a1a2e;
            border: none;
            padding: 1.2rem;
            font-size: 1.3rem;
            font-weight: 700;
            border-radius: 15px;
            cursor: pointer;
            transition: all 0.3s ease;
            text-transform: uppercase;
            letter-spacing: 1px;
        }
        
        .submit-btn:hover:not(:disabled) {
            transform: translateY(-3px);
            box-shadow: 0 15px 35px rgba(79,172,254,0.5);
        }

        .monitoring-panel {
            background: rgba(255,255,255,0.03);
            border-radius: 15px;
            padding: 1.5rem;
            margin-top: 2rem;
            border: 1px solid rgba(255,215,0,0.3);
        }
        
        .monitoring-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(180px, 1fr));
            gap: 1rem;
        }
        
        .monitor-item {
            text-align: center;
            padding: 1rem;
            background: rgba(255,255,255,0.05);
            border-radius: 12px;
            border: 1px solid rgba(255,255,255,0.1);
        }
        
        .monitor-label {
            font-size: 0.9rem;
            color: #b0b3c1;
            margin-bottom: 0.5rem;
        }
        
        .monitor-value {
            font-size: 1.8rem;
            font-weight: 700;
            color: #ffd700;
        }

        @media (max-width: 768px) {
            .materi-grid { grid-template-columns: 1fr; }
            .process-selector { flex-direction: column; align-items: center; }
        }
    </style>
</head>
<body>
    <!-- Navigation -->
    <nav class="navbar">
        <div class="nav-container">
            <div class="logo">🔬 Simulasi Gas Ideal SMA</div>
            <div class="progress-bar">
                <div class="progress-fill" id="progressFill"></div>
            </div>
            <div>Halaman <span id="currentPage">1</span>/4</div>
        </div>
    </nav>

    <!-- Page 1: Landing -->
    <div id="page1" class="page-container active">
        <div class="landing">
            <h1 class="hero-title">Simulasi Gas Ideal</h1>
            <p class="hero-subtitle">
                Pelajari <strong>Teori Kinetik Gas</strong>, <strong>Gas Ideal</strong>, dan <strong>3 Proses Termodinamika</strong> 
                (Isokhorik, Isobarik, Isotermal) dengan simulasi interaktif 100% sesuai kurikulum SMA.
            </p>
            <button class="cta-button" onclick="nextPage()">🚀 Mulai Simulasi</button>
        </div>
    </div>

    <!-- Page 2: Materi SMA -->
    <div id="page2" class="page-container materi-page">
        <h2 class="section-title">📚 Materi SMA Kelas XII</h2>
        
        <div class="materi-grid">
            <div class="materi-card">
                <span class="materi-icon">⚛️</span>
                <h3>Teori Kinetik Gas</h3>
                <p><strong>Gas Ideal:</strong> partikel bergerak acak, tidak berinteraksi, tumbukan elastis</p>
                <div class="formula">P = ⅓ ρ v_rms²</div>
                <p><strong>Hubungan Suhu:</strong> T ↑ → v_rms ↑ → P ↑</p>
            </div>
            
            <div class="materi-card">
                <span class="materi-icon">📐</span>
                <h3>Persamaan Gas Ideal</h3>
                <div class="formula">PV = nRT</div>
                <p><strong>R = 8,31 J/mol·K</strong></p>
                <ul style="margin-top: 1rem; padding-left: 1.5rem;">
                    <li>P → Tekanan (Pa)</li>
                    <li>V → Volume (m³)</li>
                    <li>T → Suhu (K)</li>
                </ul>
            </div>
            
            <div class="materi-card">
                <span class="materi-icon">🔄</span>
                <h3>Proses Isotermal</h3>
                <p><strong>Suhu TETAP (ΔT = 0)</strong></p>
                <div class="formula">P₁V₁ = P₂V₂</div>
                <p><strong>Grafik:</strong> Hiperbola PV = konstan</p>
            </div>
            
            <div class="materi-card">
                <span class="materi-icon">📏</span>
                <h3>Proses Isobarik</h3>
                <p><strong>Tekanan TETAP (ΔP = 0)</strong></p>
                <div class="formula">V/T = konstan</div>
                <p><strong>Grafik:</strong> Garis lurus V-T</p>
            </div>
            
            <div class="materi-card">
                <span class="materi-icon">📦</span>
                <h3>Proses Isokhorik</h3>
                <p><strong>Volume TETAP (ΔV = 0)</strong></p>
                <div class="formula">P/T = konstan</div>
                <p><strong>Grafik:</strong> Garis lurus P-T</p>
            </div>
        </div>
        
        <div style="text-align: center;">
            <button class="cta-button" onclick="nextPage()">➡️ Mulai Simulasi</button>
        </div>
    </div>

    <!-- Page 3: Simulasi -->
    <div id="page3" class="page-container simulasi-container">
        <div class="simulasi-header">
            <h2 class="section-title">🎮 Simulasi Proses Termodinamika</h2>
            <div class="process-selector" id="processSelector">
                <button class="process-btn active" data-process="free">Gerak Bebas</button>
                <button class="process-btn" data-process="isothermal">Isotermal</button>
                <button class="process-btn" data-process="isobaric">Isobarik</button>
                <button class="process-btn" data-process="isochoric">Isokhorik</button>
            </div>
        </div>

        <div class="canvas-container">
            <canvas id="gasCanvas" width="1000" height="500"></canvas>
        </div>

        <div class="monitoring-panel">
            <div class="monitoring-grid" id="monitoringGrid">
                <!-- Diisi oleh JavaScript -->
            </div>
        </div>

        <div style="text-align: center; margin-top: 2rem;">
            <button class="cta-button" onclick="nextPage()" style="padding: 1rem 2rem;">📝 Kerjakan LKPD</button>
        </div>
    </div>

    <!-- Page 4: LKPD -->
    <div id="page4" class="page-container">
        <div class="lkpd-container">
            <h2 style="text-align: center; margin-bottom: 2rem;">📝 Lembar Kerja Prakt
