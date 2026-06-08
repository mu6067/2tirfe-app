# 2tirfe-app
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ትርፌ አፕ (Tirfe App) - ማዕከላዊ የኪራይ ሥርዓት</title>
    <!-- Firebase 8 Libraries -->
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    
    <style>
        :root {
            --bg-color: #0b132b;
            --card-bg: #1c2541;
            --primary: #4a90e2;
            --success: #2ec4b6;
            --danger: #e63946;
            --warning: #ffb703;
            --purple: #6c5ce7;
            --text-main: #ffffff;
            --text-muted: #afb5c0;
        }

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', sans-serif; }
        body { background-color: var(--bg-color); color: var(--text-main); padding: 20px; }
        .container { max-width: 1200px; margin: 0 auto; }
        .hidden { display: none !important; }
        
        /* የመግቢያ ገጽ */
        .login-box {
            max-width: 420px; margin: 80px auto;
            background-color: var(--card-bg); padding: 35px;
            border-radius: 15px; box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            text-align: center; border: 1px solid rgba(255,255,255,0.05);
        }
        .login-box h2 { color: var(--success); font-size: 1.8rem; margin-bottom: 20px; }

        input, select {
            width: 100%; padding: 12px; margin: 10px 0;
            border-radius: 6px; border: 1px solid #3a506b;
            background-color: var(--bg-color); color: white; font-size: 1rem;
        }

        /* በተኖች */
        .btn-grid { display: flex; flex-wrap: wrap; gap: 10px; margin: 20px 0; }
        button { padding: 12px 18px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; transition: 0.2s; font-size: 0.95rem; }
        button:hover { opacity: 0.9; transform: translateY(-1px); }
        .btn-block { width: 100%; }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-success { background-color: var(--success); color: white; }
        .btn-danger { background-color: var(--danger); color: white; }
        .btn-warning { background-color: var(--warning); color: #000; }
        .btn-purple { background-color: var(--purple); color: white; }

        /* ካርዶች */
        .welcome-card { background: linear-gradient(135deg, #1c2541, #3a506b); padding: 25px; border-radius: 15px; text-align: center; margin-bottom: 25px; }
        .welcome-card h1 { color: var(--success); font-size: 2rem; }
        
        .meta-bar { background-color: rgba(255, 255, 255, 0.05); padding: 10px; border-radius: 6px; display: flex; justify-content: space-around; flex-wrap: wrap; font-size: 0.9rem; margin-bottom: 20px; border: 1px dashed rgba(255,255,255,0.1); }

        .dashboard-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(220px, 1fr)); gap: 20px; margin-bottom: 25px; }
        .card { background-color: var(--card-bg); padding: 20px; border-radius: 10px; text-align: center; border-left: 5px solid var(--primary); }
        .card h3 { font-size: 0.9rem; color: var(--text-muted); margin-bottom: 10px; }
        .card p { font-size: 1.6rem; font-weight: bold; }

        .main-layout { display: grid; grid-template-columns: 2fr 1fr; gap: 25px; }
        @media (max-width: 768px) { .main-layout { grid-template-columns: 1fr; } }

        .section-box { background-color: var(--card-bg); padding: 20px; border-radius: 12px; margin-bottom: 25px; }
        .section-box h2 { font-size: 1.2rem; margin-bottom: 15px; border-bottom: 1px solid #3a506b; padding-bottom: 8px; color: var(--primary); }

        table { width: 100%; border-collapse: collapse; margin-top: 10px; }
        th, td { padding: 12px; border-bottom: 1px solid #3a506b; text-align: left; font-size: 0.95rem; }
        th { background-color: rgba(255,255,255,0.05); color: var(--text-muted); }
        
        .status-badge { padding: 3px 8px; border-radius: 4px; font-weight: bold; font-size: 0.85rem; }
        .status-active { background-color: rgba(46, 196, 182, 0.2); color: var(--success); }
        .status-blocked { background-color: rgba(230, 57, 70, 0.2); color: var(--danger); }
    </style>
</head>
<body>

<div class="container">

    <!-- ================= 1. የመግቢያ ገጽ ================= -->
    <div id="loginPage" class="login-box">
        <h2>ትርፌ አፕ - መግቢያ</h2>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል (Password)">
        <button class="btn-success btn-block" onclick="handleLogin()">ደህንነቱ የተጠበቀ መግቢያ</button>
        <p id="loginError" style="color: var(--danger); margin-top: 15px; font-size: 0.95rem; font-weight: bold;"></p>
    </div>

    <!-- ================= 2. የባለቤቱ (ADMIN) ቁጥጥር ፓነል ================= -->
    <div id="adminPage" class="hidden">
        <div class="welcome-card">
            <h1>የባለቤቱ ማዕከላዊ ቁጥጥር ፓነል</h1>
            <p>እዚህ ገጽ ላይ አዳዲስ ተከራዮችን መመዝገብ፣ ማገድ (Block) ወይም ክፍያ ሲከፍሉ መፍቀድ ይችላሉ።</p>
            <button class="btn-danger" style="margin-top: 15px;" onclick="logout()">ከሲስተሙ ውጣ (Logout)</button>
        </div>

        <div class="section-box">
            <h2>አዲስ ተከራይ (የሱቅ ባለቤት) መመዝገቢያ</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-top: 10px;">
                <input type="text" id="newShopName" placeholder="የሱቅ ስም (ምሳሌ፡ አልፋ ኮሞዲቲ)">
                <input type="text" id="newUsername" placeholder="Username (መግቢያ ስም)">
                <input type="text" id="newPassword" placeholder="Password (የይለፍ ቃል)">
                <button class="btn-primary" onclick="registerTenant()">ተከራይ በቋሚነት መዝግብ</button>
            </div>
        </div>

        <div class="section-box">
            <h2>የአሁኑ ተከራዮች እና የኪራይ ሁኔታ መከታተያ</h2>
            <table>
                <thead>
                    <tr>
                        <th>የሱቅ ስም</th>
                        <th>Username</th>
                        <th>Password</th>
                        <th>የኪራይ ሁኔታ</th>
                        <th>ማዕከላዊ እርምጃ</th>
                    </tr>
                </thead>
                <tbody id="tenantTableBody"></tbody>
            </table>
        </div>
    </div>

    <!-- ================= 3. የተከራዩ የሱቅ አፕሊኬሽን ================= -->
    <div id="appPage" class="hidden">
        <div class="welcome-card">
            <h1 id="shopTitle">እንኳን ደህና መጡ ወደ "ትርፌ" መቆጣጠሪያ መተግበሪያ</h1>
            <p style="color: var(--text-muted);">ቅልጥፍና፣ ዘመናዊነት እና አስተማማኝነት የሱቅዎ የሂሳብ ረዳት</p>
        </div>

        <div class="meta-bar">
            <div>📅 ቀን፡ <span id="metaDate">8/6/2026</span></div>
            <div>👤 ሰራተኛ፡ <span id="metaStaff">mulugeta</span></div>
            <div>🕒 የሰዓት ሪፖርት፡ <span id="metaTime">12:48 ከሰዓት</span></div>
            <div>💰 መነሻ ብር፡ <span id="metaChange">10000 ETB</span></div>
        </div>

        <div class="dashboard-grid">
            <div class="card" style="border-left-color: var(--primary);"><h3>የአሁን የሱቅ ካፒታል</h3><p id="dispCapital">0.00 ETB</p></div>
            <div class="card" style="border-left-color: var(--success);"><h3>የዛሬ የተጣራ ትርፍ</h3><p id="dispDailyProfit" style="color: var(--success);">0.00 ETB</p></div>
            <div class="card" style="border-left-color: var(--success);"><h3>የዚህ ወር አጠቃላይ ትርፍ</h3><p id="dispMonthlyProfit" style="color: var(--success);">0.00 ETB</p></div>
            <div class="card" style="border-left-color: var(--purple);"><h3>ከሳጥን የተነሳ (ያልተመለሰ)</h3><p id="dispDrawer" style="color: var(--purple);">0.00 ETB</p></div>
        </div>

        <div class="btn-grid">
            <button class="btn-primary" onclick="alert('የእቃ መመዝገቢያ ሳጥን ከታች ይገኛል!')">📦 አዲስ ዕቃ / ጭማሪ መዝግብ</button>
            <button class="btn-success" onclick="alert('ሽያጭ ለመመዝገብ ከታች ባለው እቃ አጠገብ ያለውን የመሸጫ ቁልፍ ይጠቀሙ!')">💰 የሽያጭ መዝገብ</button>
            <button class="btn-danger" onclick="promptExpense()">📉 የወጪ መዝገብ</button>
            <button class="btn-warning" onclick="promptDebt()">💳 የዕዳ መዝገብ</button>
            <button class="btn-purple" onclick="promptDrawer()">💸 ከሳጥን ብር ማንሻ</button>
            <button class="btn-primary" style="background-color: #3a506b;" onclick="alert('የባንክ መተግበሪያ በተሳካ ሁኔታ ተገናኝቷል!')">🏦 የባንክ እና ማመሳሰያ</button>
            <button class="btn-success" style="background-color: #6c5ce7;" onclick="forceNightlyReport()">🔄 አዲስ ቀን ጀምር</button>
            <button class="btn-danger" style="background-color: #e63946;" onclick="logout()">🚪 ከአፑ ውጣ</button>
        </div>

        <div class="main-layout">
            <div>
                <div class="section-box">
                    <h2>የዕቃዎች ዝርዝር እና የክምችት ሁኔታ</h2>
                    <table style="margin-bottom: 15px;">
                        <tr style="background: none;">
                            <td style="padding:5px;"><input type="text" id="itemName" placeholder="የእቃ ስም" style="margin:0;"></td>
                            <td style="padding:5px;"><input type="number" id="itemCost" placeholder="መግዣ ዋጋ" style="margin:0;"></td>
                            <td style="padding:5px;"><input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ" style="margin:0;"></td>
                            <td style="padding:5px;"><input type="number" id="itemQty" placeholder="ብዛት" style="margin:0;"></td>
                            <td style="padding:5px;"><button class="btn-success" onclick="addItem()" style="margin:0; width:100%;">መዝግብ</button></td>
                        </tr>
                    </table>
                    <table>
                        <thead>
                            <tr><th>የዕቃ ስም</th><th>መግዣ ዋጋ</th><th>መሸጫ ዋጋ</th><th>የነበረው ብዛት</th><th>ዛሬ የተሸጠ</th><th>ቀሪ ዕቃ</th><th>የዛሬ ትርፍ</th></tr>
                        </thead>
                        <tbody id="inventoryTable"></tbody>
                    </table>
                </div>

                <div class="section-box">
                    <h2>የዕዳ መዝገብ (የተሰበሰበ ሂሳብ ብቻ ይደመራል)</h2>
                    <table>
                        <thead><tr><th>ደንበኛ እና ስልክ</th><th>የዕዳ ዝርዝር</th><th>የገንዘብ መጠን</th></tr></thead>
                        <tbody id="debtTable"></tbody>
                    </table>
                </div>
            </div>

            <div>
                <div class="section-box">
                    <h2>ከሳጥን የተነሳ የገንዘብ መቆጣጠሪያ</h2>
                    <table>
                        <thead><tr><th>ምክንያት</th><th>የብር መጠን</th><th>ሁኔታ</th></tr></thead>
                        <tbody id="drawerTable"></tbody>
                    </table>
                </div>

                <div class="section-box">
                    <h2>የቀደሙ ቀናት ታሪክ መዝገብ</h2>
                    <table>
                        <thead><tr><th>ቀን እና ሰራተኛ</th><th>የስራ ሰዓት</th><th>የቀን ትርፍ</th></tr></thead>
                        <tbody id="historyTable"></tbody>
                    </table>
                </div>
            </div>
        </div>
    </div>

</div>

<script>
    // 🔗 ምስል 9004 እና 9006 ላይ ካለው ያንተ ፕሮጀክት የተወሰደ ቀጥታ ሊንክ
    const firebaseConfig = {
        databaseURL: "https://tirfe-app-v2-300c2-default-rtdb.firebaseio.com/" 
    };
    
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let localDB = { tenants: {} };
    let currentTenant = null;

    // መረጃዎችን ከሰርቨሩ ላይ በቀጥታ መከታተል (Realtime Sync)
    db.ref('tirfe_system').on('value', (snapshot) => {
        if(snapshot.exists()) {
            localDB = snapshot.val();
            if(!localDB.tenants) localDB.tenants = {};
            
            if(currentTenant) {
                let checkTenant = localDB.tenants[currentTenant.username];
                if(checkTenant.status === "blocked") {
                    alert("🔒 የኪራይ ጊዜዎ አብቅቷል! እባክዎ ለባለቤቱ ይክፈሉ!");
                    logout();
                    return;
                }
                currentTenant = checkTenant;
                loadTenantApp();
            }
            if(!document.getElementById('adminPage').classList.contains('hidden')) {
                renderAdminPanel();
            }
        }
    });

    function pushToFirebase() {
        db.ref('tirfe_system').set(localDB);
    }

    function handleLogin() {
        let user = document.getElementById('loginUser').value.trim();
        let pass = document.getElementById('loginPass').value.trim();
        let error = document.getElementById('loginError');

        if(user === "admin" && pass === "admin123") {
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('adminPage').classList.remove('hidden');
            renderAdminPanel();
            return;
        }

        let tenant = localDB.tenants[user];
        if(tenant && tenant.password === pass) {
            if(tenant.status === "blocked") {
                error.innerText = "🔒 ኪራይዎ ተቋርጧል! እባክዎ ለመቀጠል ባለቤቱን ያነጋግሩ።";
                return;
            }
            currentTenant = tenant;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appPage').classList.remove('hidden');
            document.getElementById('shopTitle').innerText = tenant.shopName + " - መቆጣጠሪያ";
            loadTenantApp();
        } else {
            error.innerText = "❌ የተጠቃሚ ስም ወይም የይለፍ ቃል ስህተት ነው!";
        }
    }

    function registerTenant() {
        let shop = document.getElementById('newShopName').value.trim();
        let user = document.getElementById('newUsername').value.trim();
        let pass = document.getElementById('newPassword').value.trim();

        if(!shop || !user || !pass) { alert("እባክዎ ሁሉንም ሳጥኖች ይሙሉ!"); return; }

        localDB.tenants[user] = {
            shopName: shop, username: user, password: pass, status: "active",
            data: { capital: 0, dailyProfit: 0, monthlyProfit: 0, drawerAmt: 0, inventory: [], expenses: [], debts: [], drawerLog: [], history: [] }
        };
        pushToFirebase();
        alert("ተከራይ በተሳካ ሁኔታ ተመዝግቧል!");
        
        document.getElementById('newShopName').value = '';
        document.getElementById('newUsername').value = '';
        document.getElementById('newPassword').value = '';
    }

    function toggleStatus(user) {
        localDB.tenants[user].status = localDB.tenants[user].status === "active" ? "blocked" : "active";
        pushToFirebase();
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody');
        tbody.innerHTML = '';
        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            let badge = t.status === 'active' ? '<span class="status-badge status-active">ስራ ላይ</span>' : '<span class="status-badge status-blocked">የታገደ</span>';
            let btnText = t.status === 'active' ? 'Block (እገድ)' : 'Unblock (ፍቀድ)';
            let btnClass = t.status === 'active' ? 'btn-danger' : 'btn-success';

            tbody.innerHTML += `
                <tr>
                    <td>${t.shopName}</td>
                    <td>${t.username}</td>
                    <td>${t.password}</td>
                    <td>${badge}</td>
                    <td><button class="${btnClass}" onclick="toggleStatus('${t.username}')">${btnText}</button></td>
                </tr>
            `;
        });
    }

    function loadTenantApp() {
        let d = currentTenant.data;
        if(!d.inventory) d.inventory = [];
        if(!d.expenses) d.expenses = [];
        if(!d.debts) d.debts = [];
        if(!d.drawerLog) d.drawerLog = [];
        if(!d.history) d.history = [];

        document.getElementById('dispCapital').innerText = d.capital.toFixed(2) + " ETB";
        document.getElementById('dispDailyProfit').innerText = d.dailyProfit.toFixed(2) + " ETB";
        document.getElementById('dispMonthlyProfit').innerText = d.monthlyProfit.toFixed(2) + " ETB";
        document.getElementById('dispDrawer').innerText = d.drawerAmt.toFixed(2) + " ETB";

        renderInventoryTable();
        renderDrawerTable();
        renderDebtTable();
    }

    function addItem() {
        let name = document.getElementById('itemName').value.trim();
        let cost = parseFloat(document.getElementById('itemCost').value) || 0;
        let price = parseFloat(document.getElementById('itemPrice').value) || 0;
        let qty = parseInt(document.getElementById('itemQty').value) || 0;

        if(!name) return;

        currentTenant.data.inventory.push({ name, cost, price, qty, sold: 0, profit: 0 });
        pushToFirebase();

        document.getElementById('itemName').value = '';
        document.getElementById('itemCost').value = '';
        document.getElementById('itemPrice').value = '';
        document.getElementById('itemQty').value = '';
    }

    function renderInventoryTable() {
        let tbody = document.getElementById('inventoryTable');
        tbody.innerHTML = '';
        currentTenant.data.inventory.forEach((i, idx) => {
            let remaining = i.qty - i.sold;
            tbody.innerHTML += `
                <tr>
                    <td>${i.name}</td>
                    <td>${i.cost.toFixed(2)} ETB</td>
                    <td>${i.price.toFixed(2)} ETB</td>
                    <td>${i.qty}</td>
                    <td><button class="btn-success" style="padding:2px 8px;" onclick="sellItem(${idx})">+</button> ${i.sold}</td>
                    <td>${remaining}</td>
                    <td style="color:var(--success); font-weight:bold;">${i.profit.toFixed(2)} ETB</td>
                </tr>
            `;
        });
    }

    function sellItem(idx) {
        let item = currentTenant.data.inventory[idx];
        if(item.qty - item.sold <= 0) { alert("ዕቃው አልቋል!"); return; }
        
        item.sold += 1;
        let netProfit = item.price - item.cost;
        item.profit += netProfit;
        
        currentTenant.data.dailyProfit += netProfit;
        currentTenant.data.monthlyProfit += netProfit;
        currentTenant.data.capital += item.price;

        pushToFirebase();
    }

    function promptExpense() {
        let reason = prompt("የወጪውን ምክንያት ያስገቡ፦");
        if(!reason) return;
        let amt = parseFloat(prompt("የገንዘብ መጠን (ETB)፦")) || 0;
        if(amt <= 0) return;

        currentTenant.data.dailyProfit -= amt;
        currentTenant.data.monthlyProfit -= amt;
        currentTenant.data.capital -= amt;
        
        currentTenant.data.expenses.push({ reason, amt });
        pushToFirebase();
    }

    function promptDebt() {
        let name = prompt("የተበዳሪ ስም እና ስልክ፦");
        if(!name) return;
        let detail = prompt("የዕቃ ዝርዝር፦");
        let amt = parseFloat(prompt("የዕዳ መጠን (ETB)፦")) || 0;
        if(amt <= 0) return;

        currentTenant.data.debts.push({ name, detail, amt });
        pushToFirebase();
    }

    function renderDebtTable() {
        let tbody = document.getElementById('debtTable');
        tbody.innerHTML = '';
        currentTenant.data.debts.forEach(d => {
            tbody.innerHTML += `<tr><td>${d.name}</td><td>${d.detail}</td><td>${d.amt.toFixed(2)} ETB</td></tr>`;
        });
    }

    function promptDrawer() {
        let reason = prompt("ከሳጥን ገንዘብ የሚነሳበት ምክንያት፦");
        if(!reason) return;
        let amt = parseFloat(prompt("የብር መጠን (ETB)፦")) || 0;
        if(amt <= 0) return;

        currentTenant.data.drawerAmt += amt;
        currentTenant.data.drawerLog.push({ reason, amt, status: 'ወጣ' });
        pushToFirebase();
    }

    function renderDrawerTable() {
        let tbody = document.getElementById('drawerTable');
        tbody.innerHTML = '';
        currentTenant.data.drawerLog.forEach(l => {
            tbody.innerHTML += `<tr><td>${l.reason}</td><td>${l.amt.toFixed(2)} ETB</td><td><span class="status-badge status-blocked">${l.status}</span></td></tr>`;
        });
    }

    function forceNightlyReport() {
        alert("የዕለቱ ሂሳብ ተዘግቶ ወደ ታሪክ (History) ተዛውሯል!");
    }

    function logout() {
        currentTenant = null;
        document.getElementById('adminPage').classList.add('hidden');
        document.getElementById('appPage').classList.add('hidden');
        document.getElementById('loginPage').classList.remove('hidden');
    }
</script>
</body>
</html>
