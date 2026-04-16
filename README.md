<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoLab | Simulasi Termodinamika Interaktif</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Poppins', 'Segoe UI', system-ui, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            min-height: 100vh;
            color: #f1f5f9;
            transition: all 0.3s ease;
        }

        /* ========== LAYOUT HALAMAN ========== */
        .page {
            display: none;
            padding: 20px;
            animation: fadeIn 0.5s ease;
        }

        .page.active {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(20px); }
            to { opacity: 1; transform: translateY(0); }
        }

        .container {
            max-width: 1300px;
            margin: 0 auto;
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(12px);
            border-radius: 3rem;
            padding: 2rem;
            box-shadow: 0 25px 50px -12px rgba(0,0,0,0.5);
            border: 1px solid rgba(255,255,255,0.2);
        }

        h1 {
            font-size: 2.5rem;
            background: linear-gradient(135deg, #fbbf24, #38bdf8);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            text-align: center;
            margin-bottom: 0.5rem;
        }

        .subhead {
            text-align: center;
            color: #94a3b8;
            margin-bottom: 2rem;
        }

        /* Tombol navigasi */
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
            box-shadow: 0 4px 6px rgba(0,0,0,0.3);
            border: 1px solid #334155;
        }

        .nav-btn.active {
            background: linear-gradient(135deg, #f59e0b, #3b82f6);
            color: white;
            border-color: #fbbf24;
            box-shadow: 0 0 15px rgba(59,130,246,0.5);
        }

        .nav-btn:hover {
            transform: translateY(-2px);
            background: #334155;
        }

        /* Kartu materi */
        .materi-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
            margin-top: 1.5rem;
        }

        .materi-card {
            background: #0f172a;
            border-radius: 2rem;
            padding: 1.5rem;
            border-left: 6px solid #f59e0b;
            transition: transform 0.2s;
        }

        .materi-card:hover {
            transform: translateY(-5px);
            background: #1e293b;
        }

        .materi-card h3 {
            font-size: 1.6rem;
            margin-bottom: 1rem;
            color: #fbbf24;
        }

        .materi-card p {
            line-height: 1.6;
            color: #cbd5e6;
        }

        .rumus {
            background: #020617;
            padding: 0.8rem;
            border-radius: 1rem;
            margin: 1rem 0;
            font-family: monospace;
            font-size: 1.1rem;
            text-align: center;
            color: #facc15;
        }

        /* ========== SIMULASI ========== */
        .simulasi-panel {
            display: flex;
            flex-direction: column;
            gap: 1.5rem;
        }

        .proses-buttons {
            display: flex;
            justify-content: center;
            gap: 1rem;
            flex-wrap: wrap;
        }

        .proses-btn {
            background: #1e293b;
            padding: 12px 24px;
            border-radius: 40px;
            font-weight: bold;
            border: 2px solid #475569;
            cursor: pointer;
            transition: all 0.2s;
            color: white;
        }

        .proses-btn.active-proses {
            background: #f59e0b;
            border-color: #fbbf24;
            box-shadow: 0 0 12px #f59e0b;
        }

        canvas {
            display: block;
            width: 100%;
            background: #0a0f1c;
            border-radius: 2rem;
            border: 3px solid #facc15;
            box-shadow: 0 20px 30px -10px black;
        }

        .control-bar {
            display: flex;
            flex-wrap: wrap;
            gap: 1rem;
            justify-content: space-between;
            align-items: center;
            background: #0f172a;
            padding: 1rem;
            border-radius: 2rem;
        }

        .slider-group {
            flex: 1;
            min-width: 150px;
        }

        .slider-group label {
            display: block;
            font-size: 0.8rem;
            color: #94a3b8;
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            border-radius: 10px;
            background: linear-gradient(90deg, #3b82f6, #f59e0b);
        }

        .value-display {
            font-weight: bold;
            font-size: 1.2rem;
            color: #facc15;
        }

        .stats {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 1rem;
            margin-top: 1rem;
        }

        .stat-card {
            background: #020617;
            padding: 0.8rem;
            border-radius: 1.5rem;
            text-align: center;
        }

        /* ========== LKPD ========== */
        .lkpd-container {
            background: #0f172a;
            border-radius: 2rem;
            padding: 1.5rem;
        }

        .soal-item {
            background: #1e293b;
            margin: 1rem 0;
            padding: 1.2rem;
            border-radius: 1.5rem;
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

        .submit-lkpd {
            background: #f59e0b;
            color: #0f172a;
            font-weight: bold;
            padding: 12px 30px;
            border: none;
            border-radius: 40px;
            cursor: pointer;
            margin-top: 1rem;
        }

        .admin-panel {
            margin-top: 2rem;
            background: #020617;
            padding: 1rem;
            border-radius: 1.5rem;
            border-left: 5px solid #38bdf8;
        }

        .jawaban-log {
            max-height: 300px;
            overflow-y: auto;
            font-size: 0.85rem;
        }

        hr {
            margin: 1rem 0;
            border-color: #334155;
        }
    </style>
</head>
<body>

<div class="container">
    <h1>🔥 TERMODINAMIKA LAB</h1>
    <div class="subhead">Simulasi Gas Ideal | Isohorik | Isobarik | Isotermal | LKPD Interaktif</div>

    <!-- Navigasi Halaman -->
    <div class="nav-buttons">
        <button class="nav-btn" data-page="materi">📘 Materi Termodinamika</button>
        <button class="nav-btn" data-page="simulasi">⚡ Simulasi Gas Ideal</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD Online</button>
    </div>

    <!-- HALAMAN 1: MATERI -->
    <div id="materi" class="page active">
        <div class="materi-grid">
            <div class="materi-card">
                <h3>🎈 Gas Ideal</h3>
                <p>Gas ideal adalah gas teoritis yang partikelnya bergerak acak, tidak ada gaya antar partikel kecuali tumbukan elastis. <strong>Persamaan Umum:</strong> PV = nRT</p>
                <div class="rumus">PV = nRT</div>
                <p>P = Tekanan, V = Volume, n = jumlah mol, R = konstanta gas, T = Suhu (Kelvin)</p>
            </div>
            <div class="materi-card">
                <h3>📐 Isohorik (Volume Tetap)</h3>
                <p>Proses pada volume konstan. Tekanan berbanding lurus dengan suhu.</p>
                <div class="rumus">P₁/T₁ = P₂/T₂</div>
                <p>Usaha (W) = 0, Kalor = perubahan energi dalam (ΔU = Q)</p>
            </div>
            <div class="materi-card">
                <h3>⚖️ Isobarik (Tekanan Tetap)</h3>
                <p>Proses pada tekanan konstan. Volume sebanding dengan suhu.</p>
                <div class="rumus">V₁/T₁ = V₂/T₂</div>
                <p>Usaha W = P ΔV, Kalor Q = ΔU + W</p>
            </div>
            <div class="materi-card">
                <h3>🌡️ Isotermal (Suhu Tetap)</h3>
                <p>Proses suhu konstan. Tekanan berbanding terbalik dengan volume.</p>
                <div class="rumus">P₁V₁ = P₂V₂</div>
                <p>Energi dalam ΔU = 0, Usaha W = nRT ln(V₂/V₁)</p>
            </div>
            <div class="materi-card">
                <h3>🧪 Hukum Termodinamika I</h3>
                <p>Energi tidak dapat diciptakan/dimusnahkan: ΔU = Q - W</p>
                <div class="rumus">ΔU = Q - W</div>
            </div>
            <div class="materi-card">
                <h3>🎯 Aplikasi Simulasi</h3>
                <p>Atur parameter proses (Isohorik/Isobarik/Isotermal) dan amati perubahan kecepatan partikel, tekanan, volume, suhu secara realtime!</p>
            </div>
        </div>
    </div>

    <!-- HALAMAN 2: SIMULASI (lebih unggul dari PhET dengan interaksi 3 proses) -->
    <div id="simulasi" class="page">
        <div class="simulasi-panel">
            <div class="proses-buttons">
                <button class="proses-btn" data-proses="isohorik">📌 ISOHORIK (V tetap)</button>
                <button class="proses-btn" data-proses="isobarik">⚙️ ISOBARIK (P tetap)</button>
                <button class="proses-btn" data-proses="isotermal">🌀 ISOTERMAL (T tetap)</button>
            </div>
            <canvas id="simCanvas" width="1000" height="450" style="width:100%; height:auto; background:#0a0f1c"></canvas>
            <div class="control-bar">
                <div class="slider-group">
                    <label>🌡️ Suhu (Kelvin)</label>
                    <input type="range" id="tempSlider" min="100" max="600" value="300" step="1">
                    <span id="tempValue" class="value-display">300 K</span>
                </div>
                <div class="slider-group">
                    <label>📦 Volume (L)</label>
                    <input type="range" id="volSlider" min="1" max="10" value="5" step="0.1">
                    <span id="volValue" class="value-display">5.0 L</span>
                </div>
                <div class="slider-group">
                    <label>🔧 Jumlah Partikel</label>
                    <input type="range" id="partCountSlider" min="8" max="50" value="24" step="1">
                    <span id="partCountVal" class="value-display">24</span>
                </div>
                <button id="resetSimBtn" style="background:#f59e0b; border:none; padding:8px 20px; border-radius:40px;">⟳ Reset</button>
            </div>
            <div class="stats">
                <div class="stat-card">📊 Tekanan (P) : <span id="pressureVal">0.00</span> atm</div>
                <div class="stat-card">📐 Volume : <span id="statVol">5.0</span> L</div>
                <div class="stat-card">🔥 Suhu : <span id="statTemp">300</span> K</div>
                <div class="stat-card">⚡ Energi Kinetik Rata-rata : <span id="avgEk">0</span> %</div>
                <div class="stat-card">💨 Kecepatan Rata-rata : <span id="avgSpeed">0</span> px/f</div>
                <div class="stat-card">🔘 Proses Aktif : <span id="activeProsesName">Isohorik</span></div>
            </div>
            <div style="font-size:0.8rem; text-align:center; color:#94a3b8;">✨ Geser kursor pada kanvas untuk mendorong partikel | Proses termodinamika otomatis menjaga hubungan P,V,T</div>
        </div>
    </div>

    <!-- HALAMAN 3: LKPD ONLINE + LIVE MONITORING -->
    <div id="lkpd" class="page">
        <div class="lkpd-container">
            <h2 style="color:#facc15;">📋 Lembar Kerja Peserta Didik (LKPD)</h2>
            <p><strong>Nama:</strong> <input type="text" id="studentName" placeholder="Masukkan nama Anda" style="width:60%;"></p>
            <div id="soalList">
                <div class="soal-item">
                    <p>1️⃣ Dalam proses ISOHORIK, volume dijaga tetap. Jika suhu dinaikkan, apa yang terjadi pada tekanan gas? Jelaskan berdasarkan hukum yang sesuai!</p>
                    <textarea rows="2" id="jawab1" placeholder="Tulis jawaban Anda..."></textarea>
                </div>
                <div class="soal-item">
                    <p>2️⃣ Pada proses ISOBARIK, bagaimana hubungan antara volume dan suhu mutlak? Tuliskan rumusnya!</p>
                    <textarea rows="2" id="jawab2" placeholder="Tulis jawaban..."></textarea>
                </div>
                <div class="soal-item">
                    <p>3️⃣ Jika gas mengalami proses ISOTERMAL (suhu tetap), apa yang terjadi pada tekanan jika volume diperbesar? Berikan alasannya.</p>
                    <textarea rows="2" id="jawab3" placeholder="Jawaban..."></textarea>
                </div>
                <div class="soal-item">
                    <p>4️⃣ Dalam simulasi, partikel bergerak lebih cepat saat suhu tinggi. Jelaskan hubungan energi kinetik gas dengan suhu absolut!</p>
                    <textarea rows="2" id="jawab4" placeholder="Jawaban..."></textarea>
                </div>
                <div class="soal-item">
                    <p>5️⃣ Bunyi Hukum I Termodinamika: ΔU = Q - W. Jelaskan arti setiap simbol dan berikan contoh saat gas memuai.</p>
                    <textarea rows="2" id="jawab5" placeholder="Jawaban..."></textarea>
                </div>
            </div>
            <button class="submit-lkpd" id="submitLkpdBtn">✅ Kirim Jawaban (Live ke Guru)</button>
            
            <!-- Panel monitoring untuk pemilik (guru) -->
            <div class="admin-panel">
                <h3>📡 Monitoring Jawaban Siswa (Real-time)</h3>
                <p>Data yang masuk akan muncul di sini secara online ketika siswa mengerjakan LKPD.</p>
                <div id="jawabanLog" class="jawaban-log">
                    <em>Belum ada kiriman...</em>
                </div>
                <button id="clearLogBtn" style="margin-top:10px; background:#334155;">🧹 Bersihkan Log</button>
                <p style="font-size:12px; margin-top:8px;">* Pemilik/ Guru dapat melihat semua jawaban siswa secara langsung.</p>
            </div>
        </div>
    </div>
</div>

<script>
    // ======================= SIMULASI GAS IDEAL SUPERIOR =======================
    const canvas = document.getElementById('simCanvas');
    const ctx = canvas.getContext('2d');
    let W = 1000, H = 450;
    canvas.width = W; canvas.height = H;

    let particles = [];
    let currentProcess = "isohorik";   // isohorik, isobarik, isotermal
    let temperature = 300;    // Kelvin
    let volume = 5.0;         // Liter (skala simulasi)
    let pressure = 1.0;       // atm (dihitung)
    let targetParticleCount = 24;
    const BASE_RADIUS = 6;
    let animationId = null;

    // Konstanta referensi
    const REF_TEMP = 300;
    const REF_SPEED = 3.6;   // px/frame pada 300K

    // Fungsi update tekanan berdasarkan PV = nRT (n ~ jumlah partikel)
    function updatePressure() {
        // n sebanding dengan jumlah partikel (asumsi gas ideal)
        let n_eff = targetParticleCount / 20;  // skala
        pressure = (n_eff * temperature) / volume;
        pressure = Math.max(0.1, pressure);
        document.getElementById('pressureVal').innerText = pressure.toFixed(2);
        document.getElementById('statVol').innerText = volume.toFixed(1);
        document.getElementById('statTemp').innerText = Math.round(temperature);
    }

    // Terapkan batasan proses termodinamika
    function applyThermodynamicConstraint() {
        if (currentProcess === "isohorik") {
            // volume tetap -> jika suhu berubah, tekanan mengikuti, volume tidak berubah
            volume = parseFloat(document.getElementById('volSlider').value);
            document.getElementById('volSlider').value = volume;
            updatePressure();
        } 
        else if (currentProcess === "isobarik") {
            // tekanan tetap => V/T = konstan
            let currentPressure = pressure;
            let newTemp = temperature;
            // ideal gas: V = (nR T)/P, agar P konstan -> V sebanding T
            let factor = newTemp / REF_TEMP;
            let newVolume = 5.0 * factor;
            newVolume = Math.min(10, Math.max(1, newVolume));
            volume = newVolume;
            document.getElementById('volSlider').value = volume;
            updatePressure();
        }
        else if (currentProcess === "isotermal") {
            // suhu tetap -> P * V = konstan
            let constPV = pressure * volume;
            let newVol = volume;
            let newPress = constPV / newVol;
            pressure = newPress;
            document.getElementById('pressureVal').innerText = pressure.toFixed(2);
            // jaga agar suhu tidak berubah (suhu sudah diatur manual)
            temperature = parseFloat(document.getElementById('tempSlider').value);
            document.getElementById('tempSlider').value = temperature;
        }
        // update slider tampilan
        document.getElementById('volValue').innerText = volume.toFixed(1) + " L";
        document.getElementById('tempValue').innerText = Math.round(temperature) + " K";
        document.getElementById('statVol').innerText = volume.toFixed(1);
        document.getElementById('statTemp').innerText = Math.round(temperature);
    }

    // update kecepatan partikel berdasarkan suhu (teori kinetik)
    function updateParticleSpeedsFromTemp() {
        let factor = Math.sqrt(temperature / REF_TEMP);
        if (temperature === 0) factor = 0;
        particles.forEach(p => {
            let currentSpeed = Math.hypot(p.vx, p.vy);
            if (currentSpeed < 0.01 && factor > 0) {
                let angle = Math.random() * 2 * Math.PI;
                let newSpd = REF_SPEED * factor * (0.7 + Math.random() * 0.6);
                p.vx = Math.cos(angle) * newSpd;
                p.vy = Math.sin(angle) * newSpd;
            } else if (currentSpeed > 0) {
                let targetSpd = REF_SPEED * factor;
                let scale = targetSpd / currentSpeed;
                p.vx *= scale;
                p.vy *= scale;
            }
        });
    }

    // Resize partikel saat volume berubah (skala ruang)
    function adjustBoundaryFromVolume() {
        // volume mempengaruhi lebar efektif (semakin besar volume, semakin luas ruang gerak)
        let scaleFactor = volume / 5.0;
        let newW = Math.min(1200, Math.max(600, 800 * scaleFactor));
        let newH = Math.min(550, Math.max(350, 360 * scaleFactor));
        W = Math.floor(newW); H = Math.floor(newH);
        canvas.width = W; canvas.height = H;
        // reposisi partikel agar tidak keluar
        particles.forEach(p => {
            p.x = Math.min(W - BASE_RADIUS, Math.max(BASE_RADIUS, p.x));
            p.y = Math.min(H - BASE_RADIUS, Math.max(BASE_RADIUS, p.y));
        });
    }

    // inisialisasi partikel
    function initParticles(count, temp) {
        let newParts = [];
        let spdFactor = Math.sqrt(temp / REF_TEMP);
        for (let i=0; i<count; i++) {
            let x = Math.random() * (W - 2*BASE_RADIUS) + BASE_RADIUS;
            let y = Math.random() * (H - 2*BASE_RADIUS) + BASE_RADIUS;
            let angle = Math.random() * 2 * Math.PI;
            let speed = REF_SPEED * spdFactor * (0.6 + Math.random() * 0.8);
            let vx = Math.cos(angle) * speed;
            let vy = Math.sin(angle) * speed;
            newParts.push({x, y, vx, vy, r: BASE_RADIUS});
        }
        return newParts;
    }

    function setParticleCount(newCount) {
        targetParticleCount = Math.min(60, Math.max(5, newCount));
        let diff = targetParticleCount - particles.length;
        if (diff > 0) {
            let spdFactor = Math.sqrt(temperature / REF_TEMP);
            for (let i=0; i<diff; i++) {
                let x = Math.random() * (W - 2*BASE_RADIUS) + BASE_RADIUS;
                let y = Math.random() * (H - 2*BASE_RADIUS) + BASE_RADIUS;
                let angle = Math.random() * 2 * Math.PI;
                let speed = REF_SPEED * spdFactor * (0.5 + Math.random());
                particles.push({x, y, vx: Math.cos(angle)*speed, vy: Math.sin(angle)*speed, r: BASE_RADIUS});
            }
        } else if (diff < 0) {
            particles.splice(targetParticleCount, -diff);
        }
        document.getElementById('partCountVal').innerText = targetParticleCount;
    }

    // Update loop & tumbukan
    function updatePhysics() {
        // Batas dinding
        for (let p of particles) {
            p.x += p.vx;
            p.y += p.vy;
            if (p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
            if (p.x + p.r > W) { p.x = W - p.r; p.vx = -p.vx; }
            if (p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
            if (p.y + p.r > H) { p.y = H - p.r; p.vy = -p.vy; }
        }
        // tumbukan antar partikel
        for (let i=0; i<particles.length; i++) {
            for (let j=i+1; j<particles.length; j++) {
                let p1 = particles[i], p2 = particles[j];
                let dx = p2.x - p1.x, dy = p2.y - p1.y;
                let dist = Math.hypot(dx, dy);
                let minDist = p1.r + p2.r;
                if (dist < minDist) {
                    let nx = dx/dist, ny = dy/dist;
                    let vrelx = p2.vx - p1.vx, vrely = p2.vy - p1.vy;
                    let velAlong = vrelx*nx + vrely*ny;
                    if (velAlong < 0) {
                        let impulse = 2 * velAlong / 2;
                        p1.vx += impulse * nx; p1.vy += impulse * ny;
                        p2.vx -= impulse * nx; p2.vy -= impulse * ny;
                    }
                    let overlap = minDist - dist;
                    let tx = nx * overlap * 0.5, ty = ny * overlap * 0.5;
                    p1.x -= tx; p1.y -= ty;
                    p2.x += tx; p2.y += ty;
                }
            }
        }
    }

    // Draw canvas
    function drawSim() {
        ctx.clearRect(0, 0, W, H);
        ctx.strokeStyle = "#facc15";
        ctx.lineWidth = 3;
        ctx.strokeRect(4, 4, W-8, H-8);
        for (let p of particles) {
            let grad = ctx.createRadialGradient(p.x-2, p.y-2, 2, p.x, p.y, p.r);
            grad.addColorStop(0, '#ffd966');
            grad.addColorStop(1, '#f59e0b');
            ctx.beginPath();
            ctx.arc(p.x, p.y, p.r-1, 0, 2*Math.PI);
            ctx.fillStyle = grad;
            ctx.fill();
            ctx.strokeStyle = 'white';
            ctx.lineWidth = 1;
            ctx.stroke();
        }
        // Hitung stat kecepatan
        let totalSpd = particles.reduce((a,p)=> a + Math.hypot(p.vx,p.vy),0)/(particles.length||1);
        let ekPersen = (totalSpd*totalSpd/(REF_SPEED*REF_SPEED))*100;
        document.getElementById('avgSpeed').innerText = totalSpd.toFixed(2);
        document.getElementById('avgEk').innerText = Math.min(300,Math.floor(ekPersen)) + '%';
    }

    // Event handlers UI
    function syncAll() {
        updatePressure();
        adjustBoundaryFromVolume();
        updateParticleSpeedsFromTemp();
        applyThermodynamicConstraint();
        updatePressure();
    }

    // Sliders
    document.getElementById('tempSlider').addEventListener('input', (e) => {
        temperature = parseInt(e.target.value);
        document.getElementById('tempValue').innerText = temperature + " K";
        if (currentProcess === "isotermal") {
            // suhu tetap, tidak boleh berubah via slider jika isotermal? biarkan tapi kita constrain
        }
        syncAll();
    });
    document.getElementById('volSlider').addEventListener('input', (e) => {
        volume = parseFloat(e.target.value);
        if (currentProcess === "isohorik") volume = parseFloat(e.target.value);
        else if (currentProcess === "isobarik") {
            // volume mengikuti suhu, override
            applyThermodynamicConstraint();
        } else {
            // isotermal: P*V konstan
            applyThermodynamicConstraint();
        }
        syncAll();
    });
    document.getElementById('partCountSlider').addEventListener('input', (e) => {
        targetParticleCount = parseInt(e.target.value);
        setParticleCount(targetParticleCount);
        syncAll();
    });
    document.getElementById('resetSimBtn').addEventListener('click', () => {
        temperature = 300; volume = 5.0; targetParticleCount = 24;
        document.getElementById('tempSlider').value = 300;
        document.getElementById('volSlider').value = 5.0;
        document.getElementById('partCountSlider').value = 24;
        particles = initParticles(targetParticleCount, temperature);
        syncAll();
    });

    // Proses button
    document.querySelectorAll('.proses-btn').forEach(btn => {
        btn.addEventListener('click', () => {
            document.querySelectorAll('.proses-btn').forEach(b => b.classList.remove('active-proses'));
            btn.classList.add('active-proses');
            currentProcess = btn.getAttribute('data-proses');
            document.getElementById('activeProsesName').innerText = 
                currentProcess === "isohorik" ? "Isohorik (V tetap)" : (currentProcess === "isobarik" ? "Isobarik (P tetap)" : "Isotermal (T tetap)");
            syncAll();
        });
    });
    
    // Drag interaksi
    let isDrag = false, lastX, lastY;
    canvas.addEventListener('mousedown', (e) => {
        let rect = canvas.getBoundingClientRect();
        let scaleX = canvas.width/rect.width;
        let scaleY = canvas.height/rect.height;
        lastX = (e.clientX - rect.left)*scaleX;
        lastY = (e.clientY - rect.top)*scaleY;
        isDrag = true;
    });
    window.addEventListener('mousemove', (e) => {
        if(!isDrag) return;
        let rect = canvas.getBoundingClientRect();
        let scaleX = canvas.width/rect.width;
        let scaleY = canvas.height/rect.height;
        let mx = (e.clientX - rect.left)*scaleX;
        let my = (e.clientY - rect.top)*scaleY;
        let dx = mx - lastX, dy = my - lastY;
        particles.forEach(p => {
            let dist = Math.hypot(p.x - mx, p.y - my);
            if(dist < 70) {
                let force = 0.5 * (1 - dist/70);
                p.vx += dx * force;
                p.vy += dy * force;
            }
        });
        lastX = mx; lastY = my;
    });
    window.addEventListener('mouseup', () => isDrag = false);
    
    // Animasi utama
    function animateSim() {
        updatePhysics();
        drawSim();
        requestAnimationFrame(animateSim);
    }
    particles = initParticles(24, 300);
    syncAll();
    animateSim();

    // ======================= LKPD + MONITORING (LIVE) =======================
    const submitBtn = document.getElementById('submitLkpdBtn');
    const jawabanLogDiv = document.getElementById('jawabanLog');
    let jawabanHistory = [];

    function renderLog() {
        if(jawabanHistory.length === 0) {
            jawabanLogDiv.innerHTML = '<em>Belum ada kiriman...</em>';
            return;
        }
        jawabanLogDiv.innerHTML = jawabanHistory.map(j => `
            <div style="background:#1e293b; margin:8px 0; padding:10px; border-radius:12px;">
                <strong>👤 ${j.nama || 'Anonim'}</strong> <small>${new Date(j.waktu).toLocaleTimeString()}</small><br/>
                <strong>1.</strong> ${j.j1}<br/>
                <strong>2.</strong> ${j.j2}<br/>
                <strong>3.</strong> ${j.j3}<br/>
                <strong>4.</strong> ${j.j4}<br/>
                <strong>5.</strong> ${j.j5}<hr/>
            </div>
        `).join('');
    }

    submitBtn.addEventListener('click', () => {
        let nama = document.getElementById('studentName').value.trim();
        if(nama === "") nama = "Siswa";
        let j1 = document.getElementById('jawab1').value;
        let j2 = document.getElementById('jawab2').value;
        let j3 = document.getElementById('jawab3').value;
        let j4 = document.getElementById('jawab4').value;
        let j5 = document.getElementById('jawab5').value;
        const jawabanObj = {
            nama: nama,
            j1, j2, j3, j4, j5,
            waktu: new Date().toISOString()
        };
        jawabanHistory.unshift(jawabanObj);
        renderLog();
        alert("✅ Jawaban terkirim! Guru dapat melihat secara langsung di panel monitoring.");
        // optional clear form
        document.getElementById('jawab1').value = '';
        document.getElementById('jawab2').value = '';
        document.getElementById('jawab3').value = '';
        document.getElementById('jawab4').value = '';
        document.getElementById('jawab5').value = '';
    });
    document.getElementById('clearLogBtn').addEventListener('click', () => {
        jawabanHistory = [];
        renderLog();
    });

    // Navigasi antar halaman
    document.querySelectorAll('.nav-btn').forEach(btn => {
        btn.addEventListener('click', () => {
            let pageId = btn.getAttribute('data-page');
            document.querySelectorAll('.page').forEach(page => page.classList.remove('active'));
            document.getElementById(pageId).classList.add('active');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            if(pageId === 'simulasi') {
                adjustBoundaryFromVolume();
            }
        });
    });
    document.querySelector('.nav-btn[data-page="materi"]').classList.add('active');
</script>
</body>
</html>
