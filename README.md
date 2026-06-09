<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, minimum-scale=0.5, maximum-scale=3.0">
    <title>ትርፌ የሱቅ መቆጣጠሪያ አፕሊኬሽን - Ultimate v9.2</title>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2pdf.js/0.10.1/html2pdf.bundle.min.js"></script>

    <style>
        :root {
            --bg-color: #0b0f19;
            --card-bg: #151f32;
            --text-color: #38bdf8;
            --accent-color: #38bdf8;
            --success-color: #4ade80;
            --danger-color: #f87171;
            --warning-color: #fbbf24;
            --purple-color: #c084fc;
            --border-color: rgba(255, 255, 255, 0.08);
        }

        .theme-deepblue { --bg-color: #0b0f19; --card-bg: #151f32; --accent-color: #38bdf8; --success-color: #4ade80; }
        .theme-emerald { --bg-color: #061e1a; --card-bg: #0b3c34; --accent-color: #2ec4b6; --success-color: #4ade80; }
        .theme-purple { --bg-color: #1a0933; --card-bg: #2d1254; --accent-color: #a29bfe; --success-color: #00cec9; }
        .theme-charcoal { --bg-color: #121212; --card-bg: #1e1e1e; --accent-color: #9b59b6; --success-color: #2ecc71; }

        * { 
            box-sizing: border-box; 
            margin: 0; 
            padding: 0; 
            font-family: system-ui, -apple-system, sans-serif;
        }
        
        html, body {
            width: 100%;
            min-height: 100vh;
            background-color: var(--bg-color); 
            color: var(--text-color); 
            padding: env(safe-area-inset-top) env(safe-area-inset-right) env(safe-area-inset-bottom) env(safe-area-inset-left);
            font-size: calc(12px + 0.4vw); 
            -webkit-text-size-adjust: 100%;
        }
        
        .container { 
            width: 100%; 
            max-width: 1200px; 
            margin: 0 auto; 
            padding: 12px;
        }
        
        .hidden { display: none !important; }
        
        .text-success { color: var(--success-color) !important; }
        .text-danger { color: var(--danger-color) !important; }
        .text-warning { color: var(--warning-color) !important; }
        .text-purple { color: var(--purple-color) !important; }
        
        .gateway-box, .login-box {
            width: 100%;
            max-width: 450px; 
            margin: 40px auto;
            background-color: var(--card-bg); 
            padding: 30px 25px;
            border-radius: 16px; 
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            text-align: center; 
            border: 1px solid var(--border-color);
        }
        .gateway-box h2, .login-box h2 { color: var(--success-color); font-size: 1.6rem; margin-bottom: 8px; font-weight: 800; }
        .gateway-box p, .login-box p { color: #94a3b8; font-size: 0.85rem; margin-bottom: 20px; }

        .gateway-btn {
            background-color: rgba(255, 255, 255, 0.03);
            border: 1px solid var(--border-color);
            color: var(--text-color);
            padding: 15px;
            margin: 10px 0;
            width: 100%;
            border-radius: 10px;
            font-size: 1.05rem;
            font-weight: bold;
            cursor: pointer;
            text-align: left;
            display: flex;
            align-items: center;
            gap: 12px;
            transition: all 0.2s;
        }
        .gateway-btn:hover {
            background-color: var(--accent-color);
            color: #000;
            transform: translateY(-2px);
        }

        input, select {
            width: 100%; 
            padding: 12px; 
            margin: 8px 0;
            border-radius: 8px; 
            border: 1px solid #3a506b;
            background-color: rgba(0,0,0,0.3); 
            color: white; 
            font-size: 1rem;
            outline: none;
        }
        input:focus, select:focus { border-color: var(--accent-color); }

        .welcome-header {
            text-align: center; 
            margin-bottom: 20px; 
            padding: 20px;
            background: linear-gradient(135deg, var(--card-bg), #0f172a);
            border-radius: 12px; 
            border: 1px solid var(--border-color);
        }
        .welcome-header h1 { color: var(--success-color); margin-bottom: 5px; font-size: 1.6rem; font-weight: 800; }
        .welcome-header p { color: var(--accent-color); font-size: 0.9rem; }

        .session-badge {
            display: block; 
            text-align: center; 
            color: var(--warning-color); 
            font-size: 0.85rem; 
            margin-bottom: 15px; 
            background: rgba(251, 191, 36, 0.08);
            padding: 10px; 
            border-radius: 8px; 
            border: 1px dashed var(--warning-color);
        }
        
        .dashboard {
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(140px, 1fr)); 
            gap: 12px; 
            margin-bottom: 20px;
        }
        .card {
            background-color: var(--card-bg); 
            padding: 15px 12px; 
            border-radius: 10px; 
            text-align: center; 
            border: 1px solid var(--border-color); 
            border-left: 4px solid var(--accent-color);
        }
        .card h3 { margin: 0 0 6px 0; font-size: 0.75rem; color: #94a3b8; text-transform: uppercase; }
        .card p { margin: 0; font-size: 1.2rem; font-weight: bold; color: var(--success-color); word-break: break-all; }

        .actions { 
            display: grid; 
            grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); 
            gap: 10px; 
            margin-bottom: 20px; 
        }
        button { 
            padding: 12px 10px; 
            border: none; 
            border-radius: 8px; 
            font-weight: bold; 
            cursor: pointer; 
            font-size: 0.9rem;
            display: inline-flex; 
            align-items: center; 
            justify-content: center; 
            gap: 4px; 
        }
        .btn-block { width: 100%; }
        .btn-add { background-color: var(--accent-color); color: #000; }
        .btn-sell { background-color: var(--success-color); color: #000; }
        .btn-expense { background-color: #f43f5e; color: #fff; }
        .btn-credit { background-color: #f59e0b; color: #000; }
        .btn-draw { background-color: var(--purple-color); color: #000; }
        .btn-config { background-color: #64748b; color: #fff; }
        .btn-next-day { background-color: #a855f7; color: #fff; }
        .btn-clear { background-color: var(--danger-color); color: #000; }
        .btn-sm { padding: 6px 10px; font-size: 0.8rem; border-radius: 6px; }

        .section-box { 
            background-color: var(--card-bg); 
            padding: 15px; 
            border-radius: 12px; 
            margin-bottom: 20px; 
            border: 1px solid var(--border-color);
        }
        .section-box h2 { font-size: 1.2rem; margin-bottom: 12px; border-bottom: 2px solid var(--border-color); padding-bottom: 6px; color: var(--accent-color); }

        .table-responsive { 
            width: 100%; 
            overflow-x: auto; 
            border-radius: 8px; 
            border: 1px solid var(--border-color); 
            -webkit-overflow-scrolling: touch;
        }
        table { 
            width: 100%; 
            border-collapse: collapse; 
            min-width: 500px; 
            background-color: var(--card-bg) !important; 
        }
        th, td { 
            padding: 12px 10px; 
            text-align: left; 
            border-bottom: 1px solid var(--border-color); 
            white-space: nowrap; 
            font-size: 0.9rem; 
            color: var(--text-color) !important; 
        }
        th { 
            background-color: rgba(0,0,0,0.5) !important; 
            color: var(--accent-color) !important; 
            font-weight: 600; 
        }

        .low-stock-row { background-color: rgba(248, 113, 113, 0.1) !important; }
        .low-stock-badge { background: #f87171 !important; color: #000 !important; padding: 2px 6px; border-radius: 4px; font-weight: bold; font-size: 0.75rem; margin-left: 5px; }

        .badge-danger { background: var(--danger-color); color: #000; padding: 2px 5px; border-radius: 4px; font-size: 0.75rem; font-weight: bold; }
        .badge-success { background: var(--success-color); color: #000; padding: 2px 5px; border-radius: 4px; font-size: 0.75rem; font-weight: bold; }

        .grid-3 { display: grid; grid-template-columns: 1fr; gap: 15px; margin-bottom: 20px; }
        @media(min-width: 768px) {
            .grid-3 { grid-template-columns: repeat(3, 1fr); }
            .btn-clear { grid-column: span 2; }
        }

        .profile-data { background: rgba(0,0,0,0.2); padding: 10px; border-radius: 8px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; border: 1px solid var(--border-color); font-size: 0.9rem; }
        .profile-data label { font-weight: bold; color: #94a3b8; }
        .profile-data span { color: var(--warning-color); font-weight: bold; }

        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.85); backdrop-filter: blur(4px); display: flex; justify-content: center; align-items: center; z-index: 1000; padding: 15px; }
        .modal-card { background-color: var(--card-bg); border-radius: 14px; width: 100%; max-width: 420px; padding: 20px; box-shadow: 0 15px 30px rgba(0,0,0,0.5); border: 1px solid var(--border-color); overflow-y: auto; max-height: 90vh; }
        .modal-header { margin-bottom: 12px; border-bottom: 1px solid var(--border-color); padding-bottom: 8px; }
        .modal-header h3 { font-size: 1.2rem; color: var(--success-color); }
        .modal-body { margin-bottom: 15px; }
        .modal-footer { display: flex; justify-content: flex-end; gap: 8px; }
        
        .search-container { margin-bottom: 12px; }
        .search-input { border: 1px solid var(--accent-color); background: rgba(56, 189, 248, 0.05); }
        .offline-tag { background: #ef4444; color: white; padding: 2px 6px; border-radius: 4px; font-size: 0.75rem; font-weight: bold; animation: blink 1.5s infinite; }
        @keyframes blink { 0% { opacity: 0.4; } 50% { opacity: 1; } 100% { opacity: 0.4; } }

        /* ለደረሰኝ ውብ ቴብል ዲዛይን ስታይል */
        .receipt-container {
            background-color: #ffffff;
            color: #333333;
            padding: 20px;
            border-radius: 8px;
            font-family: 'Courier New', Courier, monospace;
            box-shadow: 0 0 10px rgba(0,0,0,0.1);
        }
        .receipt-header { text-align: center; margin-bottom: 15px; border-bottom: 2px dashed #333; padding-bottom: 10px; }
        .receipt-header h4 { margin: 0; font-size: 1.3rem; color: #111; text-transform: uppercase; }
        .receipt-header p { margin: 4px 0; font-size: 0.85rem; color: #565656; }
        .receipt-table { width: 100%; margin-top: 10px; border-collapse: collapse; }
        .receipt-table th, .receipt-table td { color: #222 !important; padding: 8px 5px; font-size: 0.85rem; border-bottom: 1px dashed #ddd; text-align: left; }
        .receipt-table th { background-color: #f5f5f5 !important; font-weight: bold; }
        .receipt-summary { margin-top: 15px; border-top: 2px dashed #333; padding-top: 8px; text-align: right; font-size: 0.95rem; font-weight: bold; color: #111; }
        .receipt-footer { text-align: center; margin-top: 20px; font-size: 0.8rem; color: #777; font-style: italic; }
        .receipt-actions-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 8px; margin-top: 12px; }
    </style>
</head>
<body class="theme-deepblue">

<div class="container">

    <div id="welcomeGateway" class="gateway-box">
        <h2>እንኳን ደህና መጡ! <span id="syncIndicator" class="hidden offline-tag">Offline ሞድ</span></h2>
        <p>እባክዎ መተግበሪያውን በምን ሁኔታ መጠቀም እንደሚፈልጉ ይምረጡ፦</p>
        <button class="gateway-btn" onclick="switchView('buyerPage')">🛍️ እኔ ገዢ ነኝ </button>
        <button class="gateway-btn" onclick="showLoginSection('admin')">👑 እኔ አከራይ ነኝ </button>
        <button class="gateway-btn" onclick="showLoginSection('merchant')">💼 እኔ ሱቅ ባለቤት ነኝ </button>
        <button class="gateway-btn" onclick="showLoginSection('staff')">🛠️ እኔ ሰራተኛ ነኝ </button>
    </div>

    <div id="loginPage" class="login-box hidden">
        <h2 id="loginTitle">ትርፌ አፕ (Tirfe Tech)</h2>
        <p id="loginDesc">የኪራይና የሱቅ መቆጣጠሪያ ማዕከላዊ ሲስተም v9.2</p>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል / ጊዜያዊ ኮድ">
        <button class="btn-sell btn-block" onclick="handleLogin()">🔒 ደህንነቱ የተጠበቀ መግቢያ</button>
        <button class="btn-config btn-block" style="margin-top: 8px;" onclick="goToGateway()">⬅️ ወደ ኋላ ተመለስ</button>
        <p id="loginError" style="color: var(--danger-color); margin-top: 15px; font-size: 0.9rem; font-weight: bold;"></p>
    </div>

    <div id="buyerPage" class="hidden">
        <div class="welcome-header">
            <h1>🛍️ የዕቃዎችና የሱቆች ማውጫ መድረክ</h1>
            <p>በአቅራቢያዎ ያሉ ሱቆችን ምርቶች፣ ዋጋዎች እና አድራሻ እዚህ ያግኙ。</p>
            <button class="btn-expense" style="margin-top:10px;" onclick="goToGateway()">⬅️ ወደ ዋናው ምርጫ ተመለስ</button>
        </div>
        <div class="section-box">
            <h2>🛒 በአሁኑ ሰዓት በገበያ ላይ ያሉ ዕቃዎች</h2>
            <div class="search-container">
                <input type="text" id="buyerSearchInput" class="search-input" placeholder="🔍 ዕቃ በስም ለመፈለግ እዚህ ይጻፉ..." oninput="renderBuyerCatalog()">
            </div>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የዕቃ ስም</th>
                            <th>የሱቅ ስም</th>
                            <th>መሸጫ ዋጋ</th>
                            <th>የሻጭ ስልክ ቁጥር</th>
                            <th>ሱቁ ያለበት ሎኬشن</th>
                        </tr>
                    </thead>
                    <tbody id="buyerTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <div id="adminPage" class="hidden">
        <div class="welcome-header">
            <h1>የባለቤቱ ማዕከላዊ ቁጥጥር ፓነል</h1>
            <p>እዚህ ገጽ ላይ አዳዲስ ተከራዮችን መመዝገብ፣ የውል ሁኔታ መወሰን እና ማስተዳደር ይችላሉ።</p>
            <button class="btn-expense" style="margin-top: 15px; width:100%;" onclick="logout()">🚪 ከሲስተሙ ውጣ</button>
        </div>

        <div class="section-box">
            <h2>👤 አዲስ ተከራይ መመዝገቢያ</h2>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <input type="text" id="newShopName" placeholder="የሱቅ ስም (ምሳሌ፡ አልፋ)">
                <input type="text" id="newFullName" placeholder="የተከራይ ሙሉ ስም">
            </div>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <input type="text" id="newUsername" placeholder="መግቢያ ስም (Username)">
                <input type="text" id="newPhone" placeholder="ስልክ ቁጥር">
            </div>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <input type="text" id="newTelegram" placeholder="የቴሌግራም መግቢያ (@username)">
                <input type="text" id="newMapsLink" placeholder="የሱቅ ጎግል ማፕ ሊንክ (Google Maps URL)">
            </div>
            <input type="text" id="newAddress" placeholder="የመኖሪያ/የሱቅ አድራሻ">
            
            <label style="font-size: 0.85rem; color: var(--accent-color); display:block; margin-top: 8px;">የውል ዓይነት (Contract Type)፦</label>
            <select id="newContractType">
                <option value="በወር">በወር (Monthly)</option>
                <option value="በዓመት">በዓመት (Yearly)</option>
            </select>

            <label style="font-size: 0.85rem; color: var(--accent-color); display:block; margin-top: 8px;">የውል ማቂያ ቀን (Expiry Date)፦</label>
            <input type="date" id="newExpiryDate">

            <button class="btn-add btn-block" style="margin-top: 15px;" onclick="registerTenant()">➕ ተከራይ በቋሚነት መዝግብ</button>
        </div>

        <div class="section-box">
            <h2>📈 የተከራዮች ሁኔታ እና የውል መቆጣጠሪያ</h2>
            <div class="search-container">
                <input type="text" id="adminSearchInput" class="search-input" placeholder="🔍 ተከራይን በዩዘርኔም (Username) ብቻ በቀላሉ ለመፈለግ..." oninput="renderAdminPanel()">
            </div>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የሱቅ ስም</th>
                            <th>የተከራይ መገለጫ (Profile)</th>
                            <th>የመግቢያ ዝርዝር / ኮድ</th>
                            <th>የውል ዓይነት</th>
                            <th>የማቂያ ቀን</th>
                            <th>የአሁን ሁኔታ</th>
                            <th>እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="tenantTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <div id="appPage" class="hidden">
        <div class="welcome-header">
            <h1 id="shopTitle">እንኳን ደህና መጡ ወደ " ትርፌ " መቆጣጠሪያ መተግበሪያ</h1>
            <p id="roleSubTitle">ቀልጣፋ፣ ዘመናዊ እና አስተማማኝ የሱቅዎ የሂሳብ ረዳት</p>
        </div>

        <div id="sessionDisplay" class="session-badge">የዕለቱ መፈላጊ መረጃ በመጠባበቅ ላይ...</div>
        <div id="shiftStatusAlert" class="session-badge hidden" style="color:var(--danger-color); background:rgba(244,63,94,0.1); border-color:var(--danger-color);">⚠️ ትኩረት፦ የዛሬው የስራ ሰዓት አብቅቷል። እባክዎ የዕለቱን ሂሳብ መዝጊያ ሪፖርት ያቅርቡ!</div>

        <div class="dashboard" id="ownerDashboard">
            <div class="card">
                <h3>የአሁን ካፒታል</h3>
                <p id="totalCapital" style="color: var(--accent-color);">0.00</p>
            </div>
            <div class="card">
                <h3>የዛሬ ትርፍ</h3>
                <p id="todayNetProfit">0.00</p>
            </div>
            <div class="card">
                <h3>የወር ትርፍ</h3>
                <p id="monthlyNetProfit" style="color: var(--warning-color);">0.00</p>
            </div>
            <div class="card">
                <h3>ከሳጥን የተነሳ</h3>
                <p id="totalDraws" style="color: var(--purple-color);">0.00</p>
            </div>
        </div>

        <div class="dashboard" style="margin-top:-10px;">
            <div class="card" style="grid-column: span 2;">
                <h3>ማታ በካሽ ሳጥን መገኘት ያለበት</h3>
                <p id="totalInCash" style="color: #38bdf8; font-size: 1.5rem;">0.00 ETB</p>
            </div>
        </div>

        <div id="chartContainer" style="height: 220px; width: 100%; margin-bottom: 20px;">
            <canvas id="businessChart"></canvas>
        </div>

        <div class="actions">
            <button class="btn-add" id="btn_add_item" onclick="focusInventoryInput()">📦 አዲስ ዕቃ መዝግብ</button>
            <button class="btn-expense" id="btn_expense" onclick="openExpenseModal()">📉 ወጪ መዝግብ</button>
            <button class="btn-credit" onclick="openDebtModal()">💳 ዕዳ መዝግብ</button>
            <button class="btn-draw" onclick="openDrawerModal()">💸 ከሳጥን ብር ማንሻ / መልስ</button>
            <button class="btn-config" onclick="configureBank()">🏦 የባንክ አፕ ማያያዣ</button>
            <button class="btn-next-day" id="btn_next_day" onclick="startNewDaySession()">🔄 አዲስ ቀን ጀምር</button>
            <button class="btn-clear" id="btn_clear_all" onclick="clearAllTenantData()">🗑️ ዳታ አጽዳ</button>
            <button class="btn-expense" id="btn_close_shift" style="background:#f59e0b; color:#000;" onclick="triggerShiftReport()">🔒 የዕለት ሂሳብ ዝጋ (ሪፖርት)</button>
        </div>

        <div class="section-box">
            <h2>📦 የዕቃዎች ዝርዝር እና ክምችት</h2>
            <div id="owner_add_box" style="background: rgba(255, 255, 255, 0.02); padding: 12px; border-radius: 8px; margin-bottom: 15px; border: 1px solid var(--border-color);">
                <input type="text" id="itemName" placeholder="የዕቃ ስም">
                <input type="number" id="itemCost" placeholder="መግዣ ዋጋ (ካፒታል)">
                <input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ">
                <input type="number" id="itemQty" placeholder="የመጣው ብዛት">
                <button class="btn-sell btn-block" style="margin-top: 5px;" onclick="addItemDirectly()">📥 ዕቃውን በዝርዝር አስገባ</button>
            </div>
            <div class="search-container">
                <input type="text" id="inventorySearchInput" class="search-input" placeholder="🔍 የዕቃ ስም በመጻፍ በፍጥነት ይፈልጉ..." oninput="renderApp()">
            </div>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr id="inventoryTableHeader"></tr>
                    </thead>
                    <tbody id="inventoryBody"></tbody>
                </table>
            </div>
        </div>

        <div class="grid-3">
            <div class="section-box">
                <h2>💳 የዕዳ መዝገብ</h2>
                <div class="table-responsive">
                    <table>
                        <tbody id="creditBody"></tbody>
                    </table>
                </div>
            </div>
            <div class="section-box">
                <h2>💸 ከሳጥን የተነሳ ገንዘብ / መልስ</h2>
                <div class="table-responsive">
                    <table>
                        <tbody id="drawBody"></tbody>
                    </table>
                </div>
            </div>
            <div class="section-box" id="historySection">
                <h2>📅 የቀደሙ ቀናት ታሪክ እና ማህደር</h2>
                <div style="margin-bottom:10px;">
                    <label style="font-size:0.8rem; color:var(--accent-color);">ቀን መምረጫ (Filter by Date)፦</label>
                    <input type="date" id="historyDateFilter" onchange="renderHistoryTable()">
                </div>
                <div class="table-responsive">
                    <table>
                        <tbody id="historyBody"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <div class="section-box">
            <h2>⚙️ መቼቶች እና ፕሮፋይል</h2>
            <div class="profile-data"><label>የሱቅ ስም፡</label> <span id="profShopName">-</span></div>
            <div class="profile-data" id="expiryRowOwner"><label>የውል ማቂያ ቀን፡</label> <span id="profExpiry" style="color: var(--danger-color);">-</span></div>
            
            <div id="ownerStaffConfig" style="margin: 15px 0; padding: 12px; background: rgba(0,0,0,0.3); border-radius: 8px; border:1px dashed var(--accent-color);">
                <h4 style="color:var(--accent-color); font-size:0.9rem; margin-bottom:8px;">🔐 ለሰራተኛ መግቢያ ኮድ መፍጠሪያ (Staff Credentials)</h4>
                <input type="text" id="staffUser" placeholder="የሰራተኛው መግቢያ ስም (Staff Username)">
                <input type="text" id="staffPass" placeholder="የሰራተኛው የይለፍ ቃል (Staff Password)">
                <button class="btn-sell btn-block" style="padding:8px;" onclick="saveStaffAccount()">💾 የሰራተኛ አካውንት አስቀምጥ</button>
            </div>

            <div class="profile-data">
                <label>የመተግበሪያ ገጽታ (Theme)፦</label> 
                <select id="themeSelector" onchange="changeTheme(this.value)" style="width: auto; margin: 0; padding: 4px 8px;">
                    <option value="theme-deepblue">Deep Blue</option>
                    <option value="theme-emerald">Emerald Green</option>
                    <option value="theme-purple">Royal Purple</option>
                    <option value="theme-charcoal">Charcoal Dark</option>
                </select>
            </div>
            <button class="btn-expense btn-block" style="margin-top: 20px; padding: 15px;" onclick="logout()">🚪 ከአፑ በሰላም ውጣ (Logout)</button>
        </div>
    </div>
</div>

<div id="modalOverlay" class="modal-overlay hidden">
    <div id="alertModal" class="modal-card hidden">
        <div class="modal-header"><h3 id="alertTitle" class="text-warning">መልዕክት</h3></div>
        <div class="modal-body"><p id="alertMessage"></p></div>
        <div class="modal-footer"><button class="btn-add" onclick="closeActiveModal()">እሺ</button></div>
    </div>
    
    <div id="confirmModal" class="modal-card hidden">
        <div class="modal-header"><h3 id="confirmTitle" class="text-danger">እርግጠኛ ኖት?</h3></div>
        <div class="modal-body"><p id="confirmMessage"></p></div>
        <div class="modal-footer">
            <button class="btn-expense" id="confirmYesBtn">አዎ</button>
            <button class="btn-add" onclick="closeActiveModal()">አይ</button>
        </div>
    </div>

    <div id="formModal" class="modal-card hidden">
        <div class="modal-header"><h3 id="formModalTitle" class="text-success">መረጃ ማስገቢያ</h3></div>
        <div class="modal-body" id="formModalBody"></div>
        <div class="modal-footer" id="formModalFooter"></div>
    </div>
</div>

<script>
    const firebaseConfig = { databaseURL: "https://tirfe-app-v2-300c2-default-rtdb.firebaseio.com/" };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let localDB = { tenants: {} };
    let currentTenant = null;
    let currentUserRole = "owner"; 
    let myChart = null;
    let currentLoginMode = "merchant"; 
    let isOnline = true;

    window.addEventListener('online', handleOnlineStatus);
    window.addEventListener('offline', handleOnlineStatus);

    function handleOnlineStatus() {
        isOnline = navigator.onLine;
        const tag = document.getElementById('syncIndicator');
        if(!isOnline) {
            tag.classList.remove('hidden');
        } else {
            tag.classList.add('hidden');
            pushToFirebase();
        }
    }

    function loadLocalStorageBackup() {
        let backup = localStorage.getItem('tirfe_local_db');
        if(backup) {
            localDB = JSON.parse(backup);
        }
    }

    function saveToLocalStorage() {
        localStorage.setItem('tirfe_local_db', JSON.stringify(localDB));
    }

    function sendTelegramAlert(message) {
        if (!currentTenant || !currentTenant.telegramToken || !currentTenant.telegramChatId) return;
        const token = currentTenant.telegramToken;
        const chatId = currentTenant.telegramChatId;
        const url = `https://api.telegram.org/bot${token}/sendMessage?chat_id=${chatId}&text=${encodeURIComponent(message)}`;
        fetch(url).catch(err => console.log("Telegram Error: ", err));
    }

    loadLocalStorageBackup();
    db.ref('tirfe_system').on('value', (snapshot) => {
        if(snapshot.exists()) {
            localDB = snapshot.val();
            if(!localDB.tenants) localDB.tenants = {};
            saveToLocalStorage();
            
            if(currentTenant) {
                let checkTenant = localDB.tenants[currentTenant.username];
                if(!checkTenant || checkTenant.status === "blocked") { logout(); return; }
                currentTenant = checkTenant;
                renderApp();
            }
            renderBuyerCatalog();
            if(!document.getElementById('adminPage').classList.contains('hidden')) { renderAdminPanel(); }
        }
    }, (error) => {
        console.log("Firebase Error, running offline mode.");
        isOnline = false;
        handleOnlineStatus();
    });

    setInterval(() => {
        let now = new Date();
        let hours = now.getHours();
        let minutes = now.getMinutes();

        if(hours === 22 && currentTenant && currentTenant.data && currentTenant.data.sessionActive && !currentTenant.data.shiftClosed) {
            document.getElementById('shiftStatusAlert').classList.remove('hidden');
        }

        if(hours === 6 && minutes === 0 && currentTenant && currentTenant.data && currentTenant.data.shiftClosed) {
            currentTenant.data.sessionActive = false;
            currentTenant.data.shiftClosed = false;
            saveAndRefresh();
        }
    }, 60000);

    function switchView(targetId) {
        document.getElementById('welcomeGateway').classList.add('hidden');
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('buyerPage').classList.add('hidden');
        document.getElementById('adminPage').classList.add('hidden');
        document.getElementById('appPage').classList.add('hidden');

        document.getElementById(targetId).classList.remove('hidden');
        if (targetId === 'buyerPage') renderBuyerCatalog();
    }

    function showLoginSection(mode) {
        currentLoginMode = mode;
        document.getElementById('loginError').innerText = "";
        document.getElementById('loginUser').value = "";
        document.getElementById('loginPass').value = "";
        
        if (mode === 'admin') {
            document.getElementById('loginTitle').innerText = "👑 የባለቤት (Admin) መግቢያ በር";
            document.getElementById('loginDesc').innerText = "አዳዲስ ሱቆችን ለመመዝገብ እና ውል ለማስተዳደር ይግቡ";
            document.getElementById('loginUser').placeholder = "የአስተዳዳሪ ስም (Admin Username)";
            document.getElementById('loginPass').placeholder = "የአስተዳዳሪ ይለፍ ቃል (Password)";
        } else if (mode === 'merchant') {
            document.getElementById('loginTitle').innerText = "💼 የሱቅ ባለቤቶች (Tenant) መግቢያ";
            document.getElementById('loginDesc').innerText = "የሱቅዎን ሂሳብ፣ ትርፍ እና ክምችት በባለቤትነት ለመቆጣጠር ይግቡ";
            document.getElementById('loginUser').placeholder = "የሱቅ ባለቤት ስም (Tenant Username)";
            document.getElementById('loginPass').placeholder = "የባለቤት ይለፍ ቃል (Password)";
        } else if (mode === 'staff') {
            document.getElementById('loginTitle').innerText = "🛠️ የሱቅ ሰራተኞች መግቢያ በር";
            document.getElementById('loginDesc').innerText = "በሱቅ ባለቤቱ የተሰጠዎትን የሰራተኛ መግቢያ ስም እና ኮድ ይጠቀሙ";
            document.getElementById('loginUser').placeholder = "የሰራተኛ ስም (Staff Username)";
            document.getElementById('loginPass').placeholder = "የሰራተኛ ይለፍ ቃል (Staff Password)";
        }
        switchView('loginPage');
    }

    function goToGateway() { switchView('welcomeGateway'); }

    function handleLogin() {
        let user = document.getElementById('loginUser').value.trim().toLowerCase();
        let pass = document.getElementById('loginPass').value.trim();
        let error = document.getElementById('loginError');

        if(currentLoginMode === 'admin') {
            if(user === "admin" && pass === "admin123") {
                switchView('adminPage');
                renderAdminPanel();
            } else {
                error.innerText = "❌ የተሳሳተ የአስተዳዳሪ (Admin) ስም ወይም ይለፍ ቃል ነው!";
            }
            return;
        }

        let tenant = localDB.tenants ? localDB.tenants[user] : null;

        if (currentLoginMode === 'merchant') {
            if (tenant) {
                if (isTenantExpired(tenant, error)) return;
                
                if (!tenant.isActivated) {
                    if (tenant.activationCode === pass) {
                        let now = new Date().getTime();
                        let twentyFourHours = 24 * 60 * 60 * 1000;
                        if ((now - tenant.codeCreatedAt) > twentyFourHours) {
                            error.innerText = "❌ ይህ ጊዜያዊ ኮድ ከ24 ሰዓት በላይ ስለቆየ ጊዜው አልፏል! እባክዎ አከራዩን አዲስ ኮድ ይጠይቁ።";
                            return;
                        }
                        
                        showFormModal("🔒 የፕራይቬሲ ፓስዎርድ ማቀናበሪያ", [
                            { id: "securePass", label: "ለእርስዎ ብቻ የሚሆን ጠንካራ ምስጢራዊ ፓስዎርድ ይፍጠሩ (አከራዩ ማየት አይችልም)፦", type: "password", placeholder: "አዲስ ፓስዎርድ" }
                        ], (res) => {
                            let sp = res.securePass.trim();
                            if (!sp) { showCustomAlert("ስህተት", "የይለፍ ቃል ባዶ መሆን አይችልም!"); return; }
                            tenant.password = sp; 
                            tenant.isActivated = true;
                            localDB.tenants[user] = tenant;
                            pushToFirebase();
                            currentUserRole = "owner";
                            launchApp(tenant);
                        });
                        return;
                    } else {
                        error.innerText = "❌ የተሳሳተ ጊዜያዊ መግቢያ ኮድ ነው!";
                        return;
                    }
                } else {
                    if (String(tenant.password).trim() === pass) {
                        currentUserRole = "owner";
                        launchApp(tenant);
                        return;
                    }
                }
            }
            error.innerText = "❌ የተሳሳተ የሱቅ ባለቤት ስም ወይም የይለፍ ቃል! (ወይም ገና አልነቃም)";
            return;
        }

        if (currentLoginMode === 'staff') {
            if(localDB.tenants) {
                for (let tKey in localDB.tenants) {
                    let t = localDB.tenants[tKey];
                    if (t.staffUser && t.staffUser === user && String(t.staffPass).trim() === pass) {
                        if (isTenantExpired(t, error)) return;
                        currentUserRole = "staff";
                        launchApp(t);
                        return;
                    }
                }
            }
            error.innerText = "❌ የሰራተኛ ስም ወይም የይለፍ ቃል ስህተት ነው!";
            return;
        }
    }

    function isTenantExpired(tenant, errorElement) {
        if(tenant.expiryDate) {
            let today = new Date(); today.setHours(0,0,0,0);
            let expiry = new Date(tenant.expiryDate); expiry.setHours(0,0,0,0);
            if(today > expiry) {
                tenant.status = "blocked";
                localDB.tenants[tenant.username] = tenant;
                pushToFirebase();
                errorElement.innerText = "🔒 የኪራይ ውልዎ ጊዜ አልቋል! እባክዎ ባለቤቱን ያነጋግሩ።";
                return true;
            }
        }
        if(tenant.status === "blocked") { errorElement.innerText = "🔒 አካውንትዎ ታግዷል!"; return true; }
        return false;
    }

    function launchApp(tenant) {
        currentTenant = tenant;
        document.getElementById('loginPage').classList.add('hidden');
        document.getElementById('appPage').removeclassList = "";
        document.getElementById('appPage').classList.remove('hidden');
        
        document.getElementById('shopTitle').innerText = tenant.shopName + (currentUserRole === "staff" ? " (የሰራተኛ ገጽ)" : " (የባለቤት ገጽ)");
        document.getElementById('roleSubTitle').innerText = currentUserRole === "staff" ? "🛠️ የተገደበ የሰራተኛ መሸጫ እና መመዝገቢያ ሞድ" : "👑 ሙሉ የሱቅና የኪራይ መቆጣጠሪያ ፓነል";
        document.getElementById('profShopName').innerText = tenant.shopName;
        document.getElementById('profExpiry').innerText = tenant.expiryDate ? `${tenant.expiryDate} (${tenant.contractType})` : "ያልተገደበ";
        
        let activeTheme = tenant.theme || 'theme-deepblue';
        document.body.className = activeTheme;
        document.getElementById('themeSelector').value = activeTheme;
        document.getElementById('inventorySearchInput').value = "";

        if (currentUserRole === "staff") {
            document.getElementById('ownerDashboard').classList.add('hidden');
            document.getElementById('chartContainer').classList.add('hidden');
            document.getElementById('btn_add_item').classList.add('hidden');
            document.getElementById('btn_expense').classList.add('hidden');
            document.getElementById('btn_next_day').classList.add('hidden');
            document.getElementById('btn_clear_all').classList.add('hidden');
            document.getElementById('owner_add_box').classList.add('hidden');
            document.getElementById('ownerStaffConfig').classList.add('hidden');
            document.getElementById('expiryRowOwner').classList.add('hidden');
        } else {
            document.getElementById('ownerDashboard').classList.remove('hidden');
            document.getElementById('chartContainer').classList.remove('hidden');
            document.getElementById('btn_add_item').classList.remove('hidden');
            document.getElementById('btn_expense').classList.remove('hidden');
            document.getElementById('btn_next_day').classList.remove('hidden');
            document.getElementById('btn_clear_all').classList.remove('hidden');
            document.getElementById('owner_add_box').classList.remove('hidden');
            document.getElementById('ownerStaffConfig').classList.remove('hidden');
            document.getElementById('expiryRowOwner').classList.remove('hidden');
            
            document.getElementById('staffUser').value = tenant.staffUser || "";
            document.getElementById('staffPass').value = tenant.staffPass || "";
        }

        setTimeout(() => {
            if(currentUserRole === "owner") initChart();
            checkMorningSession();
        }, 200);
    }

    function renderBuyerCatalog() {
        let tbody = document.getElementById('buyerTableBody');
        tbody.innerHTML = '';
        let hasItems = false;
        let query = document.getElementById('buyerSearchInput') ? document.getElementById('buyerSearchInput').value.trim().toLowerCase() : "";

        if (localDB.tenants) {
            Object.keys(localDB.tenants).forEach(tKey => {
                let t = localDB.tenants[tKey];
                if (t.status === "active" && t.data && t.data.inventory) {
                    t.data.inventory.forEach(item => {
                        if (query !== "" && !item.name.toLowerCase().includes(query)) return;
                        hasItems = true;
                        let mapButton = t.googleMapsLink ? `<a href="${t.googleMapsLink}" target="_blank" style="background-color:var(--accent-color); color:#000; padding:6px 10px; border-radius:6px; text-decoration:none; font-weight:bold; font-size:0.8rem;">📍 ካርታ አሳይ (Map)</a>` : `<span style="color:#64748b">ያልተገለጸ</span>`;
                        tbody.innerHTML += `<tr>
                            <td><b>${item.name}</b></td>
                            <td><span class="text-success">${t.shopName}</span></td>
                            <td><b class="text-warning">${item.price} ETB</b></td>
                            <td><a href="tel:${t.phone}" style="color:var(--accent-color); text-decoration:none; font-weight:bold;">📞 ${t.phone}</a></td>
                            <td>${mapButton}</td>
                        </tr>`;
                    });
                }
            });
        }
        if(!hasItems) {
            tbody.innerHTML = '<tr><td colspan="5" style="text-align:center; color:#94a3b8;">በተፈለገው ስም ወይም በአሁኑ ሰዓት በገበያ ላይ የተመዘገበ ምንም እቃ የለም።</td></tr>';
        }
    }

    function generateRandomCode() {
        return Math.floor(100000 + Math.random() * 900000).toString();
    }

    function registerTenant() {
        let shop = document.getElementById('newShopName').value.trim();
        let fullName = document.getElementById('newFullName').value.trim();
        let user = document.getElementById('newUsername').value.trim().toLowerCase(); 
        let phone = document.getElementById('newPhone').value.trim();
        let telegram = document.getElementById('newTelegram').value.trim();
        let mapsLink = document.getElementById('newMapsLink').value.trim();
        let address = document.getElementById('newAddress').value.trim();
        let contractType = document.getElementById('newContractType').value;
        let expiryDate = document.getElementById('newExpiryDate').value;

        if(!shop || !user || !expiryDate || !fullName || !phone) { 
            showCustomAlert("ስህተት", "እባክዎ መሠረታዊ መፈላጊ መረጃዎችን ያሟሉ!"); 
            return; 
        }

        if (localDB.tenants && localDB.tenants[user]) {
            showCustomAlert("⚠️ ምዝገባው አልተሳካም", `"${user}" የሚለው የተጠቃሚ ስም (Username) አስቀድሞ በሌላ ተከራይ ተወስዷል! እባክዎ የተለየ ስም ይጠቀሙ።`);
            return;
        }

        let genCode = generateRandomCode();
        let timestampNow = new Date().getTime();
        
        localDB.tenants[user] = { 
            shopName: shop, fullName: fullName, phone: phone, telegram: telegram || "-", address: address || "-",
            googleMapsLink: mapsLink || "",
            username: user, 
            password: genCode, 
            activationCode: genCode,
            codeCreatedAt: timestampNow,
            isActivated: false,
            contractType: contractType, expiryDate: expiryDate,
            status: "active", theme: "theme-deepblue",
            staffUser: "", staffPass: "",
            data: { sessionActive: false, shiftClosed: false, inventory: [], expenses: [], debts: [], drawerLog: [], history: [] } 
        };
        pushToFirebase(); renderAdminPanel();
        
        document.getElementById('newShopName').value = ''; document.getElementById('newFullName').value = '';
        document.getElementById('newUsername').value = ''; document.getElementById('newPhone').value = ''; 
        document.getElementById('newTelegram').value = ''; document.getElementById('newMapsLink').value = '';
        document.getElementById('newAddress').value = ''; document.getElementById('newExpiryDate').value = '';
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody'); tbody.innerHTML = '';
        let query = document.getElementById('adminSearchInput') ? document.getElementById('adminSearchInput').value.trim().toLowerCase() : "";

        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            if (query !== "" && !t.username.toLowerCase().includes(query)) return;

            let statusBadge = t.status === "active" ? `<span class="badge-success">Active</span>` : `<span class="badge-danger">Blocked</span>`;
            let profileInfo = `👤 <b>${t.fullName || '-'}</b><br>📞 ${t.phone || '-'}<br>📍 ${t.address || '-'}<br>✈️ ${t.telegram || '-'}`;
            
            let codeDisplay = "";
            if (!t.isActivated) {
                codeDisplay = `⏱️ ጊዜያዊ ኮድ: <b class="text-warning" style="font-size:1.1rem; background:rgba(0,0,0,0.4); padding:2px 6px; border-radius:4px;">${t.activationCode}</b>`;
            } else {
                codeDisplay = `<span class="text-success">🔒 ተከራዩ የራሱን ምስጢር ቆልፏል</span>`;
            }
            
            let loginInfo = `👤 አባል ስም: <code>${t.username}</code><br>${codeDisplay}<br>🛠️ ሰራተኛ: <code>${t.staffUser || '-'}</code>`;

            tbody.innerHTML += `<tr>
                <td><b>${t.shopName}</b></td>
                <td>${profileInfo}</td>
                <td>${loginInfo}</td>
                <td><span>${t.contractType || 'በወር'}</span></td>
                <td style="color:var(--danger-color)"><b>${t.expiryDate || '-'}</b></td>
                <td>${statusBadge}</td>
                <td>
                    <button class="btn-config btn-sm" onclick="toggleTenantStatus('${t.username}')">ሁኔታ ቀይር</button>
                    <button class="btn-expense btn-sm" onclick="deleteTenant('${t.username}')">Delete</button>
                    <button class="btn-add btn-sm" style="margin-top:4px;" onclick="regenerateTenantCode('${t.username}')">🔄 አዲስ ኮድ</button>
                </td>
            </tr>`;
        });
    }

    function regenerateTenantCode(user) {
        let t = localDB.tenants[user];
        let newCode = generateRandomCode();
        t.activationCode = newCode;
        t.password = newCode;
        t.codeCreatedAt = new Date().getTime();
        t.isActivated = false; 
        localDB.tenants[user] = t;
        pushToFirebase();
        renderAdminPanel();
        showCustomAlert("ኮድ ተለውጧል", `ለተከራዩ አዲስ ኮድ ተፈጥሯል፦ ${newCode}`);
    }

    function toggleTenantStatus(user) {
        let t = localDB.tenants[user];
        t.status = t.status === "active" ? "blocked" : "active";
        pushToFirebase(); renderAdminPanel();
    }

    function deleteTenant(user) { 
        showCustomConfirm("ተከራይ ማጥፊያ", "ይህንን ተከራይ ሙሉ በሙሉ ለማጥፋት እርግጠኛ ኖት?", () => {
            delete localDB.tenants[user]; pushToFirebase(); renderAdminPanel(); 
        });
    }

    function logout() { 
        currentTenant = null; 
        switchView('welcomeGateway');
    }

    function saveAndRefresh() { 
        localDB.tenants[currentTenant.username] = currentTenant; 
        saveToLocalStorage();
        pushToFirebase(); 
        renderApp(); 
    }

    function focusInventoryInput() { if(currentUserRole === "owner") document.getElementById('itemName').focus(); }

    function addItemDirectly() {
        if(currentUserRole === "staff") return;
        let name = document.getElementById('itemName').value.trim();
        let cost = parseFloat(document.getElementById('itemCost').value) || 0;
        let price = parseFloat(document.getElementById('itemPrice').value) || 0;
        let qty = parseInt(document.getElementById('itemQty').value) || 0;
        
        if(!name || cost <= 0 || price <= 0 || qty <= 0) { showCustomAlert("ስህተት", "እባክዎ ትክክለኛ የዕቃ መረጃ ያስገቡ!"); return; }
        
        let inv = currentTenant.data.inventory || [];
        let existingItem = inv.find(item => item.name.toLowerCase() === name.toLowerCase());
        
        if (existingItem) {
            existingItem.qty += qty;
            existingItem.cost = cost;
            existingItem.price = price;
            showCustomAlert("🔄 ዕቃው ተሞልቷል", `"${name}" አስቀድሞ ስለነበረ አዲሱ ብዛት ተደምሮበታል። አጠቃላይ የነበረው ብዛት፦ ${existingItem.qty}`);
        } else {
            inv.push({ name, cost, price, qty, sold: 0, profit: 0 });
        }
        
        currentTenant.data.inventory = inv; 
        saveAndRefresh();
        
        document.getElementById('itemName').value = ''; document.getElementById('itemCost').value = ''; 
        document.getElementById('itemPrice').value = ''; document.getElementById('itemQty').value = '';
    }

    // ማሻሻያ የተደረገበት ቴብል ዲዛይን ያለው ዲጂታል ደረሰኝ ማሳያ፣ PDF ማውረጃ እና ማጋሪያ አማራጮች
    function generateDigitalReceipt(itemName, count, totalVal) {
        let recId = Math.floor(10000 + Math.random() * 90000);
        let dateStr = new Date().toLocaleDateString('am-ET');
        let rawTextForShare = `======= ${currentTenant.shopName.toUpperCase()} =======\nደረሰኝ ቁጥር: #${recId}\nቀን: ${dateStr}\n---------------------------\nዕቃ: ${itemName}\nብዛት: ${count}\nጠቅላላ ዋጋ: ${totalVal} ETB\n---------------------------\nእናመሰግናለን!`;

        // በሞዳል ውስጥ የሚቀመጥ ውብ የደረሰኝ HTML ቴብል መዋቅር
        let receiptHTML = `
        <div class="receipt-container" id="printableReceiptArea">
            <div class="receipt-header">
                <h4>${currentTenant.shopName}</h4>
                <p>ዲጂታል የሽያጭ ደረሰኝ</p>
                <p><b>ቁጥር (No):</b> #${recId} | <b>ቀን:</b> ${dateStr}</p>
            </div>
            <table class="receipt-table">
                <thead>
                    <tr>
                        <th>የዕቃ ስም</th>
                        <th>ብዛት</th>
                        <th>ነጠላ ዋጋ</th>
                        <th>ጠቅላላ</th>
                    </tr>
                </thead>
                <tbody>
                    <tr>
                        <td><b>${itemName}</b></td>
                        <td>${count}</td>
                        <td>${(totalVal/count).toFixed(1)}</td>
                        <td><b>${totalVal} ETB</b></td>
                    </tr>
                </tbody>
            </table>
            <div class="receipt-summary">
                የተከፈለ ጠቅላላ ሂሳብ: ${totalVal}.00 ETB
            </div>
            <div class="receipt-footer">
                ~ ስለመጡ እናመሰግናለን! እንደገና ይጎብኙን ~
            </div>
        </div>
        <div class="receipt-actions-grid">
            <button class="btn-sell" onclick="downloadReceiptPDF('${itemName}_${recId}')">📥 ፒዲኤፍ (PDF)</button>
            <button class="btn-config" style="background:#0088cc; color:white;" onclick="shareToSocial('tg', \`${rawTextForShare}\`)">✈️ ቴሌግራም</button>
            <button class="btn-config" style="background:#25D366; color:white;" onclick="shareToSocial('wa', \`${rawTextForShare}\`)">💬 ዋትስአፕ</button>
            <button class="btn-expense" onclick="closeActiveModal()">❌ ዝጋ</button>
        </div>
        `;
        
        document.getElementById('formModalTitle').innerText = "🧾 የሽያጭ ደረሰኝ";
        let body = document.getElementById('formModalBody');
        body.innerHTML = receiptHTML;
        
        // ፉተሩን ባዶ እናደርገዋለን ምክንያቱም በተኖቹን እዛው ግሪድ ውስጥ ስላካተትናቸው
        document.getElementById('formModalFooter').innerHTML = '';
        
        openModalContainer();
        document.getElementById('formModal').classList.remove('hidden');
    }

    // ደረሰኙን ወደ PDF ቀይሮ የሚያወርድ ፈንክሽን
    function downloadReceiptPDF(filename) {
        const element = document.getElementById('printableReceiptArea');
        const opt = {
            margin:       10,
            filename:     'Receipt_' + filename + '.pdf',
            image:        { type: 'jpeg', quality: 0.98 },
            html2canvas:  { scale: 2, useCORS: true },
            jsPDF:        { unit: 'mm', format: 'a6', orientation: 'portrait' } // ለደረሰኝ ተስማሚ የሆነ የA6 ሳይዝ
        };
        html2pdf().set(opt).from(element).save();
    }

    // ወደ ሶሻል ሚዲያ ማጋሪያ ፈንክሽን
    function shareToSocial(platform, text) {
        let url = "";
        if(platform === 'tg') {
            url = `https://t.me/share/url?url=&text=${encodeURIComponent(text)}`;
        } else if(platform === 'wa') {
            url = `https://api.whatsapp.com/send?text=${encodeURIComponent(text)}`;
        }
        window.open(url, '_blank');
    }

    function sellItem(idx) {
        if(currentTenant.data.shiftClosed) {
            showCustomAlert("🔒 የሂሳብ መዝጊያ ቀርቧል", "የዕለቱ ፈረቃ ተዘግቷል! አዲስ ሽያጭ መመዝገብ አይቻልም።");
            return;
        }
        let item = currentTenant.data.inventory[idx];
        let remaining = item.qty - item.sold;
        
        showFormModal(`${item.name} ለመሸጥ`, [
            { id: "count", label: `ስንት ፍሬ ተሸጠ? (ቀሪ፡ ${remaining})`, type: "number", placeholder: "1", defaultValue: "1" }
        ], (res) => {
            let count = parseInt(res.count) || 0;
            if (count <= 0) return;
            if (count > remaining) { showCustomAlert("ስህተት", "በክምችት ውስጥ ካለው ቀሪ እቃ ይበልጣል!"); return; }
            item.sold += count;
            saveAndRefresh();
            
            let totalVal = item.price * count;
            sendTelegramAlert(`🛍️ የሽያጭ ማስታወቂያ (${currentUserRole === 'staff' ? 'በሰራተኛ' : 'በአሰሪ'})፦\nዕቃ፡ ${item.name}\nብዛት፡ ${count} ፍሬ\nዋጋ፡ ${totalVal} ETB`);
            
            generateDigitalReceipt(item.name, count, totalVal);
        });
    }

    function openExpenseModal() {
        if(currentUserRole === "staff") return;
        showFormModal("አዲስ ወጪ መዝግብ", [
            { id: "reason", label: "የወጪ ምክንያት", type: "text", placeholder: "ምሳሌ፡ ለመብራት ክፍያ" },
            { id: "amount", label: "የገንዘብ መጠን (ETB)", type: "number", placeholder: "0.00" }
        ], (res) => {
            let amount = parseFloat(res.amount) || 0; let reason = res.reason.trim();
            if(!reason || amount <= 0) return;
            let d = currentTenant.data || {}; if(!d.expenses) d.expenses = [];
            d.expenses.push({ reason, amount, time: new Date().toLocaleTimeString('am-ET') });
            currentTenant.data = d; saveAndRefresh();
        });
    }

    function openDebtModal() {
        showFormModal("አዲስ የዕዳ መዝገብ", [
            { id: "customer", label: "የባለዕዳ ስም", type: "text", placeholder: "ምሳሌ፡ አቶ አበበ" },
            { id: "phone", label: "ስልክ ቁጥር", type: "text", placeholder: "09..." },
            { id: "amount", label: "የዕዳ መጠን (ETB)", type: "number", placeholder: "0.00" }
        ], (res) => {
            let amount = parseFloat(res.amount) || 0; let customer = res.customer.trim();
            if(!customer || amount <= 0) return;
            let d = currentTenant.data || {}; if(!d.debts) d.debts = [];
            d.debts.push({ customer, phone: res.phone || "-", amount, paid: 0, date: new Date().toLocaleDateString('am-ET') });
            currentTenant.data = d; saveAndRefresh();
            sendTelegramAlert(`💳 አዲስ እዳ ተመዘገበ (${currentUserRole === 'staff' ? 'በሰራተኛ' : 'በአሰሪ'})፦\nባለእዳ፡ ${customer}\nየእዳ መጠን፡ ${amount} ETB`);
        });
    }

    function collectDebt(idx) {
        let debt = currentTenant.data.debts[idx];
        let remaining = debt.amount - debt.paid;
        
        showFormModal(`${debt.customer} እዳ ክፍያ መቀበያ`, [
            { id: "amount", label: `የተከፈለው ገንዘብ (ቀሪ ዕዳ፡ ${remaining} ETB)`, type: "number", placeholder: "0.00", defaultValue: remaining }
        ], (res) => {
            let amt = parseFloat(res.amount) || 0;
            if(amt <= 0 || amt > remaining) { showCustomAlert("ስህተት", "የክፍያ መጠን ልክ አይደለም!"); return; }
            debt.paid += amt;
            currentTenant.data.collectedCreditToday = (parseFloat(currentTenant.data.collectedCreditToday) || 0) + amt;
            saveAndRefresh();
            showCustomAlert("ክፍያ ተፈጽሟል", `${debt.customer} እዳ ከፍሏል!`);
        });
    }

    // ማሻሻያ የተደረገበት የብር ማንሻ እና መመለሻ (Repayment Tracking) ሲስተም
    function openDrawerModal() {
        showFormModal("ከሳጥን ብር ማንሻ / የተነሳ መመለሻ", [
            { id: "actionType", label: "የድርጊት ዓይነት ይምረጡ፦", type: "select", options: [
                { value: "withdraw", label: "💸 ከሳጥን ብር ማንሻ (Withdrawal)" },
                { value: "return", label: "📥 የተነሳ ብር መመለሻ (Repayment/Return)" }
            ]},
            { id: "reason", label: "ምክንያት / ማስታወሻ", type: "text", placeholder: "ምሳሌ፡ ለመልስ መለወጫ / የወሰድኩትን መለስኩ" },
            { id: "amount", label: "የገንዘብ መጠን (ETB)", type: "number", placeholder: "0.00" }
        ], (res) => {
            let amount = parseFloat(res.amount) || 0; 
            let reason = res.reason.trim();
            let action = res.actionType;
            if(!reason || amount <= 0) return;
            
            let d = currentTenant.data || {}; 
            if(!d.drawerLog) d.drawerLog = [];
            
            // ብሩ ከተመለሰ በኔጌቲቭ ቫልዩ (-amount) ሴቭ እናደርገዋለን፤ ይህም ማታ ከሳጥን ሂሳብ ላይ ተቀናንሶ ትክክለኛውን ካሽ ያሳያል
            let finalAmount = action === "withdraw" ? amount : -amount;
            let displayType = action === "withdraw" ? "ገንዘብ ተነሳ" : "ገንዘብ ተመለሰ";
            
            d.drawerLog.push({ 
                reason: `${action === "withdraw" ? "⚠️ [የተነሳ] " : "✅ [የተመለሰ] "} ${reason}`, 
                amount: finalAmount, 
                time: new Date().toLocaleTimeString('am-ET') 
            });
            
            currentTenant.data = d; 
            saveAndRefresh();
            sendTelegramAlert(`💸 ከሳጥን ${displayType} (${currentUserRole === 'staff' ? 'በሰራተኛ' : 'በአሰሪ'})፦\nምክንያት፡ ${reason}\nመጠን፡ ${amount} ETB`);
        });
    }

    function configureBank() {
        if(currentUserRole === "staff") {
            showCustomAlert("🏦 የባንክ ሂሳብ መረጃ ማያያዣ", `የአሰሪው የባንክ ሂሳብ ቁጥር (CBE/Telebirr)፦ ${currentTenant.bankAccount || "ያልተገናኘ"}`);
            return;
        }

        showFormModal("የባንክ እና የቴሌግራም አገናኝ መቼት", [
            { id: "telegramToken", label: "የቴሌግራም ቦት ቶከን (Telegram Bot Token)", type: "text", placeholder: "Token...", defaultValue: currentTenant.telegramToken || "" },
            { id: "telegramChatId", label: "የቴሌግራም ቻት ID (Telegram Chat ID)", type: "text", placeholder: "Chat ID...", defaultValue: currentTenant.telegramChatId || "" },
            { id: "bankAccountNumber", label: "የባንክ ሂሳብ ቁጥር (CBE/Telebirr)", type: "text", placeholder: "የባንክ ቁጥር...", defaultValue: currentTenant.bankAccount || "" }
        ], (res) => {
            currentTenant.telegramToken = res.telegramToken.trim();
            currentTenant.telegramChatId = res.telegramChatId.trim();
            currentTenant.bankAccount = res.bankAccountNumber.trim();
            saveAndRefresh();
            showCustomAlert("ተሳክቷል", "የማያያዣ መቼቶች በተሳካ ሁኔታ ተቀምጠዋል!");
        });
    }

    function saveStaffAccount() {
        if(currentUserRole === "staff") return;
        let u = document.getElementById('staffUser').value.trim().toLowerCase();
        let p = document.getElementById('staffPass').value.trim();
        if(!u || !p) { showCustomAlert("ስህተት", "እባክዎ የተሟላ የሰራተኛ መግቢያ ይሙሉ!"); return; }
        
        if (u === currentTenant.username) {
            showCustomAlert("⚠️ ስህተት", "የሰራተኛው Username ከሱቅ ባለቤቱ Username ጋር አንድ አይነት መሆን አይችልም!");
            return;
        }
        if (localDB.tenants) {
            for (let key in localDB.tenants) {
                let otherTenant = localDB.tenants[key];
                if (otherTenant.username === u) {
                    showCustomAlert("⚠️ ስህተት", "ይህ ዩዘርኔም አስቀድሞ በሲስተሙ ውስጥ በሌላ የሱቅ ባለቤት ተይዟል!");
                    return;
                }
                if (otherTenant.username !== currentTenant.username && otherTenant.staffUser && otherTenant.staffUser === u) {
                    showCustomAlert("⚠️ ስህተት", "ይህ ዩዘርኔም አስቀድሞ በሌላ ሱቅ ሰራተኛ ተይዟል!");
                    return;
                }
            }
        }

        currentTenant.staffUser = u;
        currentTenant.staffPass = p;
        saveAndRefresh();
        showCustomAlert("ተቀምጧል", "የሰራተኛ አካውንት በተሳካ ሁኔታ ተቀይሯል!");
    }

    function triggerShiftReport() {
        let d = currentTenant.data || {};
        let session = d.sessionData || {};
        
        let sysSales = parseFloat(d.collectedCreditToday || 0);
        let todayProfit = 0;
        let inv = d.inventory || [];
        inv.forEach(item => {
            sysSales += (item.price * item.sold);
            todayProfit += (item.price - item.cost) * item.sold;
        });

        showFormModal("🔒 የዕለት ሂሳብ ሪፖርት መዝጊያ ማቅረቢያ", [
            { id: "reportedCash", label: "በእጅዎ የሚገኘውን ትክክለኛ የጥሬ ገንዘብ (Cash) መጠን ያስገቡ፦", type: "number", placeholder: "0.00" }
        ], (res) => {
            let reported = parseFloat(res.reportedCash) || 0;
            let tExp = 0; let tDraw = 0;
            (d.expenses || []).forEach(e => tExp += parseFloat(e.amount) || 0);
            (d.drawerLog || []).forEach(dr => tDraw += parseFloat(dr.amount) || 0);

            let expectedCash = ((parseFloat(session.initialFloat) || 0) + sysSales) - tExp - tDraw;
            let variance = reported - expectedCash;
            let statusText = variance === 0 ? "ትክክል (Balanced)" : `ልዩነት አለ (${variance} ETB)`;

            d.shiftClosed = true;
            d.reportedCash = reported;
            d.variance = variance;
            d.expectedCash = expectedCash;

            document.getElementById('shiftStatusAlert').classList.add('hidden');
            showCustomAlert("ሪፖርት ቀርቧል", `የዕለቱ ሂሳብ በተሳካ ሁኔታ ተዘጋጅቷል!\nሁኔታ፡ ${statusText}\nበሲስተሙ የሚጠበቅ ካሽ፡ ${expectedCash} ETB\nየቀረበው ካሽ፡ ${reported} ETB`);
            
            if(!d.history) d.history = [];
            
            let todayObj = new Date();
            let yyyy = todayObj.getFullYear();
            let mm = String(todayObj.getMonth() + 1).padStart(2, '0');
            let dd = String(todayObj.getDate()).padStart(2, '0');
            let formattedDate = `${yyyy}-${mm}-${dd}`;

            d.history.push({
                date: formattedDate, 
                employee: session.employee || "ሰራተኛ",
                sales: sysSales, 
                profit: todayProfit - tExp, 
                expenses: tExp, 
                draws: tDraw,
                reportedCash: reported,
                expectedCash: expectedCash,
                variance: variance
            });
            currentTenant.data = d;
            saveAndRefresh();
        });
    }

    function startNewDaySession() {
        if(currentUserRole === "staff") return;
        showCustomConfirm("አዲስ ቀን መጀመር", "የዛሬውን ቀን ሂሳብ ሙሉ በሙሉ አጽድተው ለአዲስ ቀን ማዘጋጀት ይፈልጋሉ?", () => {
            let d = currentTenant.data || {};
            let inv = d.inventory || [];
            
            inv.forEach(item => {
                item.qty = Math.max(0, item.qty - item.sold); 
                item.sold = 0;
            });

            d.sessionActive = false; 
            d.shiftClosed = false;
            d.expenses = []; 
            d.drawerLog = []; 
            d.collectedCreditToday = 0;
            currentTenant.data = d; 
            saveAndRefresh(); 
            checkMorningSession();
        });
    }

    function clearAllTenantData() {
        if(currentUserRole === "staff") return;
        showCustomConfirm("ሁሉንም ዳታ ማጽዳት", "ሁሉንም ዳታ ለማጥፋት እርግጠኛ ኖት?", () => {
            currentTenant.data = { sessionActive: false, shiftClosed: false, inventory: [], expenses: [], debts: [], drawerLog: [], history: [] };
            saveAndRefresh(); checkMorningSession();
        });
    }

    function renderHistoryTable() {
        let d = currentTenant.data || {};
        let historyBody = document.getElementById('historyBody');
        historyBody.innerHTML = '<tr><th>ቀን</th><th>ሰራተኛ</th><th>ሽያጭ</th><th>ትርፍ</th><th>ሪፖርት ካሽ</th><th>ልዩነት</th></tr>';
        
        let historyList = d.history || [];
        let filterValue = document.getElementById('historyDateFilter').value; 

        let filtered = historyList.filter(h => {
            if(!filterValue) return true;
            return h.date === filterValue;
        });

        if(filtered.length === 0) {
            historyBody.innerHTML += '<tr><td colspan="6" style="text-align:center; color:#94a3b8;">በተፈለገው ቀን ምንም ታሪክ የለም</td></tr>';
        } else {
            filtered.forEach(h => {
                let vColor = h.variance === 0 ? 'var(--success-color)' : 'var(--danger-color)';
                historyBody.innerHTML += `<tr>
                    <td><b>${h.date}</b></td>
                    <td>${h.employee}</td>
                    <td style="color:var(--success-color)">${h.sales}</td>
                    <td style="color:var(--accent-color)"><b>${h.profit}</b></td>
                    <td>${h.reportedCash || 0}</td>
                    <td style="color:${vColor}"><b>${h.variance || 0}</b></td>
                </tr>`;
            });
        }
    }

    function renderApp() {
        let d = currentTenant.data || {}; let session = d.sessionData || {};
        if(d.sessionActive) { 
            document.getElementById('sessionDisplay').innerText = `📅 ${session.date} | 👤 አስገቢ፡ ${session.employee} | 💰 መነሻ ካዝና፡ ${session.initialFloat} ETB`; 
        }
        
        let headerRow = document.getElementById('inventoryTableHeader');
        if (currentUserRole === "staff") {
            headerRow.innerHTML = `<th>የዕቃ ስም</th><th>መሸጫ ዋጋ</th><th>የዛሬ ሽያጭ</th><th>ቀሪ ክምችት</th>`;
        } else {
            headerRow.innerHTML = `<th>የዕቃ ስም</th><th>መግዣ</th><th>መሸጫ</th><th>የነበረው</th><th>የዛሬ ሽያጭ</th><th>ቀሪ</th><th>ትርፍ</th><th>እርምጃ</th>`;
        }

        let tbody = document.getElementById('inventoryBody'); tbody.innerHTML = '';
        let collectedCredit = parseFloat(d.collectedCreditToday) || 0;
        let tSales = collectedCredit; let todayProfit = 0; let tExp = 0; let tDraw = 0; let currentTotalCapital = 0;
        
        let expensesList = d.expenses || []; expensesList.forEach(e => tExp += parseFloat(e.amount) || 0);
        let drawsList = d.drawerLog || []; drawsList.forEach(dr => tDraw += parseFloat(dr.amount) || 0);

        let query = document.getElementById('inventorySearchInput') ? document.getElementById('inventorySearchInput').value.trim().toLowerCase() : "";

        let inv = d.inventory || [];
        inv.forEach((item, idx) => {
            let remaining = Math.max(0, item.qty - item.sold); 
            let profit = (item.price - item.cost) * item.sold;
            tSales += (item.price * item.sold); todayProfit += profit; currentTotalCapital += (item.cost * remaining);
            
            if (query !== "" && !item.name.toLowerCase().includes(query)) return;

            let rowClass = remaining <= 3 ? 'low-stock-row' : '';
            let stockBadge = remaining <= 3 ? '<span class="low-stock-badge">⚠️</span>' : '';

            if (currentUserRole === "staff") {
                tbody.innerHTML += `<tr class="${rowClass}">
                    <td><strong>${item.name}</strong> ${stockBadge}</td>
                    <td>${item.price} ETB</td>
                    <td><button class="btn-sell btn-sm" onclick="sellItem(${idx})">➕ ሸጥ</button> <b>${item.sold}</b></td>
                    <td style="${remaining <= 3 ? 'color:#f87171;font-weight:bold;' : ''}">${remaining}</td>
                </tr>`;
            } else {
                tbody.innerHTML += `<tr class="${rowClass}">
                    <td><strong>${item.name}</strong> ${stockBadge}</td>
                    <td>${item.cost}</td>
                    <td>${item.price}</td>
                    <td>${item.qty}</td>
                    <td><button class="btn-sell btn-sm" onclick="sellItem(${idx})">➕ ሸጥ</button> <b>${item.sold}</b></td>
                    <td>${remaining}</td>
                    <td>${profit}</td>
                    <td><button class="btn-expense btn-sm" onclick="deleteInventoryItem(${idx})">🗑️</button></td>
                </tr>`;
            }
        });

        let finalCashInHand = ((parseFloat(session.initialFloat) || 0) + tSales) - tExp - tDraw;
        document.getElementById('totalInCash').innerText = finalCashInHand.toFixed(1) + " ETB";

        if (currentUserRole === "owner") {
            let monthlyProfit = todayProfit - tExp;
            let historyList = d.history || []; historyList.forEach(h => monthlyProfit += parseFloat(h.profit) || 0);

            document.getElementById('totalCapital').innerText = currentTotalCapital.toFixed(1) + " ETB";
            document.getElementById('todayNetProfit').innerText = (todayProfit - tExp).toFixed(1) + " ETB";
            document.getElementById('monthlyNetProfit').innerText = monthlyProfit.toFixed(1) + " ETB";
            document.getElementById('totalDraws').innerText = tDraw.toFixed(1) + " ETB";
            
            if (myChart) {
                myChart.data.datasets[0].data = [currentTotalCapital, tSales, todayProfit - tExp];
                myChart.update();
            }
            renderHistoryTable();
        }

        let creditBody = document.getElementById('creditBody');
        creditBody.innerHTML = '<tr><th>ባለዕዳ</th><th>ስልክ</th><th>ዕዳ</th><th>ቀሪ</th><th>ድርጊት</th></tr>';
        let debts = d.debts || [];
        if(debts.length === 0) {
            creditBody.innerHTML += '<tr><td colspan="5" style="text-align:center; color:#94a3b8;">ምንም የዕዳ መዝገብ የለም</td></tr>';
        } else {
            debts.forEach((debt, idx) => {
                let remaining = debt.amount - debt.paid;
                if (remaining > 0) {
                    creditBody.innerHTML += `<tr>
                        <td><b>${debt.customer}</b></td>
                        <td>${debt.phone}</td>
                        <td>${debt.amount}</td>
                        <td style="color:var(--danger-color)"><b>${remaining}</b></td>
                        <td><button class="btn-sell btn-sm" onclick="collectDebt(${idx})">ክፍያ</button></td>
                    </tr>`;
                }
            });
        }

        let drawBody = document.getElementById('drawBody');
        drawBody.innerHTML = '<tr><th>ምክንያት</th><th>የተወሰደው</th><th>ሰዓት</th></tr>';
        if(drawsList.length === 0) {
            drawBody.innerHTML += '<tr><td colspan="3" style="text-align:center; color:#94a3b8;">ምንም የተነሳ ገንዘብ የለም</td></tr>';
        } else {
            drawsList.forEach(dr => {
                let isReturn = dr.amount < 0;
                let displayAmt = isReturn ? Math.abs(dr.amount) + " ETB (መለሰ)" : dr.amount + " ETB";
                let displayColor = isReturn ? "var(--success-color)" : "var(--purple-color)";
                tbodyColor = `style="color:${displayColor}; font-weight:bold;"`;
                
                drawBody.innerHTML += `<tr>
                    <td>${dr.reason}</td>
                    <td ${tbodyColor}>${displayAmt}</td>
                    <td>${dr.time}</td>
                </tr>`;
            });
        }
    }

    function initChart() {
        let canvasElement = document.getElementById('businessChart');
        if (!canvasElement || currentUserRole === "staff") return;
        let ctx = canvasElement.getContext('2d');
        if (myChart) myChart.destroy();
        myChart = new Chart(ctx, {
            type: 'bar',
            data: {
                labels: ['ካፒታል', 'የዛሬ ሽያጭ', 'የዛሬ ትርፍ'],
                datasets: [{
                    label: 'የገንዘብ መጠን (ETB)',
                    data: [0, 0, 0],
                    backgroundColor: ['#38bdf8', '#4ade80', '#fbbf24'],
                    borderRadius: 6
                }]
            },
            options: {
                responsive: true,
                maintainAspectRatio: false,
                plugins: { legend: { display: false } },
                scales: {
                    y: { grid: { color: 'rgba(255,255,255,0.05)' }, ticks: { color: '#94a3b8' } },
                    x: { ticks: { color: '#94a3b8' } }
                }
            }
        });
    }

    function checkMorningSession() {
        let d = currentTenant.data || {};
        if (!d.sessionActive) {
            showFormModal("የቀኑ መጀመሪያ መመዝገቢያ (የካዝና ማስሞያ)", [
                { id: "employee", label: "የገቢ አድራጊው/ሰራተኛው ስም ያስገቡ፦", type: "text", placeholder: "ስም", defaultValue: currentUserRole === "staff" ? "ሰራተኛ" : "አሰሪ" },
                { id: "initialFloat", label: "ጠዋት በካዝና/ሳጥን ውስጥ የተገኘ መነሻ ገንዘብ (Float)፦", type: "number", placeholder: "0.00", defaultValue: "0" }
            ], (res) => {
                let today = new Date();
                d.sessionData = {
                    date: today.toLocaleDateString('am-ET'),
                    loginTime: today.toLocaleTimeString('am-ET', {hour:'2-digit', minute:'2-digit'}),
                    employee: res.employee || "ሰራተኛ",
                    initialFloat: parseFloat(res.initialFloat) || 0
                };
                d.sessionActive = true;
                d.shiftClosed = false;
                d.expenses = [];
                d.drawerLog = [];
                d.debts = d.debts || [];
                d.collectedCreditToday = 0;
                currentTenant.data = d;
                saveAndRefresh();
            });
        } else {
            renderApp();
        }
    }

    function pushToFirebase() { 
        if(isOnline) {
            db.ref('tirfe_system').set(localDB); 
        }
    }
    
    function openModalContainer() { document.getElementById('modalOverlay').classList.remove('hidden'); }
    
    function closeActiveModal() {
        document.getElementById('modalOverlay').classList.add('hidden');
        document.querySelectorAll('.modal-card').forEach(m => m.classList.add('hidden'));
    }

    function showCustomAlert(title, message, callback) {
        document.getElementById('alertTitle').innerText = title;
        document.getElementById('alertMessage').innerText = message;
        openModalContainer();
        document.getElementById('alertModal').classList.remove('hidden');
        document.querySelector('#alertModal .btn-add').onclick = function() { closeActiveModal(); if(callback) callback(); };
    }

    function showFormModal(title, fields, onSubmit) {
        document.getElementById('formModalTitle').innerText = title;
        let body = document.getElementById('formModalBody');
        body.innerHTML = '';
        
        fields.forEach(f => {
            let label = document.createElement('label');
            label.innerText = f.label;
            label.style.fontSize = '0.85rem';
            label.style.color = 'var(--accent-color)';
            label.style.display = 'block';
            label.style.marginTop = '10px';
            
            let input;
            if(f.type === 'select') {
                input = document.createElement('select');
                f.options.forEach(opt => {
                    let o = document.createElement('option');
                    o.value = opt.value;
                    o.innerText = opt.label;
                    input.appendChild(o);
                });
            } else {
                input = document.createElement('input');
                input.type = f.type || 'text';
                input.placeholder = f.placeholder || '';
                if(f.defaultValue !== undefined) input.value = f.defaultValue;
            }
            input.id = 'formField_' + f.id;
            body.appendChild(label);
            body.appendChild(input);
        });
        
        let footer = document.getElementById('formModalFooter');
        footer.innerHTML = '';
        
        let cancelBtn = document.createElement('button');
        cancelBtn.className = 'btn-config';
        cancelBtn.innerText = 'ሰርዝ';
        cancelBtn.onclick = closeActiveModal;
        
        let submitBtn = document.createElement('button');
        submitBtn.className = 'btn-sell';
        submitBtn.innerText = 'አስገባ';
        submitBtn.onclick = function() {
            let data = {};
            fields.forEach(f => {
                let val = document.getElementById('formField_' + f.id).value;
                data[f.id] = val;
            });
            closeActiveModal();
            onSubmit(data);
        };
        
        footer.appendChild(cancelBtn);
        footer.appendChild(submitBtn);
        
        openModalContainer();
        document.getElementById('formModal').classList.remove('hidden');
    }

    function showCustomConfirm(title, message, onConfirm) {
        document.getElementById('confirmTitle').innerText = title;
        document.getElementById('confirmMessage').innerText = message;
        openModalContainer();
        document.getElementById('confirmModal').classList.remove('hidden');
        document.getElementById('confirmYesBtn').onclick = function() { closeActiveModal(); if(onConfirm) onConfirm(); };
    }

    function changeTheme(themeClass) {
        document.body.className = themeClass;
        if(currentTenant) {
            currentTenant.theme = themeClass;
            saveAndRefresh();
        }
    }

    function deleteInventoryItem(idx) { 
        if(currentUserRole === "staff") return;
        showCustomConfirm("እቃ መሰረዣ", "ይህንን እቃ ማጥፋት ይፈልጋሉ?", () => {
            currentTenant.data.inventory.splice(idx, 1); saveAndRefresh(); 
        });
    }
</script>
</body>
</html>
