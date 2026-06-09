yehulgize mrchachn tirfe app
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>ትርፌ የሱቅ መቆጣጠሪያ አፕሊኬሽን - Ultimate v6.2</title>
    <!-- Firebase SDKs -->
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-app.js"></script>
    <script src="https://www.gstatic.com/firebasejs/8.10.0/firebase-database.js"></script>
    
    <style>
        :root {
            --bg-color: #0b0f19;
            --card-bg: #151f32;
            --text-color: #f1f5f9;
            --accent-color: #38bdf8;
            --success-color: #4ade80;
            --danger-color: #f87171;
            --warning-color: #fbbf24;
            --purple-color: #c084fc;
            --border-color: rgba(255, 255, 255, 0.08);
            --base-font-size: 14px;
        }

        .theme-deepblue { --bg-color: #0b0f19; --card-bg: #151f32; --accent-color: #38bdf8; --success-color: #4ade80; }

        * { 
            box-sizing: border-box; 
            margin: 0; 
            padding: 0; 
            font-family: system-ui, -apple-system, sans-serif;
            transition: background-color 0.3s ease, font-size 0.2s ease;
        }
        
        html, body {
            max-width: 100%;
            overflow-x: hidden; 
            background-color: var(--bg-color); 
            color: var(--text-color); 
            padding: 12px; 
            font-size: var(--base-font-size); 
        }
        
        .container { 
            width: 100%; 
            max-width: 1200px; 
            margin: 0 auto; 
        }
        
        .hidden { display: none !important; }
        
        .text-success { color: var(--success-color) !important; }
        .text-danger { color: var(--danger-color) !important; }
        .text-warning { color: var(--warning-color) !important; }
        
        .login-box {
            width: 100%;
            max-width: 380px; 
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
            font-size: 0.95rem;
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
        .welcome-header h1 { color: var(--success-color); margin-bottom: 5px; font-size: 22px; font-weight: 800; }
        .welcome-header p { color: var(--accent-color); font-size: 13px; }

        .session-badge {
            display: block; 
            text-align: center; 
            color: var(--warning-color); 
            font-size: 12px; 
            margin-bottom: 15px; 
            background: rgba(251, 191, 36, 0.08);
            padding: 10px; 
            border-radius: 8px; 
            border: 1px dashed var(--warning-color);
        }
        
        .dashboard {
            display: grid; 
            grid-template-columns: repeat(2, 1fr); 
            gap: 10px; 
            margin-bottom: 20px;
        }
        .card {
            background-color: var(--card-bg); 
            padding: 12px; 
            border-radius: 10px; 
            text-align: center; 
            border: 1px solid var(--border-color); 
            border-left: 4px solid var(--accent-color);
        }
        .card h3 { margin: 0 0 6px 0; font-size: 11px; color: #94a3b8; text-transform: uppercase; }
        .card p { margin: 0; font-size: 15px; font-weight: bold; color: var(--success-color); word-break: break-all; }

        .actions { 
            display: grid; 
            grid-template-columns: repeat(2, 1fr); 
            gap: 8px; 
            margin-bottom: 20px; 
        }
        button { 
            padding: 12px 10px; 
            border: none; 
            border-radius: 8px; 
            font-weight: bold; 
            cursor: pointer; 
            font-size: 12px;
            display: inline-flex; 
            align-items: center; 
            justify-content: center; 
            gap: 4px; 
        }
        .btn-block { grid-column: span 2; width: 100%; }
        .btn-add { background-color: var(--accent-color); color: #000; }
        .btn-sell { background-color: var(--success-color); color: #000; }
        .btn-expense { background-color: #f43f5e; color: #fff; }
        .btn-config { background-color: #64748b; color: #fff; }
        .btn-sm { padding: 6px 10px; font-size: 11px; border-radius: 6px; }

        .section-box { 
            background-color: var(--card-bg); 
            padding: 15px; 
            border-radius: 12px; 
            margin-bottom: 20px; 
            border: 1px solid var(--border-color);
        }
        .section-box h2 { font-size: 1.1rem; margin-bottom: 12px; border-bottom: 2px solid var(--border-color); padding-bottom: 6px; color: var(--accent-color); }

        /* ማስተካከያ የተደረገበት የሰንጠረዥ CSS ክፍል (ከ9235.jpg ላይ የተስተካከለ) */
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
            min-width: 600px; 
            background-color: var(--card-bg) !important; 
        }
        th, td { 
            padding: 10px 12px; 
            text-align: left; 
            border-bottom: 1px solid var(--border-color); 
            white-space: nowrap; 
            font-size: 12px; 
            color: var(--text-color) !important; 
        }
        th { 
            background-color: rgba(0,0,0,0.5) !important; 
            color: var(--accent-color) !important; 
            font-weight: 600; 
        }

        .badge-danger { background: var(--danger-color); color: #000; padding: 2px 5px; border-radius: 4px; font-size: 10px; font-weight: bold; }
        .badge-success { background: var(--success-color); color: #000; padding: 2px 5px; border-radius: 4px; font-size: 10px; font-weight: bold; }

        .profile-data { background: rgba(0,0,0,0.2); padding: 10px; border-radius: 8px; margin-bottom: 8px; display: flex; justify-content: space-between; align-items: center; border: 1px solid var(--border-color); font-size: 12px; }
        .profile-data label { font-weight: bold; color: #94a3b8; }
        .profile-data span { color: var(--warning-color); font-weight: bold; }
    </style>
</head>
<body class="theme-deepblue">

<div class="container">

    <!-- 1. የመግቢያ ገጽ -->
    <div id="loginPage" class="login-box">
        <h2>ትርፌ አፕ (Tirfe Tech)</h2>
        <p>የኪራይና የሱቅ መቆጣጠሪያ ማዕከላዊ ሲስተም v6.2</p>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል (Password)">
        <button class="btn-sell btn-block" onclick="handleLogin()">🔒 ደህንነቱ የተጠበቀ መግቢያ</button>
        <p id="loginError" style="color: var(--danger-color); margin-top: 15px; font-size: 0.85rem; font-weight: bold;"></p>
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
            <input type="text" id="newShopName" placeholder="የሱቅ ስም (ምሳሌ፡ አልፋ)">
            <input type="text" id="newUsername" placeholder="መግቢያ ስም (Username)">
            <input type="text" id="newPassword" placeholder="የይለፍ ቃል (Password)">
            
            <label style="font-size: 12px; color: var(--accent-color); display:block; margin-top: 8px;">የውል ዓይነት (Contract Type)፦</label>
            <select id="newContractType">
                <option value="በወር">በወር (Monthly)</option>
                <option value="በዓመት">በዓመት (Yearly)</option>
            </select>

            <label style="font-size: 12px; color: var(--accent-color); display:block; margin-top: 8px;">የውል ማቂያ ቀን (Expiry Date)፦</label>
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
                            <th>Username</th>
                            <th>Password</th>
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
            <p>ቀልጣፋ፣ ዘመናዊ እና አስተማማኝ የሱቅዎ የሂሳብ ረዳት</p>
        </div>

        <div id="sessionDisplay" class="session-badge">የዕለቱ መረጃ በመጠባበቅ ላይ...</div>

        <div class="dashboard">
            <div class="card">
                <h3>የአሁን ካፒታል</h3>
                <p id="totalCapital" style="color: var(--accent-color);">0.00</p>
            </div>
            <div class="card">
                <h3>የዛሬ ትርፍ</h3>
                <p id="todayNetProfit">0.00</p>
            </div>
            <div class="card" style="grid-column: span 2;">
                <h3>ማታ በካሽ ሳጥን መገኘት ያለበት</h3>
                <p id="totalInCash" style="color: #38bdf8; font-size: 20px;">0.00 ETB</p>
            </div>
        </div>

        <div class="actions">
            <button class="btn-add" onclick="focusInventoryInput()">📦 አዲስ ዕቃ መዝግብ</button>
            <button class="btn-config" onclick="logout()">🚪 ከአፑ ውጣ</button>
        </div>

        <!-- የዕቃዎች ዝርዝር ሰንጠረዥ -->
        <div class="section-box">
            <h2>📦 የዕቃዎች ዝርዝር እና ክምችት</h2>
            <div style="background: rgba(255, 255, 255, 0.02); padding: 12px; border-radius: 8px; margin-bottom: 15px; border: 1px solid var(--border-color);">
                <input type="text" id="itemName" placeholder="የዕቃ ስም">
                <input type="number" id="itemCost" placeholder="መግዣ ዋጋ (ካፒታል)">
                <input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ">
                <input type="number" id="itemQty" placeholder="የመጣው ብዛት">
                <button class="btn-sell btn-block" style="margin-top: 5px;" onclick="addItemDirectly()">📥 ዕቃውን በዝርዝር አስገባ</button>
            </div>
            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የዕቃ ስም</th>
                            <th>መግዣ</th>
                            <th>መሸጫ</th>
                            <th>የነበረው</th>
                            <th>የዛሬ ሽያጭ</th>
                            <th>ቀሪ ዕቃ</th>
                            <th>የዛሬ ትርፍ</th>
                            <th>እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="inventoryBody"></tbody>
                </table>
            </div>
        </div>

        <!-- መቼቶች እና ፕሮፋይል -->
        <div class="section-box">
            <h2>⚙️ መቼቶች እና ፕሮፋይል</h2>
            <div class="profile-data"><label>የሱቅ ስም፡</label> <span id="profShopName">-</span></div>
            <div class="profile-data"><label>የውል ማቂያ ቀን፡</label> <span id="profExpiry" style="color: var(--danger-color);">-</span></div>
        </div>
    </div>
</div>

<script>
    // Firebase ማገናኛ (ካለህበት ዳታቤዝ ጋር የተገናኘ)
    const firebaseConfig = { databaseURL: "https://tirfe-app-v2-300c2-default-rtdb.firebaseio.com/" };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let localDB = { tenants: {} };
    let currentTenant = null;

    // ዳታ ከፊልድቤዝ በሪልታይም ማንበቢያ
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

    function pushToFirebase() { db.ref('tirfe_system').set(localDB); }

    // መግቢያ እና አውቶማቲክ ብሎክ ማረጋገጫ
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
            
            // የውል ቀን ማለፉን ማረጋገጫ (Autoblock)
            if(tenant.expiryDate) {
                let today = new Date();
                today.setHours(0,0,0,0);
                let expiry = new Date(tenant.expiryDate);
                expiry.setHours(0,0,0,0);

                if(today > expiry) {
                    tenant.status = "blocked";
                    localDB.tenants[user] = tenant;
                    pushToFirebase();
                    error.innerText = "🔒 የኪራይ ውልዎ ጊዜ አልቋል! እባክዎ ባለቤቱን ያነጋግሩ።";
                    return;
                }
            }

            if(tenant.status === "blocked") { error.innerText = "🔒 አካውንትዎ ታግዷል!"; return; }
            
            currentTenant = tenant;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appPage').classList.remove('hidden');
            document.getElementById('shopTitle').innerText = tenant.shopName + " - መቆጣጠሪያ";
            document.getElementById('profShopName').innerText = tenant.shopName;
            document.getElementById('profExpiry').innerText = tenant.expiryDate ? `${tenant.expiryDate} (${tenant.contractType})` : "ያልተገደበ";
            checkMorningSession();
        } else { error.innerText = "❌ የተጠቃሚ ስም ወይም የይለፍ ቃል ስህተት ነው!"; }
    }

    function checkMorningSession() {
        let d = currentTenant.data || {};
        if (!d.sessionActive) {
            let today = new Date();
            d.sessionData = { date: today.toLocaleDateString('am-ET'), loginTime: today.toLocaleTimeString('am-ET', {hour:'2-digit', minute:'2-digit'}), initialFloat: 0 };
            d.sessionActive = true; d.inventory = d.inventory || [];
            currentTenant.data = d; saveAndRefresh();
        }
        renderApp();
    }

    function registerTenant() {
        let shop = document.getElementById('newShopName').value.trim();
        let user = document.getElementById('newUsername').value.trim().toLowerCase(); 
        let pass = document.getElementById('newPassword').value.trim();
        let contractType = document.getElementById('newContractType').value;
        let expiryDate = document.getElementById('newExpiryDate').value;

        if(!shop || !user || !pass || !expiryDate) { alert("እባክዎ ሁሉንም መረጃዎች ይሙሉ!"); return; }
        
        localDB.tenants[user] = { 
            shopName: shop, 
            username: user, 
            password: pass, 
            contractType: contractType,
            expiryDate: expiryDate,
            status: "active", 
            data: { sessionActive: false, inventory: [] } 
        };
        pushToFirebase(); renderAdminPanel();
        
        document.getElementById('newShopName').value = ''; document.getElementById('newUsername').value = ''; 
        document.getElementById('newPassword').value = ''; document.getElementById('newExpiryDate').value = '';
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody'); tbody.innerHTML = '';
        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            let statusBadge = t.status === "active" ? `<span class="badge-success">Active</span>` : `<span class="badge-danger">Blocked</span>`;
            tbody.innerHTML += `<tr>
                <td><b>${t.shopName}</b></td>
                <td>${t.username}</td>
                <td>${t.password}</td>
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

    function deleteTenant(user) { if(confirm("ይህንን ተከራይ ለማጥፋት እርግጠኛ ኖት?")) { delete localDB.tenants[user]; pushToFirebase(); renderAdminPanel(); } }
    function logout() { currentTenant = null; document.getElementById('adminPage').classList.add('hidden'); document.getElementById('appPage').classList.add('hidden'); document.getElementById('loginPage').classList.remove('hidden'); }
    function saveAndRefresh() { localDB.tenants[currentTenant.username] = currentTenant; pushToFirebase(); renderApp(); }
    function focusInventoryInput() { document.getElementById('itemName').focus(); }

    function addItemDirectly() {
        let name = document.getElementById('itemName').value.trim();
        let cost = parseFloat(document.getElementById('itemCost').value) || 0;
        let price = parseFloat(document.getElementById('itemPrice').value) || 0;
        let qty = parseInt(document.getElementById('itemQty').value) || 0;
        if(!name || cost <= 0 || price <= 0 || qty <= 0) return;
        let inv = currentTenant.data.inventory || [];
        inv.push({ name, cost, price, qty, sold: 0, profit: 0 });
        currentTenant.data.inventory = inv; saveAndRefresh();
        document.getElementById('itemName').value = ''; document.getElementById('itemCost').value = ''; document.getElementById('itemPrice').value = ''; document.getElementById('itemQty').value = '';
    }

    function sellItem(idx) {
        let item = currentTenant.data.inventory[idx];
        let remaining = item.qty - item.sold;
        let count = parseInt(prompt(`ስንት ፍሬ ተሸጠ? (ቀሪ፡ ${remaining})፦`)) || 1;
        if (count > remaining) return;
        item.sold += count; saveAndRefresh();
    }

    function renderApp() {
        let d = currentTenant.data || {}; let session = d.sessionData || {};
        if(d.sessionActive) { document.getElementById('sessionDisplay').innerText = `📅 ዕለት፦ ${session.date} | ሁኔታ፦ ክፍት`; }
        let tbody = document.getElementById('inventoryBody'); tbody.innerHTML = '';
        let tSales = 0, todayProfit = 0, currentTotalCapital = 0;
        let inv = d.inventory || [];
        inv.forEach((item, idx) => {
            let remaining = Math.max(0, item.qty - item.sold); let profit = (item.price - item.cost) * item.sold;
            tSales += (item.price * item.sold); todayProfit += profit; currentTotalCapital += (item.cost * remaining);
            tbody.innerHTML += `<tr><td><strong>${item.name}</strong></td><td>${item.cost}</td><td>${item.price}</td><td>${item.qty}</td><td><button class="btn-sell btn-sm" onclick="sellItem(${idx})">➕</button> <b>${item.sold}</b></td><td style="color:#f87171;font-weight:bold;">${remaining}</td><td>${profit}</td><td><button class="btn-expense btn-sm" onclick="deleteInventoryItem(${idx})">🗑️</button></td></tr>`;
        });
        document.getElementById('totalCapital').innerText = currentTotalCapital.toFixed(1) + " ETB";
        document.getElementById('todayNetProfit').innerText = todayProfit.toFixed(1) + " ETB";
        document.getElementById('totalInCash').innerText = tSales.toFixed(1) + " ETB";
    }
    function deleteInventoryItem(idx) { currentTenant.data.inventory.splice(idx, 1); saveAndRefresh(); }
</script>
</body>
</html>
