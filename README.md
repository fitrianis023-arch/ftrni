<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Simulasi Gas Ideal | Tekanan Tetap (Isobarik)</title>
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
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .main-card {
            max-width: 1300px;
            width: 100%;
            background: rgba(15, 23, 42, 0.8);
            backdrop-filter: blur(10px);
            border-radius: 2rem;
            padding: 1.5rem;
            box-shadow: 0 25px 45px rgba(0,0,0,0.5);
            border: 1px solid rgba(250, 204, 21, 0.3);
        }

        h1 {
            font-size: 1.8rem;
            text-align: center;
            background: linear-gradient(135deg, #fbbf24, #38bdf8);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 5px;
        }

        .subhead {
            text-align: center;
            color: #94a3b8;
            font-size: 0.8rem;
            margin-bottom: 1.5rem;
        }

        .two-columns {
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
        }

        .simulation-box {
            flex: 3;
            min-width: 550px;
        }

        .control-box {
            flex: 1.3;
            min-width: 260px;
            background: #0f172aee;
            border-radius: 1.8rem;
            padding: 1.2rem;
            border: 1px solid #facc1544;
        }

        canvas {
            display: block;
            width: 100%;
            background: #030712;
            border-radius: 1.5rem;
            border: 3px solid #facc15;
            cursor: grab;
            box-shadow: 0 15px 25px -8px black;
        }

        canvas:active {
            cursor: grabbing;
        }

        /* Info panel - diperjelas */
        .info-panel {
            background: #020617;
            border-radius: 1.2rem;
            padding: 1rem;
            margin-top: 1rem;
            display: grid;
            grid-template-columns: repeat(4, 1fr);
            gap: 10px;
            text-align: center;
        }

        .info-card {
            background: #1e293b;
            border-radius: 1rem;
            padding: 8px 5px;
        }

        .info-card .label {
            font-size: 0.7rem;
            color: #94a3b8;
            letter-spacing: 1px;
        }

        .info-card .value {
            font-size: 1.3rem;
            font-weight: bold;
            color: #facc15;
        }

        .info-card .unit {
            font-size: 0.6rem;
            color: #64748b;
        }

        .info-card .keterangan {
            font-size: 0.6rem;
            color: #22c55e;
            margin-top: 3px;
        }

        /* Highlight untuk yang berubah */
        .changing {
            animation: pulse 0.5s ease;
        }

        @keyframes pulse {
            0% { transform: scale(1); color: #facc15; }
            50% { transform: scale(1.05); color: #fbbf24; text-shadow: 0 0 5px #facc15; }
            100% { transform: scale(1); color: #facc15; }
        }

        /* Kontrol */
        .control-group {
            margin-bottom: 1.2rem;
        }

        .control-group label {
            display: flex;
            justify-content: space-between;
            color: #cbd5e1;
            font-weight: 500;
            margin-bottom: 6px;
        }

        input[type="range"] {
            width: 100%;
            height: 6px;
            border-radius: 10px;
            background: linear-gradient(90deg, #3b82f6, #facc15);
            -webkit-appearance: none;
        }

        input[type="range"]::-webkit-slider-thumb {
            width: 18px;
            height: 18px;
            background: #facc15;
            border-radius: 50%;
            cursor: pointer;
            border: 2px solid white;
        }

        .value-badge {
            background: #1e293b;
            padding: 2px 12px;
            border-radius: 30px;
            font-weight: bold;
            color: #facc15;
        }

        .button-row {
            display: flex;
            gap: 10px;
            margin: 12px 0;
            flex-wrap: wrap;
        }

        button {
            background: #1e293b;
            border: none;
            padding: 10px 18px;
            border-radius: 40px;
            font-weight: 600;
            color: white;
            cursor: pointer;
            transition: all 0.1s ease;
            border: 1px solid #475569;
            display: inline-flex;
            align-items: center;
            gap: 6px;
            font-size: 0.9rem;
        }

        button:active {
            transform: scale(0.95);
        }

        .btn-gas {
            background: #0f172a;
            border: 1px solid #38bdf8;
        }

        .btn-gas-active {
            background: #facc15 !important;
            color: #0f172a !important;
            border-color: #fbbf24;
        }

        .btn-tekanan-tetap {
            background: #22c55e;
            color: #0f172a;
            border-color: #4ade80;
            cursor: default;
            opacity: 0.8;
        }

        hr {
            margin: 12px 0;
            border-color: #334155;
        }

        /* Petunjuk */
        .tutorial {
            background: #020617aa;
            border-radius: 1.2rem;
            padding: 0.9rem;
            margin-top: 1rem;
            border: 1px dashed #facc1566;
        }

        .tutorial h4 {
            color: #facc15;
            font-size: 0.9rem;
            margin-bottom: 8px;
        }

        .tutorial ul {
            padding-left: 1.2rem;
            font-size: 0.75rem;
            color: #cbd5e6;
        }

        .tutorial li {
            margin: 5px 0;
        }

        .fixed-warning {
            background: #22c55e20;
            border-left: 3px solid #22c55e;
            padding: 8px;
            border-radius: 10px;
            margin-bottom: 12px;
            font-size: 0.75rem;
            color: #4ade80;
            text-align: center;
        }

        @media (max-width: 900px) {
            .simulation-box { min-width: 100%; }
            .two-columns { flex-direction: column; }
            .info-panel { grid-template-columns: repeat(2, 1fr); }
        }
    </style>
</head>
<body>
<div class="main-card">
    <h1>⚛️ GAS IDEAL LAB | TEKANAN TETAP (ISOBARIK)</h1>
    <div class="subhead">Simulasi Gas Ideal | Tekanan Konstan | Volume dan Suhu Berubah</div>

    <div class="two-columns">
        <!-- AREA SIMULASI -->
        <div class="simulation-box">
            <canvas id="gasCanvas" width="950" height="450" style="width:100%; height:auto; background:#030712"></canvas>
            
            <!-- INFO PANEL DIPERJELAS: angka apa yang berubah -->
            <div class="info-panel">
                <div class="info-card">
                    <div class="label">📊 TEKANAN</div>
                    <div class="value" id="pressureValue">1.00</div>
                    <div class="unit">atm</div>
                    <div class="keterangan">🔒 TETAP (tidak berubah)</div>
                </div>
                <div class="info-card">
                    <div class="label">🌡️ SUHU</div>
                    <div class="value" id="tempDisplay">300</div>
                    <div class="unit">Kelvin (K)</div>
                    <div class="keterangan">⬆️ BERUBAH (geser slider)</div>
                </div>
                <div class="info-card">
                    <div class="label">📦 VOLUME</div>
                    <div class="value" id="volDisplay">5.0</div>
                    <div class="unit">Liter (L)</div>
                    <div class="keterangan">⬆️ BERUBAH (otomatis mengikuti suhu)</div>
                </div>
                <div class="info-card">
                    <div class="label">🔢 PARTIKEL</div>
                    <div class="value" id="partikelDisplay">24</div>
                    <div class="unit">buah (N)</div>
                    <div class="keterangan">⬆️ BERUBAH (tombol +/-)</div>
                </div>
            </div>
            <div style="font-size: 0.7rem; text-align: center; margin-top: 8px; color: #6b7280;">
                ✨ Geser mouse di area partikel untuk mendorong | Tekanan dijaga TETAP 1 atm (proses isobarik)
            </div>
        </div>

        <!-- PANEL KONTROL -->
        <div class="control-box">
            <div class="fixed-warning">
                🔒 TEKANAN TETAP = <strong>1,00 atm</strong> (tidak bisa diubah)
            </div>

            <!-- SLIDER SUHU (ini yang mengubah segalanya) -->
            <div class="control-group">
                <label>🌡️ SUHU (Kelvin) <span id="tempLabel" class="value-badge">300 K</span></label>
                <input type="range" id="suhuSlider" min="100" max="800" value="300" step="1">
                <div style="font-size: 0.65rem; color: #22c55e; margin-top: 4px;">* Menggeser suhu → volume otomatis berubah agar tekanan tetap</div>
            </div>

            <!-- SLIDER VOLUME (readonly/dinonaktifkan karena otomatis) -->
            <div class="control-group">
                <label>📦 VOLUME (Liter) <span id="volLabel" class="value-badge">5.0 L</span></label>
                <input type="range" id="volumeSlider" min="1.5" max="12.0" value="5.0" step="0.1" disabled style="opacity:0.6; cursor:not-allowed;">
                <div style="font-size: 0.65rem; color: #94a3b8; margin-top: 4px;">* Volume otomatis mengikuti suhu (V sebanding T)</div>
            </div>

            <!-- JENIS PARTIKEL (Monoatomik) -->
            <div class="control-group">
                <label>🧪 JENIS PARTIKEL</label>
                <div class="button-row">
                    <button id="monoBtn" class="btn-gas btn-gas-active">⚛️ Monoatomik (He, Ne, Ar)</button>
                </div>
            </div>

            <!-- JUMLAH PARTIKEL + TOMBOL +/- -->
            <div class="control-group">
                <label>🔢 JUMLAH PARTIKEL <span id="partikelLabel" class="value-badge">24</span></label>
                <div class="button-row">
                    <button id="kurangPartikel" style="background:#ef4444;">➖ Kurangi (2)</button>
                    <button id="tambahPartikel" style="background:#22c55e;">➕ Tambah (2)</button>
                </div>
                <input type="range" id="partikelSlider" min="6" max="55" value="24" step="1" style="margin-top: 6px;">
                <div style="font-size: 0.65rem; color: #94a3b8; margin-top: 4px;">* Mengubah jumlah partikel → volume menyesuaikan agar tekanan tetap</div>
            </div>

            <button id="resetBtn" style="width:100%; background:#334155; margin: 5px 0;">⟳ RESET KE KONDISI AWAL</button>
            
            <hr>
            
            <!-- PETUNJUK PENGGUNAAN -->
            <div class="tutorial">
                <h4>📖 PANDUAN PENGGUNAAN SIMULASI</h4>
                <ul>
                    <li>🖱️ <strong>Klik & drag</strong> pada area partikel → mendorong partikel secara langsung.</li>
                    <li>🌡️ <strong>Slider SUHU</strong> → mengubah temperatur gas, <strong class="text-green">VOLUME otomatis berubah</strong> agar TEKANAN TETAP.</li>
                    <li>📊 <strong>Tekanan selalu 1 atm</strong> (tidak berubah) karena ini simulasi proses isobarik.</li>
                    <li>📦 <strong>Volume</strong> akan mengikuti perubahan suhu (Hukum Charles: V₁/T₁ = V₂/T₂).</li>
                    <li>🧬 <strong>Jenis Partikel</strong> : Monoatomik (gas mulia seperti Helium, Neon, Argon).</li>
                    <li>➕/➖ <strong>Tombol partikel</strong> → menambah/mengurangi jumlah partikel gas, volume menyesuaikan.</li>
                    <li>🔄 <strong>Reset</strong> mengembalikan semua ke 300K, 5L, 24 partikel.</li>
                    <li>💡 <strong>Angka yang berubah</strong> : SUHU, VOLUME, JUMLAH PARTIKEL. <strong>TEKANAN TETAP</strong>.</li>
                </ul>
            </div>
        </div>
    </div>
</div>

<script>
    (function(){
        // CANVAS
        const canvas = document.getElementById('gasCanvas');
        const ctx = canvas.getContext('2d');
        let W = 950, H = 450;
        canvas.width = W; canvas.height = H;
        
        // ========== VARIABEL GAS IDEAL ==========
        let jumlahPartikel = 24;      // N (sebanding n)
        let suhu = 300;               // Kelvin
        let volume = 5.0;             // Liter
        const TEKANAN_TETAP = 1.0;    // atm (TEKANAN TIDAK BERUBAH)
        
        const RADIUS = 6;
        const REF_SPEED_300K = 3.6;   // px/frame pada 300K
        const R_SEMU = 0.0821;        // konstanta gas (L·atm/(mol·K))
        
        let particles = [];
        
        // ========== FUNGSI MENGHITUNG VOLUME DARI TEKANAN TETAP ==========
        // Rumus: PV = nRT  →  V = (nRT) / P
        // n sebanding dengan jumlahPartikel / 20 (normalisasi 20 partikel = 1 mol)
        function hitungVolumeDariTekananTetap() {
            let n = jumlahPartikel / 20.0;
            let V_baru = (n * R_SEMU * suhu) / TEKANAN_TETAP;
            V_baru = Math.min(12.0, Math.max(1.5, V_baru));
            volume = parseFloat(V_baru.toFixed(2));
            return volume;
        }
        
        // Update volume berdasarkan suhu dan jumlah partikel (karena tekanan tetap)
        function updateVolume() {
            hitungVolumeDariTekananTetap();
            document.getElementById('volumeSlider').value = volume;
            document.getElementById('volLabel').innerHTML = volume.toFixed(1) + " L";
            document.getElementById('volDisplay').innerHTML = volume.toFixed(1);
            
            // Animasi perubahan pada elemen volume
            const volElem = document.getElementById('volDisplay');
            volElem.classList.add('changing');
            setTimeout(() => volElem.classList.remove('changing'), 500);
        }
        
        // ========== UPDATE KECEPATAN PARTIKEL ==========
        function updateKecepatanDariSuhu() {
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
        
        // ========== SINKRONISASI UI ==========
        function sinkronkanUI() {
            document.getElementById('pressureValue').innerHTML = TEKANAN_TETAP.toFixed(2);
            document.getElementById('tempDisplay').innerHTML = Math.round(suhu);
            document.getElementById('volDisplay').innerHTML = volume.toFixed(1);
            document.getElementById('partikelDisplay').innerHTML = jumlahPartikel;
            document.getElementById('suhuSlider').value = suhu;
            document.getElementById('partikelSlider').value = jumlahPartikel;
            document.getElementById('tempLabel').innerHTML = Math.round(suhu) + " K";
            document.getElementById('volLabel').innerHTML = volume.toFixed(1) + " L";
            document.getElementById('partikelLabel').innerHTML = jumlahPartikel;
        }
        
        // Animasi perubahan pada suhu
        function animateSuhuChange() {
            const tempElem = document.getElementById('tempDisplay');
            tempElem.classList.add('changing');
            setTimeout(() => tempElem.classList.remove('changing'), 500);
        }
        
        // Animasi perubahan partikel
        function animatePartikelChange() {
            const partElem = document.getElementById('partikelDisplay');
            partElem.classList.add('changing');
            setTimeout(() => partElem.classList.remove('changing'), 500);
        }
        
        // ========== INISIALISASI PARTIKEL ==========
        function initParticles(count, suhuAwal) {
            let parts = [];
            let faktor = Math.sqrt(suhuAwal / 300);
            for(let i=0; i<count; i++) {
                let x = Math.random() * (W - 2*RADIUS) + RADIUS;
                let y = Math.random() * (H - 2*RADIUS) + RADIUS;
                let angle = Math.random() * 2 * Math.PI;
                let speed = REF_SPEED_300K * faktor * (0.6 + Math.random() * 0.8);
                parts.push({
                    x, y,
                    vx: Math.cos(angle) * speed,
                    vy: Math.sin(angle) * speed,
                    r: RADIUS
                });
            }
            return parts;
        }
        
        // ========== UBAH JUMLAH PARTIKEL ==========
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
            updateVolume();      // volume menyesuaikan karena tekanan tetap
            sinkronkanUI();
            animatePartikelChange();
        }
        
        // ========== UBAH SUHU ==========
        function ubahSuhu(newSuhu) {
            suhu = Math.min(800, Math.max(100, newSuhu));
            updateVolume();      // volume otomatis berubah agar tekanan tetap
            updateKecepatanDariSuhu();
            sinkronkanUI();
            animateSuhuChange();
        }
        
        // ========== FISIKA PARTIKEL ==========
        function updateFisika() {
            for(let p of particles) {
                p.x += p.vx;
                p.y += p.vy;
                if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
                if(p.x + p.r > W) { p.x = W - p.r; p.vx = -p.vx; }
                if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
                if(p.y + p.r > H) { p.y = H - p.r; p.vy = -p.vy; }
            }
            // Tumbukan antar partikel
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
        
        // ========== GAMBAR ==========
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
            ctx.font = "bold 11px monospace";
            ctx.fillStyle = "#facc15";
            ctx.fillText(`Kecepatan rata-rata: ${avgSpd.toFixed(1)} px/frame`, 12, 25);
            ctx.font = "10px monospace";
            ctx.fillStyle = "#4ade80";
            ctx.fillText(`Tekanan tetap: ${TEKANAN_TETAP} atm (Isobarik)`, 12, 45);
        }
        
        // ========== DRAG INTERAKSI ==========
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
                        let force = 0.55 * (1 - dist/70);
                        p.vx += dx * force;
                        p.vy += dy * force;
                    }
                }
                lastX = mx; lastY = my;
            });
            window.addEventListener('mouseup', () => isDragging = false);
        }
        
        // ========== EVENT LISTENER ==========
        function initEvents() {
            const suhuSlider = document.getElementById('suhuSlider');
            const partikelSlider = document.getElementById('partikelSlider');
            const tambahBtn = document.getElementById('tambahPartikel');
            const kurangBtn = document.getElementById('kurangPartikel');
            const resetBtn = document.getElementById('resetBtn');
            
            suhuSlider.addEventListener('input', (e) => {
                ubahSuhu(parseInt(e.target.value));
            });
            
            partikelSlider.addEventListener('input', (e) => {
                let val = parseInt(e.target.value);
                ubahJumlahPartikel(val);
            });
            
            tambahBtn.addEventListener('click', () => ubahJumlahPartikel(jumlahPartikel + 2));
            kurangBtn.addEventListener('click', () => ubahJumlahPartikel(jumlahPartikel - 2));
            
            resetBtn.addEventListener('click', () => {
                suhu = 300;
                jumlahPartikel = 24;
                particles = initParticles(jumlahPartikel, suhu);
                updateVolume();
                updateKecepatanDariSuhu();
                sinkronkanUI();
                document.getElementById('suhuSlider').value = 300;
                document.getElementById('partikelSlider').value = 24;
            });
        }
        
        // ========== ANIMASI LOOP ==========
        function animate() {
            updateFisika();
            draw();
            requestAnimationFrame(animate);
        }
        
        // ========== START ==========
        particles = initParticles(24, 300);
        updateVolume();      // hitung volume awal agar tekanan = 1 atm
        updateKecepatanDariSuhu();
        initEvents();
        setupDrag();
        sinkronkanUI();
        animate();
    })();
</script>
</body>
</html>
