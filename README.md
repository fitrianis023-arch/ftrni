<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Simulasi Gas Ideal | Tekanan Realistis | Termodinamika</title>
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

        .main-container {
            max-width: 1400px;
            width: 100%;
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(12px);
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
            margin-bottom: 1.2rem;
        }

        /* Layout 2 kolom */
        .flex-dashboard {
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
        }

        .simulation-area {
            flex: 3;
            min-width: 580px;
        }

        .controls-area {
            flex: 1.4;
            min-width: 280px;
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

        /* Pressure meter */
        .pressure-meter {
            background: #020617;
            border-radius: 1.5rem;
            padding: 1rem;
            margin-top: 1rem;
            display: flex;
            justify-content: space-between;
            flex-wrap: wrap;
            gap: 12px;
            align-items: center;
        }

        .gauge {
            background: #1e293b;
            border-radius: 60px;
            padding: 6px 18px;
            font-weight: bold;
        }

        .gauge span {
            color: #facc15;
            font-size: 1.6rem;
            font-weight: 800;
        }

        .volume-badge {
            background: #0f172a;
            padding: 6px 16px;
            border-radius: 40px;
            border-left: 4px solid #f59e0b;
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

        .button-group {
            display: flex;
            gap: 10px;
            margin: 12px 0;
            flex-wrap: wrap;
        }

        button {
            background: #1e293b;
            border: none;
            padding: 8px 16px;
            border-radius: 40px;
            font-weight: 600;
            color: white;
            cursor: pointer;
            transition: all 0.1s ease;
            border: 1px solid #475569;
            display: inline-flex;
            align-items: center;
            gap: 6px;
        }

        button:active {
            transform: scale(0.96);
        }

        .btn-primary {
            background: #f59e0b;
            color: #0f172a;
            border-color: #fbbf24;
        }

        .process-active {
            background: #f59e0b !important;
            color: #0f172a !important;
            border-color: #fbbf24;
            box-shadow: 0 0 8px #f59e0b;
        }

        hr {
            margin: 12px 0;
            border-color: #334155;
        }

        /* Petunjuk penggunaan (tanpa materi) */
        .tutorial-box {
            background: #020617aa;
            border-radius: 1.2rem;
            padding: 0.9rem;
            margin-top: 1rem;
            border: 1px dashed #facc1566;
        }

        .tutorial-box h4 {
            color: #facc15;
            font-size: 0.9rem;
            margin-bottom: 8px;
        }

        .tutorial-box ul {
            padding-left: 1.2rem;
            font-size: 0.8rem;
            color: #cbd5e6;
        }

        .tutorial-box li {
            margin: 5px 0;
        }

        @media (max-width: 900px) {
            .simulation-area { min-width: 100%; }
            .flex-dashboard { flex-direction: column; }
        }
    </style>
</head>
<body>
<div class="main-container">
    <h1>⚛️ GAS IDEAL LAB | PV = nRT</h1>
    <div class="subhead">Simulasi Termodinamika | Tekanan Akurat | Proses Isohorik · Isobarik · Isotermal</div>

    <div class="flex-dashboard">
        <!-- AREA SIMULASI -->
        <div class="simulation-area">
            <canvas id="gasCanvas" width="950" height="470" style="width:100%; height:auto; background:#030712"></canvas>
            
            <div class="pressure-meter">
                <div class="gauge">📊 TEKANAN = <span id="pressureValue">0.00</span> atm</div>
                <div class="gauge">🌡️ SUHU = <span id="tempValueDisplay">300</span> K</div>
                <div class="volume-badge">📦 VOLUME = <span id="volumeValueDisplay">5.0</span> L</div>
                <div class="gauge">🔢 JUMLAH PARTIKEL = <span id="partikelJumlahDisplay">24</span></div>
            </div>
            <div style="font-size: 0.7rem; text-align: center; margin-top: 8px; color: #6b7280;">
                ✨ Geser mouse di area partikel untuk mendorong | Tekanan dihitung real-time dari PV=nRT
            </div>
        </div>

        <!-- PANEL KONTROL & PETUNJUK -->
        <div class="controls-area">
            <!-- SLIDER SUHU -->
            <div class="control-group">
                <label>🌡️ SUHU (Kelvin) <span id="tempSliderLabel" class="value-badge">300 K</span></label>
                <input type="range" id="suhuSlider" min="50" max="800" value="300" step="1">
            </div>

            <!-- SLIDER VOLUME -->
            <div class="control-group">
                <label>📦 VOLUME RUANG (Liter) <span id="volSliderLabel" class="value-badge">5.0 L</span></label>
                <input type="range" id="volumeSlider" min="1.5" max="12.0" value="5.0" step="0.1">
            </div>

            <!-- JENIS PARTIKEL -->
            <div class="control-group">
                <label>🧪 JENIS PARTIKEL</label>
                <div class="button-group">
                    <button id="monoBtn" style="background:#facc15; color:#0f172a;">⚛️ Monoatomik (He)</button>
                    <button id="diBtn">🔗 Diatomik (N₂/O₂)</button>
                </div>
            </div>

            <!-- JUMLAH PARTIKEL dengan tombol +/- -->
            <div class="control-group">
                <label>🔢 JUMLAH PARTIKEL (n ~ N) <span id="partikelCountLabel" class="value-badge">24</span></label>
                <div class="button-group">
                    <button id="kurangiPartikel" style="background:#ef4444;">➖ Kurangi (2)</button>
                    <button id="tambahPartikel" style="background:#22c55e;">➕ Tambah (2)</button>
                </div>
                <input type="range" id="partikelSlider" min="6" max="55" value="24" step="1" style="margin-top: 6px;">
            </div>

            <!-- PROSES TERMODINAMIKA -->
            <div class="control-group">
                <label>⚙️ PROSES TERMODINAMIKA</label>
                <div class="button-group">
                    <button id="isohorikBtn" class="btn-primary">📌 Isohorik (V tetap)</button>
                    <button id="isobarikBtn">⚖️ Isobarik (P tetap)</button>
                    <button id="isotermalBtn">🌀 Isotermal (T tetap)</button>
                </div>
            </div>

            <button id="resetBtn" style="width:100%; background:#334155; margin: 5px 0;">⟳ RESET KE KONDISI AWAL</button>
            
            <hr>
            <!-- PETUNJUK PENGGUNAAN (HANYA CARA PAKAI) -->
            <div class="tutorial-box">
                <h4>📖 PANDUAN PENGGUNAAN SIMULASI</h4>
                <ul>
                    <li><strong>🖱️ Klik & drag</strong> pada area partikel → mendorong partikel secara langsung.</li>
                    <li><strong>🌡️ Slider SUHU</strong> → mengubah temperatur gas, partikel bergerak lebih cepat/ lambat.</li>
                    <li><strong>📦 Slider VOLUME</strong> → memperbesar/memperkecil ruang, tekanan berubah otomatis.</li>
                    <li><strong>🧬 Jenis Partikel</strong> : Monoatomik / Diatomik (mempengaruhi energi kinetik).</li>
                    <li><strong>➕/➖ Tombol partikel</strong> → menambah/mengurangi jumlah partikel gas.</li>
                    <li><strong>⚙️ Proses</strong> : Isohorik (V tetap), Isobarik (P tetap), Isotermal (T tetap) → parameter terkunci sesuai hukum gas.</li>
                    <li><strong>🔄 Reset</strong> mengembalikan semua nilai ke 300K, 5L, 24 partikel, proses isohorik.</li>
                    <li><strong>📊 Tekanan</strong> dihitung dari <strong>PV = nRT</strong> (n sebanding jumlah partikel).</li>
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
        let W = 950, H = 470;
        canvas.width = W; canvas.height = H;
        
        // ========== VARIABEL GAS IDEAL ==========
        let jumlahPartikel = 24;      // N (sebanding n)
        let suhu = 300;               // Kelvin
        let volume = 5.0;             // Liter
        let tekanan = 1.0;            // atm
        
        let proses = "isohorik";      // isohorik, isobarik, isotermal
        let jenisGas = "mono";        // mono atau di
        
        const RADIUS = 6;
        const REF_SPEED_300K = 3.6;   // px/frame pada 300K
        
        // partikel array
        let particles = [];
        
        // ========== FUNGSI TEKANAN REALISTIS (PV = nRT) ==========
        // n sebanding dengan jumlahPartikel / 20 (normalisasi)
        function hitungTekanan() {
            // n_eff sebanding jumlah partikel (skala 20 partikel = 1 mol relatif)
            let n_eff = jumlahPartikel / 20.0;
            // R semu = 0.0821 (L·atm/(mol·K)) tapi kita skala untuk tampilan realistic 1-8 atm
            // Agar pada 300K, 5L, 24 partikel => tekanan sekitar 1.97 atm (masuk akal)
            let R_semu = 0.0821;
            let P = (n_eff * R_semu * suhu) / volume;
            P = Math.min(12.0, Math.max(0.15, P));
            tekanan = parseFloat(P.toFixed(3));
            return tekanan;
        }
        
        // sinkronkan semua tampilan & terapkan batasan proses
        function sinkronkanUI() {
            document.getElementById('pressureValue').innerText = tekanan.toFixed(2);
            document.getElementById('tempValueDisplay').innerText = Math.round(suhu);
            document.getElementById('volumeValueDisplay').innerText = volume.toFixed(1);
            document.getElementById('partikelJumlahDisplay').innerText = jumlahPartikel;
            document.getElementById('suhuSlider').value = suhu;
            document.getElementById('volumeSlider').value = volume;
            document.getElementById('partikelSlider').value = jumlahPartikel;
            document.getElementById('tempSliderLabel').innerHTML = Math.round(suhu) + " K";
            document.getElementById('volSliderLabel').innerHTML = volume.toFixed(1) + " L";
            document.getElementById('partikelCountLabel').innerHTML = jumlahPartikel;
        }
        
        // batasan proses termodinamika (menjaga kondisi)
        function terapkanBatasanProses() {
            if(proses === "isohorik") {
                // Volume tetap: tidak diubah, hitung tekanan dari suhu & jumlah partikel
                volume = parseFloat(document.getElementById('volumeSlider').value);
                hitungTekanan();
            }
            else if(proses === "isobarik") {
                // Tekanan tetap: pertahankan tekanan saat ini, sesuaikan volume agar P konstan
                let targetP = tekanan;
                let n_eff = jumlahPartikel / 20.0;
                let R_semu = 0.0821;
                // V = nRT / P
                let newVolume = (n_eff * R_semu * suhu) / targetP;
                newVolume = Math.min(12.0, Math.max(1.5, newVolume));
                volume = parseFloat(newVolume.toFixed(2));
                document.getElementById('volumeSlider').value = volume;
                hitungTekanan();
            }
            else if(proses === "isotermal") {
                // Suhu tetap: P*V konstan, volume bisa berubah, tekanan menyesuaikan
                let n_eff = jumlahPartikel / 20.0;
                let R_semu = 0.0821;
                let P_konstan = (n_eff * R_semu * suhu) / volume;
                tekanan = P_konstan;
            }
            sinkronkanUI();
        }
        
        // update kecepatan partikel berdasarkan suhu (teori kinetik)
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
        
        // inisialisasi / reset partikel
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
        
        // mengubah jumlah partikel
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
            hitungTekanan();
            sinkronkanUI();
            terapkanBatasanProses();
        }
        
        // FISIKA: gerak & tumbukan elastis
        function updateFisika() {
            for(let p of particles) {
                p.x += p.vx;
                p.y += p.vy;
                if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
                if(p.x + p.r > W) { p.x = W - p.r; p.vx = -p.vx; }
                if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
                if(p.y + p.r > H) { p.y = H - p.r; p.vy = -p.vy; }
            }
            // tumbukan antar partikel
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
        
        // GAMBAR
        function draw() {
            ctx.clearRect(0, 0, W, H);
            ctx.strokeStyle = "#facc15";
            ctx.lineWidth = 3;
            ctx.strokeRect(5, 5, W-10, H-10);
            
            for(let p of particles) {
                let grad;
                if(jenisGas === "mono") {
                    grad = ctx.createRadialGradient(p.x-2, p.y-2, 2, p.x, p.y, p.r);
                    grad.addColorStop(0, "#ffd966");
                    grad.addColorStop(1, "#f59e0b");
                } else {
                    grad = ctx.createRadialGradient(p.x-2, p.y-2, 2, p.x, p.y, p.r);
                    grad.addColorStop(0, "#7dd3fc");
                    grad.addColorStop(1, "#0284c7");
                }
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.r-1, 0, 2*Math.PI);
                ctx.fillStyle = grad;
                ctx.fill();
                ctx.strokeStyle = "white";
                ctx.lineWidth = 1;
                ctx.stroke();
                // efek diatomik
                if(jenisGas === "di") {
                    ctx.beginPath();
                    ctx.ellipse(p.x+2, p.y-1.5, 2.2, 1.5, 0, 0, 2*Math.PI);
                    ctx.fillStyle = "#bae6fd";
                    ctx.fill();
                }
            }
            // info kecepatan rata2
            let avgSpd = particles.reduce((a,b)=> a+Math.hypot(b.vx,b.vy),0)/(particles.length||1);
            ctx.font = "bold 11px monospace";
            ctx.fillStyle = "#facc15";
            ctx.fillText(`v_avg: ${avgSpd.toFixed(1)} px/f`, 12, 25);
        }
        
        // ========== EVENT LISTENER ==========
        function initEventListeners() {
            const suhuSlider = document.getElementById('suhuSlider');
            const volumeSlider = document.getElementById('volumeSlider');
            const partikelSlider = document.getElementById('partikelSlider');
            const tambahBtn = document.getElementById('tambahPartikel');
            const kurangBtn = document.getElementById('kurangiPartikel');
            const resetBtn = document.getElementById('resetBtn');
            const monoBtn = document.getElementById('monoBtn');
            const diBtn = document.getElementById('diBtn');
            const isohorik = document.getElementById('isohorikBtn');
            const isobarik = document.getElementById('isobarikBtn');
            const isotermal = document.getElementById('isotermalBtn');
            
            suhuSlider.addEventListener('input', (e) => {
                suhu = parseInt(e.target.value);
                if(proses === "isotermal") {
                    // tetap pertahankan P*V konstan
                }
                hitungTekanan();
                updateKecepatanDariSuhu();
                terapkanBatasanProses();
                sinkronkanUI();
            });
            
            volumeSlider.addEventListener('input', (e) => {
                volume = parseFloat(e.target.value);
                if(proses === "isohorik") volume = parseFloat(e.target.value);
                hitungTekanan();
                terapkanBatasanProses();
                sinkronkanUI();
                updateKecepatanDariSuhu();
            });
            
            partikelSlider.addEventListener('input', (e) => {
                let val = parseInt(e.target.value);
                ubahJumlahPartikel(val);
            });
            
            tambahBtn.addEventListener('click', () => ubahJumlahPartikel(jumlahPartikel + 2));
            kurangBtn.addEventListener('click', () => ubahJumlahPartikel(jumlahPartikel - 2));
            
            resetBtn.addEventListener('click', () => {
                suhu = 300;
                volume = 5.0;
                jumlahPartikel = 24;
                proses = "isohorik";
                jenisGas = "mono";
                particles = initParticles(jumlahPartikel, suhu);
                hitungTekanan();
                updateKecepatanDariSuhu();
                sinkronkanUI();
                terapkanBatasanProses();
                document.getElementById('suhuSlider').value = 300;
                document.getElementById('volumeSlider').value = 5.0;
                document.getElementById('partikelSlider').value = 24;
                // reset style proses
                isohorik.classList.add('btn-primary'); isohorik.classList.add('process-active');
                isobarik.classList.remove('btn-primary', 'process-active');
                isotermal.classList.remove('btn-primary', 'process-active');
                isobarik.style.background = "#1e293b";
                isotermal.style.background = "#1e293b";
                monoBtn.style.background = "#facc15"; monoBtn.style.color = "#0f172a";
                diBtn.style.background = "#1e293b"; diBtn.style.color = "white";
            });
            
            monoBtn.addEventListener('click', () => {
                jenisGas = "mono";
                monoBtn.style.background = "#facc15"; monoBtn.style.color = "#0f172a";
                diBtn.style.background = "#1e293b"; diBtn.style.color = "white";
                draw();
            });
            diBtn.addEventListener('click', () => {
                jenisGas = "di";
                diBtn.style.background = "#facc15"; diBtn.style.color = "#0f172a";
                monoBtn.style.background = "#1e293b"; monoBtn.style.color = "white";
                draw();
            });
            
            isohorik.addEventListener('click', () => {
                proses = "isohorik";
                isohorik.classList.add('btn-primary','process-active');
                isobarik.classList.remove('btn-primary','process-active');
                isotermal.classList.remove('btn-primary','process-active');
                isobarik.style.background = "#1e293b";
                isotermal.style.background = "#1e293b";
                terapkanBatasanProses();
            });
            isobarik.addEventListener('click', () => {
                proses = "isobarik";
                isobarik.classList.add('btn-primary','process-active');
                isohorik.classList.remove('btn-primary','process-active');
                isotermal.classList.remove('btn-primary','process-active');
                isohorik.style.background = "#1e293b";
                isotermal.style.background = "#1e293b";
                terapkanBatasanProses();
            });
            isotermal.addEventListener('click', () => {
                proses = "isotermal";
                isotermal.classList.add('btn-primary','process-active');
                isohorik.classList.remove('btn-primary','process-active');
                isobarik.classList.remove('btn-primary','process-active');
                isohorik.style.background = "#1e293b";
                isobarik.style.background = "#1e293b";
                terapkanBatasanProses();
            });
        }
        
        // DRAG interaksi
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
        
        // ANIMASI LOOP
        function animasi() {
            updateFisika();
            draw();
            requestAnimationFrame(animasi);
        }
        
        // INIT
        particles = initParticles(24, 300);
        jumlahPartikel = 24;
        suhu = 300;
        volume = 5.0;
        hitungTekanan();
        updateKecepatanDariSuhu();
        initEventListeners();
        setupDrag();
        sinkronkanUI();
        terapkanBatasanProses();
        animasi();
    })();
</script>
</body>
</html>
