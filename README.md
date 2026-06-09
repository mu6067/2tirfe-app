<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <!-- ተጠቃሚዎች በጣታቸው Zoom እንዲያደርጉ የተፈቀደበት እና በራሱ ስክሪን እንዲያስተካክል የተደረገ Viewport -->
    <meta name="viewport" content="width=device-width, initial-scale=1.0, user-scalable=yes, minimum-scale=0.5, maximum-scale=3.0">
    <title>ትርፌ የሱቅ መቆጣጠሪያ አፕሊኬሽን - Ultimate v7.5 (Auto-Adjust & Zoom Enabled)</title>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>

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

        /* ገጽታዎች (Themes) */
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
            /* በሁሉም ስልኮች ላይ ፅሁፎችን በራሱ ስክሪን ልክ እንዲያስተካክል */
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
        
        /* ረዳት ክፍሎች */
        .text-success { color: var(--success-color) !important; }
        .text-danger { color: var(--danger-color) !important; }
        .text-warning { color: var(--warning-color) !important; }
        .text-purple { color: var(--purple-color) !important; }
        
        /* የመግቢያ ክፍል */
        .login-box {
            width: 100%;
            max-width: 400px; 
            margin: 40px auto;
            background-color: var(--card-bg); 
            padding: 25px;
            border-radius: 16px; 
            box-shadow: 0 15px 35px rgba(0,0,0,0.5);
            text-align: center; 
            border: 1px solid var(--border-color);
        }
        .login-box h2 { color: var(--success-color); font-size: 1.6rem; margin-bottom: 8px; font-weight: 800; }
        .login-box p { color: #94a3b8; font-size: 0.85rem; margin-bottom: 20px; }

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

        /* ሄደሮች */
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
        
        /* ዳሽቦርድ */
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

        /* አዝራሮች */
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

        /* ሰንጠረዦች (በየትኛውም ስልክ እንዳይጨፈለቁ ተደርገዋል) */
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

        /* የሞዳል እይታ ማስተካከያ */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.85); backdrop-filter: blur(4px); display: flex; justify-content: center; align-items: center; z-index: 1000; padding: 15px; }
        .modal-card { background-color: var(--card-bg); border-radius: 14px; width: 100%; max-width: 420px; padding: 20px; box-shadow: 0 15px 30px rgba(0,0,0,0.5); border: 1px solid var(--border-color); overflow-y: auto; max-height: 90vh; }
        .modal-header { margin-bottom: 12px; border-bottom: 1px solid var(--border-color); padding-bottom: 8px; }
        .modal-header h3 { font-size: 1.2rem; color: var(--success-color); }
        .modal-body { margin-bottom: 15px; }
        .modal-footer { display: flex; justify-content: flex-end; gap: 8px; }
    </style>
</head>
<body class="theme-deepblue">

