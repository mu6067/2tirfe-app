2tirfe app hulgize mrchachn new
<!DOCTYPE html>
<html lang="am">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ትርፌ የሱቅ መቆጣጠሪያ አፕሊኬሽን - Ultimate v6 (Firebase Integrated)</title>
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

        /* ገጽታዎች (Themes) */
        .theme-deepblue {
            --bg-color: #0b0f19;
            --card-bg: #151f32;
            --accent-color: #38bdf8;
            --success-color: #4ade80;
        }
        .theme-emerald {
            --bg-color: #061e1a;
            --card-bg: #0b3c34;
            --accent-color: #2ec4b6;
            --success-color: #4ade80;
        }
        .theme-purple {
            --bg-color: #1a0933;
            --card-bg: #2d1254;
            --accent-color: #a29bfe;
            --success-color: #00cec9;
        }
        .theme-charcoal {
            --bg-color: #121212;
            --card-bg: #1e1e1e;
            --accent-color: #9b59b6;
            --success-color: #2ecc71;
        }

        * { 
            box-sizing: border-box; 
            margin: 0; 
            padding: 0; 
            font-family: system-ui, -apple-system, sans-serif;
            transition: background-color 0.3s ease, font-size 0.2s ease;
        }
        
        body { 
            background-color: var(--bg-color); 
            color: var(--text-color); 
            padding: 20px; 
            font-size: var(--base-font-size); 
        }
        
        .container { max-width: 1200px; margin: 0 auto; }
        .hidden { display: none !important; }
        
        /* ረዳት ክፍሎች */
        .text-success { color: var(--success-color) !important; }
        .text-danger { color: var(--danger-color) !important; }
        .text-warning { color: var(--warning-color) !important; }
        .text-purple { color: var(--purple-color) !important; }
        
        /* የመግቢያ ክፍል */
        .login-box {
            max-width: 400px; margin: 80px auto;
            background-color: var(--card-bg); padding: 35px;
            border-radius: 16px; box-shadow: 0 15px 35px rgba(0,0,0,0.6);
            text-align: center; border: 1px solid var(--border-color);
        }
        .login-box h2 { color: var(--success-color); font-size: 1.8rem; margin-bottom: 8px; font-weight: 800; }
        .login-box p { color: #94a3b8; font-size: 0.9rem; margin-bottom: 25px; }

        input, select {
            width: 100%; padding: 12px 15px; margin: 8px 0;
            border-radius: 8px; border: 1px solid #3a506b;
            background-color: rgba(0,0,0,0.2); color: white; font-size: 0.95rem;
            outline: none;
        }
        input:focus { border-color: var(--accent-color); }

        /* ሄደሮች */
        .welcome-header {
            text-align: center; margin-bottom: 25px; padding: 25px;
            background: linear-gradient(135deg, var(--card-bg), #0f172a);
            border-radius: 12px; border: 1px solid var(--border-color);
            box-shadow: 0 4px 15px rgba(0,0,0,0.3);
        }
        .welcome-header h1 { color: var(--success-color); margin-bottom: 5px; font-size: 28px; font-weight: 800; }
        .welcome-header p { color: var(--accent-color); font-size: 14px; }

        .session-badge {
            display: block; text-align: center; color: var(--warning-color); 
            font-size: 13px; margin-bottom: 20px; background: rgba(251, 191, 36, 0.1);
            padding: 12px; border-radius: 8px; border: 1px dashed var(--warning-color);
        }
        
        /* ዳሽቦርድ */
        .dashboard {
            display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 15px; margin-bottom: 25px;
        }
        .card {
            background-color: var(--card-bg); padding: 18px; border-radius: 12px; text-align: center; 
            border: 1px solid var(--border-color); border-left: 5px solid var(--accent-color);
            box-shadow: 0 4px 10px rgba(0,0,0,0.2);
        }
        .card h3 { margin: 0 0 10px 0; font-size: 12px; color: #94a3b8; text-transform: uppercase; letter-spacing: 0.5px; }
        .card p { margin: 0; font-size: 20px; font-weight: bold; color: var(--success-color); }

        /* አዝራሮች */
        .actions { display: flex; gap: 10px; margin-bottom: 25px; flex-wrap: wrap; }
        button { padding: 12px 18px; border: none; border-radius: 8px; font-weight: bold; cursor: pointer; transition: all 0.2s; display: inline-flex; align-items: center; justify-content: center; gap: 6px; }
        button:active { transform: scale(0.96); }
        .btn-block { width: 100%; }
        .btn-add { background-color: var(--accent-color); color: #000; }
        .btn-sell { background-color: var(--success-color); color: #000; }
        .btn-expense { background-color: #f43f5e; color: #fff; }
        .btn-credit { background-color: #f59e0b; color: #000; }
        .btn-draw { background-color: var(--purple-color); color: #000; }
        .btn-config { background-color: #64748b; color: #fff; }
        .btn-next-day { background-color: #a855f7; color: #fff; }
        .btn-clear { background-color: var(--danger-color); color: #000; margin-left: auto; }
        .btn-sm { padding: 5px 10px; font-size: 0.8rem; border-radius: 4px; }
        button:hover { opacity: 0.9; }

        .section-box { 
            background-color: var(--card-bg); padding: 20px; border-radius: 14px; margin-bottom: 25px; 
            box-shadow: 0 4px 10px rgba(0,0,0,0.2); border: 1px solid var(--border-color);
        }
        .section-box h2 { font-size: 1.2rem; margin-bottom: 15px; border-bottom: 2px solid var(--border-color); padding-bottom: 8px; color: var(--accent-color); }

        /* የፍለጋ ሳጥን */
        .search-container { position: relative; margin-bottom: 15px; }
        .search-container input { padding-left: 35px; margin: 0; }
        .search-icon { position: absolute; left: 12px; top: 13px; color: #94a3b8; }

        /* ሰንጠረዦች */
        .table-responsive { width: 100%; overflow-x: auto; border-radius: 8px; border: 1px solid var(--border-color); }
        table { width: 100%; border-collapse: collapse; min-width: 700px; background-color: rgba(0,0,0,0.1); }
        th, td { padding: 12px 15px; text-align: left; border-bottom: 1px solid var(--border-color); white-space: nowrap; }
        th { background-color: rgba(0,0,0,0.3); color: var(--accent-color); font-size: 14px; font-weight: 600; }
        tr:hover { background-color: rgba(255,255,255,0.02); }

        .low-stock { background-color: rgba(248, 113, 113, 0.12); }
        .badge-danger { background: var(--danger-color); color: #000; padding: 3px 6px; border-radius: 4px; font-size: 11px; font-weight: bold; cursor: pointer; }

        .grid-3 { display: grid; grid-template-columns: repeat(auto-fit, minmax(320px, 1fr)); gap: 20px; margin-bottom: 25px; }

        .report-section { background-color: #090d16; border: 2px dashed var(--success-color); padding: 20px; border-radius: 10px; margin-top: 20px; display: none; }
        
        .profile-data { background: rgba(0,0,0,0.2); padding: 12px; border-radius: 8px; margin-bottom: 10px; display: flex; justify-content: space-between; align-items: center; border: 1px solid var(--border-color); }
        .profile-data label { font-weight: bold; color: #94a3b8; }
        .profile-data span { color: var(--warning-color); font-weight: bold; }

        /* ሞዳል */
        .modal-overlay { position: fixed; top: 0; left: 0; width: 100%; height: 100%; background: rgba(0, 0, 0, 0.85); backdrop-filter: blur(5px); display: flex; justify-content: center; align-items: center; z-index: 1000; }
        .modal-card { background-color: var(--card-bg); border-radius: 16px; width: 90%; max-width: 480px; padding: 25px; box-shadow: 0 20px 40px rgba(0,0,0,0.6); border: 1px solid var(--border-color); }
        .modal-header { margin-bottom: 15px; border-bottom: 1px solid var(--border-color); padding-bottom: 10px; }
        .modal-header h3 { font-size: 1.2rem; color: var(--success-color); }
        .modal-body { margin-bottom: 20px; font-size: 0.95rem; }
        .modal-footer { display: flex; justify-content: flex-end; gap: 10px; }
    </style>
</head>
<body class="theme-deepblue">

<div class="container">

    <div id="loginPage" class="login-box">
        <h2>ትርፌ አፕ (Tirfe Tech)</h2>
        <p>የኪራይና የሱቅ መቆጣጠሪያ ማዕከላዊ ሲስተም v6</p>
        <input type="text" id="loginUser" placeholder="የተጠቃሚ ስም (Username)">
        <input type="password" id="loginPass" placeholder="የይለፍ ቃል (Password)">
        <button class="btn-sell btn-block" onclick="handleLogin()">🔒 ደህንነቱ የተጠበቀ መግቢያ</button>
        <p id="loginError" style="color: var(--danger-color); margin-top: 15px; font-size: 0.85rem; font-weight: bold;"></p>
    </div>

    <div id="adminPage" class="hidden">
        <div class="welcome-header">
            <h1>የባለቤቱ ማዕከላዊ ቁጥጥር ፓነል</h1>
            <p>እዚህ ገጽ ላይ አዳዲስ ተከራዮችን መመዝገብ፣ ጊዜያዊ ማገድ፣ እና ሙሉ በሙሉ መሰረዝ ይችላሉ።</p>
            <button class="btn-expense" style="margin-top: 15px;" onclick="logout()">🚪 ከሲስተሙ ውጣ</button>
        </div>

        <div class="section-box">
            <h2>👤 አዲስ ተከራይ መመዝገቢያ</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(200px, 1fr)); gap: 10px;">
                <input type="text" id="newShopName" placeholder="የሱቅ ስም (ምሳሌ፡ አልፋ)">
                <input type="text" id="newUsername" placeholder="መግቢያ ስም (Username)">
                <input type="text" id="newPassword" placeholder="የይለፍ ቃል (Password)">
            </div>
            <button class="btn-add btn-block" style="margin-top: 10px;" onclick="registerTenant()">➕ ተከራይ በቋሚነት መዝግብ</button>
        </div>

        <div class="section-box">
            <h2>📈 የተከራዮች እና የኪራይ ሁኔታ መከታተያ</h2>
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
        <div class="welcome-header">
            <h1 id="shopTitle">እንኳን ደህና መጡ ወደ " ትርፌ " መቆጣጠሪያ መተግበሪያ</h1>
            <p>ቀልጣፋ፣ ዘመናዊ እና አስተማማኝ የሱቅዎ የሂሳብ ረዳት</p>
        </div>

        <div id="sessionDisplay" class="session-badge">የዕለቱ መረጃ በመጠባበቅ ላይ...</div>

        <div class="dashboard">
            <div class="card">
                <h3>የአሁን የሱቅ ካፒታል</h3>
                <p id="totalCapital" style="color: var(--accent-color);">0.00 ETB</p>
            </div>
            <div class="card">
                <h3>የዛሬ የተጣራ ትርፍ</h3>
                <p id="todayNetProfit">0.00 ETB</p>
            </div>
            <div class="card">
                <h3>የዚህ ወር አጠቃላይ ትርፍ</h3>
                <p id="monthlyNetProfit" style="color: var(--warning-color);">0.00 ETB</p>
            </div>
            <div class="card">
                <h3>ከሳጥን የተነሳ (ያልተመለሰ)</h3>
                <p id="totalDraws" style="color: var(--purple-color);">0.00 ETB</p>
            </div>
            <div class="card">
                <h3>ማታ በካሽ ሳጥን መገኘት ያለበት</h3>
                <p id="totalInCash" style="color: #38bdf8;">0.00 ETB</p>
            </div>
        </div>

        <div class="actions">
            <button class="btn-add" onclick="focusInventoryInput()">📦 አዲስ ዕቃ / ጭማሪ መዝግብ</button>
            <button class="btn-expense" onclick="openExpenseModal()">📉 ወጪ መዝግብ</button>
            <button class="btn-credit" onclick="openDebtModal()">💳 ዕዳ መዝግብ</button>
            <button class="btn-draw" onclick="openDrawerModal()">💸 ከሳጥን ብር አንሳ</button>
            <button class="btn-config" onclick="configureBank()">🏦 የባንክ አፕ ማያያዣ</button>
            <button class="btn-next-day" onclick="startNewDaySession()">🔄 አዲስ ቀን ጀምር</button>
            <button class="btn-clear" onclick="clearAllTenantData()">🗑️ ዳታ አጽዳ</button>
        </div>

        <div class="section-box">
            <h2>📦 የዕቃዎች ዝርዝር እና የክምችት ሁኔታ</h2>
            
            <div style="background: rgba(255, 255, 255, 0.02); padding: 15px; border-radius: 8px; margin-bottom: 15px; border: 1px solid var(--border-color);">
                <h3 style="font-size: 0.9rem; margin-bottom: 10px; color: var(--success-color);">በቀጥታ አዲስ ዕቃ ማስገቢያ</h3>
                <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(130px, 1fr)); gap: 8px;">
                    <input type="text" id="itemName" placeholder="የዕቃ ስም">
                    <input type="number" id="itemCost" placeholder="መግዣ ዋጋ (ካፒታል)">
                    <input type="number" id="itemPrice" placeholder="መሸጫ ዋጋ">
                    <input type="number" id="itemQty" placeholder="የመጣው ብዛት">
                </div>
                <button class="btn-sell btn-block" style="margin-top: 10px;" onclick="addItemDirectly()">📥 ዕቃውን በዝርዝር አስገባ</button>
            </div>

            <div class="search-container">
                <span class="search-icon">🔍</span>
                <input type="text" id="inventorySearch" placeholder="ዕቃዎችን በስም እዚህ ይፈልጉ..." oninput="renderInventoryTable()">
            </div>

            <div class="table-responsive">
                <table>
                    <thead>
                        <tr>
                            <th>የዕቃ ስም</th>
                            <th>መግዣ ዋጋ (ካፒታል)</th>
                            <th>መሸጫ ዋጋ</th>
                            <th>የነበረው ብዛት</th>
                            <th>ዛሬ የተሸጠ</th>
                            <th>ቀሪ ዕቃ</th>
                            <th>የዛሬ ትርፍ</th>
                            <th>እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="inventoryBody"></tbody>
                </table>
            </div>
        </div>

        <div class="grid-3">
            <div class="section-box">
                <h2>💳 የዕዳ መዝገብ (የተሰበሰበ ብቻ ይደመራል)</h2>
                <div class="table-responsive">
                    <table>
                        <thead>
                            <tr>
                                <th>ደንበኛ እና ስልክ</th>
                                <th>የዕዳ ዝርዝር</th>
                                <th>ገንዘብ መጠን</th>
                                <th>ድርጊት</th>
                            </tr>
                        </thead>
                        <tbody id="creditBody"></tbody>
                    </table>
                </div>
            </div>

            <div class="section-box">
                <h2>💸 ከሳጥን የተነሳ ገንዘብ መቆጣጠሪያ</h2>
                <div class="table-responsive">
                    <table>
                        <thead>
                            <tr>
                                <th>ምክንያት</th>
                                <th>የብር መጠን</th>
                                <th>ሁኔታ</th>
                            </tr>
                        </thead>
                        <tbody id="drawBody"></tbody>
                    </table>
                </div>
            </div>

            <div class="section-box">
                <h2>🔄 የቀደሙ ቀናት ታሪክ መዝገብ</h2>
                <div class="table-responsive">
                    <table>
                        <thead>
                            <tr>
                                <th>ቀን እና ሰራተኛ</th>
                                <th>የስራ ሰዓት</th>
                                <th>ሽያጭ</th>
                                <th>የቀን ትርፍ</th>
                            </tr>
                        </thead>
                        <tbody id="historyBody"></tbody>
                    </table>
                </div>
            </div>
        </div>

        <div id="reportBox" class="report-section">
            <h2 style="color: var(--success-color); margin: 0 0 10px 0;">🌙 የእለቱ ማታ 4:00 አውቶማቲክ ሪፖርት</h2>
            <textarea id="reportText" style="width: 100%; height: 240px; background: #000; color: #4ade80; border: 1px solid #334155; padding: 10px; font-family: monospace; border-radius: 5px; resize: none;" readonly></textarea>
            <button style="background: var(--success-color); color: #000; margin-top: 10px;" onclick="copyReport()">📋 ሪፖርቱን ኮፒ አድርግ</button>
        </div>

        <div class="section-box">
            <h2>⚙️ መቼቶች እና የተጠቃሚ ፕሮፋይል (Settings & Profile)</h2>
            <div style="display: grid; grid-template-columns: repeat(auto-fit, minmax(280px, 1fr)); gap: 15px; margin-top: 10px;">
                <div>
                    <h3 style="font-size: 0.95rem; color: var(--success-color); margin-bottom: 10px;">👤 የእኔ ፕሮፋይል</h3>
                    <div class="profile-data"><label>የሱቅ ስም፡</label> <span id="profShopName">-</span></div>
                    <div class="profile-data"><label>Username፡</label> <span id="profUsername">-</span></div>
                    <div class="profile-data"><label>Password፡</label> <span id="profPassword">-</span></div>
                    <button class="btn-add btn-sm btn-block" onclick="openEditProfileModal()">✏️ መግቢያ መረጃ ቀይር</button>
                </div>
                <div>
                    <h3 style="font-size: 0.95rem; color: var(--accent-color); margin-bottom: 10px;">🔍 የማሳያ መጠን (Zoom Settings)</h3>
                    <div style="display: flex; gap: 5px; flex-wrap: wrap;">
                        <button class="btn-add btn-sm" onclick="adjustFontSize(1)">➕ አሳድግ</button>
                        <button class="btn-credit btn-sm" onclick="adjustFontSize(-1)">➖ ቀንስ</button>
                        <button class="btn-expense btn-sm" onclick="resetFontSize()">🔄 ወደ መደበኛ</button>
                    </div>

                    <h3 style="font-size: 0.95rem; color: var(--accent-color); margin-top: 20px; margin-bottom: 10px;">🎨 የአፕሊኬሽኑ ገጽታ (Themes)</h3>
                    <div style="display: grid; grid-template-columns: repeat(2, 1fr); gap: 6px;">
                        <button class="btn-add btn-sm" onclick="changeTheme('deepblue')">Deep Blue</button>
                        <button class="btn-sell btn-sm" onclick="changeTheme('emerald')">Emerald Green</button>
                        <button class="btn-draw btn-sm" onclick="changeTheme('purple')">Royal Purple</button>
                        <button class="btn-config btn-sm" onclick="changeTheme('charcoal')">Charcoal Dark</button>
                    </div>
                </div>
            </div>
        </div>

        <button class="btn-expense btn-block" style="margin-top: 25px; padding: 15px; font-size: 1rem;" onclick="logout()">🚪 ከአፑ በሰላም ውጣ (Logout)</button>
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

    <div id="expenseModal" class="modal-card hidden">
        <div class="modal-header"><h3>📉 አዲስ የወጪ መመዝገቢያ</h3></div>
        <div class="modal-body">
            <input type="text" id="expenseReason" placeholder="የወጪው ምክንያት (ምሳሌ፡ ኤሌክትሪክ)">
            <input type="number" id="expenseAmount" placeholder="የገንዘብ መጠን በብር">
        </div>
        <div class="modal-footer">
            <button class="btn-sell" onclick="saveExpense()">መዝግብ</button>
            <button class="btn-expense" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <div id="debtModal" class="modal-card hidden">
        <div class="modal-header"><h3>💳 አዲስ የተበዳሪ መመዝገቢያ</h3></div>
        <div class="modal-body">
            <input type="text" id="debtCustomer" placeholder="የባለዕዳው ሙሉ ስም እና ስልክ">
            <input type="text" id="debtItemName" placeholder="የወሰደው ዕቃ ስም">
            <input type="number" id="debtAmount" placeholder="የዕዳው ጠቅላላ መጠን (ወይም ባዶ ይተዉት)">
        </div>
        <div class="modal-footer">
            <button class="btn-credit" onclick="saveDebt()">ዕዳውን መዝግብ</button>
            <button class="btn-expense" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <div id="drawerModal" class="modal-card hidden">
        <div class="modal-header"><h3>💸 ከሳጥን ገንዘብ መውሰጃ</h3></div>
        <div class="modal-body">
            <input type="text" id="drawerReason" placeholder="የወሰደው ሰው ስም ወይም ምክንያት">
            <input type="number" id="drawerAmount" placeholder="የተነሳው የገንዘብ መጠን">
        </div>
        <div class="modal-footer">
            <button class="btn-draw" onclick="saveDrawer()">ወጪ መዝግብ</button>
            <button class="btn-expense" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <div id="editProfileModal" class="modal-card hidden">
        <div class="modal-header"><h3>✏️ የመግቢያ መረጃ መቀየሪያ</h3></div>
        <div class="modal-body">
            <input type="text" id="editShopName" placeholder="አዲስ የሱቅ ስም">
            <input type="text" id="editPassword" placeholder="አዲስ የይለፍ ቃል">
        </div>
        <div class="modal-footer">
            <button class="btn-sell" onclick="saveProfileChanges()">አሻሽል</button>
            <button class="btn-expense" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>

    <div id="editInventoryModal" class="modal-card hidden">
        <div class="modal-header"><h3>✏️ የዕቃ መረጃ ማስተካከያ</h3></div>
        <div class="modal-body">
            <input type="hidden" id="editItemIndex">
            <input type="text" id="editItemName" placeholder="የዕቃ ስም">
            <input type="number" id="editItemCost" placeholder="መግዣ ዋጋ">
            <input type="number" id="editItemPrice" placeholder="መሸጫ ዋጋ">
            <input type="number" id="editItemQty" placeholder="ጠቅላላ ብዛት">
        </div>
        <div class="modal-footer">
            <button class="btn-sell" onclick="saveInventoryChanges()">አሻሽል</button>
            <button class="btn-expense" onclick="closeActiveModal()">ተመለስ</button>
        </div>
    </div>
</div>

<script>
    // 1. Firebase ማዋቀሪያ (Realtime Database ሊንክህን በትክክል አስቀምጧል)
    const firebaseConfig = {
        databaseURL: "https://tirfe-app-v2-300c2-default-rtdb.firebaseio.com/" 
    };
    firebase.initializeApp(firebaseConfig);
    const db = firebase.database();

    let localDB = { tenants: {} };
    let currentTenant = null;
    let currentFontSize = 14;

    // 2. የሪልታይም ዳታቤዝ ክትትል (Realtime Database listener)
    db.ref('tirfe_system').on('value', (snapshot) => {
        if(snapshot.exists()) {
            localDB = snapshot.val();
            if(!localDB.tenants) localDB.tenants = {};
            
            if(currentTenant) {
                let checkTenant = localDB.tenants[currentTenant.username];
                if(!checkTenant) {
                    showCustomAlert("❌ አካውንት ጠፍቷል", "ይህ አካውንት በባለቤቱ ተሰርዟል!", () => { logout(); });
                    return;
                }
                if(checkTenant.status === "blocked") {
                    showCustomAlert("🔒 የኪራይ ማብቂያ", "የኪራይ ጊዜዎ አብቅቷል! እባክዎ ባለቤቱን ያነጋግሩ።", () => { logout(); });
                    return;
                }
                currentTenant = checkTenant;
                renderApp();
            }
            if(!document.getElementById('adminPage').classList.contains('hidden')) {
                renderAdminPanel();
            }
        }
    });

    function pushToFirebase() {
        db.ref('tirfe_system').set(localDB);
    }

    // የሞዳል መቆጣጠሪያዎች
    function openModalContainer() { document.getElementById('modalOverlay').classList.remove('hidden'); }
    function closeActiveModal() {
        document.getElementById('modalOverlay').classList.add('hidden');
        const modals = document.querySelectorAll('.modal-card');
        modals.forEach(m => m.classList.add('hidden'));
    }

    function showCustomAlert(title, message, callback) {
        document.getElementById('alertTitle').innerText = title;
        document.getElementById('alertMessage').innerText = message;
        openModalContainer();
        document.getElementById('alertModal').classList.remove('hidden');
        document.querySelector('#alertModal .btn-add').onclick = function() {
            closeActiveModal();
            if(callback) callback();
        };
    }

    function showCustomConfirm(title, message, onConfirm) {
        document.getElementById('confirmTitle').innerText = title;
        document.getElementById('confirmMessage').innerText = message;
        openModalContainer();
        document.getElementById('confirmModal').classList.remove('hidden');
        document.getElementById('confirmYesBtn').onclick = function() {
            closeActiveModal();
            if(onConfirm) onConfirm();
        };
    }

    // የመግቢያ ሲስተም (Login)
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
            if(tenant.status === "blocked") {
                error.innerText = "🔒 ኪራይዎ በባለቤቱ ታግዷል!";
                return;
            }
            currentTenant = tenant;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appPage').classList.remove('hidden');
            document.getElementById('shopTitle').innerText = tenant.shopName + " - መቆጣጠሪያ መተግበሪያ";
            
            updateProfileSectionUI();
            checkMorningSession();
        } else {
            error.innerText = "❌ የተጠቃሚ ስም ወይም የይለፍ ቃል ስህተት ነው!";
        }
    }

    function updateProfileSectionUI() {
        if(currentTenant) {
            document.getElementById('profShopName').innerText = currentTenant.shopName;
            document.getElementById('profUsername').innerText = currentTenant.username;
            document.getElementById('profPassword').innerText = currentTenant.password;
        }
    }

    // የዕለቱ ሰራተኛ መመዝገቢያ (Morning Session Logic)
    function checkMorningSession() {
        let d = currentTenant.data || {};
        if (!d.sessionActive) {
            let today = new Date();
            let dateStr = today.toLocaleDateString('am-ET');
            let timeStr = today.toLocaleTimeString('am-ET', { hour: '2-digit', minute: '2-digit' });

            let employee = prompt("የዛሬውን ሰራተኛ ስም ያስገቡ፦"); if(!employee) employee = "ያልተመዘገበ ሰራተኛ";
            let floatMoney = parseFloat(prompt("ለመልስ የሚሆን በሳጥን የተተወ መነሻ ገንዘብ (Float)፦")) || 0;
            
            d.sessionData = { date: dateStr, loginTime: timeStr, employee: employee, initialFloat: floatMoney };
            d.sessionActive = true;
            d.expenses = []; 
            d.collectedCreditToday = 0;
            
            currentTenant.data = d;
            localDB.tenants[currentTenant.username] = currentTenant;
            pushToFirebase();
        }
        renderApp();
    }

    function focusInventoryInput() { document.getElementById('itemName').focus(); }

    // ዕቃን በቀጥታ ከፎርም መመዝገቢያ
    function addItemDirectly() {
        let name = document.getElementById('itemName').value.trim();
        let cost = parseFloat(document.getElementById('itemCost').value) || 0;
        let price = parseFloat(document.getElementById('itemPrice').value) || 0;
        let qty = parseInt(document.getElementById('itemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0 || qty <= 0) { 
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም የዕቃ መረጃዎች በትክክል ያስገቡ!"); 
            return; 
        }

        let inv = currentTenant.data.inventory || [];
        let existing = inv.find(i => i.name.toLowerCase() === name.toLowerCase());
        
        if (existing) {
            existing.qty = (existing.qty - existing.sold) + qty;
            existing.cost = cost; 
            existing.price = price;
            existing.sold = 0;
        } else {
            inv.push({ name, cost, price, qty, sold: 0, profit: 0 });
        }
        
        currentTenant.data.inventory = inv;
        saveAndRefresh();

        document.getElementById('itemName').value = '';
        document.getElementById('itemCost').value = '';
        document.getElementById('itemPrice').value = '';
        document.getElementById('itemQty').value = '';
        showCustomAlert("📦 ስኬት", "ዕቃው በተሳካ ሁኔታ ተመዝግቧል!");
    }

    // የዕቃ መሸጫ ሎጂክ
    function sellItem(idx) {
        let item = currentTenant.data.inventory[idx];
        let remaining = item.qty - item.sold;
        
        let count = parseInt(prompt(`ስንት ፍሬ ተሸጠ? (ቀሪ ክምችት፡ ${remaining})፦`)) || 1;
        if (count > remaining) { showCustomAlert("⚠️ ስህተት", "በሱቁ ውስጥ ካለው ቀሪ ዕቃ በላይ መሸጥ አይቻልም!"); return; }

        item.sold += count;
        saveAndRefresh();
    }

    function restockAndPay(itemName) {
        let bankApp = currentTenant.data.preferredBankApp || "telebirr://";
        let payChoice = prompt(`የ ${itemName} ማዘዣ ዘዴ ይምረጡ:\n1 = በጥሬ ብር (Cash)\n2 = በባንክ አፕሊኬሽን (Bank App)`);
        if(payChoice === "2") {
            alert("ክፍያውን ለመፈጸም አውቶማቲክ ወደ ተመረጠው የባንክ መተግበሪያ ያሻግርዎታል።");
            window.location.href = bankApp;
        }
        focusInventoryInput();
    }

    // ወጪ መዝግብ
    function openExpenseModal() {
        document.getElementById('expenseReason').value = '';
        document.getElementById('expenseAmount').value = '';
        openModalContainer();
        document.getElementById('expenseModal').classList.remove('hidden');
    }

    function saveExpense() {
        let reason = document.getElementById('expenseReason').value.trim();
        let amt = parseFloat(document.getElementById('expenseAmount').value) || 0;
        if(!reason || amt <= 0) return;

        let exp = currentTenant.data.expenses || [];
        exp.push({ reason, amount: amt });
        currentTenant.data.expenses = exp;

        saveAndRefresh();
        closeActiveModal();
    }

    // ዕዳ መዝግብ
    function openDebtModal() {
        document.getElementById('debtCustomer').value = '';
        document.getElementById('debtItemName').value = '';
        document.getElementById('debtAmount').value = '';
        openModalContainer();
        document.getElementById('debtModal').classList.remove('hidden');
    }

    function saveDebt() {
        let customer = document.getElementById('debtCustomer').value.trim();
        let itemName = document.getElementById('debtItemName').value.trim();
        let amtInput = parseFloat(document.getElementById('debtAmount').value) || 0;

        if(!customer || !itemName) return;

        let inv = currentTenant.data.inventory || [];
        let item = inv.find(i => i.name.toLowerCase() === itemName.toLowerCase());
        let calculatedPrice = 0;
        let costValue = 0;

        if (item) {
            let count = parseInt(prompt(`የወሰደው ዕቃ ብዛት? (የአንዱ ዋጋ: ${item.price} ETB)፦`)) || 1;
            let remaining = item.qty - item.sold;
            if (count > remaining) { showCustomAlert("⚠️ ስህተት", "በክምችት ውስጥ በቂ ዕቃ የለም!"); return; }
            
            calculatedPrice = item.price * count;
            costValue = item.cost * count;
            item.qty -= count; 
        } else {
            calculatedPrice = amtInput > 0 ? amtInput : parseFloat(prompt("ዕቃው ስላልተገኘ ጠቅላላ ዋጋውን እራስዎ ያስገቡ፦")) || 0;
            costValue = calculatedPrice * 0.8; 
        }

        let debts = currentTenant.data.debts || [];
        debts.push({ customer, description: itemName, amount: calculatedPrice, cost: costValue });
        currentTenant.data.debts = debts;

        saveAndRefresh();
        closeActiveModal();
    }

    // ዕዳ መክፈያ/ማውረጃ
    function payCredit(idx) {
        showCustomConfirm("💳 የዕዳ አከፋፈል", "ይህ ዕዳ ሙሉ በሙሉ ተከፍሎ ተወራርዷል? (አሁን ወደ የዛሬ ገቢ ይደመራል)", () => {
            let debts = currentTenant.data.debts || [];
            let paidAmount = debts[idx].amount;
            let paidCost = debts[idx].cost;
            
            currentTenant.data.collectedCreditToday = (parseFloat(currentTenant.data.collectedCreditToday) || 0) + paidAmount;

            let inv = currentTenant.data.inventory || [];
            inv.push({ name: `የተሰበሰበ እዳ (${debts[idx].description})`, cost: paidCost, price: paidAmount, qty: 1, sold: 1, isCreditPay: true });
            currentTenant.data.inventory = inv;

            debts.splice(idx, 1);
            currentTenant.data.debts = debts;
            saveAndRefresh();
        });
    }

    // ከሳጥን ብር መውሰጃ
    function openDrawerModal() {
        document.getElementById('drawerReason').value = '';
        document.getElementById('drawerAmount').value = '';
        openModalContainer();
        document.getElementById('drawerModal').classList.remove('hidden');
    }

    function saveDrawer() {
        let reason = document.getElementById('drawerReason').value.trim();
        let amt = parseFloat(document.getElementById('drawerAmount').value) || 0;
        if(!reason || amt <= 0) return;

        let draws = currentTenant.data.drawerLog || [];
        draws.push({ reason, amount: amt, status: "ያልተመለሰ" });
        currentTenant.data.drawerLog = draws;

        saveAndRefresh();
        closeActiveModal();
    }

    function returnDraw(idx) {
        showCustomConfirm("💸 የገንዘብ መመለሻ", "ይህ የተነሳው ገንዘብ ሙሉ በሙሉ ወደ ሳጥን ተመልሶ ገብቷል?", () => {
            currentTenant.data.drawerLog[idx].status = "ገብቷል";
            saveAndRefresh();
        });
    }

    // የባንክ ማያያዣ መቼት
    function configureBank() {
        let choice = prompt("ለክፍያ የሚጠቀሙበትን ዋና የባንክ አፕ ይምረጡ:\n1 = Telebirr\n2 = CBE Birr\n3 = Awash Sol");
        let appLink = "telebirr://";
        if(choice === "2") appLink = "cbebirr://";
        else if(choice === "3") appLink = "awashsol://";
        currentTenant.data.preferredBankApp = appLink;
        saveAndRefresh();
        alert("የባንክ መተግበሪያው በተሳካ ሁኔታ ተገናኝቷል!");
    }

    // አዲስ ቀን መጀመሪያ እና ታሪክ መዝጊያ
    function startNewDaySession() {
        let inv = currentTenant.data.inventory || [];
        if (inv.length === 0) return;
        
        showCustomConfirm("🔄 አዲስ ቀን መጀመር", "የዛሬውን ሂሳብ ዘግተው ወደ ቀጣይ ቀን ታሪክ ማስተላለፍ ይፈልጋሉ?", () => {
            let today = new Date();
            let logoutTime = today.toLocaleTimeString('am-ET', { hour: '2-digit', minute: '2-digit' });
            let session = currentTenant.data.sessionData || {};

            let collectedCredit = parseFloat(currentTenant.data.collectedCreditToday) || 0;
            let tSales = collectedCredit, todayProfit = 0, tExp = 0;
            
            inv.forEach(item => {
                tSales += (item.price * item.sold);
                todayProfit += ((item.price - item.cost) * item.sold);
            });
            
            let exp = currentTenant.data.expenses || [];
            exp.forEach(e => tExp += e.amount);

            let history = currentTenant.data.history || [];
            history.push({
                date: session.date,
                employee: session.employee,
                hours: `ከ ${session.loginTime} እስከ ${logoutTime}`,
                sales: tSales,
                profit: todayProfit - tExp
            });
            currentTenant.data.history = history;

            // የጽዳት ስራ
            currentTenant.data.inventory = inv.filter(item => !item.isCreditPay);
            currentTenant.data.inventory.forEach(item => { item.qty = item.qty - item.sold; item.sold = 0; });

            currentTenant.data.sessionActive = false;
            currentTenant.data.expenses = [];
            currentTenant.data.collectedCreditToday = 0;
            currentTenant.data.drawerLog = [];

            localDB.tenants[currentTenant.username] = currentTenant;
            pushToFirebase();
            location.reload();
        });
    }

    // አውቶማቲክ ሪፖርት ማመንጫ
    function checkAutoReportTime() {
        let now = new Date();
        if (now.getHours() === 22 && now.getMinutes() === 0) { generateReport(); }
    }

    function generateReport() {
        let d = currentTenant.data;
        let session = d.sessionData || {};
        let collectedCredit = parseFloat(d.collectedCreditToday) || 0;
        let tSales = collectedCredit, todayProfit = 0, details = "", tExp = 0, expDetails = "", tDraw = 0;
        
        let inv = d.inventory || [];
        inv.forEach(item => {
            if(item.sold > 0) {
                tSales += (item.price * item.sold);
                todayProfit += ((item.price - item.cost) * item.sold);
                if(!item.isCreditPay) details += `- ${item.name}: ${item.sold} ፍሬ ተሸጠ\n`;
            }
        });
        
        let exp = d.expenses || [];
        exp.forEach(e => { tExp += e.amount; expDetails += `- ${e.reason}: ${e.amount} ETB\n`; });
        
        let draws = d.drawerLog || [];
        draws.forEach(dr => { if(dr.status === "ያልተመለሰ") tDraw += dr.amount; });
        
        let initialFloat = parseFloat(session.initialFloat) || 0;
        let finalInCash = initialFloat + tSales - tExp - tDraw;

        let reportStr = `=== ትርፌ እለታዊ የሂሳብ ሪፖርት ===\nቀን: ${session.date}\n------------------------------\nመነሻ ካሽ: ${initialFloat.toFixed(2)} ETB\nጠቅላላ የዛሬ ገቢ (ሽያጭ + የተሰበሰበ እዳ): ${tSales.toFixed(2)} ETB\n  * ከዕዳ የተሰበሰበ ገቢ: ${collectedCredit.toFixed(2)} ETB\n\nየዛሬ ወጪዎች:\n${expDetails || "- ወጪ የለም\n"}ጠቅላላ ወጪ: ${tExp.toFixed(2)} ETB\nከሳጥን የተነሳ ብር: ${tDraw.toFixed(2)} ETB\n------------------------------\n💰 ማታ በሳጥን መገኘት ያለበት ካሽ:\n👉 [ ${finalInCash.toFixed(2)} ETB ] 👈\n==============================`;
        
        document.getElementById('reportText').value = reportStr; 
        document.getElementById('reportBox').style.display = "block";
    }

    function copyReport() {
        let copyText = document.getElementById("reportText"); copyText.select(); navigator.clipboard.writeText(copyText.value);
        alert("ሪፖርቱ በተሳካ ሁኔታ ተገልብጧል!");
    }

    // አፕሊኬሽኑን በስክሪን ላይ የመሳል ዋና ፈንክሽን (Render Engine)
    function renderApp() {
        let d = currentTenant.data || {};
        let session = d.sessionData || {};
        
        if(d.sessionActive) {
            document.getElementById('sessionDisplay').innerText = `📅 ቀን፦ ${session.date} | 👤 ሰራተኛ፦ ${session.employee} | 🕒 የገባበት ሰዓት፦ ${session.loginTime} | 💰 መነሻ ብር፦ ${session.initialFloat} ETB`;
        }

        let tbody = document.getElementById('inventoryBody'); tbody.innerHTML = '';
        let collectedCredit = parseFloat(d.collectedCreditToday) || 0;
        let tSales = collectedCredit, todayProfit = 0, tExp = 0, tDraw = 0, currentTotalCapital = 0;
        let searchTerm = document.getElementById('inventorySearch').value.toLowerCase().trim();

        let inv = d.inventory || [];
        inv.forEach((item, idx) => {
            if(searchTerm && !item.name.toLowerCase().includes(searchTerm)) return;

            let remaining = item.qty - item.sold;
            if(remaining < 0) remaining = 0;
            let profit = (item.price - item.cost) * item.sold;
            
            tSales += (item.price * item.sold); 
            todayProfit += profit;
            
            if(!item.isCreditPay) {
                currentTotalCapital += (item.cost * remaining);
            }

            let isLowStock = remaining <= 5 ? 'class="low-stock"' : '';
            let alertBadge = remaining <= 5 ? `<span class="badge-danger" onclick="restockAndPay('${item.name}')">⚠️ አልቋል!</span>` : '';

            if(!item.isCreditPay) {
                tbody.innerHTML += `<tr ${isLowStock}>
                    <td><strong>${item.name}</strong> ${alertBadge}</td>
                    <td>${item.cost.toFixed(2)} ETB</td>
                    <td>${item.price.toFixed(2)} ETB</td>
                    <td>${item.qty}</td>
                    <td><button class="btn-sell btn-sm" onclick="sellItem(${idx})">➕ ሽጥ</button> <b>${item.sold}</b></td>
                    <td style="color:#f87171; font-weight:bold;">${remaining}</td>
                    <td style="font-weight:bold;color:#4ade80;">${profit.toFixed(2)} ETB</td>
                    <td>
                        <button class="btn-add btn-sm" onclick="openEditInventoryModal(${idx})">✏️ አሻሽል</button>
                        <button class="btn-expense btn-sm" onclick="deleteInventoryItem(${idx})">🗑️ ሰርዝ</button>
                    </td>
                </tr>`;
            }
        });

        let exp = d.expenses || []; exp.forEach(e => tExp += e.amount);
        let draws = d.drawerLog || []; draws.forEach(dr => { if(dr.status === "ያልተመለሰ") tDraw += dr.amount; });
        
        let initialFloat = parseFloat(session.initialFloat) || 0;
        let history = d.history || [];
        let totalMonthlyProfit = todayProfit - tExp;
        history.forEach(h => totalMonthlyProfit += h.profit);

        // የዳሽቦርድ መረጃዎችን ማዘመን
        document.getElementById('totalCapital').innerText = currentTotalCapital.toFixed(2) + " ETB";
        document.getElementById('todayNetProfit').innerText = (todayProfit - tExp).toFixed(2) + " ETB";
        document.getElementById('monthlyNetProfit').innerText = totalMonthlyProfit.toFixed(2) + " ETB";
        document.getElementById('totalDraws').innerText = tDraw.toFixed(2) + " ETB";
        document.getElementById('totalInCash').innerText = (initialFloat + tSales - tExp - tDraw).toFixed(2) + " ETB";

        // የዕዳ ሰንጠረዥ
        let cbody = document.getElementById('creditBody'); cbody.innerHTML = '';
        let debts = d.debts || [];
        debts.forEach((c, index) => {
            cbody.innerHTML += `<tr><td>${c.customer}</td><td>${c.description}</td><td style="color:var(--warning-color);font-weight:bold;">${c.amount.toFixed(2)} ETB</td><td><button class="btn-sell btn-sm" onclick="payCredit(${index})">ተከፈለ</button></td></tr>`;
        });

        // የሳጥን ሰንጠረዥ
        let dbody = document.getElementById('drawBody'); dbody.innerHTML = '';
        draws.forEach((dr, index) => {
            let statusBtn = dr.status === "ያልተመለሰ" ? `<button class="btn-add btn-sm" onclick="returnDraw(${index})">መልስ</button>` : `<span style="color:var(--success-color);">✅ ገብቷል</span>`;
            dbody.innerHTML += `<tr><td>${dr.reason}</td><td style="color:var(--purple-color);font-weight:bold;">${dr.amount.toFixed(2)} ETB</td><td>${statusBtn}</td></tr>`;
        });

        // የታሪክ ሰንጠረዥ
        let hbody = document.getElementById('historyBody'); hbody.innerHTML = '';
        history.forEach(h => {
            hbody.innerHTML += `<tr><td><strong>${h.date}</strong><br><small>${h.employee}</small></td><td>${h.hours}</td><td>${h.sales.toFixed(2)}</td><td style="color:#4ade80;font-weight:bold;">${h.profit.toFixed(2)}</td></tr>`;
        });

        checkAutoReportTime();
    }

    function saveAndRefresh() {
        localDB.tenants[currentTenant.username] = currentTenant;
        pushToFirebase();
        renderApp();
    }

    // የዕቃ ማሻሻያ/መሰረዝ ስራዎች
    function deleteInventoryItem(idx) {
        showCustomConfirm("🗑️ ዕቃ መሰረዝ", "ይህንን ዕቃ ከዝርዝር ውስጥ ማጥፋት ይፈልጋሉ?", () => {
            currentTenant.data.inventory.splice(idx, 1);
            saveAndRefresh();
        });
    }

    function openEditInventoryModal(idx) {
        let item = currentTenant.data.inventory[idx];
        document.getElementById('editItemIndex').value = idx;
        document.getElementById('editItemName').value = item.name;
        document.getElementById('editItemCost').value = item.cost;
        document.getElementById('editItemPrice').value = item.price;
        document.getElementById('editItemQty').value = item.qty;
        openModalContainer();
        document.getElementById('editInventoryModal').classList.remove('hidden');
    }

    function saveInventoryChanges() {
        let idx = parseInt(document.getElementById('editItemIndex').value);
        let name = document.getElementById('editItemName').value.trim();
        let cost = parseFloat(document.getElementById('editItemCost').value) || 0;
        let price = parseFloat(document.getElementById('editItemPrice').value) || 0;
        let qty = parseInt(document.getElementById('editItemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0) return;

        let item = currentTenant.data.inventory[idx];
        item.name = name;
        item.cost = cost;
        item.price = price;
        item.qty = qty;

        closeActiveModal();
        saveAndRefresh();
    }

    // ፕሮፋይል እና መቼቶች ማስተካከያ
    function openEditProfileModal() {
        document.getElementById('editShopName').value = currentTenant.shopName;
        document.getElementById('editPassword').value = currentTenant.password;
        openModalContainer();
        document.getElementById('editProfileModal').classList.remove('hidden');
    }

    function saveProfileChanges() {
        let shop = document.getElementById('editShopName').value.trim();
        let pass = document.getElementById('editPassword').value.trim();
        if(!shop || !pass) return;

        currentTenant.shopName = shop;
        currentTenant.password = pass;
        
        document.getElementById('shopTitle').innerText = shop + " - መቆጣጠሪያ መተግበሪያ";
        updateProfileSectionUI();
        closeActiveModal();
        saveAndRefresh();
    }

    function adjustFontSize(val) {
        currentFontSize += val;
        document.documentElement.style.setProperty('--base-font-size', currentFontSize + 'px');
    }
    function resetFontSize() {
        currentFontSize = 14;
        document.documentElement.style.setProperty('--base-font-size', '14px');
    }
    function changeTheme(themeName) {
        document.body.className = '';
        document.body.classList.add('theme-' + themeName);
    }

    function clearAllTenantData() {
        showCustomConfirm("⚠️ ዳታ ማጽጃ", "ሁሉንም የሱቅዎን ዳታ ማጥፋት ይፈልጋሉ?", () => {
            currentTenant.data = { inventory: [], expenses: [], debts: [], drawerLog: [], history: [], sessionActive: false };
            saveAndRefresh();
            location.reload();
        });
    }

    // የባለቤት (Admin) ፓነል አስተዳደር
    function registerTenant() {
        let shop = document.getElementById('newShopName').value.trim();
        let user = document.getElementById('newUsername').value.trim().toLowerCase(); 
        let pass = document.getElementById('newPassword').value.trim();

        if(!shop || !user || !pass) return;
        if(localDB.tenants[user]) { alert("ይህ የተጠቃሚ ስም ቀድሞ ተወስዷል!"); return; }

        localDB.tenants[user] = {
            shopName: shop, username: user, password: pass, status: "active",
            data: { sessionActive: false, inventory: [], expenses: [], debts: [], drawerLog: [], history: [] }
        };
        pushToFirebase();
        
        document.getElementById('newShopName').value = '';
        document.getElementById('newUsername').value = '';
        document.getElementById('newPassword').value = '';
        renderAdminPanel();
    }

    function toggleStatus(user) {
        localDB.tenants[user].status = localDB.tenants[user].status === "active" ? "blocked" : "active";
        pushToFirebase();
    }

    function deleteTenant(user) {
        if(confirm(`"${localDB.tenants[user].shopName}" ሙሉ በሙሉ ይጥፋ?`)) {
            delete localDB.tenants[user];
            pushToFirebase();
            renderAdminPanel();
        }
    }

    function renderAdminPanel() {
        let tbody = document.getElementById('tenantTableBody'); tbody.innerHTML = '';
        Object.keys(localDB.tenants).forEach(key => {
            let t = localDB.tenants[key];
            let badge = t.status === 'active' ? '<span style="color:#4ade80;">active</span>' : '<span style="color:#f87171;">blocked</span>';
            tbody.innerHTML += `<tr>
                <td><b>${t.shopName}</b></td>
                <td>${t.username}</td>
                <td>${t.password}</td>
                <td>${badge}</td>
                <td>
                    <button class="btn-add btn-sm" onclick="toggleStatus('${t.username}')">Block/Unblock</button>
                    <button class="btn-expense btn-sm" onclick="deleteTenant('${t.username}')">Delete</button>
                </td>
            </tr>`;
        });
    }

    function logout() {
        currentTenant = null;
        document.getElementById('adminPage').classList.add('hidden');
        document.getElementById('appPage').classList.add('hidden');
        document.getElementById('loginPage').classList.remove('hidden');
    }

    setInterval(renderApp, 60000); // በየደቂቃው ሪፖርት ታይም መፈተሻ
</script>
</body>
</html>
