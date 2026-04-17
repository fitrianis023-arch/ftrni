<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>TermoXplorer | Simulasi Gas Ideal & Termodinamika Interaktif</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Poppins', 'Segoe UI', system-ui, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0a0f2a 0%, #03081a 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .app-container {
            width: 1400px;
            max-width: 98vw;
            background: rgba(8, 15, 28, 0.78);
            backdrop-filter: blur(14px);
            border-radius: 2.5rem;
            box-shadow: 0 30px 50px rgba(0,0,0,0.7), 0 0 0 2px rgba(255, 200, 100, 0.25);
            overflow: hidden;
        }

        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 1.2rem;
            padding: 1rem 1.8rem;
            background: #0b112ee6;
            border-bottom: 2px solid #ffc28555;
            flex-wrap: wrap;
        }

        .nav-btn {
            background: #1e2f44;
            border: none;
            padding: 10px 30px;
            border-radius: 60px;
            font-weight: 600;
            font-size: 1rem;
            color: #fef3d6;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 5px 10px rgba(0,0,0,0.3);
        }

        .nav-btn.active {
            background: #ffaa33;
            color: #0e1a2a;
            box-shadow: 0 0 15px #ffaa66;
        }

        .page {
            display: none;
            padding: 1.8rem;
            animation: fadeSlide 0.4s ease;
        }

        .page.active-page {
            display: block;
        }

        @keyframes fadeSlide {
            from { opacity: 0; transform: translateY(10px);}
            to { opacity: 1; transform: translateY(0);}
        }

        .sim-card {
            background: #0a1124cc;
            border-radius: 2rem;
            padding: 1.2rem;
        }

        .canvas-container {
            background: #010514;
            border-radius: 1.8rem;
            padding: 8px;
            box-shadow: inset 0 0 10px #00000055, 0 10px 20px black;
        }

        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(circle at 20% 30%, #19233f, #030817);
            border-radius: 1.5rem;
            cursor: grab;
            border: 2px solid #ffcf8a;
        }
        canvas:active { cursor: grabbing; }

        .control-3col {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 20px;
        }
        .ctrl-panel {
            flex: 1;
            background: #0e193ae0;
            border-radius: 1.6rem;
            padding: 18px;
            border: 1px solid #ffbc6e;
            backdrop-filter: blur(4px);
        }
        .ctrl-panel h3 {
            color: #ffe0aa;
            margin-bottom: 15px;
            font-size: 1.2rem;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        .slider-group {
            margin-bottom: 18px;
        }
        .slider-group label {
            display: flex;
            justify-content: space-between;
            color: #ffefcf;
            font-weight: 500;
        }
        input[type=range] {
            width: 100%;
            margin-top: 8px;
            height: 6px;
            border-radius: 10px;
            background: linear-gradient(90deg, #2c7da0, #ffb347);
        }
        .stat-badge {
            background: #010a1b;
            border-radius: 1.2rem;
            padding: 12px;
            text-align: center;
            border-left: 4px solid #ffaa44;
        }
        .stats-row {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(120px,1fr));
            gap: 12px;
            margin-top: 18px;
        }
        .real-life-note {
            background: #2a2f45cc;
            border-radius: 1.5rem;
            padding: 15px 20px;
            margin-top: 20px;
            border: 1px solid #ffc285;
            color: #fff2df;
        }
        .materi-card {
            background: linear-gradient(145deg, #101b36, #09112a);
            border-radius: 2rem;
            padding: 2rem;
            color: #f5f0e6;
            box-shadow: 0 15px 25px rgba(0,0,0,0.4);
        }
        .rumus-box {
            background: #00000055;
            border-radius: 1.5rem;
            padding: 1rem;
            margin: 15px 0;
            font-family: monospace;
            font-size: 1.1rem;
            text-align: center;
        }
        .lkpd-container {
            background: #fffef7;
            border-radius: 2rem;
            padding: 2rem;
            color: #2c2a24;
        }
        .jawaban-siswa {
            width: 100%;
            padding: 12px;
            border-radius: 24px;
            border: 2px solid #f0a34b;
            margin: 10px 0;
            font-size: 0.95rem;
        }
        button {
            cursor: pointer;
            transition: 0.2s;
        }
        .btn-submit {
            background: #ff9f2e;
            border: none;
            padding: 12px 28px;
            border-radius: 40px;
            font-weight: bold;
        }
        .btn-submit:hover {
            background: #ffb347;
            transform: scale(1.02);
        }
        select {
            background: #ffefcf;
            border: none;
            padding: 8px 12px;
            border-radius: 40px;
            font-weight: bold;
        }
        .reset-btn {
            background: #6d4c2e;
            color: white;
            border: none;
            padding: 8px 18px;
            border-radius: 40px;
        }
        .daily-icon {
            font-size: 1.3rem;
            margin-right: 8px;
        }
        .warning-info {
            font-size: 0.75rem;
            color: #ffd966;
            margin-top: 5px;
        }
    </style>
</head>
<body>
<div class="app-container">
    <div class="nav-buttons">
        <button class="nav-btn active" data-page="simulasi">🔥 SIMULASI GAS</button>
        <button class="nav-btn" data-page="materi">📖 MATERI + RUMUS</button>
        <button class="nav-btn" data-page="lkpd">📝 LKPD DIGITAL</button>
    </div>

    <!-- HALAMAN SIMULASI -->
    <div id="simulasi" class="page active-page">
        <div class="sim-card">
            <div class="canvas-container">
                <canvas id="gasCanvas" width="1150" height="520"></canvas>
            </div>

            <div class="control-3col">
                <div class="ctrl-panel">
                    <h3>📦 VOLUME RUANG</h3>
                    <div class="slider-group">
                        <label>🔘 Volume <span id="volumeValueLabel">100%</span></label>
                        <input type="range" id="volumeSlider" min="40" max="160" value="100" step="1">
                        <p class="warning-info">★ Mengatur volume ruang gas</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>⚙️ TEKANAN EKSTERNAL</h3>
                    <div class="slider-group">
                        <label>🎛️ Tekanan luar <span id="extPressureLabel">1.00</span> atm</label>
                        <input type="range" id="pressureSlider" min="0.2" max="3.5" value="1.0" step="0.02">
                        <p class="warning-info">★ Tekanan eksternal mempengaruhi gerak partikel</p>
                    </div>
                </div>
                <div class="ctrl-panel">
                    <h3>🌡️ SUHU & PARTIKEL</h3>
                    <div class="slider-group">
                        <label>🔥 Suhu (K) <span id="tempShow">350 K</span></label>
                        <input type="range" id="tempSlider" min="100" max="800" value="350" step="5">
                    </div>
                    <div class="slider-group">
                        <label>🧪 Jumlah Partikel <span id="partCountShow">60</span></label>
                        <input type="range" id="partikelSlider" min="20" max="120" value="60" step="2">
                    </div>
                    <div style="display: flex; gap: 8px; flex-wrap:wrap;">
                        <select id="particleTypeSelect">
                            <option value="mono">Monoatomik (He, Ne)</option>
                            <option value="di">Diatomik (O₂, N₂)</option>
                        </select>
                        <select id="elementSelect">
                            <option value="Helium">Helium (He)</option>
                            <option value="Neon">Neon (Ne)</option>
                            <option value="Oksigen">Oksigen (O₂)</option>
                            <option value="Nitrogen">Nitrogen (N₂)</option>
                        </select>
                        <button id="resetSimBtn" class="reset-btn">⟳ Reset</button>
                    </div>
                </div>
            </div>

            <div class="stats-row">
                <div class="stat-badge">🌀 Tekanan Gas<br><span id="pressureActual">---</span> atm</div>
                <div class="stat-badge">📐 Volume<br><span id="volumeActual">100</span> %</div>
                <div class="stat-badge">⚡ Kecepatan Rata-rata<br><span id="rmsSpeed">0</span> px/frame</div>
                <div class="stat-badge">💥 Energi Kinetik<br><span id="ekStat">0</span> a.u</div>
                <div class="stat-badge">🔁 Tumbukan/dtk<br><span id="collisionRate">0</span></div>
            </div>

            <div class="real-life-note">
                <h4>🌍 <u>SIMULASI INI SEPERTI KEJADIAN NYATA:</u></h4>
                <div style="display: flex; flex-wrap: wrap; gap: 18px; margin-top: 8px;">
                    <div><span class="daily-icon">🚗</span> Ban Mobil Panas : Suhu ↑ → Tekanan ↑</div>
                    <div><span class="daily-icon">💨</span> Semprotan Deodoran : Gas memuai cepat</div>
                    <div><span class="daily-icon">🍲</span> Panci Presto : Volume tetap, suhu & tekanan naik</div>
                    <div><span class="daily-icon">🎈</span> Balon Udara Panas : Pemuaian gas karena suhu</div>
                    <div><span class="daily-icon">💉</span> Jarum Suntik : Tekan piston → volume mengecil</div>
                </div>
            </div>
        </div>
    </div>

    <!-- HALAMAN MATERI -->
    <div id="materi" class="page">
        <div class="materi-card">
            <h2 style="color:#ffcd7e;">📘 Termodinamika & Gas Ideal (Kurikulum Merdeka)</h2>
            <div style="display: flex; flex-wrap: wrap; gap: 25px; margin-top: 20px;">
                <div style="flex:1.2;">
                    <h3>✨ Hukum Gas Ideal</h3>
                    <div class="rumus-box">
                        <strong>PV = nRT</strong><br>
                        P = Tekanan, V = Volume, n = jumlah mol,<br>
                        R = konstanta gas, T = Suhu (Kelvin)
                    </div>
                    <h3>⚙️ Proses Termodinamika</h3>
                    <ul style="margin-left: 1.2rem; line-height: 1.7;">
                        <li><strong>Isobarik</strong> (Tekanan Tetap) → V₁/T₁ = V₂/T₂</li>
                        <li><strong>Isokhorik</strong> (Volume Tetap) → P₁/T₁ = P₂/T₂</li>
                        <li><strong>Isotermal</strong> (Suhu Tetap) → P₁V₁ = P₂V₂ (Hukum Boyle)</li>
                        <li><strong>Adiabatik</strong> (Tanpa kalor) → PV^γ = konstan</li>
                    </ul>
                </div>
                <div style="flex:1; background:#00000033; border-radius: 1.5rem; padding: 1.2rem;">
                    <h3>🔬 Penerapan Nyata</h3>
                    <p>✔️ Kulkas & AC : Kompresi & ekspansi gas refrigeran.<br>
                    ✔️ Mesin Kendaraan : Siklus Otto (kompresi, pembakaran).<br>
                    ✔️ Meteorologi : Pemanasan udara menyebabkan ekspansi.<br>
                    ✔️ Pernapasan : Paru-paru mengembang → tekanan turun.</p>
                    <div class="rumus-box" style="margin-top: 15px;">
                        💡 <strong>Energi Dalam Gas Ideal</strong><br>
                        ΔU = (3/2)nRΔT (monoatomik)<br>
                        ΔU = (5/2)nRΔT (diatomik)
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- HALAMAN LKPD -->
    <div id="lkpd" class="page">
        <div class="lkpd-container">
            <h2>📄 LKPD - Eksplorasi Gas Ideal & Termodinamika</h2>
            <p><strong>Capaian Pembelajaran (CP)</strong> : Siswa mampu menyelidiki dan menyimpulkan hubungan P, V, T gas ideal melalui simulasi.</p>
            <hr style="margin: 15px 0;">
            <textarea id="rumusanMasalah" rows="2" class="jawaban-siswa" placeholder="📌 Rumusan Masalah"></textarea>
            <textarea id="hipotesis" rows="2" class="jawaban-siswa" placeholder="🔬 Hipotesis / Dugaan sementara"></textarea>
            <textarea id="dataObservasi" rows="3" class="jawaban-siswa" placeholder="📊 Data & Observasi"></textarea>
            <textarea id="kesimpulan" rows="3" class="jawaban-siswa" placeholder="💡 Kesimpulan"></textarea>
            <button id="saveLkpdBtn" class="btn-submit">💾 SIMPAN JAWABAN</button>
            <div id="liveStatus" style="background:#e9f5e9; border-radius: 1.5rem; padding: 12px; margin-top: 15px;">✅ Jawaban tersimpan di penyimpanan lokal</div>
            <button id="exportDataBtn" style="margin-top: 12px; background:#5f7f9e; border: none; padding: 10px 24px; border-radius: 60px; color:white;">📎 Ekspor Jawaban</button>
        </div>
    </div>
</div>

<script>
    (function(){
        // CANVAS
        const canvas = document.getElementById('gasCanvas');
        const ctx = canvas.getContext('2d');
        let width = 1150, height = 520;
        canvas.width = width; canvas.height = height;

        // PARTIKEL
        let particles = [];
        const particleRadius = 5;
        
        // PARAMETER
        let temperature = 350;
        let volumePercent = 100;
        let externalPressure = 1.0;
        let particleType = 'mono';
        let elementName = 'Helium';
        let massFactor = 1.0;
        
        // BATAS DINDING (volume tetap, tidak bergerak sendiri)
        let rightWallX = width * (volumePercent / 100);
        const leftWallX = 0;
        
        // STATISTIK
        let collisionCounter = 0;
        let lastCollisionUpdate = Date.now();
        let collisionRate = 0;
        
        const baseSpeedRef = 6.5;
        
        // DRAG
        let dragging = false;
        let dragX = 0, dragY = 0;
        
        function getSpeedScale() {
            return Math.sqrt(temperature / 300) / Math.sqrt(massFactor);
        }
        
        function updateMassFactor() {
            if (particleType === 'mono') {
                if (elementName === 'Helium') massFactor = 1.0;
                else if (elementName === 'Neon') massFactor = 1.9;
                else massFactor = 1.2;
            } else {
                if (elementName === 'Oksigen') massFactor = 2.66;
                else if (elementName === 'Nitrogen') massFactor = 2.33;
                else massFactor = 2.0;
            }
            applyTemperatureToAll();
        }
        
        function applyTemperatureToAll() {
            const scale = getSpeedScale();
            const targetAvgSpeed = baseSpeedRef * scale;
            
            for (let p of particles) {
                let currentSpeed = Math.hypot(p.vx, p.vy);
                if (currentSpeed < 0.1) {
                    let angle = Math.random() * 2 * Math.PI;
                    let speed = targetAvgSpeed * (0.6 + Math.random() * 0.8);
                    p.vx = Math.cos(angle) * speed;
                    p.vy = Math.sin(angle) * speed;
                } else {
                    let ratio = targetAvgSpeed / currentSpeed;
                    ratio = Math.min(1.5, Math.max(0.6, ratio));
                    p.vx *= ratio;
                    p.vy *= ratio;
                }
            }
            updateStats();
        }
        
        function initParticles(count, temp) {
            let arr = [];
            const currentRight = rightWallX;
            const scale = Math.sqrt(temp / 300) / Math.sqrt(massFactor);
            const avgSpeed = baseSpeedRef * scale;
            
            for (let i = 0; i < count; i++) {
                let x = Math.random() * (currentRight - 2 * particleRadius) + particleRadius;
                let y = Math.random() * (height - 2 * particleRadius) + particleRadius;
                let angle = Math.random() * 2 * Math.PI;
                let speed = avgSpeed * (0.5 + Math.random() * 1.0);
                arr.push({x, y, vx: Math.cos(angle) * speed, vy: Math.sin(angle) * speed, r: particleRadius});
            }
            return arr;
        }
        
        function setParticleCount(newCount) {
            newCount = Math.min(120, Math.max(20, newCount));
            let current = particles.length;
            
            if (newCount > current) {
                const scale = getSpeedScale();
                const avgSpeed = baseSpeedRef * scale;
                for (let i = 0; i < newCount - current; i++) {
                    let x = Math.random() * (rightWallX - 2 * particleRadius) + particleRadius;
                    let y = Math.random() * (height - 2 * particleRadius) + particleRadius;
                    let angle = Math.random() * 2 * Math.PI;
                    let speed = avgSpeed * (0.5 + Math.random() * 1.0);
                    particles.push({x, y, vx: Math.cos(angle) * speed, vy: Math.sin(angle) * speed, r: particleRadius});
                }
            } else if (newCount < current) {
                particles.splice(newCount, current - newCount);
            }
            document.getElementById('partCountShow').innerText = particles.length;
            updateStats();
        }
        
        function updateVolume() {
            rightWallX = width * (volumePercent / 100);
            rightWallX = Math.min(width * 1.6, Math.max(width * 0.4, rightWallX));
            
            for (let p of particles) {
                if (p.x + p.r > rightWallX) {
                    p.x = rightWallX - p.r;
                    p.vx = -Math.abs(p.vx) * 0.8;
                }
            }
            document.getElementById('volumeActual').innerText = Math.round(volumePercent) + '%';
            document.getElementById('volumeValueLabel').innerText = Math.round(volumePercent) + '%';
        }
        
        function calculatePressure() {
            let pressure = (particles.length * (temperature / 300)) * (100 / volumePercent);
            pressure = pressure * (massFactor > 1.5 ? 1.2 : 1.0);
            pressure = Math.min(8, Math.max(0.1, pressure));
            return pressure;
        }
        
        function handleCollisionsAndBoundaries() {
            const currentRight = rightWallX;
            
            for (let p of particles) {
                p.x += p.vx;
                p.y += p.vy;
                
                if (p.x - p.r < leftWallX) {
                    p.x = leftWallX + p.r;
                    p.vx = -p.vx;
                    collisionCounter++;
                }
                if (p.x + p.r > currentRight) {
                    p.x = currentRight - p.r;
                    p.vx = -p.vx;
                    collisionCounter++;
                }
                if (p.y - p.r < 0) {
                    p.y = p.r;
                    p.vy = -p.vy;
                    collisionCounter++;
                }
                if (p.y + p.r > height) {
                    p.y = height - p.r;
                    p.vy = -p.vy;
                    collisionCounter++;
                }
            }
            
            // Tumbukan antar partikel
            for (let i = 0; i < particles.length; i++) {
                for (let j = i + 1; j < particles.length; j++) {
                    let p1 = particles[i];
                    let p2 = particles[j];
                    let dx = p2.x - p1.x;
                    let dy = p2.y - p1.y;
                    let distance = Math.hypot(dx, dy);
                    let minDistance = p1.r + p2.r;
                    
                    if (distance < minDistance) {
                        let nx = dx / distance;
                        let ny = dy / distance;
                        let vrelx = p2.vx - p1.vx;
                        let vrely = p2.vy - p1.vy;
                        let velAlong = vrelx * nx + vrely * ny;
                        
                        if (velAlong < 0) {
                            let impulse = velAlong;
                            p1.vx += impulse * nx;
                            p1.vy += impulse * ny;
                            p2.vx -= impulse * nx;
                            p2.vy -= impulse * ny;
                            collisionCounter++;
                        }
                        
                        let overlap = minDistance - distance;
                        let correctionX = nx * overlap * 0.5;
                        let correctionY = ny * overlap * 0.5;
                        p1.x -= correctionX;
                        p1.y -= correctionY;
                        p2.x += correctionX;
                        p2.y += correctionY;
                    }
                }
            }
        }
        
        function updateStats() {
            let totalSpeed = 0;
            for (let p of particles) totalSpeed += Math.hypot(p.vx, p.vy);
            let avgSpeed = particles.length ? totalSpeed / particles.length : 0;
            document.getElementById('rmsSpeed').innerText = avgSpeed.toFixed(1);
            
            let avgEk = 0.5 * massFactor * avgSpeed * avgSpeed;
            document.getElementById('ekStat').innerText = avgEk.toFixed(1);
            
            let pressure = calculatePressure();
            document.getElementById('pressureActual').innerHTML = pressure.toFixed(2) + " atm";
            document.getElementById('tempShow').innerText = temperature + " K";
            
            let now = Date.now();
            let dt = (now - lastCollisionUpdate) / 1000;
            if (dt >= 0.8) {
                collisionRate = Math.round(collisionCounter / dt);
                collisionCounter = 0;
                lastCollisionUpdate = now;
            }
            document.getElementById('collisionRate').innerHTML = collisionRate;
        }
        
        function draw() {
            ctx.clearRect(0, 0, width, height);
            
            ctx.strokeStyle = "#ffcc77";
            ctx.lineWidth = 3;
            ctx.strokeRect(leftWallX + 5, 5, rightWallX - 10, height - 10);
            
            ctx.fillStyle = "#bd7f3ad9";
            ctx.fillRect(rightWallX - 12, 0, 14, height);
            ctx.fillStyle = "#f3bc6c";
            ctx.fillRect(rightWallX - 10, 0, 10, height);
            
            for (let p of particles) {
                let grad = ctx.createRadialGradient(p.x - 3, p.y - 3, 2, p.x, p.y, p.r + 3);
                if (temperature > 500) {
                    grad.addColorStop(0, '#ffaa66');
                    grad.addColorStop(1, '#cc4411');
                } else if (temperature < 200) {
                    grad.addColorStop(0, '#88ccff');
                    grad.addColorStop(1, '#2266cc');
                } else {
                    grad.addColorStop(0, '#ffdd99');
                    grad.addColorStop(1, '#e0872c');
                }
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.r, 0, Math.PI * 2);
                ctx.fillStyle = grad;
                ctx.fill();
                ctx.strokeStyle = '#fff6e5';
                ctx.lineWidth = 1;
                ctx.stroke();
            }
            
            if (dragging) {
                ctx.beginPath();
                ctx.arc(dragX, dragY, 70, 0, Math.PI * 2);
                ctx.strokeStyle = '#ffcc88';
                ctx.setLineDash([6, 10]);
                ctx.lineWidth = 2;
                ctx.stroke();
                ctx.setLineDash([]);
            }
            updateStats();
        }
        
        function getCanvasCoords(e) {
            const rect = canvas.getBoundingClientRect();
            const scaleX = canvas.width / rect.width;
            const scaleY = canvas.height / rect.height;
            let clientX, clientY;
            if (e.touches) {
                clientX = e.touches[0].clientX;
                clientY = e.touches[0].clientY;
            } else {
                clientX = e.clientX;
                clientY = e.clientY;
            }
            return { x: (clientX - rect.left) * scaleX, y: (clientY - rect.top) * scaleY };
        }
        
        function onDragStart(e) { e.preventDefault(); dragging = true; let c = getCanvasCoords(e); dragX = c.x; dragY = c.y; }
        function onDragMove(e) {
            if (!dragging) return;
            e.preventDefault();
            let c = getCanvasCoords(e);
            let dx = c.x - dragX, dy = c.y - dragY;
            if (Math.hypot(dx, dy) > 0.5) {
                for (let p of particles) {
                    let dist = Math.hypot(p.x - c.x, p.y - c.y);
                    if (dist < 80) {
                        let force = (1 - dist / 80) * 1.2;
                        p.vx += dx * force;
                        p.vy += dy * force;
                        let speed = Math.hypot(p.vx, p.vy);
                        if (speed > 25) { p.vx = (p.vx / speed) * 25; p.vy = (p.vy / speed) * 25; }
                    }
                }
            }
            dragX = c.x; dragY = c.y;
            updateStats();
        }
        function onDragEnd(e) { dragging = false; }
        
        canvas.addEventListener('mousedown', onDragStart);
        window.addEventListener('mousemove', onDragMove);
        window.addEventListener('mouseup', onDragEnd);
        canvas.addEventListener('touchstart', onDragStart);
        window.addEventListener('touchmove', onDragMove);
        window.addEventListener('touchend', onDragEnd);
        
        function animate() {
            handleCollisionsAndBoundaries();
            draw();
            requestAnimationFrame(animate);
        }
        
        // INISIALISASI
        particles = initParticles(60, 350);
        
        document.getElementById('tempSlider').addEventListener('input', (e) => { temperature = parseInt(e.target.value); applyTemperatureToAll(); });
        document.getElementById('partikelSlider').addEventListener('input', (e) => { setParticleCount(parseInt(e.target.value)); });
        document.getElementById('volumeSlider').addEventListener('input', (e) => { volumePercent = parseFloat(e.target.value); updateVolume(); updateStats(); });
        document.getElementById('pressureSlider').addEventListener('input', (e) => { externalPressure = parseFloat(e.target.value); document.getElementById('extPressureLabel').innerText = externalPressure.toFixed(2); });
        document.getElementById('particleTypeSelect').onchange = (e) => { particleType = e.target.value; updateMassFactor(); };
        document.getElementById('elementSelect').onchange = (e) => { elementName = e.target.value; updateMassFactor(); };
        document.getElementById('resetSimBtn').onclick = () => {
            temperature = 350; volumePercent = 100; externalPressure = 1.0; particleType = 'mono'; elementName = 'Helium';
            document.getElementById('tempSlider').value = 350;
            document.getElementById('volumeSlider').value = 100;
            document.getElementById('pressureSlider').value = 1.0;
            document.getElementById('particleTypeSelect').value = 'mono';
            document.getElementById('elementSelect').value = 'Helium';
            updateMassFactor();
            updateVolume();
            setParticleCount(60);
            applyTemperatureToAll();
        };
        
        document.getElementById('saveLkpdBtn').onclick = () => {
            const data = {
                rumusan: document.getElementById('rumusanMasalah').value,
                hipotesis: document.getElementById('hipotesis').value,
                observasi: document.getElementById('dataObservasi').value,
                kesimpulan: document.getElementById('kesimpulan').value
            };
            localStorage.setItem('lkpdGasIdeal', JSON.stringify(data));
            document.getElementById('liveStatus').innerHTML = '✅ Tersimpan! (' + new Date().toLocaleTimeString() + ')';
        };
        
        document.getElementById('exportDataBtn').onclick = () => {
            const saved = localStorage.getItem('lkpdGasIdeal');
            if (saved) {
                const data = JSON.parse(saved);
                const text = `RUMUSAN:\n${data.rumusan}\n\nHIPOTESIS:\n${data.hipotesis}\n\nDATA:\n${data.observasi}\n\nKESIMPULAN:\n${data.kesimpulan}`;
                const blob = new Blob([text], {type: 'text/plain'});
                const a = document.createElement('a');
                const url = URL.createObjectURL(blob);
                a.href = url; a.download = 'lkpd_termodinamika.txt';
                a.click(); URL.revokeObjectURL(url);
            } else { alert('Belum ada data yang tersimpan.'); }
        };
        
        // NAVIGASI
        document.querySelectorAll('.nav-btn').forEach(btn => {
            btn.addEventListener('click', () => {
                document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                document.querySelectorAll('.page').forEach(p => p.classList.remove('active-page'));
                document.getElementById(btn.getAttribute('data-page')).classList.add('active-page');
            });
        });
        
        animate();
    })();
</script>
</body>
</html>
