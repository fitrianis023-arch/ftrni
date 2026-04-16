<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoLab | Gas Ideal & Termodinamika</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', 'Poppins', system-ui, sans-serif;
            background: linear-gradient(145deg, #0a0f1e 0%, #0b1120 100%);
            min-height: 100vh;
            padding: 20px;
        }

        /* Container Utama */
        .app-container {
            max-width: 1400px;
            margin: 0 auto;
        }

        /* Header */
        .header {
            text-align: center;
            margin-bottom: 1.5rem;
        }

        .header h1 {
            font-size: 2rem;
            background: linear-gradient(135deg, #fbbf24, #38bdf8);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
        }

        .header p {
            color: #94a3b8;
            font-size: 0.85rem;
        }

        /* Navigasi Tombol */
        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 1rem;
            margin-bottom: 2rem;
            flex-wrap: wrap;
        }

        .nav-btn {
            background: #1e293b;
            border: none;
            padding: 12px 28px;
            border-radius: 60px;
            font-size: 1rem;
            font-weight: 600;
            color: #cbd5e1;
            cursor: pointer;
            transition: all 0.2s;
            border: 1px solid #334155;
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #f59e0b, #3b82f6);
            color: white;
            border-color: #fbbf24;
            box-shadow: 0 0 15px rgba(59,130,246,0.3);
        }

        .nav-btn:hover {
            transform: translateY(-2px);
            background: #334155;
        }

        /* Halaman */
        .page {
            display: none;
            animation: fadeIn 0.4s ease;
        }

        .page.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(15px); }
            to { opacity: 1; transform: translateY(0); }
        }

        /* ========== HALAMAN MATERI ========== */
        .materi-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
            gap: 1.5rem;
        }

        .materi-card {
            background: rgba(15, 23, 42, 0.9);
            backdrop-filter: blur(10px);
            border-radius: 1.5rem;
            padding: 1.5rem;
            border-left: 5px solid #f59e0b;
            transition: transform 0.2s;
        }

        .materi-card:hover {
            transform: translateY(-5px);
            background: #1e293b;
        }

        .materi-card h3 {
            font-size: 1.4rem;
            color: #fbbf24;
            margin-bottom: 1rem;
        }

        .materi-card p {
            color: #cbd5e6;
            line-height: 1.6;
            font-size: 0.9rem;
        }

        .rumus-box {
            background: #020617;
            padding: 0.8rem;
            border-radius: 1rem;
            margin: 1rem 0;
            text-align: center;
            font-family: monospace;
            font-size: 1.1rem;
            color: #facc15;
        }

        /* ========== HALAMAN SIMULASI ========== */
        .simulasi-wrapper {
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(10px);
            border-radius: 2rem;
            padding: 1.5rem;
            border: 1px solid #facc1544;
        }

        .sim-area {
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
        }

        .canvas-box {
            flex: 3;
            min-width: 550px;
        }

        .control-box {
            flex: 1.3;
            min-width: 260px;
            background: #0f172a;
            border-radius: 1.5rem;
            padding: 1.2rem;
        }

        canvas {
            display: block;
            width: 100%;
            background: #030712;
            border-radius: 1.2rem;
            border: 3px solid #facc15;
            cursor: grab;
        }

        canvas:active {
            cursor: grabbing;
        }

        /* Info Panel Simulasi */
        .sim-info {
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            margin-top: 1rem;
            text-align: center;
        }

        .info-item {
            background: #020617;
            border-radius: 1rem;
            padding: 8px;
        }

        .info-item .label {
            font-size: 0.7rem;
            color: #94a3b8;
        }

        .info-item .value {
            font-size: 1.3rem;
            font-weight: bold;
            color: #facc15;
        }

        .info-item .note {
            font-size: 0.6rem;
            color: #22c55e;
        }

        /* Tombol Tekanan (hanya display, tidak bisa dimanipulasi) */
        .pressure-button {
            background: #1e293b;
            border: 2px solid #22c55e;
            border-radius: 50px;
            padding: 12px 20px;
            text-align: center;
            margin-bottom: 1rem;
        }

        .pressure-button .label {
            font-size: 0.7rem;
            color: #94a3b8;
        }

        .pressure-button .value {
            font-size: 2rem;
            font-weight: bold;
            color: #22c55e;
        }

        .pressure-button .fixed {
            font-size: 0.6rem;
            color: #4ade80;
        }

        /* Kontrol Slider */
        .control-group {
            margin-bottom: 1.2rem;
        }

        .control-group label {
            display: flex;
            justify-content: space-between;
            color: #cbd5e1;
            margin-bottom: 5px;
        }

        input[type="range"] {
            width: 100%;
            height: 5px;
            border-radius: 10px;
            background: linear-gradient(90deg, #3b82f6, #facc15);
            -webkit-appearance: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            width: 16px;
            height: 16px;
            background: #facc15;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
        }

        .value-badge {
            background: #1e293b;
            padding: 2px 10px;
            border-radius: 20px;
            font-size: 0.8rem;
            color: #facc15;
        }

        .button-row {
            display: flex;
            gap: 10px;
            margin: 10px 0;
        }

        button {
            background: #1e293b;
            border: none;
            padding: 8px 16px;
            border-radius: 40px;
            font-weight: 600;
            color: white;
            cursor: pointer;
            border: 1px solid #475569;
        }

        button:active {
            transform: scale(0.96);
        }

        .btn-reset {
            background: #334155;
            width: 100%;
            margin-top: 10px;
        }

        /* ========== HALAMAN LKPD ========== */
        .lkpd-container {
            background: rgba(15, 23, 42, 0.9);
            border-radius: 1.5rem;
            padding: 1.5rem;
        }

        .soal-item {
            background: #1e293b;
            margin: 1rem 0;
            padding: 1rem;
            border-radius: 1rem;
        }

        .soal-item p {
            font-weight: 600;
            margin-bottom: 0.8rem;
        }

        input, textarea {
            width: 100%;
            padding: 10px;
            border-radius: 1rem;
            border: none;
            background: #0f172a;
            color: white;
            border: 1px solid #facc15;
        }

        .submit-btn {
            background: #22c55e;
            color: #0f172a;
            font-weight: bold;
            padding: 12px 24px;
            margin-top: 1rem;
            width: 100%;
        }

        .monitor-panel {
            margin-top: 2rem;
            background: #020617;
            padding: 1rem;
            border-radius: 1rem;
            border-left: 4px solid #38bdf8;
        }

        .jawaban-log {
            max-height: 300px;
            overflow-y: auto;
            font-size: 0.8rem;
        }

        .log-entry {
            background: #1e293b;
            margin: 8px 0;
            padding: 8px;
            border-radius: 8px;
        }

        hr {
            margin: 1rem 0;
            border-color: #334155;
        }

        @media (max-width: 900px) {
            .canvas-box { min-width: 100%; }
            .sim-area { flex-direction: column; }
            .sim-info { grid-template-columns: repeat(2, 1fr); }
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="header">
        <h1>🔥 TERMODINAMIKA LAB</h1>
        <p>Simulasi Gas Ideal | Tekanan Tetap (Isobarik) | LKPD Interaktif</p>
    </div>

    <!-- Tombol Navigasi -->
    <div class="nav-buttons">
        <button class="nav-btn active" data-page="materi">📘 Materi Termodinamika</button>
        <button class="nav-btn" data-page="simulasi">⚡ Simulasi Gas Ideal</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD Online</button>
    </div>

    <!-- ==================== HALAMAN 1: MATERI ==================== -->
    <div id="page-materi" class="page active">
        <div class="materi-grid">
            <div class="materi-card">
                <h3>🎈 Gas Ideal</h3>
                <p>Gas ideal adalah gas teoritis yang mengikuti hukum gas ideal. Partikel bergerak acak, tidak ada gaya antar partikel kecuali tumbukan elastis.</p>
                <div class="rumus-box">PV = nRT</div>
                <p>P = Tekanan (atm), V = Volume (L), n = jumlah mol, R = konstanta (0.0821), T = Suhu (K)</p>
            </div>
            <div class="materi-card">
                <h3>📐 Hukum Boyle (Isotermal)</h3>
                <p>Suhu tetap → Tekanan berbanding terbalik dengan volume.</p>
                <div class="rumus-box">P₁V₁ = P₂V₂</div>
            </div>
            <div class="materi-card">
                <h3>⚖️ Hukum Charles (Isobarik)</h3>
                <p>Tekanan tetap → Volume sebanding dengan suhu mutlak.</p>
                <div class="rumus-box">V₁/T₁ = V₂/T₂</div>
            </div>
            <div class="materi-card">
                <h3>📌 Hukum Gay-Lussac (Isohorik)</h3>
                <p>Volume tetap → Tekanan sebanding dengan suhu.</p>
                <div class="rumus-box">P₁/T₁ = P₂/T₂</div>
            </div>
            <div class="materi-card">
                <h3>🧪 Hukum I Termodinamika</h3>
                <p>Energi tidak dapat diciptakan atau dimusnahkan.</p>
                <div class="rumus-box">ΔU = Q - W</div>
                <p>ΔU = perubahan energi dalam, Q = kalor, W = usaha</p>
            </div>
            <div class="materi-card">
                <h3>🎯 Teori Kinetik Gas</h3>
                <p>Energi kinetik rata-rata partikel sebanding dengan suhu mutlak.</p>
                <div class="rumus-box">EK = (3/2)kT</div>
                <p>Semakin tinggi suhu, partikel bergerak semakin cepat.</p>
            </div>
        </div>
    </div>

    <!-- ==================== HALAMAN 2: SIMULASI ==================== -->
    <div id="page-simulasi" class="page">
        <div class="simulasi-wrapper">
            <div class="sim-area">
                <div class="canvas-box">
                    <canvas id="gasCanvas" width="950" height="450" style="width:100%; height:auto"></canvas>
                    <div class="sim-info">
                        <div class="info-item">
                            <div class="label">📊 TEKANAN</div>
                            <div class="value" id="pressureDisplay">1.00</div>
                            <div class="note">🔒 TETAP (Isobarik)</div>
                        </div>
                        <div class="info-item">
                            <div class="label">🌡️ SUHU</div>
                            <div class="value" id="tempDisplay">300</div>
                            <div class="note">BERUBAH</div>
                        </div>
                        <div class="info-item">
                            <div class="label">📦 VOLUME</div>
                            <div class="value" id="volDisplay">5.0</div>
                            <div class="note">OTOMATIS</div>
                        </div>
                        <div class="info-item">
                            <div class="label">🔢 PARTIKEL</div>
                            <div class="value" id="partikelDisplay">24</div>
                            <div class="note">BERUBAH</div>
                        </div>
                    </div>
                    <div style="font-size:0.7rem; text-align:center; margin-top:8px; color:#6b7280;">
                        ✨ Geser mouse pada partikel untuk mendorong | Tekanan TETAP 1 atm (proses Isobarik)
                    </div>
                </div>

                <div class="control-box">
                    <!-- Tombol Tekanan (HANYA DISPLAY, TIDAK BISA DI MANIPULASI) -->
                    <div class="pressure-button">
                        <div class="label">🎛️ TEKANAN (TETAP)</div>
                        <div class="value" id="pressureFixed">1.00 atm</div>
                        <div class="fixed">🔒 Tekanan tidak bisa diubah (Isobarik)</div>
                    </div>

                    <!-- Slider Suhu -->
                    <div class="control-group">
                        <label>🌡️ SUHU (Kelvin) <span id="tempLabel" class="value-badge">300 K</span></label>
                        <input type="range" id="suhuSlider" min="100" max="800" value="300" step="1">
                        <div style="font-size:0.65rem; color:#22c55e;">* Geser untuk mengubah suhu → volume otomatis berubah</div>
                    </div>

                    <!-- Slider Volume (Disabled karena otomatis) -->
                    <div class="control-group">
                        <label>📦 VOLUME (Liter) <span id="volLabel" class="value-badge">5.0 L</span></label>
                        <input type="range" id="volumeSlider" min="1.5" max="12" value="5.0" step="0.1" disabled style="opacity:0.6">
                        <div style="font-size:0.65rem; color:#94a3b8;">* Volume mengikuti suhu (V₁/T₁ = V₂/T₂)</div>
                    </div>

                    <!-- Jumlah Partikel -->
                    <div class="control-group">
                        <label>🔢 JUMLAH PARTIKEL <span id="partikelLabel" class="value-badge">24</span></label>
                        <div class="button-row">
                            <button id="kurangPartikel" style="background:#ef4444;">➖ Kurangi (2)</button>
                            <button id="tambahPartikel" style="background:#22c55e;">➕ Tambah (2)</button>
                        </div>
                        <input type="range" id="partikelSlider" min="6" max="55" value="24" step="1">
                    </div>

                    <button id="resetSimBtn" class="btn-reset">⟳ Reset ke Kondisi Awal</button>

                    <hr>
                    <div style="font-size:0.7rem; color:#94a3b8; text-align:center;">
                        💡 <strong>Proses Isobarik</strong>: Tekanan tetap (1 atm).<br>
                        Saat suhu naik, volume membesar. Saat partikel ditambah, volume membesar.
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- ==================== HALAMAN 3: LKPD ==================== -->
    <div id="page-lkpd" class="page">
        <div class="lkpd-container">
            <h2 style="color:#facc15; margin-bottom:1rem;">📋 Lembar Kerja Peserta Didik (LKPD)</h2>
            <p><strong>Nama Siswa:</strong> <input type="text" id="studentName" placeholder="Masukkan nama lengkap" style="width:100%;"></p>
            
            <div id="soalList">
                <div class="soal-item">
                    <p>1️⃣ Jelaskan bunyi Hukum Charles (Isobarik) dan tuliskan rumusnya!</p>
                    <textarea id="jawab1" rows="2" placeholder="Jawaban Anda..."></textarea>
                </div>
                <div class="soal-item">
                    <p>2️⃣ Dalam simulasi gas ideal ini, tekanan dibuat tetap (1 atm). Jika suhu dinaikkan, apa yang terjadi pada volume gas? Mengapa?</p>
                    <textarea id="jawab2" rows="2" placeholder="Jawaban Anda..."></textarea>
                </div>
                <div class="soal-item">
                    <p>3️⃣ Tuliskan persamaan gas ideal (PV = nRT) dan jelaskan arti setiap simbolnya!</p>
                    <textarea id="jawab3" rows="2" placeholder="Jawaban Anda..."></textarea>
                </div>
                <div class="soal-item">
                    <p>4️⃣ Berdasarkan teori kinetik gas, bagaimana hubungan antara suhu dan kecepatan gerak partikel?</p>
                    <textarea id="jawab4" rows="2" placeholder="Jawaban Anda..."></textarea>
                </div>
                <div class="soal-item">
                    <p>5️⃣ Bunyi Hukum I Termodinamika adalah ΔU = Q - W. Jelaskan apa yang dimaksud dengan ΔU, Q, dan W!</p>
                    <textarea id="jawab5" rows="2" placeholder="Jawaban Anda..."></textarea>
                </div>
            </div>
            
            <button id="submitLkpd" class="submit-btn">✅ Kirim Jawaban (Live ke Guru)</button>
            
            <!-- Panel Monitoring untuk Pemilik/Guru -->
            <div class="monitor-panel">
                <h3 style="color:#38bdf8;">📡 Monitoring Jawaban Siswa (Real-time)</h3>
                <p style="font-size:0.75rem; color:#94a3b8;">Data masuk akan muncul di sini saat siswa mengerjakan LKPD</p>
                <div id="jawabanLog" class="jawaban-log">
                    <em>Belum ada kiriman...</em>
                </div>
                <button id="clearLogBtn" style="margin-top:10px; background:#334155;">🧹 Bersihkan Log</button>
            </div>
        </div>
    </div>
</div>

<script>
    // ==================== SIMULASI GAS IDEAL (TEKANAN TETAP) ====================
    const canvas = document.getElementById('gasCanvas');
    const ctx = canvas.getContext('2d');
    let W = 950, H = 450;
    canvas.width = W; canvas.height = H;

    // Variabel gas
    let jumlahPartikel = 24;
    let suhu = 300;           // Kelvin
    let volume = 5.0;         // Liter
    const TEKANAN_TETAP = 1.0; // atm (TIDAK BISA DIUBAH)
    const R_SEMU = 0.0821;

    // Partikel
    let particles = [];
    const RADIUS = 6;
    const REF_SPEED_300K = 3.6;

    // ========== FUNGSI VOLUME DARI TEKANAN TETAP ==========
    // PV = nRT → V = nRT/P
    function hitungVolume() {
        let n = jumlahPartikel / 20.0;
        let V = (n * R_SEMU * suhu) / TEKANAN_TETAP;
        V = Math.min(12.0, Math.max(1.5, V));
        volume = parseFloat(V.toFixed(2));
        return volume;
    }

    function updateVolumeOtomatis() {
        hitungVolume();
        document.getElementById('volDisplay').innerHTML = volume.toFixed(1);
        document.getElementById('volLabel').innerHTML = volume.toFixed(1) + " L";
        document.getElementById('volumeSlider').value = volume;
        
        // Animasi perubahan volume
        const volElem = document.getElementById('volDisplay');
        volElem.classList.add('changing');
        setTimeout(() => volElem.classList.remove('changing'), 300);
    }

    // Update kecepatan partikel dari suhu
    function updateKecepatan() {
        let faktor = Math.sqrt(suhu / 300);
        if(suhu <= 0) faktor = 0;
        for(let p of particles) {
            let speed = Math.hypot(p.vx, p.vy);
            if(speed < 0.05 && faktor > 0) {
                let angle = Math.random() * 2 * Math.PI;
                let newSpd = REF_SPEED_300K * faktor * (0.65 + Math.random()*0.7);
                p.vx = Math.cos(angle) * newSpd;
                p.vy = Math.sin(angle) * newSpd;
            } else if(speed > 0.01) {
                let targetSpd = REF_SPEED_300K * faktor;
                let scale = targetSpd / speed;
                p.vx *= scale;
                p.vy *= scale;
            }
        }
    }

    // Sinkronisasi UI
    function sinkronUI() {
        document.getElementById('pressureDisplay').innerHTML = TEKANAN_TETAP.toFixed(2);
        document.getElementById('pressureFixed').innerHTML = TEKANAN_TETAP.toFixed(2) + " atm";
        document.getElementById('tempDisplay').innerHTML = Math.round(suhu);
        document.getElementById('volDisplay').innerHTML = volume.toFixed(1);
        document.getElementById('partikelDisplay').innerHTML = jumlahPartikel;
        document.getElementById('suhuSlider').value = suhu;
        document.getElementById('partikelSlider').value = jumlahPartikel;
        document.getElementById('tempLabel').innerHTML = Math.round(suhu) + " K";
        document.getElementById('volLabel').innerHTML = volume.toFixed(1) + " L";
        document.getElementById('partikelLabel').innerHTML = jumlahPartikel;
    }

    // Inisialisasi partikel
    function initParticles(count, suhuAwal) {
        let parts = [];
        let faktor = Math.sqrt(suhuAwal / 300);
        for(let i=0; i<count; i++) {
            let x = Math.random() * (W - 2*RADIUS) + RADIUS;
            let y = Math.random() * (H - 2*RADIUS) + RADIUS;
            let angle = Math.random() * 2 * Math.PI;
            let speed = REF_SPEED_300K * faktor * (0.6 + Math.random() * 0.8);
            parts.push({x, y, vx: Math.cos(angle)*speed, vy: Math.sin(angle)*speed, r: RADIUS});
        }
        return parts;
    }

    // Ubah suhu
    function ubahSuhu(newSuhu) {
        suhu = Math.min(800, Math.max(100, newSuhu));
        updateVolumeOtomatis();
        updateKecepatan();
        sinkronUI();
        
        // Animasi suhu
        const tempElem = document.getElementById('tempDisplay');
        tempElem.classList.add('changing');
        setTimeout(() => tempElem.classList.remove('changing'), 300);
    }

    // Ubah jumlah partikel
    function ubahJumlahPartikel(newCount) {
        newCount = Math.min(55, Math.max(6, newCount));
        let diff = newCount - particles.length;
        if(diff > 0) {
            let faktor = Math.sqrt(suhu / 300);
            for(let i=0; i<diff; i++) {
                let x = Math.random() * (W - 2*RADIUS) + RADIUS;
                let y = Math.random() * (H - 2*RADIUS) + RADIUS;
                let angle = Math.random() * 2 * Math.PI;
                let spd = REF_SPEED_300K * faktor * (0.6 + Math.random()*0.7);
                particles.push({x, y, vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: RADIUS});
            }
        } else if(diff < 0) {
            particles.splice(newCount, -diff);
        }
        jumlahPartikel = particles.length;
        updateVolumeOtomatis();
        sinkronUI();
        
        const partElem = document.getElementById('partikelDisplay');
        partElem.classList.add('changing');
        setTimeout(() => partElem.classList.remove('changing'), 300);
    }

    // Fisika partikel
    function updateFisika() {
        for(let p of particles) {
            p.x += p.vx;
            p.y += p.vy;
            if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
            if(p.x + p.r > W) { p.x = W - p.r; p.vx = -p.vx; }
            if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
            if(p.y + p.r > H) { p.y = H - p.r; p.vy = -p.vy; }
        }
        for(let i=0; i<particles.length; i++) {
            for(let j=i+1; j<particles.length; j++) {
                let p1=particles[i], p2=particles[j];
                let dx = p2.x-p1.x, dy = p2.y-p1.y;
                let dist = Math.hypot(dx,dy);
                let minDist = p1.r+p2.r;
                if(dist < minDist) {
                    let nx = dx/dist, ny = dy/dist;
                    let vrelx = p2.vx-p1.vx, vrely = p2.vy-p1.vy;
                    let velAlong = vrelx*nx + vrely*ny;
                    if(velAlong < 0) {
                        let impulse = 2 * velAlong / 2;
                        p1.vx += impulse * nx; p1.vy += impulse * ny;
                        p2.vx -= impulse * nx; p2.vy -= impulse * ny;
                    }
                    let overlap = minDist - dist;
                    let shiftX = nx * overlap * 0.5;
                    let shiftY = ny * overlap * 0.5;
                    p1.x -= shiftX; p1.y -= shiftY;
                    p2.x += shiftX; p2.y += shiftY;
                }
            }
        }
    }

    // Gambar
    function draw() {
        ctx.clearRect(0, 0, W, H);
        ctx.strokeStyle = "#facc15";
        ctx.lineWidth = 3;
        ctx.strokeRect(5, 5, W-10, H-10);
        
        for(let p of particles) {
            let grad = ctx.createRadialGradient(p.x-2, p.y-2, 2, p.x, p.y, p.r);
            grad.addColorStop(0, "#ffd966");
            grad.addColorStop(1, "#f59e0b");
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.r-1, 0, 2*Math.PI);
            ctx.fillStyle = grad;
            ctx.fill();
            ctx.strokeStyle = "white";
            ctx.lineWidth = 1;
            ctx.stroke();
        }
        
        let avgSpd = particles.reduce((a,b)=> a+Math.hypot(b.vx,b.vy),0)/(particles.length||1);
        ctx.font = "bold 10px monospace";
        ctx.fillStyle = "#facc15";
        ctx.fillText(`Kecepatan rata-rata: ${avgSpd.toFixed(1)} px/f`, 12, 25);
        ctx.fillStyle = "#4ade80";
        ctx.fillText(`Tekanan tetap: ${TEKANAN_TETAP} atm (Isobarik)`, 12, 45);
    }

    // Animasi
    function animate() {
        updateFisika();
        draw();
        requestAnimationFrame(animate);
    }

    // Drag interaksi
    let isDragging = false, lastX=0, lastY=0;
    function setupDrag() {
        canvas.addEventListener('mousedown', (e) => {
            const rect = canvas.getBoundingClientRect();
            const sx = canvas.width/rect.width, sy = canvas.height/rect.height;
            lastX = (e.clientX - rect.left)*sx;
            lastY = (e.clientY - rect.top)*sy;
            isDragging = true;
            e.preventDefault();
        });
        window.addEventListener('mousemove', (e) => {
            if(!isDragging) return;
            const rect = canvas.getBoundingClientRect();
            const sx = canvas.width/rect.width, sy = canvas.height/rect.height;
            let mx = (e.clientX - rect.left)*sx;
            let my = (e.clientY - rect.top)*sy;
            let dx = mx - lastX, dy = my - lastY;
            for(let p of particles) {
                let dist = Math.hypot(p.x - mx, p.y - my);
                if(dist < 70) {
                    let force = 0.5 * (1 - dist/70);
                    p.vx += dx * force;
                    p.vy += dy * force;
                }
            }
            lastX = mx; lastY = my;
        });
        window.addEventListener('mouseup', () => isDragging = false);
    }

    // Event listeners simulasi
    function initSimEvents() {
        document.getElementById('suhuSlider').addEventListener('input', (e) => {
            ubahSuhu(parseInt(e.target.value));
        });
        document.getElementById('partikelSlider').addEventListener('input', (e) => {
            ubahJumlahPartikel(parseInt(e.target.value));
        });
        document.getElementById('tambahPartikel').addEventListener('click', () => {
            ubahJumlahPartikel(jumlahPartikel + 2);
        });
        document.getElementById('kurangPartikel').addEventListener('click', () => {
            ubahJumlahPartikel(jumlahPartikel - 2);
        });
        document.getElementById('resetSimBtn').addEventListener('click', () => {
            suhu = 300;
            jumlahPartikel = 24;
            particles = initParticles(jumlahPartikel, suhu);
            updateVolumeOtomatis();
            updateKecepatan();
            sinkronUI();
            document.getElementById('suhuSlider').value = 300;
            document.getElementById('partikelSlider').value = 24;
        });
    }

    // ==================== LKPD + MONITORING LIVE ====================
    let jawabanHistory = [];

    function renderLog() {
        const logDiv = document.getElementById('jawabanLog');
        if(jawabanHistory.length === 0) {
            logDiv.innerHTML = '<em>Belum ada kiriman...</em>';
            return;
        }
        logDiv.innerHTML = jawabanHistory.map(j => `
            <div class="log-entry">
                <strong>👤 ${j.nama || 'Anonim'}</strong> <small>${new Date(j.waktu).toLocaleString()}</small><br/>
                <strong>1.</strong> ${j.j1.substring(0,100)}${j.j1.length>100?'...':''}<br/>
                <strong>2.</strong> ${j.j2.substring(0,100)}${j.j2.length>100?'...':''}<br/>
                <strong>3.</strong> ${j.j3.substring(0,100)}${j.j3.length>100?'...':''}<br/>
                <strong>4.</strong> ${j.j4.substring(0,100)}${j.j4.length>100?'...':''}<br/>
                <strong>5.</strong> ${j.j5.substring(0,100)}${j.j5.length>100?'...':''}
            </div>
        `).join('');
    }

    document.getElementById('submitLkpd').addEventListener('click', () => {
        let nama = document.getElementById('studentName').value.trim();
        if(nama === "") nama = "Siswa";
        let j1 = document.getElementById('jawab1').value;
        let j2 = document.getElementById('jawab2').value;
        let j3 = document.getElementById('jawab3').value;
        let j4 = document.getElementById('jawab4').value;
        let j5 = document.getElementById('jawab5').value;
        
        if(!j1 && !j2 && !j3 && !j4 && !j5) {
            alert("Silakan isi minimal satu jawaban!");
            return;
        }
        
        jawabanHistory.unshift({
            nama: nama,
            j1, j2, j3, j4, j5,
            waktu: new Date().toISOString()
        });
        renderLog();
        alert("
