<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>FF Sensi Hub | 2026 Pro Settings</title>
    <style>
        :root {
            --neon-orange: #ff6a00;
            --bg-dark: #0f0f12;
            --card-bg: #1e1e26;
            --accent-blue: #00d2ff;
        }

        body {
            font-family: 'Inter', system-ui, -apple-system, sans-serif;
            background-color: var(--bg-dark);
            color: #e0e0e0;
            margin: 0;
            padding: 20px;
            display: flex;
            flex-direction: column;
            align-items: center;
        }

        .container {
            width: 100%;
            max-width: 600px;
        }

        header {
            text-align: center;
            padding: 40px 0;
        }

        header h1 {
            font-size: 2.5rem;
            margin: 0;
            background: linear-gradient(to right, var(--neon-orange), #ffc107);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            text-transform: uppercase;
            letter-spacing: 2px;
        }

        .search-section {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 20px;
            border: 1px solid #333;
            margin-bottom: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.5);
        }

        label { display: block; margin-bottom: 10px; font-size: 0.9rem; color: #888; }

        select {
            width: 100%;
            padding: 15px;
            border-radius: 12px;
            border: 2px solid #333;
            background: #121217;
            color: white;
            font-size: 1rem;
            outline: none;
            transition: border 0.3s;
        }

        select:focus { border-color: var(--neon-orange); }

        .result-card {
            background: var(--card-bg);
            border-radius: 24px;
            padding: 30px;
            display: none;
            animation: fadeIn 0.5s ease;
            border-top: 4px solid var(--neon-orange);
        }

        @keyframes fadeIn { from { opacity: 0; transform: translateY(10px); } to { opacity: 1; transform: translateY(0); } }

        .device-header {
            text-align: center;
            margin-bottom: 25px;
        }

        .device-header h2 { margin: 0; color: #fff; font-size: 1.5rem; }

        .grid-stats {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 15px;
        }

        .stat-box {
            background: #121217;
            padding: 15px;
            border-radius: 15px;
            text-align: center;
        }

        .stat-label { font-size: 0.75rem; text-transform: uppercase; color: #777; margin-bottom: 5px; }
        .stat-val { font-size: 1.4rem; font-weight: 800; color: var(--neon-orange); }

        .pro-tips {
            margin-top: 25px;
            padding-top: 20px;
            border-top: 1px solid #333;
        }

        .tip-item {
            background: rgba(0, 210, 255, 0.1);
            border-left: 4px solid var(--accent-blue);
            padding: 12px;
            border-radius: 4px;
            margin-bottom: 10px;
            font-size: 0.9rem;
        }

        footer { margin-top: 40px; text-align: center; font-size: 0.8rem; color: #555; }
    </style>
</head>
<body>

    <div class="container">
        <header>
            <h1>FF Sensi Hub</h1>
            <p>Select your device for "Only Red" settings</p>
        </header>

        <div class="search-section">
            <label>CHOOSE DEVICE CATEGORY</label>
            <select id="phoneSelect" onchange="updateSensi()">
                <option value="">-- Select Device --</option>
                
                <optgroup label="Samsung A-Series">
                    <option value="samsung_a_low">Samsung A03 / A10 / A12</option>
                    <option value="samsung_a_mid">Samsung A23 / A34 / A54</option>
                    <option value="samsung_a_high">Samsung A73 / Galaxy A80</option>
                </optgroup>

                <optgroup label="Classic iPhones (Low End)">
                    <option value="iphone_old">iPhone 6s / 7 / 8 (Plus)</option>
                    <option value="iphone_se">iPhone SE (All Generations)</option>
                    <option value="iphone_x">iPhone X / XR / XS</option>
                </optgroup>

                <optgroup label="Budget Androids">
                    <option value="low_ram">Generic 2GB - 3GB RAM Phones</option>
                    <option value="redmi_budget">Redmi 9 / 10 / 12C</option>
                    <option value="infinix_tecno">Infinix Hot / Tecno Spark</option>
                </optgroup>
            </select>
        </div>

        <div id="result" class="result-card">
            <div class="device-header">
                <h2 id="dispModel">Device Name</h2>
            </div>

            <div class="grid-stats">
                <div class="stat-box"><div class="stat-label">General</div><div class="stat-val" id="s1">0</div></div>
                <div class="stat-box"><div class="stat-label">Red Dot</div><div class="stat-val" id="s2">0</div></div>
                <div class="stat-box"><div class="stat-label">2x Scope</div><div class="stat-val" id="s3">0</div></div>
                <div class="stat-box"><div class="stat-label">4x Scope</div><div class="stat-val" id="s4">0</div></div>
                <div class="stat-box"><div class="stat-label">Sniper</div><div class="stat-val" id="s5">0</div></div>
                <div class="stat-box"><div class="stat-label">Free Look</div><div class="stat-val" id="s6">0</div></div>
            </div>

            <div class="pro-tips">
                <div class="tip-item"> <strong>Fire Button:</strong> <span id="fireButton">45%</span></div>
                <div class="tip-item"> <strong>DPI Recommendation:</strong> <span id="dpiVal">Standard</span></div>
            </div>
        </div>

        <footer>Updated for 2026 Game Version</footer>
    </div>

    <script>
        const data = {
            "samsung_a_low": { g: 100, r: 98, x2: 100, x4: 100, snp: 50, fl: 80, btn: "55%", dpi: "411" },
            "samsung_a_mid": { g: 98, r: 95, x2: 92, x4: 90, snp: 45, fl: 75, btn: "48%", dpi: "450" },
            "samsung_a_high": { g: 95, r: 90, x2: 88, x4: 85, snp: 40, fl: 70, btn: "45%", dpi: "480" },
            "iphone_old": { g: 100, r: 100, x2: 95, x4: 95, snp: 50, fl: 90, btn: "42%", dpi: "Default" },
            "iphone_se": { g: 98, r: 95, x2: 90, x4: 90, snp: 45, fl: 85, btn: "45%", dpi: "Default" },
            "iphone_x": { g: 95, r: 92, x2: 88, x4: 88, snp: 40, fl: 80, btn: "46%", dpi: "Default" },
            "low_ram": { g: 100, r: 100, x2: 100, x4: 100, snp: 60, fl: 100, btn: "60%", dpi: "390" },
            "redmi_budget": { g: 100, r: 98, x2: 98, x4: 95, snp: 50, fl: 85, btn: "52%", dpi: "430" },
            "infinix_tecno": { g: 100, r: 100, x2: 95, x4: 95, snp: 55, fl: 90, btn: "50%", dpi: "411" }
        };

        function updateSensi() {
            const val = document.getElementById('phoneSelect').value;
            const res = document.getElementById('result');
            
            if(val) {
                const item = data[val];
                document.getElementById('s1').innerText = item.g;
                document.getElementById('s2').innerText = item.r;
                document.getElementById('s3').innerText = item.x2;
                document.getElementById('s4').innerText = item.x4;
                document.getElementById('s5').innerText = item.snp;
                document.getElementById('s6').innerText = item.fl;
                document.getElementById('fireButton').innerText = item.btn;
                document.getElementById('dpiVal').innerText = item.dpi;
                document.getElementById('dispModel').innerText = document.getElementById('phoneSelect').options[document.getElementById('phoneSelect').selectedIndex].text;
                res.style.display = 'block';
            } else {
                res.style.display = 'none';
            }
        }
    </script>
</body>
</html>
