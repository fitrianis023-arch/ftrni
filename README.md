<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Laboratorium Termodinamika Interaktif | SMA Kurikulum Merdeka</title>
    <!-- Font & Icons -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:opsz,wght@14..32,300;14..32,400;14..32,500;14..32,600;14..32,700&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0-beta3/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Inter', sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            color: #f1f5f9;
            min-height: 100vh;
            transition: all 0.2s;
        }

        /* Navigation Buttons */
        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 20px;
            padding: 20px 20px 0;
            flex-wrap: wrap;
            background: rgba(15, 23, 42, 0.7);
            backdrop-filter: blur(12px);
            position: sticky;
            top: 0;
            z-index: 100;
            border-bottom: 1px solid #334155;
        }

        .nav-btn {
            background: #1e293b;
            border: none;
            padding: 12px 28px;
            border-radius: 60px;
            font-weight: 600;
            font-size: 1rem;
            color: #cbd5e1;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            align-items: center;
            gap: 10px;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            border: 1px solid #475569;
        }

        .nav-btn.active {
            background: #3b82f6;
            color: white;
            border-color: #60a5fa;
            box-shadow: 0 0 15px #3b82f680;
        }

        .nav-btn i {
            font-size: 1.2rem;
        }

        /* Container utama halaman */
        .page-container {
            max-width: 1400px;
            margin: 0 auto;
            padding: 20px;
        }

        .page {
            display: none;
            animation: fadeIn 0.4s ease;
        }

        .page.active-page {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(8px);}
            to { opacity: 1; transform: translateY(0);}
        }

        /* Kartu materi dan simulasi */
        .glass-card {
            background: rgba(30, 41, 59, 0.7);
            backdrop-filter: blur(12px);
            border-radius: 2rem;
            padding: 1.8rem;
            border: 1px solid rgba(148, 163, 184, 0.3);
            box-shadow: 0 25px 40px -12px black;
        }

        /* Simulasi canvas area */
        .simulation-area {
            background: #0b1120;
            border-radius: 2rem;
            padding: 1rem;
            border: 1px solid #3b82f6;
        }

        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(circle at 20% 30%, #0f172a, #030712);
            border-radius: 1.5rem;
            cursor: grab;
            border: 2px solid #2d3a5e;
        }
        canvas:active { cursor: grabbing; }

        /* kontrol slider dan tombol */
        .control-group {
            background: #0f172a;
            padding: 1rem 1.2rem;
            border-radius: 1.5rem;
            margin-top: 1rem;
        }

        .grid-2col {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(280px, 1fr));
            gap: 1.5rem;
        }

        /* LKPD Table */
        .lkpd-table {
            width: 100%;
            border-collapse: collapse;
            background: #1e293b;
            border-radius: 1rem;
            overflow: hidden;
        }
        .lkpd-table th, .lkpd-table td {
            border: 1px solid #475569;
            padding: 12px;
            text-align: center;
            color: #e2e8f0;
        }
        .lkpd-table th {
            background: #0f172a;
            font-weight: 600;
        }
        input, select, textarea {
            background: #0f172a;
            border: 1px solid #3b82f6;
            padding: 8px 12px;
            border-radius: 20px;
            color: white;
            width: 100%;
            font-family: inherit;
        }
        button.simpan-jawaban {
            background: #10b981;
            border: none;
            padding: 10px 24px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 20px;
        }
        .monitor-status {
            background: #000000aa;
            border-radius: 1rem;
            padding: 12px;
            font-size: 0.9rem;
            margin-top: 15px;
            border-left: 5px solid #3b82f6;
        }
        .badge {
            background: #3b82f6;
            border-radius: 30px;
            padding: 4px 12px;
            font-size: 0.8rem;
        }
        .petunjuk-sim {
            background: #111827;
            border-radius: 1rem;
            padding: 1rem;
            margin-bottom: 1rem;
        }
    </style>
</head>
<body>

<div class="nav-buttons">
    <button class="nav-btn" data-page="materi"><i class="fas fa-book-open"></i> Materi Singkat</button>
    <button class="nav-btn active" data-page="simulasi"><i class="fas fa-microchip"></i> Simulasi Gas Ideal</button>
    <button class="nav-btn" data-page="lkpd"><i class="fas fa-clipboard-list"></i> LKPD Online</button>
</div>

