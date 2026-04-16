<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Simulasi Gas Ideal | Termodinamika Interaktif</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            user-select: none;
        }

        body {
            font-family: 'Segoe UI', 'Poppins', system-ui, sans-serif;
            background: linear-gradient(145deg, #0a0f1e 0%, #0f172a 100%);
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        /* Container Utama */
        .sim-card {
            max-width: 1300px;
            width: 100%;
            background: rgba(15, 23, 42, 0.65);
            backdrop-filter: blur(12px);
            border-radius: 2.5rem;
            padding: 1.8rem;
            box-shadow: 0 25px 45px rgba(0,0,0,0.6), 0 0 0 1px rgba(250, 204, 21, 0.3);
            border: 1px solid rgba(255,215,0,0.3);
        }

        h1 {
            font-size: 2rem;
            text-align: center;
            background: linear-gradient(135deg, #fbbf24, #38bdf8);
            -webkit-background-clip: text;
            background-clip: text;
            color: transparent;
            margin-bottom: 5px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 12px;
        }

        .sub {
            text-align: center;
            color: #94a3b8;
            font-size: 0.85rem;
            margin-bottom: 20px;
            letter-spacing: 1px;
        }

        /* Layout dua kolom: kiri simulasi, kanan kontrol */
        .dashboard {
            display: flex;
            flex-wrap: wrap;
            gap: 1.5rem;
        }

        .sim-area {
            flex: 3;
            min-width: 580px;
        }

        .control-panel {
            flex: 1.5;
            min-width: 260px;
            background: #0f172ae6;
            border-radius: 2rem;
            padding: 1.2rem;
            backdrop-filter: blur(4px);
            border: 1px solid #facc1533;
        }

        canvas {
            display: block;
            width: 100%;
            background: #050a15;
            border-radius: 1.8rem;
            border: 3px solid #facc15;
            box-shadow: 0 20px 30px -10px black;
            cursor: grab;
        }

        canvas:active {
            cursor: grabbing;
        }

        /* Statistik tekanan & ruang */
        .pressure-gauge {
            background: #020617;
            border-radius: 2rem;
            padding: 1rem;
            margin-top: 1rem;
            display: flex;
            justify-content: space-between;
            align-items: center;
            flex-wrap: wrap;
            gap: 12px;
        }

        .meter {
            background: #1e293b;
            border-radius: 40px;
            padding: 8px 20px;
            font-weight: bold;
        }

        .meter span {
            color: #facc15;
            font-size: 1.8rem;
            font-weight: 800;
        }

        .ruang-visual {
            background: #0f172a;
            border-radius: 1.5rem;
            padding: 8px 15px;
            text-align: center;
            border-left: 5px solid #f59e0b;
        }

        /* kontrol slider & tombol */
        .slider-group {
            margin-bottom: 1.2rem;
        }

        .slider-group label {
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
            padding: 4px 12px;
            border-radius: 30px;
            font-weight: bold;
            color: #facc15;
        }

        .tombol-group {
            display: flex;
            gap: 12px;
            margin: 15px 0;
            flex-wrap: wrap;
        }

        button {
            background: #1e293b;
            border: none;
            padding: 10px 18px;
            border-radius: 50px;
            font-weight: bold;
            color: white;
            display: inline-flex;
            align-items: center;
            gap: 8px;
            cursor: pointer;
            transition: 0.1s linear;
            box-shadow: 0 4px 8px rgba(0,0,0,0.3);
            border: 1px solid #475569;
        }

        button:active {
            transform: scale(0.96);
        }

        .btn-warning {
            background: #f59e0b;
            color: #0f172a;
            border-color: #fbbf24;
        }

        .btn-gas {
            background: #0f172a;
            border: 1px solid #38bdf8;
        }

        /* Petunjuk penggunaan */
        .tutorial {
            background: #020617aa;
            border-radius: 1.5rem;
            padding: 1rem;
            margin-top: 1rem;
            border: 1px dashed #facc15;
        }

        .tutorial h4 {
            color: #facc15;
            margin-bottom: 8px;
            font-size: 1rem;
            display: flex;
            gap: 6px;
        }

        .tutorial ul {
            padding-left: 1.3rem;
            font-size: 0.85rem;
            color: #cbd5e6;
        }

        .tutorial li {
            margin: 6px 0;
        }

        hr {
            margin: 12px 0;
            border-color: #334155;
        }

        @media (max-width: 900px) {
            .sim-area { min-width: 100%; }
            .dashboard { flex-direction: column; }
        }
    </style>
</head>
<body>
<div class="sim-card">
    <h1>
        ⚛️ GAS IDEAL LAB
        <span style="font-size: 0.8rem; background: #1e293b; padding: 4px 12px; border-radius: 60px;">PV = nRT</span>
    </h1>
    <div class="sub">Simulasi Isobarik · Isohorik · Isotermal | Tekanan & Partikel Realistis</div>

    <div class="dashboard">
        <!-- KIRI: AREA SIMULASI + RUANG -->
        <div class="sim-area">
            <canvas id="gasCanvas" width="900" height="450" style="width:100%; height:auto; background:#050a15"></canvas>
            
            <!-- Panel tekanan & visual ruang -->
            <div class="pressure-gauge">
                <div class="meter">📊 TEKANAN (P) = <span id="pressureValue">0.00</span> atm</div>
                <div class="meter">🌡️ SUHU = <span id="tempDisplay">300</span> K</div>
                <div class="ruang-visual">📦 VOLUME RUANG = <span id="volumeDisplay">5.0</span> L</div>
            </div>
            <div style="margin-top: 10px; font-size:0.75rem; text-align:center; color:#7e8aa2;">
                💡 Geser kursor di area partikel → dorong partikel secara interaktif
            </div>
        </div>

        <!-- KANAN: KONTROL & PETUNJUK -->
        <div class="control-panel">
            <div class="slider-group">
                <label>🌡️ SUHU (Kelvin) <span id="tempValLabel" class="value-badge">300 K</span></label>
                <input type="range" id="tempSlider" min="50" max="800" value="300" step="1">
            </div>
            <div class="slider-group">
                <label>📦 VOLUME RUANG (Liter) <span id="volValLabel" class="value-badge">5.0 L</span></label>
                <input type="range" id="volumeSlider" min="2.0" max="12.0" value="5.0" step="0.2">
            </div>

            <!-- Jenis Partikel: Monoatomik / Diatomik -->
            <div class="slider-group">
                <label>🧪 JENIS PARTIKEL</label>
                <div class="tombol-group" style="justify-content: space-between;">
                    <button id="monoBtn" class="btn-gas" style="background:#facc15; color:#0f172a;">⚛️ Monoatomik</button>
                    <button id="diBtn" class="btn-gas">🔗 Diatomik (N₂/O₂)</button>
                </div>
            </div>

            <!-- JUMLAH PARTIKEL + TOMBOL +/– -->
            <div class="slider-group">
                <label>🔢 JUMLAH PARTIKEL (n ~ N) <span id="partCountLabel" class="value-badge">24</span></label>
                <div class="tombol-group">
                    <button id="kurangPartikelBtn" style="background:#ef4444;">➖ Kurangi</button>
                    <button id="tambahPartikelBtn" style="background:#22c55e;">➕ Tambah</button>
                </div>
                <input type="range" id="partikelSlider" min="6" max="55" value="24" step="1" style="margin-top: 10px;">
            </div>

            <!-- Pilih Proses Termodinamika -->
            <div class="slider-group">
                <label>⚙️ PROSES TERMODINAMIKA</label>
                <div class="tombol-group">
                    <button id="isohorikBtn" class="btn-warning">📌 Isohorik (V tetap)</button>
                    <button id="isobarikBtn">⚖️ Isobarik (P tetap)</button>
                    <button id="isotermalBtn">🌀 Isotermal (T tetap)</button>
                </div>
            </div>

            <button id="resetSimBtn" style="width:100%; background:#334155; margin-top:5px;">⟳ Reset ke Kondisi Awal (300K, 5L, 24 partikel)</button>
            
            <hr>
            <!-- PETUNJUK PENGGUNAAN SIMULASI (hanya cara pakai, tanpa materi) -->
            <div class="tutorial">
                <h4>📖 PANDUAN PENGGUNAAN SIMULASI</h4>
                <ul>
                    <li>🖱️ <strong>Klik & drag</strong> di area partikel → mendorong partikel secara langsung.</li>
                    <li>🌡️ <strong>Slider SUHU</strong> → mengubah energi kinetik partikel (gerak makin cepat).</li>
                    <li>📦 <strong>Slider VOLUME</strong> → memperbesar/memperkecil ruang (tekanan otomatis berubah).</li>
                    <li>🧬 <strong>Jenis Partikel</strong> : Monoatomik (He, Ne) atau Diatomik (N₂, O₂) berpengaruh pada derajat kebebasan & energi.</li>
                    <li>➕/➖ <strong>Tambah/Kurangi partikel</strong> → jumlah gas berubah, tekanan ikut teori gas ideal.</li>
                    <li>⚙️ <strong>Pilih Proses</strong> : Isohorik (V tetap), Isobarik (P tetap), Isotermal (T tetap) → parameter akan terkunci sesuai proses.</li>
                    <li>🔄 Tombol Reset mengembalikan semua ke nilai awal.</li>
                    <li>📊 Tekanan, suhu, volume selalu sinkron dengan persamaan <strong>PV = nRT</strong>.</li>
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
        let W = 900, H = 450;
        canvas.width = W; canvas.height = H;

        // ---------- PARAMETER GAS IDEAL ----------
        let particles = [];
        let jumlahPartikel = 24;       // N (sebanding dengan n)
        let suhuKelvin = 300;          // T (K)
        let volumeLiter = 5.0;         // V (L)
        let tekananAtm = 1.0;          // P (atm) dihitung
        
        let prosesAktif = "isohorik";   // 'isohorik', 'isobarik', 'isotermal'
        let jenisGas = "mono";          // 'mono' atau 'di' (diatomik)
        
        const RADIUS_PARTIKEL = 6;
        const REF_SPEED_AT_300K = 3.4;   // kecepatan referensi pada 300K (px/frame)
        
        // konstanta skala untuk PV = nRT (R semu)
        // n sebanding jumlahPartikel, kita set P = (n * T) / V dalam satuan relatif (atm)
        function hitungTekanan() {
            // n_eff = jumlahPartikel / 20 (normalisasi)
            let nEff = jumlahPartikel / 20;
            let P = (nEff * suhuKelvin) / volumeLiter;
            P = Math.max(0.08, Math.min(8.0, P));
            tekananAtm = parseFloat(P.toFixed(3));
            return tekananAtm;
        }
        
        // update seluruh tampilan nilai & sinkronisasi proses
        function updateUIAndConstraints() {
            // update display angka
            document.getElementById('pressureValue').innerText = tekananAtm.toFixed(2);
            document.getElementById('tempDisplay').innerText = Math.round(suhuKelvin);
            document.getElementById('volumeDisplay').innerText = volumeLiter.toFixed(1);
            document.getElementById('tempValLabel').innerHTML = Math.round(suhuKelvin) + " K";
            document.getElementById('volValLabel').innerHTML = volumeLiter.toFixed(1) + " L";
            document.getElementById('partCountLabel').innerHTML = jumlahPartikel;
            document.getElementById('partikelSlider').value = jumlahPartikel;
            document.getElementById('tempSlider').value = suhuKelvin;
            document.getElementById('volumeSlider').value = volumeLiter;
            
            // Terapkan batasan proses termodinamika (menjaga kondisi)
            if(prosesAktif === "isohorik") {
                // volume tetap: tidak ada perubahan volume via slider eksternal? tetap sinkron
                volumeLiter = parseFloat(document.getElementById('volumeSlider').value);
                hitungTekanan();
            }
            else if(prosesAktif === "isobarik") {
                // Tekanan tetap -> V/T = konstan, maka V = (P tetap)* ... 
                // agar P konstan, volume harus proporsional terhadap suhu
                let P_target = tekananAtm;
                let nEff = jumlahPartikel / 20;
                // dari P = nRT/V => V = nRT/P
                let newVolume = (nEff * suhuKelvin) / P_target;
                newVolume = Math.min(12.0, Math.max(2.0, newVolume));
                volumeLiter = parseFloat(newVolume.toFixed(2));
                document.getElementById('volumeSlider').value = volumeLiter;
                hitungTekanan();
            }
            else if(prosesAktif === "isotermal") {
                // suhu tetap, P*V konstan
                let konstantaPV = tekananAtm * volumeLiter;
                let newVol = volumeLiter;
                let newP = konstantaPV / newVol;
                tekananAtm = newP;
                // suhu di slider jangan berubah, karena isotermal
                suhuKelvin = parseFloat(document.getElementById('tempSlider').value);
                document.getElementById('tempDisplay').innerText = Math.round(suhuKelvin);
                hitungTekanan();
            }
            // update slider volume & suhu agar sesuai
            document.getElementById('volValLabel').innerHTML = volumeLiter.toFixed(1) + " L";
            document.getElementById('tempValLabel').innerHTML = Math.round(suhuKelvin) + " K";
            document.getElementById('pressureValue').innerText = tekananAtm.toFixed(2);
        }
        
        // Fungsi kecepatan partikel berdasarkan suhu (teori kinetik gas)
        function updateKecepatanDariSuhu() {
            let faktorKecepatan = Math.sqrt(suhuKelvin / 300);
            if(suhuKelvin <= 0) faktorKecepatan = 0;
            for(let p of particles) {
                let speedNow = Math.hypot(p.vx, p.vy);
                if(speedNow < 0.05 && faktorKecepatan > 0) {
                    let angle = Math.random() * 2 * Math.PI;
                    let newSpd = REF_SPEED_AT_300K * faktorKecepatan * (0.7 + Math.random()*0.6);
                    p.vx = Math.cos(angle) * newSpd;
                    p.vy = Math.sin(angle) * newSpd;
                } else if(speedNow > 0.01) {
                    let targetSpd = REF_SPEED_AT_300K * faktorKecepatan;
                    let scale = targetSpd / speedNow;
                    p.vx *= scale;
                    p.vy *= scale;
                }
            }
        }
        
        // Inisialisasi / reset partikel
        function initParticles(count, suhu, volume) {
            let newParts = [];
            let faktorSpd = Math.sqrt(suhu / 300);
            for(let i=0; i<count; i++) {
                let x = Math.random() * (W - 2*RADIUS_PARTIKEL) + RADIUS_PARTIKEL;
                let y = Math.random() * (H - 2*RADIUS_PARTIKEL) + RADIUS_PARTIKEL;
                let angle = Math.random() * 2 * Math.PI;
                let speed = REF_SPEED_AT_300K * faktorSpd * (0.6 + Math.random() * 0.8);
                let vx = Math.cos(angle) * speed;
                let vy = Math.sin(angle) * speed;
                newParts.push({x, y, vx, vy, r: RADIUS_PARTIKEL});
            }
            return newParts;
        }
        
        // Mengubah jumlah partikel (teori: tekanan berbanding lurus dgn jumlah)
        function setJumlahPartikel(newCount) {
            newCount = Math.min(55, Math.max(6, newCount));
            let diff = newCount - particles.length;
            if(diff > 0) {
                let faktor = Math.sqrt(suhuKelvin / 300);
                for(let i=0; i<diff; i++) {
                    let x = Math.random() * (W - 2*RADIUS_PARTIKEL) + RADIUS_PARTIKEL;
                    let y = Math.random() * (H - 2*RADIUS_PARTIKEL) + RADIUS_PARTIKEL;
                    let angle = Math.random() * 2 * Math.PI;
                    let spd = REF_SPEED_AT_300K * faktor * (0.5 + Math.random());
                    particles.push({x, y, vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: RADIUS_PARTIKEL});
                }
            } else if(diff < 0) {
                particles.splice(newCount, -diff);
            }
            jumlahPartikel = particles.length;
            hitungTekanan();
            updateUIAndConstraints();
            document.getElementById('partCountLabel').innerHTML = jumlahPartikel;
            document.getElementById('partikelSlider').value = jumlahPartikel;
        }
        
        // Update keseluruhan parameter ketika slider berubah
        function refreshSimulasi() {
            hitungTekanan();
            updateKecepatanDariSuhu();
            updateUIAndConstraints();
            // sesuaikan batasan proses
            if(prosesAktif === "isobarik") updateUIAndConstraints();
            if(prosesAktif === "isotermal") updateUIAndConstraints();
        }
        
        // Fisika: gerak & tumbukan
        function updateGerakPartikel() {
            for(let p of particles) {
                p.x += p.vx;
                p.y += p.vy;
                if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
                if(p.x + p.r > W) { p.x = W - p.r; p.vx = -p.vx; }
                if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
                if(p.y + p.r > H) { p.y = H - p.r; p.vy = -p.vy; }
            }
            // tumbukan antar partikel (elastis)
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
        
        // GAMBAR PARTIKEL + efek jenis gas (warna berbeda untuk diatomik)
        function drawSimulation() {
            ctx.clearRect(0, 0, W, H);
            ctx.strokeStyle = "#facc15";
            ctx.lineWidth = 3;
            ctx.strokeRect(4, 4, W-8, H-8);
            
            for(let p of particles) {
                // Warna partikel: emas untuk monoatomik, biru kehijauan untuk diatomik
                let color1, color2;
                if(jenisGas === "mono") {
                    color1 = "#ffd966";
                    color2 = "#f59e0b";
                } else {
                    color1 = "#5ee0fa";
                    color2 = "#0ea5e9";
                }
                let grad = ctx.createRadialGradient(p.x-2, p.y-2, 2, p.x, p.y, p.r);
                grad.addColorStop(0, color1);
                grad.addColorStop(1, color2);
                ctx.beginPath();
                ctx.arc(p.x, p.y, p.r-1, 0, 2*Math.PI);
                ctx.fillStyle = grad;
                ctx.fill();
                ctx.strokeStyle = "white";
                ctx.lineWidth = 1;
                ctx.stroke();
                
                // efek visual untuk diatomik (sedikit oval)
                if(jenisGas === "di") {
                    ctx.beginPath();
                    ctx.ellipse(p.x+2, p.y-1, 2, 1.5, 0, 0, 2*Math.PI);
                    ctx.fillStyle = "#b0f0ff";
                    ctx.fill();
                }
            }
            
            // Tambahan info kecepatan rata2 di pojok (opsional)
            let totalSpeed = particles.reduce((a,b)=> a + Math.hypot(b.vx,b.vy),0)/(particles.length||1);
            ctx.font = "bold 12px 'Segoe UI'";
            ctx.fillStyle = "#facc15";
            ctx.shadowBlur = 0;
            ctx.fillText(`⚡ v_avg: ${totalSpeed.toFixed(1)} px/f`, 15, 30);
        }
        
        // =============== EVENT & LISTENER ===============
        function bindControls() {
            const tempSlider = document.getElementById('tempSlider');
            const volSlider = document.getElementById('volumeSlider');
            const partSlider = document.getElementById('partikelSlider');
            const tambahBtn = document.getElementById('tambahPartikelBtn');
            const kurangBtn = document.getElementById('kurangPartikelBtn');
            const resetBtn = document.getElementById('resetSimBtn');
            const monoBtn = document.getElementById('monoBtn');
            const diBtn = document.getElementById('diBtn');
            const isohorik = document.getElementById('isohorikBtn');
            const isobarik = document.getElementById('isobarikBtn');
            const isotermal = document.getElementById('isotermalBtn');
            
            tempSlider.addEventListener('input', (e) => {
                suhuKelvin = parseInt(e.target.value);
                if(prosesAktif === "isotermal") {
                    // isotermal: suhu tetap, tapi user boleh mengubah? paksa biarkan, tapi jaga P*V konstan
                }
                refreshSimulasi();
            });
            volSlider.addEventListener('input', (e) => {
                volumeLiter = parseFloat(e.target.value);
                if(prosesAktif === "isohorik") volumeLiter = parseFloat(e.target.value);
                else if(prosesAktif === "isobarik") {
                    // akan disesuaikan di constraint
                }
                refreshSimulasi();
            });
            partSlider.addEventListener('input', (e) => {
                let val = parseInt(e.target.value);
                setJumlahPartikel(val);
            });
            tambahBtn.addEventListener('click', () => setJumlahPartikel(jumlahPartikel + 2));
            kurangBtn.addEventListener('click', () => setJumlahPartikel(jumlahPartikel - 2));
            
            resetBtn.addEventListener('click', () => {
                suhuKelvin = 300;
                volumeLiter = 5.0;
                jumlahPartikel = 24;
                prosesAktif = "isohorik";
                jenisGas = "mono";
                particles = initParticles(jumlahPartikel, suhuKelvin, volumeLiter);
                hitungTekanan();
                updateKecepatanDariSuhu();
                updateUIAndConstraints();
                document.getElementById('tempSlider').value = 300;
                document.getElementById('volumeSlider').value = 5.0;
                document.getElementById('partikelSlider').value = 24;
                // update tampilan tombol proses
                document.querySelectorAll('#isohorikBtn, #isobarikBtn, #isotermalBtn').forEach(btn => btn.style.background = "#1e293b");
                isohorik.style.background = "#f59e0b";
            });
            
            monoBtn.addEventListener('click', () => {
                jenisGas = "mono";
                monoBtn.style.background = "#facc15"; monoBtn.style.color = "#0f172a";
                diBtn.style.background = "#1e293b"; diBtn.style.color = "white";
                drawSimulation();
            });
            diBtn.addEventListener('click', () => {
                jenisGas = "di";
                diBtn.style.background = "#facc15"; diBtn.style.color = "#0f172a";
                monoBtn.style.background = "#1e293b"; monoBtn.style.color = "white";
                drawSimulation();
            });
            
            isohorik.addEventListener('click', () => {
                prosesAktif = "isohorik";
                isohorik.style.background = "#f59e0b"; isobarik.style.background = "#1e293b"; isotermal.style.background = "#1e293b";
                refreshSimulasi();
            });
            isobarik.addEventListener('click', () => {
                prosesAktif = "isobarik";
                isobarik.style.background = "#f59e0b"; isohorik.style.background = "#1e293b"; isotermal.style.background = "#1e293b";
                refreshSimulasi();
            });
            isotermal.addEventListener('click', () => {
                prosesAktif = "isotermal";
                isotermal.style.background = "#f59e0b"; isohorik.style.background = "#1e293b"; isobarik.style.background = "#1e293b";
                refreshSimulasi();
            });
        }
        
        // Drag interaksi (dorong partikel)
        let isDragging = false, lastMX=0, lastMY=0;
        function setupDrag() {
            canvas.addEventListener('mousedown', (e) => {
                const rect = canvas.getBoundingClientRect();
                const sx = canvas.width/rect.width, sy = canvas.height/rect.height;
                lastMX = (e.clientX - rect.left) * sx;
                lastMY = (e.clientY - rect.top) * sy;
                isDragging = true;
                e.preventDefault();
            });
            window.addEventListener('mousemove', (e) => {
                if(!isDragging) return;
                const rect = canvas.getBoundingClientRect();
                const sx = canvas.width/rect.width, sy = canvas.height/rect.height;
                let mx = (e.clientX - rect.left)*sx;
                let my = (e.clientY - rect.top)*sy;
                let dx = mx - lastMX, dy = my - lastMY;
                for(let p of particles) {
                    let dist = Math.hypot(p.x - mx, p.y - my);
                    if(dist < 65) {
                        let force = 0.55 * (1 - dist/65);
                        p.vx += dx * force;
                        p.vy += dy * force;
                    }
                }
                lastMX = mx; lastMY = my;
            });
            window.addEventListener('mouseup', () => isDragging = false);
        }
        
        // Animasi utama
        function animate() {
            updateGerakPartikel();
            drawSimulation();
            requestAnimationFrame(animate);
        }
        
        // Inisialisasi awal
        particles = initParticles(24, 300, 5.0);
        jumlahPartikel = 24;
        suhuKelvin = 300;
        volumeLiter = 5.0;
        hitungTekanan();
        updateKecepatanDariSuhu();
        bindControls();
        setupDrag();
        updateUIAndConstraints();
        animate();
    })();
</script>
</body>
</html>