<div class="container">

    <!-- 1. የመግቢያ ገጽ -->
    <div id="loginPage" class="login-box">
        <h2>ትርፌ አፕ (Tirfe Tech)</h2>
        <p>የኪራይና የሱቅ መቆጣጠሪያ ማዕከላዊ ሲስተም v7.5</p>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል (Password)">
        <button class="btn-sell btn-block" onclick="handleLogin()">🔒 ደህንነቱ የተጠበቀ መግቢያ</button>
        <p id="loginError" style="color: var(--danger-color); margin-top: 15px; font-size: 0.9rem; font-weight: bold;"></p>
    </div>

    <!-- 2. የባለቤቱ ማዕከላዊ መቆጣጠሪያ ገጽ (ADMIN) -->
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
                <input type="text" id="newPassword" placeholder="የይለፍ ቃል (Password)">
            </div>
            <div style="display: grid; grid-template-columns: 1fr 1fr; gap: 10px;">
                <input type="text" id="newPhone" placeholder="ስልክ ቁጥር">
                <input type="text" id="newTelegram" placeholder="የቴሌግራም መግቢያ (@username)">
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
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የሱቅ ስም</th>
                            <th>የተከራይ መገለጫ (Profile)</th>
                            <th>የመግቢያ ዝርዝር</th>
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

    <!-- 3. የተከራይ የሱቅ መቆጣጠሪያ ገጽ (MAIN APP) -->
    <div id="appPage" class="hidden">
        <div class="welcome-header">
            <h1 id="shopTitle">እንኳን ደህና መጡ ወደ " ትርፌ " መቆጣጠሪያ መተግበሪያ</h1>
            <p id="roleSubTitle">ቀልጣፋ፣ ዘመናዊ እና አስተማማኝ የሱቅዎ የሂሳብ ረዳት</p>
        </div>

        <div id="sessionDisplay" class="session-badge">የዕለቱ መፈላጊ መረጃ በመጠባበቅ ላይ...</div>

        <!-- የባለቤት ዳሽቦርድ (ለሰራተኛ አይታይም) -->
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

        <!-- የጋራ የካሽ ማሳያ (ለሁሉም የሚታይ) -->
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
            <button class="btn-draw" onclick="openDrawerModal()">💸 ከሳጥን መልስ/ብር አንሳ</button>
            <button class="btn-config" onclick="configureBank()">🏦 የባንክ አፕ ማያያዣ</button>
            <button class="btn-next-day" id="btn_next_day" onclick="startNewDaySession()">🔄 አዲስ ቀን ጀምር</button>
            <button class="btn-clear" id="btn_clear_all" onclick="clearAllTenantData()">🗑️ ዳታ አጽዳ</button>
        </div>

        <!-- የዕቃዎች ዝርዝር ሰንጠረዥ -->
        <div class="section-box">
            <h2>📦 የዕቃዎች ዝርዝር እና ክምችት</h2>
            <div id="owner_add_box" style="background: rgba(255, 255, 255, 0.02); padding: 12px; border-radius: 8px; margin-bottom: 15px; border: 1px solid var(--border-color);">
                <input type="text" id="itemName" placeholder="የዕቃ ስም">
                <input type="number" id="itemCost" placeholder="መግዣ ዋጋ (ካፒታል)">
                <input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ">
                <input type="number" id="itemQty" placeholder="የመጣው ብዛት">
                <button class="btn-sell btn-block" style="margin-top: 5px;" onclick="addItemDirectly()">📥 ዕቃውን በዝርዝር አስገባ</button>
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

        <!-- የታችኛው መዝገቦች -->
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
                <h2>🔄 የቀደሙ ቀናት ታሪክ (የአሰሪው ብቻ)</h2>
                <div class="table-responsive">
                    <table>
                        <tbody id="historyBody"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- መቼቶች እና ፕሮፋይል -->
        <div class="section-box">
            <h2>⚙️ መቼቶች እና ፕሮፋይል</h2>
            <div class="profile-data"><label>የሱቅ ስም፡</label> <span id="profShopName">-</span></div>
            <div class="profile-data" id="expiryRowOwner"><label>የውል ማቂያ ቀን፡</label> <span id="profExpiry" style="color: var(--danger-color);">-</span></div>
            
            <!-- አሰሪው ለሰራተኛ ኮድ የሚሰጥበት ክፍል -->
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