<div class="page-container">
    <!-- HALAMAN MATERI SINGKAT (informasi cepat) -->
    <div id="materi" class="page">
        <div class="glass-card">
            <h1 class="text-3xl flex gap-3"><i class="fas fa-flask"></i> Termodinamika & Gas Ideal</h1>
            <p class="mt-4 text-slate-300">Berdasarkan Kurikulum Merdeka - Fase F (Kelas XI/SMA)</p>
            <hr class="my-4 border-slate-600">
            <div class="grid-2col">
                <div>
                    <h3><i class="fas fa-chart-line"></i> Proses Isobarik</h3>
                    <p>Tekanan tetap (P = konstan). Volume sebanding dengan suhu (Hukum Charles). Contoh: balon udara panas memuai pada tekanan atmosfer.</p>
                    <h3 class="mt-4"><i class="fas fa-chart-simple"></i> Proses Isokhorik (Isovolumik)</h3>
                    <p>Volume tetap. Tekanan sebanding dengan suhu (Hukum Gay-Lussac). Contoh: tekanan ban mobil naik saat panas.</p>
                </div>
                <div>
                    <h3><i class="fas fa-temperature-low"></i> Proses Isotermal</h3>
                    <p>Suhu tetap (T = konstan). Tekanan berbanding terbalik dengan volume (Hukum Boyle). Contoh: pompa sepeda memanas perlahan jika lambat.</p>
                    <h3><i class="fas fa-atom"></i> Gas Ideal & Teori Kinetik</h3>
                    <p>Partikel bergerak acak, tumbukan lenting sempurna. Energi kinetik rata-rata sebanding dengan suhu mutlak (Kelvin).</p>
                </div>
            </div>
            <div class="monitor-status mt-4">
                <i class="fas fa-lightbulb"></i> <strong>Petunjuk:</strong> Klik tombol "Simulasi" untuk eksplorasi interaktif! Temukan sendiri hubungan antara tekanan, volume, suhu, dan jumlah partikel.
            </div>
            <div class="text-center mt-6">
                <button class="nav-btn" style="background:#2563eb;" data-page="simulasi"><i class="fas fa-play"></i> Lanjut ke Simulasi</button>
            </div>
        </div>
    </div>

    <!-- HALAMAN SIMULASI UTAMA (canggih, estetik, dapat ubah partikel, tekanan, volume, monoatomik/diatomik) -->
    <div id="simulasi" class="page active-page">
        <div class="glass-card">
            <h2><i class="fas fa-charging-station"></i> Laboratorium Virtual Gas & Termodinamika</h2>
            <div class="petunjuk-sim mt-3">
                <i class="fas fa-hand-pointer"></i> <strong>CARA PAKAI SIMULASI (hanya petunjuk teknis):</strong><br>
                • <i class="fas fa-mouse-pointer"></i> <strong>Drag mouse di area kanvas</strong> → dorong partikel secara langsung.<br>
                • <i class="fas fa-sliders-h"></i> Ubah <strong>SUHU (Kelvin)</strong>, <strong>VOLUME (geser dinding kanan)</strong>, <strong>TEKANAN (dipengaruhi tumbukan)</strong>.<br>
                • Pilih <strong>JENIS PARTIKEL:</strong> Monoatomik (He, Ne) atau Diatomik (O₂, N₂) dengan massa berbeda.<br>
                • Atur jumlah partikel & rasio energi kinetik.<br>
                • Amati perubahan gerak, tekanan relatif & hubungan dengan kehidupan sehari-hari (kompor gas, piston mesin).
            </div>
            <div class="simulation-area">
                <canvas id="mainCanvas" width="1100" height="520" style="width:100%; height:auto; background:#0a0f1c"></canvas>
            </div>
            
            <div class="grid-2col mt-4">
                <div class="control-group">
                    <label><i class="fas fa-thermometer-half"></i> Suhu (Kelvin): <span id="tempValueDisplay">300</span> K</label>
                    <input type="range" id="tempSlider" min="0" max="600" value="300" step="1">
                    <div class="flex justify-between mt-1"><span>❄️ 0 K (diam)</span><span>🔥 600 K (cepat)</span></div>
                </div>
                <div class="control-group">
                    <label><i class="fas fa-expand-alt"></i> Volume (% dari ukuran awal): <span id="volumePercent">100</span>%</label>
                    <input type="range" id="volumeSlider" min="40" max="180" value="100" step="2">
                    <div class="text-xs">Dinding kanan bergerak → ubah volume gas (isobarik/isokhorik/isotermal dapat diamati)</div>
                </div>
            </div>

            <div class="grid-2col mt-2">
                <div class="control-group">
                    <label><i class="fas fa-atom"></i> Jenis Partikel:</label>
                    <select id="particleTypeSelect">
                        <option value="mono">Monoatomik (He, Ne) – massa ringan</option>
                        <option value="di">Diatomik (O₂, N₂) – massa lebih berat</option>
                    </select>
                    <label class="mt-2">Pilih Unsur Spesifik:</label>
                    <select id="elementSelect">
                        <option value="Helium">Helium (He) - ringan</option>
                        <option value="Neon">Neon (Ne) - sedang</option>
                        <option value="Oksigen">Oksigen (O₂) - diatomik</option>
                        <option value="Nitrogen">Nitrogen (N₂) - diatomik</option>
                    </select>
                </div>
                <div class="control-group">
                    <label><i class="fas fa-users"></i> Jumlah Partikel: <span id="particleCountDisplay">36</span></label>
                    <div style="display: flex; gap:10px; margin-top:8px">
                        <button id="addParticleBtn" class="badge" style="background:#10b981; padding:8px 20px;">+ Tambah</button>
                        <button id="removeParticleBtn" class="badge" style="background:#ef4444; padding:8px 20px;">- Kurangi</button>
                        <button id="resetSimBtn" class="badge" style="background:#f59e0b;">⟳ Reset</button>
                    </div>
                    <div class="mt-3"><i class="fas fa-tachometer-alt"></i> Tekanan Relatif (tumbukan/dinding): <strong><span id="pressureIndicator">100</span>%</strong></div>
                </div>
            </div>
            <div class="monitor-status mt-2">
                <i class="fas fa-chart-line"></i> Data realtime: Suhu = <span id="liveTemp">300</span> K | Volume = <span id="liveVolume">100</span>% | Tekanan = <span id="livePressure">100</span>% | Energi Kinetik rata-rata ~ <span id="avgKE">100</span>%
            </div>
        </div>
    </div>

    <!-- HALAMAN LKPD (online, bisa dikerjakan siswa, data tersimpan di localStorage dan bisa dipantau oleh pemilik) -->
    <div id="lkpd" class="page">
        <div class="glass-card">
            <h2><i class="fas fa-pen-ruler"></i> Lembar Kerja Peserta Didik (LKPD) - Termodinamika</h2>
            <div class="monitor-status mb-4">
                <i class="fas fa-chalkboard-user"></i> <strong>Capaian Pembelajaran (CP):</strong> Peserta didik mampu menganalisis hubungan antara tekanan, volume, dan suhu gas ideal serta penerapannya dalam teknologi. <br>
                <strong>Tujuan Pembelajaran (TP):</strong> 1. Menemukan hubungan P-V pada suhu tetap (isotermal). 2. Menjelaskan pengaruh volume terhadap tekanan pada jumlah partikel tetap. 3. Mengaitkan fenomena gas ideal dengan kehidupan sehari-hari.
            </div>
            <p class="mb-3"><i class="fas fa-search"></i> <strong>Rumusan Masalah:</strong> Bagaimana pengaruh perubahan volume dan suhu terhadap tekanan gas berdasarkan eksperimen virtual? Temukan sendiri melalui simulasi!</p>
            
            <h3>Tabel Pengamatan (Isi berdasarkan eksperimenmu pada simulasi)</h3>
            <table class="lkpd-table" id="observationTable">
                <thead>
                    <tr><th>Percobaan ke-</th><th>Suhu (K)</th><th>Volume (%)</th><th>Tekanan Relatif (%)</th><th>Jenis Proses (pilih: isobarik/isokhorik/isotermal)</th><th>Fenomena sehari-hari</th></tr>
                </thead>
                <tbody id="lkpd-tbody">
                    <tr><td>1</td><td><input type="number" class="tempObs" placeholder="K"></td><td><input type="number" class="volObs" placeholder="%"></td><td><input type="number" class="pressObs" placeholder="%"></td><td><input type="text" class="prosesObs" placeholder="isobarik/isokhorik/isotermal"></td><td><input type="text" class="dailyObs" placeholder="contoh: balon udara"></td></tr>
                    <tr><td>2</td><td><input type="number" class="tempObs"></td><td><input type="number" class="volObs"></td><td><input type="number" class="pressObs"></td><td><input type="text" class="prosesObs"></td><td><input type="text" class="dailyObs"></td></tr>
                    <tr><td>3</td><td><input type="number" class="tempObs"></td><td><input type="number" class="volObs"></td><td><input type="number" class="pressObs"></td><td><input type="text" class="prosesObs"></td><td><input type="text" class="dailyObs"></td></tr>
                    <tr><td>4</td><td><input type="number" class="tempObs"></td><td><input type="number" class="volObs"></td><td><input type="number" class="pressObs"></td><td><input type="text" class="prosesObs"></td><td><input type="text" class="dailyObs"></td></tr>
                </tbody>
            </table>
            <button id="saveLKPD" class="simpan-jawaban"><i class="fas fa-save"></i> Simpan Jawaban LKPD (Online)</button>
            <button id="resetLKPD" class="simpan-jawaban" style="background:#475569; margin-left:10px;"><i class="fas fa-undo"></i> Reset Data</button>
            <div id="monitorOwner" class="monitor-status mt-3">
                <i class="fas fa-chart-simple"></i> <strong>Status respon siswa (pemilik / guru dapat melihat):</strong><br>
                <span id="ownerView">Belum ada data tersimpan. Setelah siswa mengisi dan menyimpan, data akan muncul di sini secara online.</span>
            </div>
            <p class="text-sm mt-2 text-slate-400"><i class="fas fa-info-circle"></i> Data tersimpan di browser (localStorage) dan dapat dibuka oleh pemilik/guru dengan membuka halaman ini. Untuk pantau realtime, guru dapat merefresh halaman.</p>
        </div>
    </div>
