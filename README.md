tirfe app merchant new
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ትርፌ አፕ (Tirfe App) - ማዕከላዊ የኪራይ ሥርዓት</title>
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

        * { box-sizing: border-box; margin: 0; padding: 0; font-family: 'Segoe UI', Arial, sans-serif; }
        body { background-color: var(--bg-color); color: var(--text-main); padding: 15px; font-size: 14px; }
        .container { max-width: 1200px; margin: 0 auto; }
        .hidden { display: none !important; }
        
        /* Login Box */
        .login-box {
            max-width: 360px; margin: 60px auto;
            background-color: var(--card-bg); padding: 25px;
            border-radius: 12px; box-shadow: 0 10px 25px rgba(0,0,0,0.5);
            text-align: center; border: 1px solid rgba(255,255,255,0.05);
        }
        .login-box h2 { color: var(--success); font-size: 1.5rem; margin-bottom: 20px; }

        input {
            width: 100%; padding: 11px; margin: 8px 0;
            border-radius: 6px; border: 1px solid #3a506b;
            background-color: var(--bg-color); color: white; font-size: 0.95rem;
            outline: none;
        }
        input:focus { border-color: var(--primary); }

        /* Buttons */
        .btn-grid { display: grid; grid-template-columns: repeat(auto-fit, minmax(100px, 1fr)); gap: 8px; margin: 15px 0; }
        button { padding: 10px 12px; border: none; border-radius: 6px; cursor: pointer; font-weight: bold; font-size: 0.85rem; text-align: center; }
        button:active { transform: scale(0.98); }
        .btn-block { width: 100%; }
        .btn-primary { background-color: var(--primary); color: white; }
        .btn-success { background-color: var(--success); color: white; }
        .btn-danger { background-color: var(--danger); color: white; }
        .btn-warning { background-color: var(--warning); color: #000; }
        .btn-purple { background-color: var(--purple); color: white; }

        /* Cards & Dashboard */
        .welcome-card { background: linear-gradient(135deg, #1c2541, #3a506b); padding: 15px; border-radius: 12px; text-align: center; margin-bottom: 15px; }
        .welcome-card h1 { color: var(--success); font-size: 1.4rem; }
        .welcome-card p { font-size: 0.85rem; color: var(--text-muted); margin-top: 4px; }
        
        .meta-bar { background-color: rgba(255, 255, 255, 0.05); padding: 10px; border-radius: 6px; display: grid; grid-template-columns: repeat(auto-fit, minmax(120px, 1fr)); gap: 10px; font-size: 0.8rem; margin-bottom: 15px; text-align: center; }
        .dashboard-grid { display: grid; grid-template-columns: repeat(2, 1fr); gap: 10px; margin-bottom: 15px; }
        @media (min-width: 768px) { .dashboard-grid { grid-template-columns: repeat(4, 1fr); } }
        
        .card { background-color: var(--card-bg); padding: 15px; border-radius: 8px; text-align: center; border-left: 4px solid var(--primary); }
        .card h3 { font-size: 0.75rem; color: var(--text-muted); margin-bottom: 5px; }
        .card p { font-size: 1.2rem; font-weight: bold; }

        .section-box { background-color: var(--card-bg); padding: 15px; border-radius: 10px; margin-bottom: 15px; }
        .section-box h2 { font-size: 1rem; margin-bottom: 12px; border-bottom: 1px solid #3a506b; padding-bottom: 5px; color: var(--primary); }

        /* Tables */
        .table-responsive { width: 100%; overflow-x: auto; -webkit-overflow-scrolling: touch; border-radius: 6px; }
        table { width: 100%; border-collapse: collapse; min-width: 650px; }
        th, td { padding: 10px 12px; border-bottom: 1px solid #3a506b; text-align: left; font-size: 0.85rem; white-space: nowrap; }
        th { background-color: rgba(255,255,255,0.05); color: var(--text-muted); }
        
        .status-badge { padding: 3px 8px; border-radius: 4px; font-weight: bold; font-size: 0.75rem; display: inline-block; }
        .status-badge.status-active { background-color: rgba(46, 196, 182, 0.2); color: var(--success); }
        .status-badge.status-blocked { background-color: rgba(230, 57, 70, 0.2); color: var(--danger); }
        .action-btns { display: flex; gap: 5px; }
    </style>
</head>
<body>

<div class="container">

    <div id="loginPage" class="login-box">
        <h2>ትርፌ አፕ - መግቢያ</h2>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል (Password)">
        <button class="btn-success btn-block" onclick="handleLogin()">ደህንነቱ የተጠበቀ መግቢያ</button>
        <p id="loginError" style="color: var(--danger); margin-top: 15px; font-size: 0.85rem; font-weight: bold;"></p>
    </div>

    <div id="adminPage" class="hidden">
        <div class="welcome-card">
            <h1>የባለቤቱ ማዕከላዊ ቁጥጥር ፓነል</h1>
            <p>እዚህ ገጽ ላይ አዳዲስ ተከራዮችን መመዝገብ፣ ማገድ እና ሙሉ በሙሉ መሰረዝ ይችላሉ።</p>
            <button class="btn-danger" style="margin-top: 10px; padding: 6px 12px;" onclick="logout()">ከሲስተሙ ውጣ (Logout)</button>
        </div>

        <div class="section-box">
            <h2>አዲስ ተከራይ መመዝገቢያ</h2>
            <input type="text" id="newShopName" placeholder="የሱቅ ስም (ምሳሌ፡ አልፋ)">
            <input type="text" id="newUsername" placeholder="Username (መግቢያ ስም)">
            <input type="text" id="newPassword" placeholder="Password (የይለፍ ቃል)">
            <button class="btn-primary btn-block" onclick="registerTenant()">ተከራይ በቋሚነት መዝግብ</button>
        </div>

        <div class="section-box">
            <h2>የተከራዮች እና የኪራይ ሁኔታ መከታተያ</h2>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የሱቅ ስም</th>
                            <th>Username</th>
                            <th>Password</th>
                            <th>የኪራይ ሁኔታ</th>
                            <th>ማዕከላዊ እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="tenantTableBody"></tbody>
                </table>
            </div>
        </div>
    </div>

    <div id="appPage" class="hidden">
        <div class="welcome-card">
            <h1 id="shopTitle">"ትርፌ" መቆጣጠሪያ መተግበሪያ</h1>
            <p>ቅልጥፍና እና አስተማማኝነት ለሱቅዎ</p>
        </div>

        <div class="meta-bar">
            <div>📅 ቀን፡ <span id="metaDate">8/6/2026</span></div>
            <div>👤 ሰራተኛ፡ <span id="metaStaff">mulugeta</span></div>
            <div>🕒 ሰዓት፡ <span id="metaTime">12:48 ከሰዓት</span></div>
        </div>

        <div class="dashboard-grid">
            <div class="card"><h3>የሱቅ ካፒታል</h3><p id="dispCapital">0.00</p></div>
            <div class="card"><h3>የዛሬ ትርፍ</h3><p id="dispDailyProfit" style="color: var(--success);">0.00</p></div>
            <div class="card"><h3>የወር ትርፍ</h3><p id="dispMonthlyProfit" style="color: var(--success);">0.00</p></div>
            <div class="card"><h3>ከሳጥን የተነሳ</h3><p id="dispDrawer" style="color: var(--purple);">0.00</p></div>
        </div>

        <div class="btn-grid">
            <button class="btn-primary" onclick="document.getElementById('itemName').focus();">📦 ዕቃ መዝግብ</button>
            <button class="btn-danger" onclick="promptExpense()">📉 የወጪ ማስገቢያ</button>
            <button class="btn-warning" onclick="promptDebt()">💳 የዕዳ መዝገብ</button>
            <button class="btn-purple" onclick="promptDrawer()">💸 ሳጥን ብር</button>
        </div>

        <div class="section-box">
            <h2>የዕቃዎች ዝርዝር እና ክምችት</h2>
            <input type="text" id="itemName" placeholder="የእቃ ስም">
            <div style="display: flex; gap: 5px;">
                <input type="number" id="itemCost" placeholder="መግዣ ዋጋ">
                <input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ">
                <input type="number" id="itemQty" placeholder="ብዛት">
            </div>
            <button class="btn-success btn-block" style="margin-top: 5px;" onclick="addItem()">ዕቃውን በዝርዝር አስገባ</button>
            
            <div class="table-responsive" style="margin-top: 15px;">
                <table>
                    <thead>
                        <tr><th>የዕቃ ስም</th><th>መግዣ</th><th>መሸጫ</th><th>የመጀመሪያ ብዛት</th><th>ሽያጭ</th><th>ቀሪ ክምችት</th><th>ያፈራው ትርፍ</th></tr>
                    </thead>
                    <tbody id="inventoryTable"></tbody>
                </table>
            </div>
        </div>

        <div class="section-box">
            <h2>የዕዳ መዝገብ</h2>
            <div class="table-responsive">
                <table>
                    <thead><tr><th>ደንበኛ</th><th>ዝርዝር</th><th>መጠን (ETB)</th></tr></thead>
                    <tbody id="debtTable"></tbody>
                </table>
            </div>
        </div>

        <div class="section-box">
            <h2>ከሳጥን የተነሳ የገንዘብ መቆጣጠሪያ</h2>
            <div class="table-responsive">
                <table>
                    <thead><tr><th>ምክንያት</th><th>የብር መጠን</th><th>ሁኔታ</th></tr></thead>
                    <tbody id="drawerTable"></tbody>
                </table>
            </div>
        </div>
        
        <button class="btn-danger btn-block" style="margin-top: 15px; padding: 12px;" onclick="logout()">🚪 ከአፑ ውጣ (Logout)</button>
    </div>

</div>

<script>
    const firebaseConfig = {
        databaseURL: "https://tirfe-app-v2-300c2-default-rtdb.firebaseio.com/" 
    };
    
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let localDB = { tenants: {} };
    let currentTenant = null;

    // የሪልታይም ዳታ መከታተያ
    db.ref('tirfe_system').on('value', (snapshot) => {
        if(snapshot.exists()) {
            localDB = snapshot.val();
            if(!localDB.tenants) localDB.tenants = {};
            
            if(currentTenant) {
                let checkTenant = localDB.tenants[currentTenant.username];
                if(!checkTenant) {
                    alert("❌ ይህ አካውንት በባለቤቱ ተሰርዟል!");
                    logout();
                    return;
                }
                if(checkTenant.status === "blocked") {
                    alert("🔒 የኪራይ ጊዜዎ አብቅቷል! እባክዎ ባለቤቱን ያነጋግሩ።");
                    logout();
                    return;
                }
                currentTenant = checkTenant;
                loadTenantApp();
            }
            if(!document.getElementById('adminPage').classList.contains('hidden')) {
                renderAdminPanel();
            }
        } else {
            localDB = { tenants: {} };
        }
    });

    function pushToFirebase() {
        db.ref('tirfe_system').set(localDB);
    }

    // የተስተካከለው የመግቢያ ተግባር
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

        // ዳታው በቅድሚያ መኖሩን ማረጋገጥ
        if (!localDB.tenants) {
            error.innerText = "❌ ምንም የተመዘገበ ተከራይ የለም!";
            return;
        }

        let tenant = localDB.tenants[user];
        if(tenant && String(tenant.password).trim() === pass) {
            if(tenant.status === "blocked") {
                error.innerText = "🔒 ኪራይዎ ተቋርጧል!";
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
        let user = document.getElementById('newUsername').value.trim().toLowerCase(); // ለጥንቃቄ ወደ ስማርት ኬዝ መቀየር
        let pass = document.getElementById('newPassword').value.trim();

        if(!shop || !user || !pass) { alert("እባክዎ ሁሉንም መረጃዎች ይሙሉ!"); return; }
        if(localDB.tenants && localDB.tenants[user]) { alert("ይህ የተጠቃሚ ስም ቀድሞ ተወስዷል!"); return; }

        if(!localDB.tenants) localDB.tenants = {};

        localDB.tenants[user] = {
            shopName: shop, username: user, password: pass, status: "active",
            data: { capital: 0, dailyProfit: 0, monthlyProfit: 0, drawerAmt: 0, inventory: [], expenses: [], debts: [], drawerLog: [] }
        };
        
        pushToFirebase();
        alert("ተከራይ በስኬት ተመዝግቧል!");
        
        document.getElementById('newShopName').value = '';
        document.getElementById('newUsername').value = '';
        document.getElementById('newPassword').value = '';
        renderAdminPanel();
    }

    function toggleStatus(user) {
        localDB.tenants[user].status = localDB.tenants[user].status === "active" ? "blocked" : "active";
        pushToFirebase();
    }

    // አዲስ የተጨመረው የተከራይ መሰረዣ ተግባር (Delete Tenant)
    function deleteTenant(user) {
        if(confirm(`⚠️ እርግጠኛ ነዎት? "${localDB.tenants[user].shopName}" የተባለውን ተከራይና ጠቅላላ ዳታውን ሙሉ በሙሉ ማጥፋት ይፈልጋሉ?`)) {
            delete localDB.tenants[user];
            pushToFirebase();
            alert("ተከራዩ ሙሉ በሙሉ ተሰርዟል!");
            renderAdminPanel();
        }
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody');
        tbody.innerHTML = '';
        if(!localDB.tenants) return;

        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            let badge = t.status === 'active' ? '<span class="status-badge status-active">ስራ ላይ</span>' : '<span class="status-badge status-blocked">የታገደ</span>';
            let btnText = t.status === 'active' ? 'Block' : 'Unblock';
            let btnClass = t.status === 'active' ? 'btn-warning' : 'btn-success';

            tbody.innerHTML += `
                <tr>
                    <td><b>${t.shopName}</b></td>
                    <td>${t.username}</td>
                    <td>${t.password}</td>
                    <td>${badge}</td>
                    <td>
                        <div class="action-btns">
                            <button class="${btnClass}" style="padding:4px 8px; font-size:0.75rem;" onclick="toggleStatus('${t.username}')">${btnText}</button>
                            <button class="btn-danger" style="padding:4px 8px; font-size:0.75rem;" onclick="deleteTenant('${t.username}')">Delete</button>
                        </div>
                    </td>
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

        document.getElementById('dispCapital').innerText = Number(d.capital).toFixed(2) + " ETB";
        document.getElementById('dispDailyProfit').innerText = Number(d.dailyProfit).toFixed(2) + " ETB";
        document.getElementById('dispMonthlyProfit').innerText = Number(d.monthlyProfit).toFixed(2) + " ETB";
        document.getElementById('dispDrawer').innerText = Number(d.drawerAmt).toFixed(2) + " ETB";

        renderInventoryTable();
        renderDrawerTable();
        renderDebtTable();
    }

    function addItem() {
        let name = document.getElementById('itemName').value.trim();
        let cost = parseFloat(document.getElementById('itemCost').value) || 0;
        let price = parseFloat(document.getElementById('itemPrice').value) || 0;
        let qty = parseInt(document.getElementById('itemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0 || qty <= 0) { alert("እባክዎ ሁሉንም ትክክለኛ የዕቃ መረጃዎች ያስገቡ!"); return; }

        if(!currentTenant.data.inventory) currentTenant.data.inventory = [];
        currentTenant.data.inventory.push({ name, cost, price, qty, sold: 0, profit: 0 });
        
        recalculateCapital();
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();

        document.getElementById('itemName').value = '';
        document.getElementById('itemCost').value = '';
        document.getElementById('itemPrice').value = '';
        document.getElementById('itemQty').value = '';
    }

    function renderInventoryTable() {
        let tbody = document.getElementById('inventoryTable');
        tbody.innerHTML = '';
        let inv = currentTenant.data.inventory || [];
        inv.forEach((i, idx) => {
            let remaining = i.qty - i.sold;
            tbody.innerHTML += `
                <tr>
                    <td><b>${i.name}</b></td>
                    <td>${i.cost.toFixed(2)}</td>
                    <td>${i.price.toFixed(2)}</td>
                    <td>${i.qty}</td>
                    <td>
                        <button class="btn-success" style="padding:3px 8px; font-size:0.75rem; margin-right:5px;" onclick="sellItem(${idx})">+</button> 
                        <b>${i.sold}</b>
                    </td>
                    <td>${remaining}</td>
                    <td style="color:var(--success); font-weight:bold;">${i.profit.toFixed(2)}</td>
                </tr>
            `;
        });
    }

    function sellItem(idx) {
        let item = currentTenant.data.inventory[idx];
        if((item.qty - item.sold) <= 0) { alert("❌ ይህ ዕቃ አልቋል!"); return; }
        
        item.sold += 1;
        let netProfit = item.price - item.cost;
        item.profit += netProfit;
        
        currentTenant.data.dailyProfit += netProfit;
        currentTenant.data.monthlyProfit += netProfit;
        
        recalculateCapital();
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
    }

    function recalculateCapital() {
        let inv = currentTenant.data.inventory || [];
        let totalAsset = 0;
        inv.forEach(i => { totalAsset += (i.qty - i.sold) * i.cost; });
        currentTenant.data.capital = totalAsset + currentTenant.data.monthlyProfit;
    }

    function promptExpense() {
        let reason = prompt("የወጪ ምክንያት፦"); if(!reason) return;
        let amt = parseFloat(prompt("መጠን (ETB)፦")) || 0; if(amt <= 0) return;
        
        currentTenant.data.dailyProfit -= amt; 
        currentTenant.data.monthlyProfit -= amt; 
        currentTenant.data.capital -= amt;
        
        if(!currentTenant.data.expenses) currentTenant.data.expenses = [];
        currentTenant.data.expenses.push({ reason, amt }); 
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
    }

    function promptDebt() {
        let name = prompt("የተበዳሪ ስም፦"); if(!name) return;
        let detail = prompt("የዕቃ ዝርዝር፦");
        let amt = parseFloat(prompt("የዕዳ መጠን (ETB)፦")) || 0; if(amt <= 0) return;
        
        if(!currentTenant.data.debts) currentTenant.data.debts = [];
        currentTenant.data.debts.push({ name, detail, amt }); 
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
    }

    function renderDebtTable() {
        let tbody = document.getElementById('debtTable'); tbody.innerHTML = '';
        let debts = currentTenant.data.debts || [];
        debts.forEach(d => { tbody.innerHTML += `<tr><td><b>${d.name}</b></td><td>${d.detail}</td><td>${d.amt.toFixed(2)}</td></tr>`; });
    }

    function promptDrawer() {
        let reason = prompt("ከሳጥን የተወሰደበት ምክንያት፦"); if(!reason) return;
        let amt = parseFloat(prompt("የገንዘብ መጠን (ETB)፦")) || 0; if(amt <= 0) return;
        
        currentTenant.data.drawerAmt += amt;
        if(!currentTenant.data.drawerLog) currentTenant.data.drawerLog = [];
        currentTenant.data.drawerLog.push({ reason, amt, status: 'ወጣ' }); 
        
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
    }

    function renderDrawerTable() {
        let tbody = document.getElementById('drawerTable'); tbody.innerHTML = '';
        let logs = currentTenant.data.drawerLog || [];
        logs.forEach(l => { tbody.innerHTML += `<tr><td>${l.reason}</td><td>${l.amt.toFixed(2)}</td><td><span class="status-badge status-blocked">${l.status}</span></td></tr>`; });
    }

    function logout() {
        currentTenant = null;
        document.getElementById('loginUser').value = '';
        document.getElementById('loginPass').value = '';
        document.getElementById('adminPage').classList.add('hidden');
        document.getElementById('appPage').classList.add('hidden');
        document.getElementById('loginPage').classList.remove('hidden');
    }
</script>
</body>
</html>

