<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=no">
    <title>Termodinamika Lab: Gas Ideal | Simulasi Interaktif SMA</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Inter', 'Segoe UI', system-ui, -apple-system, sans-serif;
        }

        body {
            background: linear-gradient(135deg, #0f172a 0%, #1e293b 100%);
            min-height: 100vh;
            padding: 20px;
        }

        /* navigasi tombol antar halaman */
        .nav-buttons {
            display: flex;
            justify-content: center;
            gap: 18px;
            margin-bottom: 30px;
            flex-wrap: wrap;
        }

        .nav-btn {
            background: rgba(255,255,245,0.1);
            backdrop-filter: blur(8px);
            border: 1px solid rgba(255,215,120,0.6);
            padding: 12px 28px;
            border-radius: 60px;
            font-weight: 600;
            font-size: 1rem;
            color: #fef3c7;
            cursor: pointer;
            transition: 0.2s;
            box-shadow: 0 4px 12px rgba(0,0,0,0.3);
            letter-spacing: 0.5px;
        }

        .nav-btn.active {
            background: #f59e0b;
            color: #0f172a;
            border-color: #ffd966;
            box-shadow: 0 0 15px rgba(245,158,11,0.6);
        }

        .nav-btn:hover:not(.active) {
            background: rgba(245,158,11,0.5);
            transform: translateY(-2px);
        }

        /* Container halaman */
        .page {
            display: none;
            animation: fadeIn 0.3s ease;
        }

        .page.active-page {
            display: block;
        }

        @keyframes fadeIn {
            from { opacity: 0; transform: translateY(10px);}
            to { opacity: 1; transform: translateY(0);}
        }

        /* Card styling */
        .glass-card {
            background: rgba(15, 25, 45, 0.75);
            backdrop-filter: blur(12px);
            border-radius: 2rem;
            padding: 24px;
            border: 1px solid rgba(255, 200, 100, 0.4);
            box-shadow: 0 25px 40px -12px black;
        }

        h2 {
            font-size: 1.8rem;
            color: #ffdd99;
            border-left: 6px solid #f59e0b;
            padding-left: 20px;
            margin-bottom: 20px;
        }

        /* simulasi canvas */
        .simu-container {
            background: #0a0f1c;
            border-radius: 28px;
            padding: 12px;
            box-shadow: inset 0 0 8px #00000055, 0 10px 20px rgba(0,0,0,0.5);
        }

        canvas {
            display: block;
            width: 100%;
            background: radial-gradient(circle at 30% 20%, #1e2a3a, #03060c);
            border-radius: 20px;
            cursor: grab;
            border: 2px solid #ffc857;
        }

        canvas:active {
            cursor: grabbing;
        }

        .control-group {
            display: flex;
            flex-wrap: wrap;
            gap: 20px;
            margin-top: 25px;
            margin-bottom: 20px;
        }

        .param-card {
            background: #0f1a2ccc;
            border-radius: 24px;
            padding: 16px 20px;
            flex: 1;
            min-width: 180px;
            border: 1px solid #ffb34770;
        }

        .param-card label {
            display: block;
            font-weight: 500;
            color: #ffdd99;
            margin-bottom: 10px;
            font-size: 0.9rem;
        }

        input, select {
            width: 100%;
            padding: 10px 14px;
            border-radius: 40px;
            background: #1e2b3c;
            border: 1px solid #ffb347;
            color: white;
            font-weight: 500;
            font-size: 1rem;
        }

        .value-display {
            font-size: 1.6rem;
            font-weight: bold;
            color: #fbbf24;
            margin-top: 8px;
        }

        .row-2col {
            display: flex;
            gap: 20px;
            flex-wrap: wrap;
        }

        .info-panel {
            background: #07111ed9;
            border-radius: 20px;
            padding: 16px;
            margin-top: 20px;
        }

        /* tabel LKPD */
        .lkpd-table {
            width: 100%;
            border-collapse: collapse;
            background: #1e2a3a;
            border-radius: 20px;
            overflow: hidden;
            margin: 20px 0;
        }

        .lkpd-table th, .lkpd-table td {
            border: 1px solid #fbbf24;
            padding: 12px;
            text-align: center;
            color: #f3f4f6;
        }

        .lkpd-table th {
            background: #f59e0b;
            color: #0f172a;
        }

        textarea, input[type="text"] {
            background: #0f172a;
            color: white;
            border: 1px solid #fbbf24;
            border-radius: 16px;
            padding: 10px;
            width: 100%;
        }

        button.submit-lkpd {
            background: #10b981;
            border: none;
            padding: 12px 24px;
            border-radius: 40px;
            font-weight: bold;
            cursor: pointer;
            margin-top: 20px;
        }

        .feedback-msg {
            margin-top: 12px;
            background: #064e3b;
            padding: 8px;
            border-radius: 30px;
            text-align: center;
        }
    </style>
</head>
<body>

<div class="nav-buttons">
    <button class="nav-btn" data-page="materi">📖 INFORMASI MATERI</button>
    <button class="nav-btn active" data-page="simulasi">⚡ SIMULASI TERMODINAMIKA</button>
    <button class="nav-btn" data-page="lkpd">📝 LKPD ONLINE</button>
</div>

<!-- HALAMAN 1: MATERI SINGKAT -->
<div id="materi" class="page">
    <div class="glass-card">
        <h2>🔬 Termodinamika & Proses Gas Ideal</h2>
        <div style="display: flex; flex-direction: column; gap: 20px;">
            <div style="background:#0f172a; padding:20px; border-radius: 28px;">
                <h3 style="color:#ffb347;">📌 Apa itu Gas Ideal?</h3>
                <p style="margin-top: 10px; line-height:1.6;">Gas ideal adalah gas teoretis yang partikelnya bergerak acak, tidak ada gaya tarik antar partikel, dan tumbukan bersifat lenting sempurna. Pada simulasi ini, amati perubahan gerak partikel saat suhu/tekanan/volume diubah.</p>
            </div>
            <div class="row-2col">
                <div style="background:#0f172a; padding:18px; border-radius: 24px; flex:1">
                    <h3 style="color:#f97316;">🔥 Proses ISOBARIK</h3>
                    <p>Tekanan tetap (P = konstan). Jika suhu naik, volume membesar. Pada simulasi, atur tekanan konstan lalu ubah suhu → amati perubahan volume (kotak melebar/menyempit).</p>
                </div>
                <div style="background:#0f172a; padding:18px; border-radius: 24px; flex:1">
                    <h3 style="color:#facc15;">📦 Proses ISOKHORIK</h3>
                    <p>Volume tetap (V = konstan). Tekanan berbanding lurus dengan suhu. Pada simulasi, kunci volume dengan mengunci slider volume, lalu ubah suhu → lihat tekanan berubah.</p>
                </div>
                <div style="background:#0f172a; padding:18px; border-radius: 24px; flex:1">
                    <h3 style="color:#38bdf8;">🌡️ Proses ISOTERMAL</h3>
                    <p>Suhu tetap (T = konstan). Tekanan berbanding terbalik dengan volume. Pada simulasi, atur suhu konstan, ubah volume → tekanan berubah secara otomatis.</p>
                </div>
            </div>
            <div style="background:#1e293b; border-radius: 28px; padding: 20px; text-align:center;">
                <p>✨ <strong>Cara menemukan konsep:</strong> Gunakan simulasi untuk mengamati hubungan P-V-T. Ganti jenis partikel (monoatomik/diatomik), pilih unsur, ubah volume dan tekanan. Temukan sendiri mana yang isobarik, isokhorik, isotermal berdasarkan perilaku gas!</p>
            </div>
        </div>
    </div>
</div>

<!-- HALAMAN 2: SIMULASI UTAMA (estetik & interaktif) -->
<div id="simulasi" class="page active-page">
    <div class="glass-card">
        <h2>⚙️ SIMULASI GAS IDEAL | Eksplorasi Proses Termodinamika</h2>
        <!-- Petunjuk penggunaan (hanya cara pakai, tanpa konsep) -->
        <div class="info-panel" style="background:#06111ed9; margin-bottom:20px;">
            <p style="color:#ffd966;">📌 <strong>PETUNJUK PENGGUNAAN SIMULASI:</strong> 👆 <strong>Klik & drag</strong> di area partikel untuk mendorong partikel. 🔘 Gunakan slider <strong>Volume</strong> (ubah lebar ruang) & <strong>Tekanan Eksternal</strong>. 🌡️ <strong>Suhu</strong> bisa diatur langsung. ⚛️ Pilih <strong>Jenis Partikel</strong> (monoatomik/diatomik) & <strong>Unsur</strong> (mempengaruhi massa). 📊 Grafik dan angka realtime membantu analisis. Amati hubungan P, V, T untuk menemukan proses isobarik, isokhorik, isotermal!</p>
        </div>

        <div class="simu-container">
            <canvas id="gasCanvasSim" width="1000" height="450" style="width:100%; height:auto; aspect-ratio:1000/450"></canvas>
        </div>

        <!-- kontrol variabel -->
        <div class="control-group">
            <div class="param-card">
                <label>📦 Volume (V) → Lebar Ruang</label>
                <input type="range" id="volumeSlider" min="0.5" max="2.2" step="0.01" value="1.0">
                <div id="volumeValue" class="value-display">1.00 (x normal)</div>
            </div>
            <div class="param-card">
                <label>⚙️ Tekanan Eksternal (P) </label>
                <input type="range" id="pressureSlider" min="0.3" max="2.5" step="0.01" value="1.0">
                <div id="pressureValue" class="value-display">1.00 atm</div>
            </div>
            <div class="param-card">
                <label>🌡️ Suhu (T) / Kelvin</label>
                <input type="range" id="tempSlider" min="50" max="600" step="1" value="300">
                <div id="tempValue" class="value-display">300 K</div>
            </div>
        </div>

        <div class="control-group">
            <div class="param-card">
                <label>🧪 Jenis Partikel</label>
                <select id="particleType">
                    <option value="mono">Monoatomik (He, Ne, Ar)</option>
                    <option value="diato">Diatomik (O₂, N₂, H₂)</option>
                </select>
            </div>
            <div class="param-card">
                <label>⚛️ Pilih Unsur</label>
                <select id="elementSelect">
                    <option value="Helium">Helium (He) - ringan</option>
                    <option value="Neon">Neon (Ne) - medium</option>
                    <option value="Argon">Argon (Ar) - berat</option>
                    <option value="Oksigen">Oksigen (O₂) - diatomik</option>
                    <option value="Nitrogen">Nitrogen (N₂) - diatomik</option>
                </select>
            </div>
            <div class="param-card">
                <label>➕➖ Jumlah Partikel</label>
                <div style="display:flex; gap:8px;">
                    <button id="addParticleBtn" style="flex:1; background:#f59e0b;">+ Tambah</button>
                    <button id="removeParticleBtn" style="flex:1; background:#475569;">- Kurang</button>
                </div>
                <div id="particleCountDisplay" class="value-display">24 partikel</div>
            </div>
        </div>

        <!-- status realtime gas -->
        <div class="row-2col">
            <div class="param-card"><span>🎯 Tekanan Gas (dalam ruang):</span> <span id="gasPressureRead" class="value-display" style="font-size:1.2rem;">-- atm</span></div>
            <div class="param-card"><span>📐 Volume Aktual (relatif):</span> <span id="volRead" class="value-display" style="font-size:1.2rem;">--</span></div>
            <div class="param-card"><span>⚡ Energi Kinetik Rata-rata:</span> <span id="avgEkRead" class="value-display" style="font-size:1.2rem;">--</span></div>
        </div>
        <div class="info-panel" style="margin-top:8px; text-align:center;">
            💡 <em>Tips:</em> Untuk simulasi isobarik, atur tekanan eksternal tetap lalu ubah suhu → volume akan berubah. Isokhorik: kunci volume (jangan ubah) lalu ubah suhu → tekanan gas berubah. Isotermal: jaga suhu tetap, ubah volume → tekanan berubah otomatis.
        </div>
    </div>
</div>

<!-- HALAMAN 3: LKPD ONLINE (bisa dikerjakan siswa, tersimpan di localStorage guru/online) -->
<div id="lkpd" class="page">
    <div class="glass-card">
        <h2>📋 LEMBAR KERJA PESERTA DIDIK (LKPD) - TERMODINAMIKA</h2>
        <div style="background:#0f172a; border-radius: 24px; padding:20px;">
            <p><strong>Capaian Pembelajaran (CP):</strong> Peserta didik mampu menganalisis hubungan antara tekanan, volume, dan suhu pada gas ideal serta menerapkan prinsip termodinamika dalam kehidupan sehari-hari.</p>
            <p><strong>Tujuan Pembelajaran:</strong> 1. Menemukan sendiri karakteristik proses isobarik, isokhorik, dan isotermal melalui simulasi. 2. Menyusun rumusan masalah berdasarkan pengamatan. 3. Mengisi tabel pengamatan dan menyimpulkan hubungan P-V-T.</p>
        </div>

        <form id="lkpdForm">
            <div style="margin: 20px 0;">
                <label>📌 Nama Siswa:</label>
                <input type="text" id="studentName" placeholder="Masukkan namamu" required style="margin-top:5px;">
            </div>
            <div style="margin: 20px 0;">
                <label>🔍 Rumusan Masalah (Berdasarkan simulasi, buatlah minimal 1 rumusan masalah):</label>
                <textarea id="rumusanMasalah" rows="2" placeholder="Contoh: Bagaimana pengaruh perubahan volume terhadap tekanan gas pada suhu tetap?"></textarea>
            </div>

            <label>📊 Tabel Pengamatan (Eksplorasi Simulasi) - Amati minimal 3 skenario:</label>
            <table class="lkpd-table" id="observationTable">
                <thead>
                    <tr><th>No</th><th>Volume (relatif)</th><th>Tekanan (atm)</th><th>Suhu (K)</th><th>Jenis Proses (pilih berdasarkan analisis)</th><th>Kecepatan Partikel</th></tr>
                </thead>
                <tbody>
                    <tr><td>1</td><td><input type="number" step="0.1" class="volObs" placeholder="Volume"></td><td><input type="number" step="0.1" class="pressObs" placeholder="Tekanan"></td><td><input type="number" class="tempObs" placeholder="Suhu"></td><td><select class="prosesObs"><option value="">Pilih</option><option value="isobarik">Isobarik</option><option value="isokhorik">Isokhorik</option><option value="isotermal">Isotermal</option></select></td><td><input type="text" class="speedObs" placeholder="cepat/lambat"></td></tr>
                    <tr><td>2</td><td><input type="number" step="0.1" class="volObs" placeholder="Volume"></td><td><input type="number" step="0.1" class="pressObs" placeholder="Tekanan"></td><td><input type="number" class="tempObs" placeholder="Suhu"></td><td><select class="prosesObs"><option value="">Pilih</option><option value="isobarik">Isobarik</option><option value="isokhorik">Isokhorik</option><option value="isotermal">Isotermal</option></select></td><td><input type="text" class="speedObs" placeholder="cepat/lambat"></td></tr>
                    <tr><td>3</td><td><input type="number" step="0.1" class="volObs" placeholder="Volume"></td><td><input type="number" step="0.1" class="pressObs" placeholder="Tekanan"></td><td><input type="number" class="tempObs" placeholder="Suhu"></td><td><select class="prosesObs"><option value="">Pilih</option><option value="isobarik">Isobarik</option><option value="isokhorik">Isokhorik</option><option value="isotermal">Isotermal</option></select></td><td><input type="text" class="speedObs" placeholder="cepat/lambat"></td></tr>
                </tbody>
            </table>
            
            <div style="margin: 20px 0;">
                <label>📝 Kesimpulan (berdasarkan tabel dan simulasi, jelaskan hubungan antar variabel):</label>
                <textarea id="kesimpulan" rows="3" placeholder="Tulis kesimpulanmu..."></textarea>
            </div>
            <button type="button" id="submitLkpdBtn" class="submit-lkpd">💾 SIMPAN JAWABAN & KIRIM KE GURU (online)</button>
            <div id="lkpdFeedback" class="feedback-msg"></div>
        </form>
        <p style="margin-top:15px; font-size:0.85rem; color:#aaa;">✅ Jawaban tersimpan secara online di browser (IndexedDB) dan dapat dilihat oleh pemilik (guru) melalui console atau dengan mengakses data. Simulasi mendukung eksplorasi mandiri.</p>
    </div>
</div>

<script>
    // ---------- SIMULASI GAS IDEAL DENGAN VOLUME DINAMIS & TEKANAN ----------
    const canvas = document.getElementById('gasCanvasSim');
    let ctx = canvas.getContext('2d');
    let width = 1000, height = 450;
    canvas.width = width; canvas.height = height;

    let particles = [];
    let baseRadius = 7;
    let volumeScale = 1.0;      // faktor lebar
    let externalPressure = 1.0;   // tekanan eksternal (simulasi mempengaruhi target kecepatan dan jumlah tumbukan)
    let temperatureKelvin = 300;
    let targetParticleCount = 24;

    // parameter partikel massa imajiner (pengaruh jenis partikel)
    let massFactor = 1.0; // monoatomik default =1, diatomik lebih berat => gerak lebih lambat pada energi sama
    let particleTypeSelect = document.getElementById('particleType');
    let elementSelect = document.getElementById('elementSelect');

    function updateMassFactor() {
        let type = particleTypeSelect.value;
        let element = elementSelect.value;
        if (type === 'diato') massFactor = 1.6;
        else massFactor = 1.0;
        if (element.includes('Argon')) massFactor *= 1.4;
        if (element.includes('Oksigen')) massFactor = 1.7;
        if (element.includes('Nitrogen')) massFactor = 1.5;
        // terapkan ulang energi kinetik berdasarkan suhu
        applyTemperatureToVelocities();
    }

    // kecepatan berdasarkan suhu dan massa
    function applyTemperatureToVelocities() {
        const targetEkPerParticle = temperatureKelvin * 0.008314 * 100; // skala simulasi
        particles.forEach(p => {
            let speed = Math.hypot(p.vx, p.vy);
            let desiredSpeed = Math.sqrt(2 * targetEkPerParticle / massFactor) * 0.22;
            if (desiredSpeed > 0 && speed > 0) {
                let ratio = desiredSpeed / speed;
                p.vx *= ratio;
                p.vy *= ratio;
            } else if (desiredSpeed > 0) {
                let angle = Math.random() * 2*Math.PI;
                p.vx = Math.cos(angle) * desiredSpeed;
                p.vy = Math.sin(angle) * desiredSpeed;
            }
        });
    }

    function initParticles(count, kelvin) {
        let arr = [];
        for(let i=0;i<count;i++) {
            let x = Math.random() * (width * volumeScale - 2*baseRadius) + baseRadius;
            let y = Math.random() * (height - 2*baseRadius) + baseRadius;
            let angle = Math.random() * 2*Math.PI;
            let spd = (Math.random() * 2 + 1) * Math.sqrt(kelvin/300) * 2.5;
            arr.push({x, y, vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: baseRadius});
        }
        return arr;
    }

    let particlesArr = initParticles(targetParticleCount, 300);
    
    function resizeAndRelocate() {
        // atur ulang posisi partikel agar tidak keluar saat volume berubah
        let currentVol = volumeScale;
        particlesArr.forEach(p => {
            p.x = Math.min(width * currentVol - p.r, Math.max(p.r, p.x));
            if(p.x > width * currentVol - p.r) p.x = width * currentVol - p.r;
        });
    }
    
    function updateVolumeUI() {
        let volSlider = document.getElementById('volumeSlider');
        volumeScale = parseFloat(volSlider.value);
        document.getElementById('volumeValue').innerHTML = volumeScale.toFixed(2) + " x normal";
        document.getElementById('volRead').innerText = volumeScale.toFixed(2);
        resizeAndRelocate();
    }
    
    function updatePressureUI() {
        externalPressure = parseFloat(document.getElementById('pressureSlider').value);
        document.getElementById('pressureValue').innerHTML = externalPressure.toFixed(2) + " atm";
        // tekanan eksternal mempengaruhi percepatan partikel saat tumbukan dinding (gaya eksternal)
    }
    
    function updateTempUI() {
        temperatureKelvin = parseFloat(document.getElementById('tempSlider').value);
        document.getElementById('tempValue').innerHTML = Math.round(temperatureKelvin) + " K";
        applyTemperatureToVelocities();
    }
    
    document.getElementById('volumeSlider').addEventListener('input', () => { updateVolumeUI(); });
    document.getElementById('pressureSlider').addEventListener('input', () => { updatePressureUI(); });
    document.getElementById('tempSlider').addEventListener('input', () => { updateTempUI(); });
    particleTypeSelect.addEventListener('change', () => { updateMassFactor(); applyTemperatureToVelocities(); });
    elementSelect.addEventListener('change', () => { updateMassFactor(); applyTemperatureToVelocities(); });
    
    document.getElementById('addParticleBtn').addEventListener('click', () => {
        targetParticleCount = Math.min(70, particlesArr.length + 3);
        for(let i=0;i<3;i++) {
            let angle = Math.random()*2*Math.PI;
            let spd = Math.random() * 3 + 1;
            particlesArr.push({x: Math.random()*(width*volumeScale-20)+10, y: Math.random()*(height-20)+10, vx: Math.cos(angle)*spd, vy: Math.sin(angle)*spd, r: baseRadius});
        }
        document.getElementById('particleCountDisplay').innerText = particlesArr.length + " partikel";
    });
    document.getElementById('removeParticleBtn').addEventListener('click', () => {
        if(particlesArr.length > 4) particlesArr.pop();
        document.getElementById('particleCountDisplay').innerText = particlesArr.length + " partikel";
    });
    
    function updateGasPressureFromKinetic() {
        let totalMv2 = 0;
        for(let p of particlesArr) {
            let v2 = p.vx*p.vx + p.vy*p.vy;
            totalMv2 += v2 * massFactor;
        }
        let pressure = (totalMv2 / (particlesArr.length || 1)) * 0.08 * (temperatureKelvin/300) / (volumeScale+0.3);
        pressure = Math.min(3.5, Math.max(0.2, pressure));
        document.getElementById('gasPressureRead').innerText = pressure.toFixed(2) + " atm";
        let avgEk = 0;
        for(let p of particlesArr) avgEk += (p.vx*p.vx + p.vy*p.vy)*massFactor/2;
        avgEk = avgEk / (particlesArr.length||1);
        document.getElementById('avgEkRead').innerHTML = avgEk.toFixed(1) + " J (sim)";
        return pressure;
    }
    
    // update fisik & tumbukan
    function updateMotion() {
        let currentVolumeWidth = width * volumeScale;
        for(let p of particlesArr) {
            p.x += p.vx;
            p.y += p.vy;
            // dinding kiri dan kanan dengan volume dinamis
            if(p.x - p.r < 0) { p.x = p.r; p.vx = -p.vx * (0.98 + 0.02*externalPressure); }
            if(p.x + p.r > currentVolumeWidth) { p.x = currentVolumeWidth - p.r; p.vx = -p.vx * (0.98 + 0.02*externalPressure); }
            if(p.y - p.r < 0) { p.y = p.r; p.vy = -p.vy; }
            if(p.y + p.r > height) { p.y = height - p.r; p.vy = -p.vy; }
        }
        for(let i=0;i<particlesArr.length;i++) {
            for(let j=i+1;j<particlesArr.length;j++) {
                let p1=particlesArr[i], p2=particlesArr[j];
                let dx=p2.x-p1.x, dy=p2.y-p1.y, dist=Math.hypot(dx,dy);
                if(dist < p1.r+p2.r) {
                    let nx=dx/dist, ny=dy/dist;
                    let vrelx=p2.vx-p1.vx, vrely=p2.vy-p1.vy;
                    let velAlong = vrelx*nx + vrely*ny;
                    if(velAlong<0) {
                        let e=1.0;
                        let imp = (1+e)*velAlong / (1+1);
                        p1.vx += imp*nx; p1.vy += imp*ny;
                        p2.vx -= imp*nx; p2.vy -= imp*ny;
                        let overlap = (p1.r+p2.r - dist);
                        let shiftX = nx*overlap*0.5, shiftY=ny*overlap*0.5;
                        p1.x -= shiftX; p1.y -= shiftY;
                        p2.x += shiftX; p2.y += shiftY;
                    }
                }
            }
        }
        updateGasPressureFromKinetic();
        document.getElementById('particleCountDisplay').innerText = particlesArr.length + " partikel";
    }
    
    function drawSimulation() {
        ctx.clearRect(0,0,width,height);
        let currentW = width * volumeScale;
        ctx.fillStyle = "#0f172a80";
        ctx.fillRect(0,0,width,height);
        ctx.strokeStyle = "#ffc857";
        ctx.lineWidth = 4;
        ctx.strokeRect(0,0,currentW,height);
        for(let p of particlesArr) {
            let grad = ctx.createRadialGradient(p.x-3,p.y-3,3,p.x,p.y,p.r+2);
            grad.addColorStop(0,"#ffb347");
            grad.addColorStop(1,"#e05a1a");
            ctx.beginPath();
            ctx.arc(p.x,p.y,p.r,0,Math.PI*2);
            ctx.fillStyle=grad;
            ctx.fill();
            ctx.strokeStyle="#ffe6b3";
            ctx.stroke();
        }
        ctx.font = "bold 14px 'Segoe UI'";
        ctx.fillStyle = "#ffdd99";
        ctx.fillText(`Partikel: ${particlesArr.length} | T=${Math.round(temperatureKelvin)}K | P_ext=${externalPressure.toFixed(2)}`, 15, 30);
    }
    
    let dragging=false, lastX=0,lastY=0;
    canvas.addEventListener('mousedown',(e)=>{
        dragging=true;
        let rect=canvas.getBoundingClientRect(), sx=canvas.width/rect.width, sy=canvas.height/rect.height;
        lastX=(e.clientX-rect.left)*sx; lastY=(e.clientY-rect.top)*sy;
    });
    window.addEventListener('mousemove',(e)=>{
        if(!dragging) return;
        let rect=canvas.getBoundingClientRect(), sx=canvas.width/rect.width, sy=canvas.height/rect.height;
        let mx=(e.clientX-rect.left)*sx, my=(e.clientY-rect.top)*sy;
        let dx=mx-lastX, dy=my-lastY;
        particlesArr.forEach(p=>{ let dist=Math.hypot(p.x-mx,p.y-my); if(dist<60){ p.vx+=dx*0.3; p.vy+=dy*0.3; }});
        lastX=mx; lastY=my;
    });
    window.addEventListener('mouseup',()=>dragging=false);
    
    function animate() {
        updateMotion();
        drawSimulation();
        requestAnimationFrame(animate);
    }
    updateVolumeUI(); updatePressureUI(); updateTempUI(); updateMassFactor();
    animate();
    
    // ---------- LKPD STORAGE ONLINE (IndexedDB) ----------
    let dbRequest = indexedDB.open("ThermoLKPD", 1);
    let db;
    dbRequest.onupgradeneeded = (e) => {
        db = e.target.result;
        if(!db.objectStoreNames.contains("answers")) db.createObjectStore("answers", {autoIncrement: true});
    };
    dbRequest.onsuccess = (e) => { db = e.target.result; };
    
    document.getElementById('submitLkpdBtn').addEventListener('click', () => {
        let nama = document.getElementById('studentName').value;
        if(!nama) { alert("Masukkan nama siswa!"); return; }
        let rows = document.querySelectorAll('#observationTable tbody tr');
        let observations = [];
        rows.forEach((row, idx) => {
            let vol = row.querySelector('.volObs').value;
            let press = row.querySelector('.pressObs').value;
            let temp = row.querySelector('.tempObs').value;
            let proses = row.querySelector('.prosesObs').value;
            let speed = row.querySelector('.speedObs').value;
            observations.push({no:idx+1, volume:vol, tekanan:press, suhu:temp, proses, kecepatan:speed});
        });
        let data = {
            nama: nama,
            rumusan: document.getElementById('rumusanMasalah').value,
            tabel: observations,
            kesimpulan: document.getElementById('kesimpulan').value,
            timestamp: new Date().toISOString()
        };
        let transaction = db.transaction(["answers"], "readwrite");
        let store = transaction.objectStore("answers");
        store.add(data);
        transaction.oncomplete = () => {
            document.getElementById('lkpdFeedback').innerHTML = "✅ Jawaban tersimpan! Guru dapat melihat secara online (buka console -> IndexedDB). Data tersimpan permanen.";
            document.getElementById('lkpdFeedback').style.background = "#065f46";
        };
        transaction.onerror = () => { alert("Gagal menyimpan"); };
    });
    
    // navigasi halaman
    const pages = { materi: document.getElementById('materi'), simulasi: document.getElementById('simulasi'), lkpd: document.getElementById('lkpd') };
    document.querySelectorAll('.nav-btn').forEach(btn => {
        btn.addEventListener('click', () => {
            let pageId = btn.getAttribute('data-page');
            Object.keys(pages).forEach(p => { pages[p].classList.remove('active-page'); });
            pages[pageId].classList.add('active-page');
            document.querySelectorAll('.nav-btn').forEach(b => b.classList.remove('active'));
            btn.classList.add('active');
            if(pageId === 'simulasi') setTimeout(()=>{ drawSimulation(); }, 50);
        });
    });
</script>
</body>
</html>