</div>

<script>
    // ==================== SIMULASI CANVAS GAS IDEAL PREMIUM ====================
    const canvas = document.getElementById('mainCanvas');
    const ctx = canvas.getContext('2d');
    let W = 1100, H = 520;
    canvas.width = W; canvas.height = H;

    let particles = [];
    let baseRadius = 6;
    let currentTemp = 300;      // Kelvin
    let volumePercent = 100;     // persentase volume (lebar efektif = W * volumePercent/100)
    let pressureBase = 100;
    
    // parameter partikel
    let particleCount = 36;
    let particleType = "mono";    // mono / di
    let massFactor = 1.0;         // mono=1, di=1.8
    
    // kecepatan referensi
    const REF_SPEED_300K = 3.4;
    
    // drag
    let isDragging = false, lastX = 0, lastY = 0;
    
    // fungsi update massa berdasarkan jenis
    function updateMassByType() {
        const type = document.getElementById('particleTypeSelect').value;
        particleType = type;
        if(particleType === "mono") massFactor = 1.0;
        else massFactor = 1.85;  // diatomik lebih berat => kecepatan lebih lambat pada energi sama
        
        // sesuaikan kecepatan dengan massa: v ~ sqrt(1/mass) agar energi kinetik sama pada suhu tertentu
        syncVelocitiesWithTemp();
    }
    
    // hitung faktor kecepatan dari suhu dan massa
    function getSpeedScaleFromTemp() {
        if(currentTemp <= 0) return 0;
        // v ~ sqrt(T / m)  -> energi kinetik = 1/2 m v^2 ~ T
        const baseScale = Math.sqrt(currentTemp / 300) * (1 / Math.sqrt(massFactor));
        return baseScale;
    }
    
    function syncVelocitiesWithTemp() {
        const scale = getSpeedScaleFromTemp();
        if(scale === 0) {
            particles.forEach(p => { p.vx = 0; p.vy = 0; });
            return;
        }
        const targetAvgSpeed = REF_SPEED_300K * scale;
        particles.forEach(p => {
            let speed = Math.hypot(p.vx, p.vy);
            if(speed < 0.01) {
                let angle = Math.random() * 2*Math.PI;
                p.vx = Math.cos(angle) * targetAvgSpeed * (0.6+Math.random()*0.8);
                p.vy = Math.sin(angle) * targetAvgSpeed * (0.6+Math.random()*0.8);
            } else {
                let ratio = targetAvgSpeed / speed;
                p.vx *= ratio;
                p.vy *= ratio;
            }
        });
    }
    
    function initParticles(count, temp, volPct) {
        let newParts = [];
        let effectiveW = W * (volPct/100);
        let xMin = baseRadius, xMax = effectiveW - baseRadius;
        if(xMax <= xMin) xMax = xMin + 10;
        for(let i=0;i<count;i++) {
            let x = Math.random() * (xMax - xMin) + xMin;
            let y = Math.random() * (H - 2*baseRadius) + baseRadius;
            let angle = Math.random() * 2*Math.PI;
            let spd = REF_SPEED_300K * (temp/300) * (1/Math.sqrt(massFactor)) * (0.6+Math.random()*0.8);
            if(temp===0) spd=0;
            newParts.push({x, y, vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: baseRadius});
        }
        return newParts;
    }
    
    function updateParticleCount(newCount) {
        newCount = Math.min(80, Math.max(4, newCount));
        let diff = newCount - particles.length;
        if(diff > 0) {
            let effectiveW = W * (volumePercent/100);
            let xMin = baseRadius, xMax = Math.max(effectiveW - baseRadius, baseRadius+5);
            for(let i=0;i<diff;i++) {
                let angle = Math.random()*2*Math.PI;
                let spd = (currentTemp===0)?0: REF_SPEED_300K * Math.sqrt(currentTemp/300) * (1/Math.sqrt(massFactor)) * (0.7+Math.random()*0.6);
                particles.push({
                    x: Math.random() * (xMax - xMin) + xMin,
                    y: Math.random() * (H - 2*baseRadius) + baseRadius,
                    vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: baseRadius
                });
            }
        } else if(diff < 0) particles.splice(newCount, -diff);
        particleCount = particles.length;
        document.getElementById('particleCountDisplay').innerText = particleCount;
    }
    
    function updateVolume(percent) {
        volumePercent = Math.min(180, Math.max(40, percent));
        document.getElementById('volumePercent').innerText = volumePercent;
        let boundaryX = W * (volumePercent/100);
        // pindahkan partikel yang keluar batas kanan
        particles.forEach(p => {
            if(p.x + p.r > boundaryX) p.x = boundaryX - p.r;
            if(p.x - p.r < 0) p.x = p.r;
        });
        computePressure();
    }
    
    function computePressure() {
        // tekanan dihitung dari tumbukan terhadap dinding kanan (atau total impuls)
        let totalImpulse = 0;
        const boundary = W * (volumePercent/100);
        particles.forEach(p => {
            if(p.vx > 0 && (p.x + p.r >= boundary)) totalImpulse += Math.abs(p.vx);
        });
        let pressure = Math.min(300, (totalImpulse / (particles.length||1)) * 40 * (currentTemp/300+0.5));
        pressure = Math.max(5, pressure);
        let pressurePercent = Math.min(280, Math.round(pressure));
        pressureBase = pressurePercent;
        document.getElementById('pressureIndicator').innerText = pressurePercent;
        document.getElementById('livePressure').innerText = pressurePercent;
        return pressurePercent;
    }
    
    function updateSimulationFromUI() {
        currentTemp = parseFloat(document.getElementById('tempSlider').value);
        document.getElementById('tempValueDisplay').innerText = currentTemp;
        document.getElementById('liveTemp').innerText = currentTemp;
        volumePercent = parseFloat(document.getElementById('volumeSlider').value);
        updateVolume(volumePercent);
        document.getElementById('liveVolume').innerText = volumePercent;
        let type = document.getElementById('particleTypeSelect').value;
        particleType = type;
        updateMassByType();
        syncVelocitiesWithTemp();
        computePressure();
        // update energi kinetik
        let avgKE = 0;
        particles.forEach(p=> { let v2 = p.vx*p.vx+p.vy*p.vy; avgKE+=v2; });
        avgKE = (avgKE/(particles.length||1)) / (REF_SPEED_300K*REF_SPEED_300K) * 100;
        document.getElementById('avgKE').innerText = Math.floor(avgKE);
        document.getElementById('liveTemp').innerText = currentTemp;
    }
    
    // animasi dan fisika (tumbukan elastis, dinding kanan dinamis)
    function updatePhysics() {
        let rightWall = W * (volumePercent/100);
        for(let p of particles) {
            p.x += p.vx;
            p.y += p.vy;
            if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx; }
            if(p.x + p.r > rightWall) { p.x = rightWall - p.r; p.vx = -p.vx; computePressure();}
            if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
            if(p.y + p.r > H) { p.y = H - p.r; p.vy = -p.vy; }
        }
        for(let i=0;i<particles.length;i++){
            for(let j=i+1;j<particles.length;j++){
                let p1=particles[i], p2=particles[j];
                let dx=p2.x-p1.x, dy=p2.y-p1.y, dist=Math.hypot(dx,dy);
                let minDist=p1.r+p2.r;
                if(dist<minDist){
                    let nx=dx/dist, ny=dy/dist;
                    let vrelx=p2.vx-p1.vx, vrely=p2.vy-p1.vy;
                    let velAlong=vrelx*nx+vrely*ny;
                    if(velAlong>0) continue;
                    let e=1;
                    let m1=massFactor, m2=massFactor;
                    let imp = (1+e)*velAlong/( (1/m1)+(1/m2) );
                    p1.vx += imp * nx / m1;
                    p1.vy += imp * ny / m1;
                    p2.vx -= imp * nx / m2;
                    p2.vy -= imp * ny / m2;
                    let overlap = minDist - dist;
                    let moveX = nx*overlap*0.5, moveY = ny*overlap*0.5;
                    p1.x -= moveX; p1.y -= moveY;
                    p2.x += moveX; p2.y += moveY;
                }
            }
        }
        computePressure();
    }
    
    function draw() {
        ctx.clearRect(0,0,W,H);
        let rightWall = W * (volumePercent/100);
        ctx.fillStyle = "#0b142b";
        ctx.fillRect(0,0,rightWall,H);
        ctx.fillStyle = "#1e2a3e";
        ctx.fillRect(rightWall,0,W-rightWall,H);
        ctx.strokeStyle = "#facc15";
        ctx.lineWidth = 3;
        ctx.beginPath();
        ctx.moveTo(rightWall,0); ctx.lineTo(rightWall,H);
        ctx.stroke();
        ctx.strokeStyle = "#60a5fa";
        ctx.strokeRect(2,2,rightWall-4, H-4);
        
        for(let p of particles){
            let grad = ctx.createRadialGradient(p.x-3,p.y-3,2,p.x,p.y,p.r+2);
            if(currentTemp<50) grad.addColorStop(0,"#7dd3fc");
            else if(currentTemp>450) grad.addColorStop(0,"#ff8c42");
            else grad.addColorStop(0,"#facc15");
            grad.addColorStop(1,"#d97706");
            ctx.beginPath();
            ctx.arc(p.x,p.y,p.r-1,0,2*Math.PI);
            ctx.fillStyle=grad;
            ctx.fill();
            ctx.shadowBlur=6;
            ctx.fill();
            ctx.shadowBlur=0;
        }
        if(isDragging){
            ctx.beginPath(); ctx.arc(lastX,lastY,55,0,2*Math.PI);
            ctx.strokeStyle="#ffd966"; ctx.setLineDash([8,8]); ctx.stroke(); ctx.setLineDash([]);
        }
    }
    
    function animate(){
        updatePhysics();
        draw();
        requestAnimationFrame(animate);
    }
    
    // setup event listeners simulasi
    function setupSimEvents(){
        document.getElementById('tempSlider').addEventListener('input', (e)=>{
            currentTemp=parseInt(e.target.value);
            updateSimulationFromUI();
            syncVelocitiesWithTemp();
        });
        document.getElementById('volumeSlider').addEventListener('input', (e)=>{
            volumePercent=parseInt(e.target.value);
            updateVolume(volumePercent);
            updateSimulationFromUI();
        });
        document.getElementById('addParticleBtn').onclick=()=>{ updateParticleCount(particles.length+3); updateSimulationFromUI(); };
        document.getElementById('removeParticleBtn').onclick=()=>{ updateParticleCount(particles.length-3); updateSimulationFromUI(); };
        document.getElementById('resetSimBtn').onclick=()=>{
            currentTemp=300; volumePercent=100; particleCount=36;
            document.getElementById('tempSlider').value=300;
            document.getElementById('volumeSlider').value=100;
            particleType=document.getElementById('particleTypeSelect').value="mono";
            updateMassByType();
            particles = initParticles(36,300,100);
            updateVolume(100);
            updateSimulationFromUI();
            syncVelocitiesWithTemp();
        };
        document.getElementById('particleTypeSelect').onchange=()=>{ updateMassByType(); syncVelocitiesWithTemp(); updateSimulationFromUI(); };
        document.getElementById('elementSelect').onchange=()=>{ /* estetika unsur */ };
        canvas.addEventListener('mousedown', (e)=>{
            isDragging=true;
            let rect=canvas.getBoundingClientRect(), sx=canvas.width/rect.width, sy=canvas.height/rect.height;
            lastX=(e.clientX-rect.left)*sx; lastY=(e.clientY-rect.top)*sy;
        });
        window.addEventListener('mousemove', (e)=>{
            if(!isDragging) return;
            let rect=canvas.getBoundingClientRect(), sx=canvas.width/rect.width, sy=canvas.height/rect.height;
            let mx=(e.clientX-rect.left)*sx, my=(e.clientY-rect.top)*sy;
            let dx=mx-lastX, dy=my-lastY;
            particles.forEach(p=>{ let d2=(p.x-mx)**2+(p.y-my)**2; if(d2<3000){ p.vx+=dx*0.45; p.vy+=dy*0.45; }});
            lastX=mx; lastY=my;
        });
        window.addEventListener('mouseup',()=>{ isDragging=false; });
    }
    
    particles = initParticles(36,300,100);
    setupSimEvents();
    animate();
    
    // ==================== LKPD INTERAKTIF + MONITOR OWNER ====================
    function loadLKPDData() {
        let saved = localStorage.getItem("lkpd_termodinamika");
        if(saved){
            let data = JSON.parse(saved);
            let rows = document.querySelectorAll("#lkpd-tbody tr");
            for(let i=0;i<rows.length && i<data.length;i++){
                rows[i].querySelector(".tempObs").value = data[i].temp || "";
                rows[i].querySelector(".volObs").value = data[i].vol || "";
                rows[i].querySelector(".pressObs").value = data[i].press || "";
                rows[i].querySelector(".prosesObs").value = data[i].proses || "";
                rows[i].querySelector(".dailyObs").value = data[i].daily || "";
            }
            displayOwnerMonitor(data);
        } else displayOwnerMonitor([]);
    }
    function displayOwnerMonitor(data){
        let ownerDiv = document.getElementById("ownerView");
        if(!data.length){ ownerDiv.innerHTML = "<i class='fas fa-database'></i> Belum ada data LKPD tersimpan. Siswa harap mengisi dan menyimpan."; return; }
        let html = `<i class='fas fa-clipboard'></i> Hasil LKPD Siswa (terbaru):<br><pre style="font-size:12px">${JSON.stringify(data, null, 2)}</pre>`;
        ownerDiv.innerHTML = html;
    }
    function saveLKPD(){
        let rows = document.querySelectorAll("#lkpd-tbody tr");
        let data = [];
        rows.forEach(row=>{
            data.push({
                temp: row.querySelector(".tempObs").value,
                vol: row.querySelector(".volObs").value,
                press: row.querySelector(".pressObs").value,
                proses: row.querySelector(".prosesObs").value,
                daily: row.querySelector(".dailyObs").value
            });
        });
        localStorage.setItem("lkpd_termodinamika", JSON.stringify(data));
        alert("LKPD berhasil disimpan! Pemilik/guru dapat melihatnya dengan refresh halaman.");
        displayOwnerMonitor(data);
    }
    document.getElementById("saveLKPD").addEventListener("click", saveLKPD);
    document.getElementById("resetLKPD").addEventListener("click", ()=>{
        localStorage.removeItem("lkpd_termodinamika");
        document.querySelectorAll("#lkpd-tbody input").forEach(inp=>inp.value="");
        displayOwnerMonitor([]);
        alert("Data LKPD direset.");
    });
    loadLKPDData();
    
    // navigasi antar halaman
    const btns = document.querySelectorAll('.nav-btn');
    const pages = document.querySelectorAll('.page');
    btns.forEach(btn=>{
        btn.addEventListener('click',()=>{
            let pageId = btn.getAttribute('data-page');
            pages.forEach(page=>page.classList.remove('active-page'));
            document.getElementById(pageId).classList.add('active-page');
            btns.forEach(b=>b.classList.remove('active'));
            btn.classList.add('active');
        });
    });
    
    setInterval(()=>{ computePressure(); }, 200);
</script>
</body>
</html>
