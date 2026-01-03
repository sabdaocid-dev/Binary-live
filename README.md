<!DOCTYPE html>
<html lang="id">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ALICORN V13 | PROFESSIONAL TRADING ENGINE</title>
    
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>

    <style>
        :root { --gold: #f1c40f; --dark: #050505; --card: #0a0a0a; --green: #00ff88; --red: #ff3333; }
        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; }
        body { background: var(--dark); color: #fff; height: 100vh; overflow: hidden; }

        /* --- SCREEN: DISCLAIMER --- */
        #disclaimer-screen {
            position: fixed; inset: 0; background: var(--dark); z-index: 9999;
            display: flex; align-items: center; justify-content: center; padding: 20px;
        }
        .disclaimer-box {
            max-width: 500px; background: var(--card); border: 2px solid var(--red);
            padding: 30px; border-radius: 20px; text-align: center;
        }
        .disclaimer-box h1 { color: var(--red); margin-bottom: 15px; font-size: 24px; }
        .disclaimer-box p { font-size: 14px; color: #ccc; line-height: 1.6; margin-bottom: 25px; }
        .btn-agree { 
            background: var(--red); color: white; border: none; padding: 15px 40px; 
            border-radius: 10px; font-weight: bold; cursor: pointer; width: 100%;
        }

        /* --- SCREEN: AUTH --- */
        #auth-screen {
            position: fixed; inset: 0; background: radial-gradient(circle, #1a1a1a 0%, #050505 100%);
            z-index: 9998; display: none; flex-direction: column; align-items: center; justify-content: center;
        }
        .auth-card { text-align: center; padding: 40px; border: 1px solid var(--gold); border-radius: 30px; background: rgba(10,10,10,0.8); }
        .logo-text { font-size: 32px; font-weight: 900; color: var(--gold); letter-spacing: 5px; margin-bottom: 5px; }
        .logo-sub { font-size: 10px; color: #666; letter-spacing: 2px; margin-bottom: 30px; }
        .btn-auth-trigger { 
            background: var(--gold); color: #000; border: none; padding: 18px 40px; 
            border-radius: 15px; font-weight: 900; cursor: pointer; width: 100%;
            box-shadow: 0 10px 30px rgba(241, 196, 15, 0.2);
        }

        /* --- SCREEN: MAIN ENGINE --- */
        #main-engine { display: none; height: 100vh; flex-direction: column; padding: 15px; }
        .header { display: flex; justify-content: space-between; align-items: center; background: #000; padding: 12px; border-radius: 15px; border: 1px solid #222; }
        .time-box .val { color: var(--gold); font-family: monospace; font-size: 16px; }
        
        .display-area { flex-grow: 1; display: flex; flex-direction: column; align-items: center; justify-content: center; position: relative; }
        .score-label { font-size: 11px; color: #555; letter-spacing: 3px; }
        #visual-score { font-size: 120px; font-weight: 900; margin: -15px 0; transition: 0.4s; color: #222; }
        #signal-msg { font-size: 18px; font-weight: 800; letter-spacing: 1px; }

        .progress-wrap { width: 80%; height: 8px; background: #111; border-radius: 10px; margin: 30px 0; overflow: hidden; border: 1px solid #222; }
        #progress-fill { width: 50%; height: 100%; background: var(--gold); transition: 0.6s cubic-bezier(0.175, 0.885, 0.32, 1.275); }
        
        .history-panel { height: 180px; background: #000; border-radius: 20px; padding: 15px; overflow-y: auto; border: 1px solid #111; }
        .log-item { display: flex; justify-content: space-between; font-size: 11px; padding: 8px; border-bottom: 1px solid #111; }

        .hidden { display: none !important; }
        .btn-logout { background: none; border: 1px solid #333; color: #555; padding: 5px 10px; border-radius: 5px; font-size: 10px; }
    </style>
</head>
<body>

    <div id="disclaimer-screen">
        <div class="disclaimer-box">
            <h1>PERINGATAN RISIKO</h1>
            <p>
                Trading aset kripto dan indeks memiliki risiko tinggi. Algoritma <b>ALICORN V13</b> adalah alat bantu analisis, bukan jaminan keuntungan. Dengan melanjutkan, Anda setuju bahwa segala kerugian finansial adalah tanggung jawab pribadi Anda secara penuh.
            </p>
            <button class="btn-agree" onclick="acceptDisclaimer()">SAYA MENGERTI & SETUJU</button>
        </div>
    </div>

    <div id="auth-screen">
        <div class="auth-card">
            <div class="logo-text">ALICORN <span style="color:#fff">V13</span></div>
            <div class="logo-sub">MEASURED MOVE PRO ENGINE</div>
            <p style="color: #888; font-size: 13px; margin-bottom: 25px;">Otorisasi diperlukan untuk mengakses sinyal.</p>
            <button class="btn-auth-trigger" onclick="netlifyIdentity.open()">MASUK KE PORTAL</button>
            <p style="margin-top: 20px; font-size: 10px; color: #333;">PROTECTED BY NETLIFY ENCRYPTION</p>
        </div>
    </div>

    <div id="main-engine">
        <div class="header">
            <div class="time-box">
                <span style="font-size: 8px; color: #666; display: block;">SERVER TIME (WITA)</span>
                <span id="live-clock" class="val">00:00:00</span>
            </div>
            <button class="btn-logout" onclick="netlifyIdentity.logout()">LOGOUT</button>
        </div>

        <div class="display-area">
            <span class="score-label">CONFIDENCE SCORE</span>
            <div id="visual-score">0</div>
            <div id="signal-msg">INITIALIZING...</div>
            
            <div class="progress-wrap">
                <div id="progress-fill"></div>
            </div>
            
            <div id="price-target" style="color: var(--gold); font-weight: bold; font-family: monospace;">TARGET: --.---</div>
        </div>

        <div class="history-panel">
            <div style="font-size: 10px; color: #444; margin-bottom: 8px; letter-spacing: 1px;">LIVE ACTIVITY LOG</div>
            <div id="log-container"></div>
        </div>
    </div>

    <script>
        // --- LOGIKA ALGORITMA (+9 Score Confidence) ---
        const PROXY = "https://api.codetabs.com/v1/proxy?quest=";
        const DATA_API = encodeURIComponent("https://tradingpoin.com/chart/api/data?type=json&pair_code=CRYIDX.B&timeframe=5&source=Binomo&val=Z-CRY/IDX&last=60");
        let isAuthorized = false;

        // 1. Disclaimer Logic
        function acceptDisclaimer() {
            localStorage.setItem('alicorn_agree', 'true');
            document.getElementById('disclaimer-screen').style.display = 'none';
            checkAuth();
        }

        // 2. Auth Logic
        function checkAuth() {
            const user = netlifyIdentity.currentUser();
            if (user) {
                showEngine();
            } else {
                document.getElementById('auth-screen').style.display = 'flex';
            }
        }

        netlifyIdentity.on('login', user => {
            netlifyIdentity.close();
            showEngine();
        });

        netlifyIdentity.on('logout', () => location.reload());

        function showEngine() {
            document.getElementById('auth-screen').style.display = 'none';
            document.getElementById('main-engine').style.display = 'flex';
            startEngine();
        }

        // 3. Engine Logic
        function startEngine() {
            setInterval(updateData, 5000); // Auto-scan tiap 5 detik
            setInterval(() => {
                document.getElementById('live-clock').innerText = new Date().toLocaleTimeString('id-ID', {hour12:false});
            }, 1000);
        }

        async function updateData() {
            try {
                const res = await fetch(PROXY + DATA_API);
                const json = await res.json();
                processLogic(json.data);
            } catch (e) { console.error("Connection lost"); }
        }

        function processLogic(data) {
            const close = data.map(d => parseFloat(d[4]));
            const high = data.map(d => parseFloat(d[2]));
            const low = data.map(d => parseFloat(d[3]));
            const current = close[close.length - 1];

            // Algoritma Measured Move
            const h45 = Math.max(...high.slice(-45, -10));
            const l45 = Math.min(...low.slice(-45, -10));
            const move = h45 - l45;
            
            let score = 0;
            let status = "SIDEWAYS";
            let color = "#444";

            // Kalkulasi Skor
            if (current > (h45 - (move * 0.2))) {
                score = 9; status = "HIGH CONVICTION BUY"; color = "#00ff88";
            } else if (current < (l45 + (move * 0.2))) {
                score = -9; status = "HIGH CONVICTION SELL"; color = "#ff3333";
            } else {
                score = 1; status = "WAITING SIGNAL"; color = "#f1c40f";
            }

            // Update UI
            const sEl = document.getElementById('visual-score');
            sEl.innerText = (score > 0 ? "+" : "") + score;
            sEl.style.color = color;
            
            const mEl = document.getElementById('signal-msg');
            mEl.innerText = status;
            mEl.style.color = color;

            document.getElementById('progress-fill').style.width = (50 + (score * 5)) + "%";
            document.getElementById('progress-fill').style.background = color;
            document.getElementById('price-target').innerText = `TARGET: ${(current + (score * 0.5)).toFixed(2)}`;
        }

        // Init App
        window.onload = () => {
            if(localStorage.getItem('alicorn_agree')) {
                document.getElementById('disclaimer-screen').style.display = 'none';
                checkAuth();
            }
        };
    </script>
</body>
</html>
![1000087617](https://github.com/user-attachments/assets/085fb333-36f3-4189-8bb2-072ec5bd2326)