<!-- ማዕከላዊ የሞዳል ሲስተም -->
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

    db.ref('tirfe_system').on('value', (snapshot) => {
        if(snapshot.exists()) {
            localDB = snapshot.val();
            if(!localDB.tenants) localDB.tenants = {};
            
            if(currentTenant) {
                let checkTenant = localDB.tenants[currentTenant.username];
                if(!checkTenant || checkTenant.status === "blocked") { logout(); return; }
                currentTenant = checkTenant;
                renderApp();
            }
            if(!document.getElementById('adminPage').classList.contains('hidden')) { renderAdminPanel(); }
        }
    });

    function sendTelegramAlert(message) {
        if(currentTenant && currentTenant.telegramToken && currentTenant.telegramChatId) {
            let url = `https://api.telegram.org/bot${currentTenant.telegramToken}/sendMessage?chat_id=${currentTenant.telegramChatId}&text=${encodeURIComponent(message)}&parse_mode=Markdown`;
            fetch(url).catch(err => console.log("Telegram Error: ", err));
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

    function pushToFirebase() { db.ref('tirfe_system').set(localDB); }
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

    function handleLogin() {
        let user = document.getElementById('loginUser').value.trim().toLowerCase();
        let pass = document.getElementById('loginPass').value.trim();
        let error = document.getElementById('loginError');

        if(user === "admin" && pass === "admin123") {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('adminPage').classList.remove('hidden');
            renderAdminPanel();
            return;
        }

        let tenant = localDB.tenants ? localDB.tenants[user] : null;
        if(tenant && String(tenant.password).trim() === pass) {
            if (isTenantExpired(tenant, error)) return;
            currentUserRole = "owner";
            launchApp(tenant);
            return;
        }

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
        error.innerText = "❌ የተጠቃሚ ስም ወይም የይለፍ ቃል ስህተት ነው!";
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
        document.getElementById('appPage').classList.remove('hidden');
        
        document.getElementById('shopTitle').innerText = tenant.shopName + (currentUserRole === "staff" ? " (የሰራተኛ ገጽ)" : " (የባለቤት ገጽ)");
        document.getElementById('roleSubTitle').innerText = currentUserRole === "staff" ? "🛠️ የተገደበ የሰራተኛ መሸጫ እና መመዝገቢያ ሞድ" : "👑 ሙሉ የሱቅና የኪራይ መቆጣጠሪያ ፓነል";
        document.getElementById('profShopName').innerText = tenant.shopName;
        document.getElementById('profExpiry').innerText = tenant.expiryDate ? `${tenant.expiryDate} (${tenant.contractType})` : "ያልተገደበ";
        
        let activeTheme = tenant.theme || 'theme-deepblue';
        document.body.className = activeTheme;
        document.getElementById('themeSelector').value = activeTheme;

        if (currentUserRole === "staff") {
            document.getElementById('ownerDashboard').classList.add('hidden');
            document.getElementById('chartContainer').classList.add('hidden');
            document.getElementById('btn_add_item').classList.add('hidden');
            document.getElementById('btn_expense').classList.add('hidden');
            document.getElementById('btn_next_day').classList.add('hidden');
            document.getElementById('btn_clear_all').classList.add('hidden');
            document.getElementById('owner_add_box').classList.add('hidden');
            document.getElementById('historySection').classList.add('hidden');
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
            document.getElementById('historySection').classList.remove('hidden');
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

    function saveStaffAccount() {
        let sUser = document.getElementById('staffUser').value.trim().toLowerCase();
        let sPass = document.getElementById('staffPass').value.trim();
        if(!sUser || !sPass) { showCustomAlert("ስህተት", "እባክዎ የሰራተኛውን መግቢያና የይለፍ ቃል በትክክል ይሙሉ!"); return; }
        
        currentTenant.staffUser = sUser;
        currentTenant.staffPass = sPass;
        saveAndRefresh();
        showCustomAlert("ተቀምጧል", "የሰራተኛው አካውንት ተፈጥሯል። አሁን ሰራተኛው በዚህ ኮድ መግባት ይችላል!");
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

    function registerTenant() {
        let shop = document.getElementById('newShopName').value.trim();
        let fullName = document.getElementById('newFullName').value.trim();
        let user = document.getElementById('newUsername').value.trim().toLowerCase(); 
        let pass = document.getElementById('newPassword').value.trim();
        let phone = document.getElementById('newPhone').value.trim();
        let telegram = document.getElementById('newTelegram').value.trim();
        let address = document.getElementById('newAddress').value.trim();
        let contractType = document.getElementById('newContractType').value;
        let expiryDate = document.getElementById('newExpiryDate').value;

        if(!shop || !user || !pass || !expiryDate || !fullName || !phone) { 
            showCustomAlert("ስህተት", "እባክዎ መሠረታዊ መረጃዎችን ያሟሉ!"); 
            return; 
        }
        
        localDB.tenants[user] = { 
            shopName: shop, fullName: fullName, phone: phone, telegram: telegram || "-", address: address || "-",
            username: user, password: pass, contractType: contractType, expiryDate: expiryDate,
            status: "active", theme: "theme-deepblue",
            staffUser: "", staffPass: "",
            data: { sessionActive: false, inventory: [], expenses: [], debts: [], drawerLog: [], history: [] } 
        };
        pushToFirebase(); renderAdminPanel();
        
        document.getElementById('newShopName').value = ''; document.getElementById('newFullName').value = '';
        document.getElementById('newUsername').value = ''; document.getElementById('newPassword').value = ''; 
        document.getElementById('newPhone').value = ''; document.getElementById('newTelegram').value = '';
        document.getElementById('newAddress').value = ''; document.getElementById('newExpiryDate').value = '';
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody'); tbody.innerHTML = '';
        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            let statusBadge = t.status === "active" ? `<span class="badge-success">Active</span>` : `<span class="badge-danger">Blocked</span>`;
            let profileInfo = `👤 <b>${t.fullName || '-'}</b><br>📞 ${t.phone || '-'}<br>📍 ${t.address || '-'}<br>✈️ ${t.telegram || '-'}`;
            let loginInfo = `👑 አሰሪ: <code>${t.username}</code> / <code>${t.password}</code><br>🛠️ ሰራተኛ: <code>${t.staffUser || 'ያልተፈጠረ'}</code> / <code>${t.staffPass || '-'}</code>`;

            tbody.innerHTML += `<tr>
                <td><b>${t.shopName}</b></td>
                <td>${profileInfo}</td>
                <td>${loginInfo}</td>
                <td><span style="color:var(--accent-color)">${t.contractType || 'በወር'}</span></td>
                <td style="color:var(--danger-color)"><b>${t.expiryDate || '-'}</b></td>
                <td>${statusBadge}</td>
                <td>
                    <button class="btn-config btn-sm" onclick="toggleTenantStatus('${t.username}')">ሁኔታ ቀይር</button>
                    <button class="btn-expense btn-sm" onclick="deleteTenant('${t.username}')">Delete</button>
                </td>
            </tr>`;
        });
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
        document.getElementById('adminPage').classList.add('hidden'); 
        document.getElementById('appPage').classList.add('hidden'); 
        document.getElementById('loginPage').classList.remove('hidden'); 
    }

    function saveAndRefresh() { 
        localDB.tenants[currentTenant.username] = currentTenant; pushToFirebase(); renderApp(); 
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
        inv.push({ name, cost, price, qty, sold: 0, profit: 0 });
        currentTenant.data.inventory = inv; saveAndRefresh();
        
        document.getElementById('itemName').value = ''; document.getElementById('itemCost').value = ''; 
        document.getElementById('itemPrice').value = ''; document.getElementById('itemQty').value = '';
    }

    function sellItem(idx) {
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
            sendTelegramAlert(`🛍️ የሽያጭ ማስታወቂያ (${currentUserRole === 'staff' ? 'በሰራተኛ' : 'በአሰሪ'})፦\nዕቃ፡ ${item.name}\nብዛት፡ ${count} ፍሬ\nዋጋ፡ ${item.price * count} ETB`);
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

    function openDrawerModal() {
        showFormModal("ከሳጥን መልስ / ብር ማንሻ", [
            { id: "reason", label: "ምክንያት / ለማን ተሰጠ (መለወጫ መልስ ከሆነ 'ለመልስ' ይበሉ)", type: "text", placeholder: "ምሳሌ፡ ለመልስ መለወጫ" },
            { id: "amount", label: "የገንዘብ መጠን (ETB)", type: "number", placeholder: "0.00" }
        ], (res) => {
            let amount = parseFloat(res.amount) || 0; let reason = res.reason.trim();
            if(!reason || amount <= 0) return;
            let d = currentTenant.data || {}; if(!d.drawerLog) d.drawerLog = [];
            d.drawerLog.push({ reason, amount, time: new Date().toLocaleTimeString('am-ET') });
            currentTenant.data = d; saveAndRefresh();
            sendTelegramAlert(`💸 ከሳጥን ገንዘብ ተነሳ (${currentUserRole === 'staff' ? 'በሰራተኛ' : 'በአሰሪ'})፦\nምክንያት፡ ${reason}\nመጠን፡ ${amount} ETB`);
        });
    }

    function configureBank() {
        if(currentUserRole === "staff") {
            showCustomAlert("🏦 የባንክ ሂሳብ መረጃ ማያያዣ", `የአሰሪው የባንክ ሂሳብ ቁጥር (CBE/Telebirr)፦ ${currentTenant.bankAccount || "ያልተገናኘ"}\n\n*ማሳሰቢያ፦ የባንክ ቁጥሩን መቀየር የሚችለው አሰሪው ብቻ ነው።`);
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

    function startNewDaySession() {
        if(currentUserRole === "staff") return;
        showCustomConfirm("አዲስ ቀን መጀመር", "የዛሬውን ቀን ሂሳብ ዘግተው አዲስ ቀን መጀመር ይፈልጋሉ?", () => {
            let d = currentTenant.data || {}; let session = d.sessionData || {};
            let tSales = parseFloat(d.collectedCreditToday || 0);
            let todayProfit = 0; let tExp = 0; let tDraw = 0; let inv = d.inventory || [];
            
            (d.expenses || []).forEach(e => tExp += parseFloat(e.amount) || 0);
            (d.drawerLog || []).forEach(dr => tDraw += parseFloat(dr.amount) || 0);
            
            inv.forEach(item => {
                tSales += (item.price * item.sold);
                todayProfit += (item.price - item.cost) * item.sold;
                item.qty = Math.max(0, item.qty - item.sold); item.sold = 0;
            });

            let netProfit = todayProfit - tExp;
            if(!d.history) d.history = [];
            d.history.push({
                date: session.date || new Date().toLocaleDateString('am-ET'), employee: session.employee || "ሰራተኛ",
                sales: tSales, profit: netProfit, expenses: tExp, draws: tDraw
            });

            d.sessionActive = false; d.expenses = []; d.drawerLog = []; d.collectedCreditToday = 0;
            currentTenant.data = d; saveAndRefresh(); checkMorningSession();
        });
    }

    function clearAllTenantData() {
        if(currentUserRole === "staff") return;
        showCustomConfirm("ሁሉንም ዳታ ማጽዳት", "ሁሉንም ዳታ ለማጥፋት እርግጠኛ ኖት?", () => {
            currentTenant.data = { sessionActive: false, inventory: [], expenses: [], debts: [], drawerLog: [], history: [] };
            saveAndRefresh(); checkMorningSession();
        });
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

        let inv = d.inventory || [];
        inv.forEach((item, idx) => {
            let remaining = Math.max(0, item.qty - item.sold); 
            let profit = (item.price - item.cost) * item.sold;
            tSales += (item.price * item.sold); todayProfit += profit; currentTotalCapital += (item.cost * remaining);
            
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
                drawBody.innerHTML += `<tr>
                    <td>${dr.reason}</td>
                    <td style="color:var(--purple-color); font-weight:bold;">${dr.amount} ETB</td>
                    <td>${dr.time}</td>
                </tr>`;
            });
        }

        if(currentUserRole === "owner") {
            let historyBody = document.getElementById('historyBody');
            historyBody.innerHTML = '<tr><th>ቀን</th><th>ሰራተኛ</th><th>ሽያጭ</th><th>ትርፍ</th><th>ወጪ</th></tr>';
            let historyList = d.history || [];
            if(historyList.length === 0) {
                historyBody.innerHTML += '<tr><td colspan="5" style="text-align:center; color:#94a3b8;">ምንም ያለፈ ታሪክ የለም</td></tr>';
            } else {
                historyList.forEach(h => {
                    historyBody.innerHTML += `<tr>
                        <td><b>${h.date}</b></td>
                        <td>${h.employee}</td>
                        <td style="color:var(--success-color)">${h.sales}</td>
                        <td style="color:var(--accent-color)"><b>${h.profit}</b></td>
                        <td>${h.expenses}</td>
                    </tr>`;
                });
            }
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
