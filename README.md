```html
<!DOCTYPE html>
<html lang="am" class="h-full">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>ትርፌ የሱቅ መቆጣጠሪያ አፕሊኬሽን - Ultimate v6</title>
    
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Google Fonts - Noto Sans Ethiopic for premium typography -->
    <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+Ethiopic:wght@300;400;600;800&family=Plus+Jakarta+Sans:wght@300;400;600;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Plus Jakarta Sans', 'Noto Sans Ethiopic', 'sans-serif'],
                    }
                }
            }
        }
    </script>

    <style>
        body {
            font-family: 'Plus Jakarta Sans', 'Noto Sans Ethiopic', 'Segoe UI', sans-serif;
            transition: background-color 0.4s ease, color 0.4s ease;
        }
        
        /* Premium custom scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: rgba(0, 0, 0, 0.1);
        }
        ::-webkit-scrollbar-thumb {
            background: rgba(255, 255, 255, 0.15);
            border-radius: 999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: rgba(255, 255, 255, 0.3);
        }

        /* Dynamic Theme Variables mapped to Tailwind classes */
        .theme-deepblue {
            --bg-gradient: linear-gradient(135deg, #0b0f19, #151f32, #0b0f19);
            --card-bg: rgba(21, 31, 50, 0.7);
            --border-glow: rgba(56, 189, 248, 0.2);
            --accent: #38bdf8;
            --accent-hover: #0284c7;
        }
        .theme-emerald {
            --bg-gradient: linear-gradient(135deg, #062f27, #0b4f43, #021f1a);
            --card-bg: rgba(11, 60, 52, 0.7);
            --border-glow: rgba(16, 185, 129, 0.2);
            --accent: #10b981;
            --accent-hover: #059669;
        }
        .theme-purple {
            --bg-gradient: linear-gradient(135deg, #1e0b36, #3b136b, #120524);
            --card-bg: rgba(45, 18, 84, 0.7);
            --border-glow: rgba(139, 92, 246, 0.2);
            --accent: #8b5cf6;
            --accent-hover: #7c3aed;
        }
        .theme-charcoal {
            --bg-gradient: linear-gradient(135deg, #111111, #222222, #0d0d0d);
            --card-bg: rgba(34, 34, 34, 0.8);
            --border-glow: rgba(156, 163, 175, 0.15);
            --accent: #6b7280;
            --accent-hover: #4b5563;
        }

        .custom-glass {
            background: var(--card-bg);
            backdrop-filter: blur(16px);
            border: 1px solid rgba(255, 255, 255, 0.08);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.37);
        }

        .custom-glass:hover {
            border-color: var(--border-glow);
            box-shadow: 0 8px 32px 0 rgba(0, 0, 0, 0.5), 0 0 12px var(--border-glow);
        }
    </style>
</head>
<body class="theme-deepblue min-h-full text-slate-100 flex flex-col" style="background: var(--bg-gradient);">

<!-- የላይኛው መረጃ እና ራስጌ -->
<header class="border-b border-white/10 px-4 py-3 custom-glass sticky top-0 z-40">
    <div class="max-w-7xl mx-auto flex justify-between items-center">
        <div class="flex items-center gap-3">
            <div class="w-10 h-10 rounded-xl bg-gradient-to-tr from-sky-400 to-indigo-600 flex items-center justify-center font-extrabold text-white text-lg shadow-lg shadow-sky-500/20">
                ተ
            </div>
            <div>
                <h1 class="font-extrabold text-lg tracking-tight bg-gradient-to-r from-sky-400 to-emerald-400 bg-clip-text text-transparent">ትርፌ የሱቅ መቆጣጠሪያ <span class="text-xs text-white/50 font-normal">v6.0</span></h1>
                <p class="text-[10px] text-slate-400">የኪራይና የሱቅ ሂሳብ ማዕከላዊ አፕሊኬሽን</p>
            </div>
        </div>
        
        <div class="flex items-center gap-3">
            <div id="connectionStatus" class="flex items-center gap-1.5 px-2.5 py-1 rounded-full bg-emerald-500/10 border border-emerald-500/20 text-emerald-400 text-xs font-semibold">
                <span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
                መስመር ላይ
            </div>
            <button id="logoutBtn" onclick="logout()" class="hidden bg-rose-500/10 hover:bg-rose-500 text-rose-400 hover:text-white px-3.5 py-1.5 rounded-lg text-xs font-bold transition-all duration-200 border border-rose-500/20 flex items-center gap-1">
                <svg class="w-3.5 h-3.5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 16l4-4m0 0l-4-4m4 4H7m6 4v1a3 3 0 01-3 3H6a3 3 0 01-3-3V7a3 3 0 013-3h4a3 3 0 013 3v1" /></svg>
                ውጣ
            </button>
        </div>
    </div>
</header>

<main class="flex-1 max-w-7xl w-full mx-auto p-4 md:p-6 transition-all duration-300" id="mainContainer">

    <!-- 1. መግቢያ ገጽ (LOGIN PAGE) -->
    <div id="loginPage" class="max-w-md mx-auto my-12 md:my-20 p-8 rounded-2xl custom-glass border border-white/10 transition-all duration-300">
        <div class="text-center mb-8">
            <h2 class="text-3xl font-extrabold text-white mb-2">እንኳን ደህና መጡ</h2>
            <p class="text-sm text-slate-400">የመግቢያ መለያዎን በማስገባት ስራዎን ይጀምሩ</p>
        </div>
        
        <div class="space-y-4">
            <div>
                <label class="block text-xs font-bold text-slate-400 mb-1.5 uppercase tracking-wider">የተጠቃሚ ስም (Username)</label>
                <div class="relative">
                    <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-500">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M16 7a4 4 0 11-8 0 4 4 0 018 0zM12 14a7 7 0 00-7 7h14a7 7 0 00-7-7z" /></svg>
                    </span>
                    <input type="text" id="loginUser" class="w-full bg-black/30 border border-slate-700/80 rounded-xl py-3 pl-11 pr-4 text-white placeholder-slate-500 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 transition-all text-sm" placeholder="ለምሳሌ፡ admin">
                </div>
            </div>

            <div>
                <label class="block text-xs font-bold text-slate-400 mb-1.5 uppercase tracking-wider">የይለፍ ቃል (Password)</label>
                <div class="relative">
                    <span class="absolute inset-y-0 left-0 pl-3.5 flex items-center text-slate-500">
                        <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" /></svg>
                    </span>
                    <input type="password" id="loginPass" class="w-full bg-black/30 border border-slate-700/80 rounded-xl py-3 pl-11 pr-4 text-white placeholder-slate-500 focus:outline-none focus:border-sky-500 focus:ring-1 focus:ring-sky-500 transition-all text-sm" placeholder="••••••••">
                </div>
            </div>

            <button onclick="handleLogin()" class="w-full bg-gradient-to-r from-sky-500 to-indigo-600 hover:from-sky-600 hover:to-indigo-700 text-white font-bold py-3 px-4 rounded-xl shadow-lg shadow-sky-500/20 active:scale-95 transition-all flex items-center justify-center gap-2 mt-2">
                <svg class="w-5 h-5" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 15v2m-6 4h12a2 2 0 002-2v-6a2 2 0 00-2-2H6a2 2 0 00-2 2v6a2 2 0 002 2zm10-10V7a4 4 0 00-8 0v4h8z" /></svg>
                ደህንነቱ የተጠበቀ መግቢያ
            </button>
            
            <p id="loginError" class="text-xs text-rose-400 font-semibold text-center mt-2 min-h-[1.25rem]"></p>
        </div>
    </div>

    <!-- 2. የባለቤቱ ማዕከላዊ መቆጣጠሪያ ገጽ (ADMIN PANEL) -->
    <div id="adminPage" class="hidden space-y-6 transition-all duration-300">
        <!-- Dashboard Admin Banner -->
        <div class="rounded-2xl p-6 md:p-8 custom-glass relative overflow-hidden flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
            <div class="relative z-10">
                <span class="px-3 py-1 bg-amber-500/10 text-amber-400 border border-amber-500/20 rounded-full text-xs font-bold uppercase tracking-wider mb-2 inline-block">ማዕከላዊ ባለቤት</span>
                <h2 class="text-2xl md:text-3xl font-extrabold text-white">የባለቤቱ ማዕከላዊ መቆጣጠሪያ ገጽ</h2>
                <p class="text-sm text-slate-400 mt-1">አዳዲስ ተከራዮችን መመዝገብ፣ ጊዜያዊ ማገድ፣ ማረም እና መሰረዝ ይችላሉ።</p>
            </div>
            <div class="flex gap-2">
                <button onclick="openRegisterTenantModal()" class="bg-sky-500 hover:bg-sky-600 text-white font-bold px-4 py-2.5 rounded-xl shadow-lg shadow-sky-500/10 flex items-center gap-1.5 transition-all text-sm">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" /></svg>
                    አዲስ ተከራይ መዝግብ
                </button>
            </div>
        </div>

        <!-- የተከራዮች ዝርዝር ሰንጠረዥ -->
        <div class="rounded-2xl custom-glass overflow-hidden">
            <div class="px-6 py-4 border-b border-white/10 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                <h3 class="font-bold text-lg text-white">የተመዘገቡ ተከራዮች እና የኪራይ ሁኔታ</h3>
                <div class="relative w-full sm:w-64">
                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-500"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg></span>
                    <input type="text" id="adminSearchTenant" oninput="renderAdminTenantList()" class="w-full bg-black/20 border border-slate-700 rounded-lg py-1.5 pl-9 pr-3 text-white placeholder-slate-500 text-xs focus:outline-none focus:border-sky-500" placeholder="በሱቅ ወይም ተጠቃሚ ስም ፈልግ...">
                </div>
            </div>
            
            <div class="overflow-x-auto w-full">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-black/20 text-slate-400 text-xs uppercase tracking-wider">
                            <th class="p-4 font-semibold">የሱቅ ስም</th>
                            <th class="p-4 font-semibold">የመግቢያ ስም (User)</th>
                            <th class="p-4 font-semibold">የይለፍ ቃል</th>
                            <th class="p-4 font-semibold">የኪራይ ሁኔታ</th>
                            <th class="p-4 font-semibold text-right">የማስተካከያ እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="tenantTableBody" class="divide-y divide-white/5 text-sm">
                        <!-- Dynamic content -->
                    </tbody>
                </table>
            </div>
        </div>
    </div>

    <!-- 3. የተከራይ የሱቅ መቆጣጠሪያ ገጽ (TENANT APP PANEL) -->
    <div id="appPage" class="hidden space-y-6 transition-all duration-300">
        <!-- Welcome banner -->
        <div class="rounded-2xl p-6 md:p-8 custom-glass flex flex-col md:flex-row justify-between items-start md:items-center gap-4">
            <div>
                <h2 id="shopTitle" class="text-2xl md:text-3xl font-extrabold text-white">የሱቅዎ ስም መቆጣጠሪያ</h2>
                <div class="flex flex-wrap items-center gap-3 mt-2 text-xs text-slate-400">
                    <span class="flex items-center gap-1">📅 ቀን፦ <span id="metaDate" class="text-slate-200">...</span></span>
                    <span class="w-1.5 h-1.5 rounded-full bg-slate-700"></span>
                    <span class="flex items-center gap-1">🕒 ሰዓት፦ <span id="metaTime" class="text-slate-200">00:00:00 AM</span></span>
                    <span class="w-1.5 h-1.5 rounded-full bg-slate-700"></span>
                    <span class="flex items-center gap-1">👤 ተጠቃሚ፦ <span id="metaStaff" class="text-emerald-400 font-semibold">...</span></span>
                </div>
            </div>
            
            <div class="flex flex-wrap gap-2">
                <button onclick="openAddInventoryModal()" class="bg-gradient-to-r from-emerald-500 to-teal-600 hover:from-emerald-600 hover:to-teal-700 text-white font-bold px-4 py-2.5 rounded-xl shadow-lg shadow-emerald-500/10 flex items-center gap-1.5 transition-all text-xs">
                    <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 6v6m0 0v6m0-6h6m-6 0H6" /></svg>
                    📦 አዲስ ዕቃ ጨምር
                </button>
            </div>
        </div>

        <!-- ዕለታዊ የመግቢያ ባጅ (Morning Session Display) -->
        <div id="sessionDisplay" class="bg-amber-500/10 text-amber-400 border border-dashed border-amber-500/30 p-3.5 rounded-xl text-center text-xs md:text-sm font-semibold flex flex-col md:flex-row gap-2 justify-center items-center">
            የእለቱ መረጃ በመጠባበቅ ላይ...
        </div>

        <!-- 5 ዋና ካርዶች (Cards Section) -->
        <div class="grid grid-cols-2 lg:grid-cols-5 gap-4">
            <div class="rounded-2xl p-4 custom-glass border-l-4 border-sky-500">
                <p class="text-[10px] md:text-xs text-slate-400 font-semibold uppercase tracking-wider mb-1">የአሁን የሱቅ ካፒታል</p>
                <h4 id="dispCapital" class="text-base md:text-lg lg:text-xl font-extrabold text-white">0.00 ETB</h4>
            </div>
            <div class="rounded-2xl p-4 custom-glass border-l-4 border-emerald-500">
                <p class="text-[10px] md:text-xs text-slate-400 font-semibold uppercase tracking-wider mb-1">የዛሬ የተጣራ ትርፍ</p>
                <h4 id="dispDailyProfit" class="text-base md:text-lg lg:text-xl font-extrabold text-emerald-400">0.00 ETB</h4>
            </div>
            <div class="rounded-2xl p-4 custom-glass border-l-4 border-violet-500">
                <p class="text-[10px] md:text-xs text-slate-400 font-semibold uppercase tracking-wider mb-1">የወር የተጣራ ትርፍ</p>
                <h4 id="dispMonthlyProfit" class="text-base md:text-lg lg:text-xl font-extrabold text-violet-400">0.00 ETB</h4>
            </div>
            <div class="rounded-2xl p-4 custom-glass border-l-4 border-amber-500">
                <p class="text-[10px] md:text-xs text-slate-400 font-semibold uppercase tracking-wider mb-1">ከሳጥን የወጣ ጠቅላላ</p>
                <h4 id="dispDrawer" class="text-base md:text-lg lg:text-xl font-extrabold text-amber-400">0.00 ETB</h4>
            </div>
            <div class="rounded-2xl p-4 custom-glass border-l-4 border-rose-500 col-span-2 lg:col-span-1">
                <p class="text-[10px] md:text-xs text-slate-400 font-semibold uppercase tracking-wider mb-1">ማታ በሳጥን መገኘት ያለበት</p>
                <h4 id="dispTotalInCash" class="text-base md:text-lg lg:text-xl font-extrabold text-rose-400">0.00 ETB</h4>
            </div>
        </div>

        <!-- Quick actions layout -->
        <div class="grid grid-cols-2 md:grid-cols-5 gap-3">
            <button onclick="openExpenseModal()" class="p-3 bg-rose-500/10 hover:bg-rose-500/20 border border-rose-500/20 text-rose-400 hover:text-rose-300 font-bold rounded-xl flex items-center justify-center gap-1.5 text-xs transition-all">
                📉 ወጪ መዝግብ
            </button>
            <button onclick="openDebtModal()" class="p-3 bg-blue-500/10 hover:bg-blue-500/20 border border-blue-500/20 text-blue-400 hover:text-blue-300 font-bold rounded-xl flex items-center justify-center gap-1.5 text-xs transition-all">
                💳 ዕዳ መዝግብ
            </button>
            <button onclick="openDrawerModal()" class="p-3 bg-purple-500/10 hover:bg-purple-500/20 border border-purple-500/20 text-purple-400 hover:text-purple-300 font-bold rounded-xl flex items-center justify-center gap-1.5 text-xs transition-all">
                💸 ብር ውሰድ
            </button>
            <button onclick="openBankConfigModal()" class="p-3 bg-cyan-500/10 hover:bg-cyan-500/20 border border-cyan-500/20 text-cyan-400 hover:text-cyan-300 font-bold rounded-xl flex items-center justify-center gap-1.5 text-xs transition-all">
                🏦 ባንክ አገናኝ
            </button>
            <button onclick="startNewDaySession()" class="col-span-2 md:col-span-1 p-3 bg-emerald-500/15 hover:bg-emerald-500/25 border border-emerald-500/20 text-emerald-400 hover:text-emerald-300 font-bold rounded-xl flex items-center justify-center gap-1.5 text-xs transition-all">
                🔄 አዲስ ቀን ጀምር
            </button>
        </div>

        <!-- Inventory List Box -->
        <div class="rounded-2xl custom-glass overflow-hidden">
            <div class="px-6 py-4 border-b border-white/10 flex flex-col sm:flex-row sm:items-center justify-between gap-4">
                <h3 class="font-bold text-lg text-white flex items-center gap-2">
                    <svg class="w-5 h-5 text-emerald-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M20 7l-8-4-8 4m16 0l-8 4m8-4v10l-8 4m0-10L4 7m8 4v10M4 7v10l8 4" /></svg>
                    የዕቃዎች ዝርዝር እና የክምችት ሁኔታ
                </h3>
                <div class="relative w-full sm:w-64">
                    <span class="absolute inset-y-0 left-0 pl-3 flex items-center text-slate-500"><svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg></span>
                    <input type="text" id="inventorySearch" oninput="renderInventoryTable()" class="w-full bg-black/20 border border-slate-700 rounded-lg py-1.5 pl-9 pr-3 text-white placeholder-slate-500 text-xs focus:outline-none focus:border-sky-500" placeholder="ዕቃዎችን በስም ፈልጉ...">
                </div>
            </div>
            
            <div class="overflow-x-auto w-full">
                <table class="w-full text-left border-collapse">
                    <thead>
                        <tr class="bg-black/20 text-slate-400 text-xs uppercase tracking-wider">
                            <th class="p-4 font-semibold">የዕቃ ስም</th>
                            <th class="p-4 font-semibold">መግዣ (Cost)</th>
                            <th class="p-4 font-semibold">መሸጫ (Price)</th>
                            <th class="p-4 font-semibold">የመጀመሪያ ክምችት</th>
                            <th class="p-4 font-semibold">የተሸጠ ብዛት</th>
                            <th class="p-4 font-semibold">ቀሪ ክምችት</th>
                            <th class="p-4 font-semibold">የዛሬ ትርፍ</th>
                            <th class="p-4 font-semibold text-right">የማስተካከያ እርምጃዎች</th>
                        </tr>
                    </thead>
                    <tbody id="inventoryTable" class="divide-y divide-white/5 text-sm">
                        <!-- Dynamic items -->
                    </tbody>
                </table>
            </div>
        </div>

        <!-- Debt and Cash withdrawal boxes -->
        <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
            <!-- Debt list -->
            <div class="rounded-2xl custom-glass overflow-hidden lg:col-span-1">
                <div class="px-6 py-4 border-b border-white/10">
                    <h3 class="font-bold text-lg text-white flex items-center gap-2">
                        <svg class="w-5 h-5 text-blue-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 8c-1.657 0-3 .895-3 2s1.343 2 3 2 3 .895 3 2-1.343 2-3 2m0-8c1.11 0 2.08.402 2.599 1M12 8V7m0 1v8m0 0v1m0-1c-1.11 0-2.08-.402-2.599-1M21 12a9 9 0 11-18 0 9 9 0 010 18z" /></svg>
                        የተበዳሪዎች መዝገብ
                    </h3>
                </div>
                <div class="overflow-y-auto max-h-[300px] w-full">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-black/20 text-slate-400 text-xs uppercase tracking-wider">
                                <th class="p-4 font-semibold">የተበዳሪ ስም</th>
                                <th class="p-4 font-semibold">የዕቃ ዝርዝር</th>
                                <th class="p-4 font-semibold">ዕዳ</th>
                                <th class="p-4 font-semibold text-right">እርምጃ</th>
                            </tr>
                        </thead>
                        <tbody id="debtTable" class="divide-y divide-white/5 text-sm">
                            <!-- Dynamic debts -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- Drawer actions history -->
            <div class="rounded-2xl custom-glass overflow-hidden lg:col-span-1">
                <div class="px-6 py-4 border-b border-white/10">
                    <h3 class="font-bold text-lg text-white flex items-center gap-2">
                        <svg class="w-5 h-5 text-amber-400" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 11H5m14 0a2 2 0 012 2v6a2 2 0 01-2 2H5a2 2 0 01-2-2v-6a2 2 0 012-2m14 0V9a2 2 0 00-2-2M5 11V9a2 2 0 012-2m0 0V5a2 2 0 012-2h6a2 2 0 012 2v2M7 7h10" /></svg>
                        ከሳጥን የተነሳ ብር
                    </h3>
                </div>
                <div class="overflow-y-auto max-h-[300px] w-full">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-black/20 text-slate-400 text-xs uppercase tracking-wider">
                                <th class="p-4 font-semibold">ማብራሪያ</th>
                                <th class="p-4 font-semibold">ብር</th>
                                <th class="p-4 font-semibold text-right">ሁኔታ</th>
                            </tr>
                        </thead>
                        <tbody id="drawerTable" class="divide-y divide-white/5 text-sm">
                            <!-- Dynamic drawers -->
                        </tbody>
                    </table>
                </div>
            </div>

            <!-- የቀደሙ ቀናት ታሪክ መዝገብ -->
            <div class="rounded-2xl custom-glass overflow-hidden lg:col-span-1">
                <div class="px-6 py-4 border-b border-white/10">
                    <h3 class="font-bold text-lg text-white flex items-center gap-2">
                        📅 የቀደሙ ቀናት ታሪክ መዝገብ
                    </h3>
                </div>
                <div class="overflow-y-auto max-h-[300px] w-full">
                    <table class="w-full text-left border-collapse">
                        <thead>
                            <tr class="bg-black/20 text-slate-400 text-xs uppercase tracking-wider">
                                <th class="p-4 font-semibold">ቀንና ሰራተኛ</th>
                                <th class="p-4 font-semibold">የስራ ሰዓት</th>
                                <th class="p-4 font-semibold">የቀን ትርፍ</th>
                            </tr>
                        </thead>
                        <tbody id="historyTable" class="divide-y divide-white/5 text-sm">
                            <!-- Dynamic history -->
                        </tbody>
                    </table>
                </div>
            </div>
        </div>

        <!-- 🌙 የእለቱ ማታ አውቶማቲክ ሪፖርት ማሳያ ሳጥን -->
        <div id="reportBox" class="rounded-2xl p-6 border-2 border-dashed border-emerald-500/50 bg-black/40 space-y-4">
            <h3 class="text-lg font-bold text-emerald-400 flex items-center gap-2">
                🌙 የእለቱ ማታ 4:00 አውቶማቲክ ሪፖርት
            </h3>
            <textarea id="reportText" class="w-full h-56 bg-black/60 text-emerald-400 border border-white/10 rounded-xl p-4 font-mono text-sm resize-none focus:outline-none" readonly></textarea>
            <button onclick="copyReport()" class="bg-emerald-500 hover:bg-emerald-600 text-black font-extrabold px-6 py-2.5 rounded-xl text-xs transition-all flex items-center gap-1.5 active:scale-95">
                📋 ሪፖርቱን ኮፒ አድርግ
            </button>
        </div>

        <!-- ⚙️ Settings and Profile -->
        <div class="rounded-2xl custom-glass p-6 space-y-6">
            <h3 class="font-bold text-lg text-white border-b border-white/10 pb-3 flex items-center gap-2">
                ⚙️ መቼቶች እና የተጠቃሚ ፕሮፋይል
            </h3>
            
            <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                <!-- User Profile Display -->
                <div class="space-y-4">
                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider">የእኔ ፕሮፋይል</h4>
                    <div class="space-y-2">
                        <div class="flex justify-between p-3 rounded-xl bg-black/20 border border-white/5">
                            <span class="text-slate-400 text-sm">የሱቅ ስም፦</span>
                            <span id="profShopName" class="font-bold text-white">-</span>
                        </div>
                        <div class="flex justify-between p-3 rounded-xl bg-black/20 border border-white/5">
                            <span class="text-slate-400 text-sm">መለያ ስም (User)፦</span>
                            <span id="profUsername" class="font-bold text-white">-</span>
                        </div>
                        <div class="flex justify-between p-3 rounded-xl bg-black/20 border border-white/5">
                            <span class="text-slate-400 text-sm">የይለፍ ቃል፦</span>
                            <span id="profPassword" class="font-bold text-amber-400">-</span>
                        </div>
                    </div>
                    <button onclick="openEditProfileModal()" class="w-full bg-blue-500 hover:bg-blue-600 text-white font-bold py-2.5 rounded-xl text-xs transition-all flex items-center justify-center gap-1.5">
                        <svg class="w-4 h-4" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" /></svg>
                        የመግቢያ መረጃ ቀይር
                    </button>
                </div>

                <!-- Display and Themes Options -->
                <div class="space-y-4">
                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider">የማሳያ ቀለም እና ገጽታ (Themes)</h4>
                    <div class="grid grid-cols-2 gap-2">
                        <button onclick="changeTheme('deepblue')" class="p-2.5 bg-blue-900/40 hover:bg-blue-900 border border-blue-500/30 rounded-xl text-xs font-semibold text-sky-200 transition-all flex items-center justify-center gap-1.5">
                            <span class="w-3 h-3 rounded-full bg-sky-500"></span>
                            Deep Blue
                        </button>
                        <button onclick="changeTheme('emerald')" class="p-2.5 bg-emerald-900/40 hover:bg-emerald-900 border border-emerald-500/30 rounded-xl text-xs font-semibold text-emerald-200 transition-all flex items-center justify-center gap-1.5">
                            <span class="w-3 h-3 rounded-full bg-emerald-500"></span>
                            Emerald Green
                        </button>
                        <button onclick="changeTheme('purple')" class="p-2.5 bg-violet-900/40 hover:bg-violet-900 border border-violet-500/30 rounded-xl text-xs font-semibold text-violet-200 transition-all flex items-center justify-center gap-1.5">
                            <span class="w-3 h-3 rounded-full bg-purple-500"></span>
                            Royal Purple
                        </button>
                        <button onclick="changeTheme('charcoal')" class="p-2.5 bg-neutral-800 hover:bg-neutral-700 border border-neutral-600 rounded-xl text-xs font-semibold text-neutral-200 transition-all flex items-center justify-center gap-1.5">
                            <span class="w-3 h-3 rounded-full bg-neutral-500"></span>
                            Charcoal Dark
                        </button>
                    </div>

                    <h4 class="text-xs font-bold text-slate-400 uppercase tracking-wider mt-4">የጽሁፍ መጠን ማሳያ</h4>
                    <div class="flex items-center gap-2">
                        <button onclick="adjustFontSize(-1)" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 rounded-xl font-bold border border-white/5 active:scale-95 transition-all text-xs">A-</button>
                        <button onclick="resetFontSize()" class="flex-1 py-2 bg-slate-800 hover:bg-slate-700 rounded-xl text-xs font-semibold border border-white/5 transition-all">መደበኛ መጠን</button>
                        <button onclick="adjustFontSize(1)" class="px-4 py-2 bg-slate-800 hover:bg-slate-700 rounded-xl font-bold border border-white/5 active:scale-95 transition-all text-xs">A+</button>
                    </div>
                </div>
            </div>
        </div>
    </div>
</main>

<!-- ────────────────────────────────────────────── -->
<!-- የማዕከላዊ የሞዳል ስርዓት (MODALS & POPUPS SYSTEM) -->
<div id="modalOverlay" class="fixed inset-0 bg-black/80 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
    
    <!-- 1. Custom Alert Modal -->
    <div id="alertModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <div class="flex items-center gap-3">
            <span id="alertIconBox" class="w-10 h-10 rounded-full flex items-center justify-center bg-amber-500/10 text-amber-400">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M12 9v2m0 4h.01m-6.938 4h13.856c1.54 0 2.502-1.667 1.732-3L13.732 4c-.77-1.333-2.694-1.333-3.464 0L3.34 16c-.77 1.333.192 3 1.732 3z" /></svg>
            </span>
            <h3 id="alertTitle" class="text-lg font-bold text-white">ማስጠንቀቂያ</h3>
        </div>
        <p id="alertMessage" class="text-sm text-slate-300">እባክዎ መረጃዎችን በትክክል ያስገቡ!</p>
        <div class="flex justify-end">
            <button id="alertOkBtn" class="bg-sky-500 hover:bg-sky-600 text-white font-bold px-5 py-2 rounded-xl text-xs transition-all active:scale-95">እሺ</button>
        </div>
    </div>

    <!-- 2. Custom Confirm Modal -->
    <div id="confirmModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <div class="flex items-center gap-3">
            <span class="w-10 h-10 rounded-full flex items-center justify-center bg-rose-500/10 text-rose-400">
                <svg class="w-6 h-6" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" /></svg>
            </span>
            <h3 id="confirmTitle" class="text-lg font-bold text-white">እርግጠኛ ኖት?</h3>
        </div>
        <p id="confirmMessage" class="text-sm text-slate-300">ይህ እርምጃ ሙሉ በሙሉ ዳታውን ሊያጠፋው ይችላል።</p>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">አይ ተመለስ</button>
            <button id="confirmYesBtn" class="bg-rose-500 hover:bg-rose-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">አዎ ይቀጥል</button>
        </div>
    </div>

    <!-- 3. Register Tenant Modal (Admin Action) -->
    <div id="registerTenantModal" class="modal-card max-w-md w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">አዲስ ተከራይ መመዝገቢያ</h3>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የሱቅ ስም</label>
                <input type="text" id="newShopName" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ አልፋ ፋሽን">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የመግቢያ ስም (Username)</label>
                <input type="text" id="newUsername" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ alfa">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የይለፍ ቃል (Password)</label>
                <input type="text" id="newPassword" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ቢያንስ 4 ዲጂት">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">አይ ይቅር</button>
            <button onclick="registerTenant()" class="bg-sky-500 hover:bg-sky-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">ይመዝገብ</button>
        </div>
    </div>

    <!-- 4. Add/Restock Inventory Item Modal -->
    <div id="addInventoryModal" class="modal-card max-w-md w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">📦 አዲስ ዕቃ / ጭማሪ መዝገብ</h3>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የዕቃ ስም</label>
                <input type="text" id="itemName" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ ሌዘር ጫማ">
            </div>
            <div class="grid grid-cols-2 gap-3">
                <div>
                    <label class="block text-xs font-semibold text-slate-400 mb-1">መግዣ ዋጋ (Cost)</label>
                    <input type="number" id="itemCost" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-400 mb-1">መሸጫ ዋጋ (Price)</label>
                    <input type="number" id="itemPrice" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
                </div>
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">ብዛት (Qty)</label>
                <input type="number" id="itemQty" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button onclick="addItem()" class="bg-emerald-500 hover:bg-emerald-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">ወደ ዝርዝር አስገባ</button>
        </div>
    </div>

    <!-- 5. Edit Inventory Item Modal -->
    <div id="editInventoryModal" class="modal-card max-w-md w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">✏️ የዕቃ መረጃ ማስተካከያ</h3>
        <input type="hidden" id="editItemIndex">
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የዕቃ ስም</label>
                <input type="text" id="editItemName" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
            <div class="grid grid-cols-2 gap-3">
                <div>
                    <label class="block text-xs font-semibold text-slate-400 mb-1">መግዣ ዋጋ (Cost)</label>
                    <input type="number" id="editItemCost" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-400 mb-1">መሸጫ ዋጋ (Price)</label>
                    <input type="number" id="editItemPrice" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
                </div>
            </div>
            <div class="grid grid-cols-2 gap-3">
                <div>
                    <label class="block text-xs font-semibold text-slate-400 mb-1">ጠቅላላ ብዛት</label>
                    <input type="number" id="editItemQty" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
                </div>
                <div>
                    <label class="block text-xs font-semibold text-slate-400 mb-1">የተሸጠው መጠን</label>
                    <input type="number" id="editItemSold" readonly class="w-full bg-slate-800 border border-slate-700 rounded-lg p-2 text-sm text-slate-400">
                </div>
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button onclick="saveInventoryChanges()" class="bg-emerald-500 hover:bg-emerald-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">አስተካክል</button>
        </div>
    </div>

    <!-- 6. Expense Modal -->
    <div id="expenseModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">📉 አዲስ የወጪ መዝገብ</h3>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የወጪው ምክንያት</label>
                <input type="text" id="expenseReason" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ ለቤት ኪራይ ወይም ለስልክ ካርድ">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የብር መጠን (ETB)</label>
                <input type="number" id="expenseAmount" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ETB">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button onclick="saveExpense()" class="bg-rose-500 hover:bg-rose-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">መዝግብ</button>
        </div>
    </div>

    <!-- 7. Debt Modal -->
    <div id="debtModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">💳 አዲስ የተበዳሪ መመዝገቢያ</h3>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የተበዳሪ ደንበኛ ስም እና ስልክ</label>
                <input type="text" id="debtName" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ ወ/ሮ አስቴር (09...)">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የተወሰደ እቃ/ዝርዝር</label>
                <input type="text" id="debtDetail" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ ጫማ">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የዕዳ መጠን (ETB)</label>
                <input type="number" id="debtAmount" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button onclick="saveDebt()" class="bg-blue-500 hover:bg-blue-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">መዝግብ</button>
        </div>
    </div>

    <!-- 8. Drawer Cash Modal -->
    <div id="drawerModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">💸 ከሳጥን ገንዘብ ማውጫ</h3>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">ምክንያት/ማብራሪያ</label>
                <input type="text" id="drawerReason" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ምሳሌ፦ ለቤት ዕቃዎች ግዢ">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የተወሰደው የብር መጠን</label>
                <input type="number" id="drawerAmount" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button onclick="saveDrawer()" class="bg-amber-500 hover:bg-amber-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">ወጪ መዝግብ</button>
        </div>
    </div>

    <!-- 9. Edit Profile Modal -->
    <div id="editProfileModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">✏️ የመግቢያ መረጃ መቀየሪያ</h3>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የሱቅ ስም</label>
                <input type="text" id="editShopName" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">አዲስ የይለፍ ቃል (Password)</label>
                <input type="text" id="editPassword" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button onclick="saveProfileChanges()" class="bg-blue-500 hover:bg-blue-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">ማስተካከያውን መዝግብ</button>
        </div>
    </div>

    <!-- 10. Debt Payoff/Settlement Modal -->
    <div id="settleDebtModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">💳 የዕዳ አከፋፈል ማስተካከያ</h3>
        <div class="space-y-2 text-sm">
            <p class="text-slate-400">የደንበኛ ስም፦ <b id="settleCustName" class="text-white">-</b></p>
            <p class="text-slate-400">ጠቅላላ ዕዳ፦ <b id="settleCustAmt" class="text-rose-400">-</b></p>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1 mt-3">የሚከፈልበት የብር መጠን (ETB)</label>
                <input type="number" id="settlePayAmount" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
            </div>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ይቅር</button>
            <button id="settleSubmitBtn" class="bg-emerald-500 hover:bg-emerald-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">ክፍያ ፈጽም</button>
        </div>
    </div>

    <!-- 11. Bank Connection Config Modal -->
    <div id="bankConfigModal" class="modal-card max-w-sm w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4">
        <h3 class="text-lg font-bold text-white">🏦 የባንክ አፕ ማያያዣ መቼት</h3>
        <div class="space-y-3">
            <label class="block text-xs font-semibold text-slate-400">ለክፍያ የሚጠቀሙበትን ዋና የባንክ መተግበሪያ ይምረጡ፦</label>
            <select id="bankAppSelect" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500">
                <option value="telebirr://">Telebirr (ቴሌብር)</option>
                <option value="cbebirr://">CBE Birr (ሲቢኢ ብር)</option>
                <option value="awashsol://">Awash Sol (አዋሽ ሶል)</option>
            </select>
        </div>
        <div class="flex justify-end gap-2.5">
            <button onclick="closeActiveModal()" class="bg-slate-800 hover:bg-slate-700 text-slate-300 font-bold px-4 py-2 rounded-xl text-xs transition-all">ተመለስ</button>
            <button onclick="saveBankConfig()" class="bg-sky-500 hover:bg-sky-600 text-white font-bold px-4 py-2 rounded-xl text-xs transition-all active:scale-95">አገናኝ</button>
        </div>
    </div>

    <!-- 12. Morning Session Config Modal -->
    <div id="morningSessionModal" class="modal-card max-w-md w-full bg-slate-900 rounded-2xl border border-white/10 shadow-2xl p-6 hidden space-y-4" data-backdrop="static">
        <div class="text-center">
            <h3 class="text-xl font-bold text-amber-400">☀️ የዕለቱ የሥራ መጀመሪያ ፎርም</h3>
            <p class="text-xs text-slate-400 mt-1">እባክዎ የዛሬውን ሥራ ለመጀመር መነሻ መረጃዎችን ይሙሉ</p>
        </div>
        <div class="space-y-3">
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">የዛሬው ሠራተኛ ስም</label>
                <input type="text" id="sessionEmployee" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ለምሳሌ፦ ሙሉጌታ">
            </div>
            <div>
                <label class="block text-xs font-semibold text-slate-400 mb-1">ለመልስ የሚሆን በሳጥን የተተወ መነሻ ገንዘብ (Float ETB)</label>
                <input type="number" id="sessionFloat" class="w-full bg-black/40 border border-slate-700 rounded-lg p-2 text-sm text-white focus:outline-none focus:border-sky-500" placeholder="ለምሳሌ፦ 1000">
            </div>
        </div>
        <div class="flex justify-end">
            <button onclick="submitMorningSession()" class="w-full bg-amber-500 hover:bg-amber-600 text-black font-extrabold py-2.5 rounded-xl text-xs transition-all active:scale-95">🚀 የዛሬውን ሥራ ጀምር</button>
        </div>
    </div>

</div>

<!-- Firestore / Authentication Implementation via ESM (v11) -->
<script type="module">
    import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
    import { getAuth, signInWithCustomToken, signInAnonymously, onAuthStateChanged } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
    import { getFirestore, doc, setDoc, getDoc, collection, onSnapshot, updateDoc, deleteDoc } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

    const appId = typeof __app_id !== 'undefined' ? __app_id : 'tirfe-app-v2';
    const rawFirebaseConfig = typeof __firebase_config !== 'undefined' ? __firebase_config : null;
    const rawAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;

    // Fallback config if environment is missing
    const fallbackConfig = {
        apiKey: "",
        authDomain: "tirfe-app-v2-300c2.firebaseapp.com",
        projectId: "tirfe-app-v2-300c2",
        storageBucket: "tirfe-app-v2-300c2.appspot.com",
        messagingSenderId: "542385108420",
        appId: "1:542385108420:web:d252feff0bb6329ef300c2"
    };

    const firebaseConfig = rawFirebaseConfig ? JSON.parse(rawFirebaseConfig) : fallbackConfig;
    const app = initializeApp(firebaseConfig);
    const auth = getAuth(app);
    const db = getFirestore(app);

    let user = null;
    let localDB = { tenants: {} };
    let currentTenant = null;
    let currentFontSize = 14;
    let collectedCreditToday = 0;
    let preferredBankApp = "telebirr://";

    const publicDataCollection = collection(db, 'artifacts', appId, 'public', 'data', 'tenants');

    // Authentication Setup
    const initAuth = async () => {
        try {
            if (rawAuthToken) {
                await signInWithCustomToken(auth, rawAuthToken);
            } else {
                await signInAnonymously(auth);
            }
        } catch (err) {
            console.error("Auth initialization failed: ", err);
            showCustomAlert("🔒 የደህንነት ችግር", "የመረጃ ቋቱ ጋር መገናኘት አልተቻለም።");
        }
    };
    initAuth();

    onAuthStateChanged(auth, (firebaseUser) => {
        user = firebaseUser;
        if (user) {
            document.getElementById('connectionStatus').innerHTML = '<span class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span> መስመር ላይ';
            setupRealtimeSync();
        } else {
            document.getElementById('connectionStatus').innerHTML = '<span class="w-2 h-2 rounded-full bg-amber-500"></span> ያልተገናኘ';
        }
    });

    // Realtime Database Sync Listener
    function setupRealtimeSync() {
        if (!user) return;

        onSnapshot(publicDataCollection, (querySnapshot) => {
            const fetchedTenants = {};
            querySnapshot.forEach((doc) => {
                fetchedTenants[doc.id] = doc.data();
            });
            localDB.tenants = fetchedTenants;

            if (currentTenant) {
                const refreshedTenant = localDB.tenants[currentTenant.username];
                if (!refreshedTenant) {
                    showCustomAlert("⚠️ የመለያ መጥፋት", "ይህ አካውንት በባለቤቱ ተሰርዟል!", () => {
                        logout();
                    });
                    return;
                }
                if (refreshedTenant.status === "blocked") {
                    showCustomAlert("🔒 የኪራይ ማብቂያ", "የኪራይ ጊዜዎ አብቅቷል! እባክዎ ባለቤቱን ያነጋግሩ።", () => {
                        logout();
                    });
                    return;
                }
                currentTenant = refreshedTenant;
                loadTenantApp();
            }

            if (!document.getElementById('adminPage').classList.contains('hidden')) {
                renderAdminTenantList();
            }
        }, (error) => {
            console.error("Database sync error: ", error);
        });
    }

    async function updateTenantDocument(username, tenantPayload) {
        if (!user) return;
        const tenantDocRef = doc(db, 'artifacts', appId, 'public', 'data', 'tenants', username);
        
        let attempts = 0;
        const maxRetries = 5;
        let delay = 1000;

        while (attempts < maxRetries) {
            try {
                await setDoc(tenantDocRef, tenantPayload, { merge: true });
                return;
            } catch (err) {
                attempts++;
                if (attempts >= maxRetries) {
                    showCustomAlert("❌ የመረጃ ማመሳሰል አልተሳካም", "እባክዎ የኢንተርኔት ግንኙነትዎን ፈትሸው እንደገና ይሞክሩ።");
                    throw err;
                }
                await new Promise(resolve => setTimeout(resolve, delay));
                delay *= 2;
            }
        }
    }

    async function deleteTenantDocument(username) {
        if (!user) return;
        const tenantDocRef = doc(db, 'artifacts', appId, 'public', 'data', 'tenants', username);
        try {
            await deleteDoc(tenantDocRef);
        } catch (err) {
            console.error("Delete failed: ", err);
        }
    }

    // UI Utilities
    window.openModalContainer = function() {
        document.getElementById('modalOverlay').classList.remove('hidden');
    };

    window.closeActiveModal = function() {
        // Prevent closing morning session modal if session is not active
        const morningModal = document.getElementById('morningSessionModal');
        if (!morningModal.classList.contains('hidden') && (!currentTenant || !currentTenant.data.sessionData)) {
            return;
        }
        document.getElementById('modalOverlay').classList.add('hidden');
        const modals = document.querySelectorAll('.modal-card');
        modals.forEach(m => m.classList.add('hidden'));
    };

    window.showCustomAlert = function(title, message, callback) {
        document.getElementById('alertTitle').innerText = title;
        document.getElementById('alertMessage').innerText = message;
        
        openModalContainer();
        document.getElementById('alertModal').classList.remove('hidden');
        
        const okBtn = document.getElementById('alertOkBtn');
        okBtn.onclick = function() {
            closeActiveModal();
            if(callback) callback();
        };
    };

    window.showCustomConfirm = function(title, message, onConfirm) {
        document.getElementById('confirmTitle').innerText = title;
        document.getElementById('confirmMessage').innerText = message;
        
        openModalContainer();
        document.getElementById('confirmModal').classList.remove('hidden');
        
        const yesBtn = document.getElementById('confirmYesBtn');
        yesBtn.onclick = function() {
            closeActiveModal();
            if(onConfirm) onConfirm();
        };
    };

    // Authentication Logic
    window.handleLogin = function() {
        const usernameInput = document.getElementById('loginUser').value.trim().toLowerCase();
        const passwordInput = document.getElementById('loginPass').value.trim();
        const errorMsg = document.getElementById('loginError');

        if (!usernameInput || !passwordInput) {
            errorMsg.innerText = "❌ እባክዎ ሁሉንም መስኮች ይሙሉ!";
            return;
        }

        if (usernameInput === "admin" && passwordInput === "admin123") {
            errorMsg.innerText = "";
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('adminPage').classList.remove('hidden');
            document.getElementById('logoutBtn').classList.remove('hidden');
            renderAdminTenantList();
            return;
        }

        const tenant = localDB.tenants[usernameInput];
        if (tenant && String(tenant.password).trim() === passwordInput) {
            if (tenant.status === "blocked") {
                errorMsg.innerText = "🔒 ኪራይዎ በባለቤቱ ታግዷል!";
                return;
            }
            errorMsg.innerText = "";
            currentTenant = tenant;
            document.getElementById('loginPage').classList.add('hidden');
            document.getElementById('appPage').classList.remove('hidden');
            document.getElementById('logoutBtn').classList.remove('hidden');
            
            document.getElementById('shopTitle').innerText = tenant.shopName + " - መቆጣጠሪያ";
            document.getElementById('metaStaff').innerText = tenant.username;
            
            updateProfileSectionUI();
            checkMorningSessionStatus();
        } else {
            errorMsg.innerText = "❌ የተጠቃሚ ስም ወይም የይለፍ ቃል ስህተት ነው!";
        }
    };

    function updateProfileSectionUI() {
        if(currentTenant) {
            document.getElementById('profShopName').innerText = currentTenant.shopName;
            document.getElementById('profUsername').innerText = currentTenant.username;
            document.getElementById('profPassword').innerText = currentTenant.password;
        }
    }

    // Real-Time Clock & Date
    setInterval(() => {
        const now = new Date();
        const optionsDate = { year: 'numeric', month: 'long', day: 'numeric' };
        const formattedDate = now.toLocaleDateString('am-ET', optionsDate);
        
        const optionsTime = { hour: '2-digit', minute: '2-digit', second: '2-digit', hour12: true };
        const formattedTime = now.toLocaleTimeString('am-ET', optionsTime);
        
        const mDate = document.getElementById('metaDate');
        const mTime = document.getElementById('metaTime');
        if (mDate) mDate.innerText = formattedDate;
        if (mTime) mTime.innerText = formattedTime;
    }, 1000);

    // Zooming font configurations
    window.adjustFontSize = function(value) {
        currentFontSize += value;
        if(currentFontSize < 11) currentFontSize = 11;
        if(currentFontSize > 22) currentFontSize = 22;
        document.documentElement.style.setProperty('--base-font-size', currentFontSize + 'px');
        document.body.style.fontSize = currentFontSize + 'px';
    };

    window.resetFontSize = function() {
        currentFontSize = 14;
        document.documentElement.style.setProperty('--base-font-size', '14px');
        document.body.style.fontSize = '14px';
    };

    window.changeTheme = function(themeName) {
        document.body.className = '';
        document.body.classList.add('theme-' + themeName, 'min-h-full', 'text-slate-100', 'flex', 'flex-col');
    };

    // ──────────────────────────────────────────────
    // OWNER ADMIN ACTIONS
    // ──────────────────────────────────────────────
    window.openRegisterTenantModal = function() {
        document.getElementById('newShopName').value = '';
        document.getElementById('newUsername').value = '';
        document.getElementById('newPassword').value = '';
        
        openModalContainer();
        document.getElementById('registerTenantModal').classList.remove('hidden');
    };

    window.registerTenant = async function() {
        const shop = document.getElementById('newShopName').value.trim();
        const userKey = document.getElementById('newUsername').value.trim().toLowerCase(); 
        const pass = document.getElementById('newPassword').value.trim();

        if(!shop || !userKey || !pass) { 
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም መረጃዎች ይሙሉ!"); 
            return; 
        }
        if(localDB.tenants && localDB.tenants[userKey]) { 
            showCustomAlert("⚠️ ስህተት", "ይህ የተጠቃሚ ስም ቀድሞ ተወስዷል!"); 
            return; 
        }

        const newTenantPayload = {
            shopName: shop,
            username: userKey,
            password: pass,
            status: "active",
            data: {
                capital: 0,
                dailyProfit: 0,
                monthlyProfit: 0,
                drawerAmt: 0,
                inventory: [],
                expenses: [],
                debts: [],
                drawerLog: [],
                history: [],
                collectedCreditToday: 0,
                preferredBankApp: "telebirr://",
                sessionData: null
            }
        };

        closeActiveModal();
        showCustomAlert("⏳ በመመዝገብ ላይ...", "እባክዎ ለጥቂት ሰኮንዶች ይጠብቁ...");
        
        await updateTenantDocument(userKey, newTenantPayload);
        closeActiveModal();
        showCustomAlert("✅ ስኬት", `የ"${shop}" ተከራይ በስኬት ተመዝግቧል!`);
        renderAdminTenantList();
    };

    window.toggleTenantStatus = async function(userKey) {
        const target = localDB.tenants[userKey];
        if (!target) return;

        const updatedStatus = target.status === "active" ? "blocked" : "active";
        target.status = updatedStatus;
        
        await updateTenantDocument(userKey, target);
    };

    window.deleteTenant = function(userKey) {
        const target = localDB.tenants[userKey];
        if (!target) return;

        showCustomConfirm(
            "⚠️ ሙሉ በሙሉ ለመሰረዝ", 
            `"${target.shopName}" የተባለውን ተከራይና ጠቅላላ መረጃውን ማጥፋት ይፈልጋሉ? ይህ ድርጊት ሊመለስ አይችልም።`,
            async () => {
                showCustomAlert("⏳ በመሰረዝ ላይ...", "እባክዎ ይጠብቁ...");
                await deleteTenantDocument(userKey);
                closeActiveModal();
                showCustomAlert("🧹 ተሰርዟል", "ተከራዩ ከሲስተሙ ላይ ሙሉ በሙሉ ተወግዷል።");
                renderAdminTenantList();
            }
        );
    };

    window.renderAdminTenantList = function() {
        const tbody = document.getElementById('tenantTableBody');
        tbody.innerHTML = '';
        if(!localDB.tenants) return;

        const searchTerm = document.getElementById('adminSearchTenant').value.toLowerCase().trim();

        Object.keys(localDB.tenants).forEach(key => {
            const t = localDB.tenants[key];
            if (searchTerm && !t.shopName.toLowerCase().includes(searchTerm) && !t.username.toLowerCase().includes(searchTerm)) {
                return;
            }

            const badge = t.status === 'active' 
                ? '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-emerald-500/10 text-emerald-400 border border-emerald-500/20">🟢 ስራ ላይ</span>' 
                : '<span class="px-2.5 py-1 rounded-full text-xs font-bold bg-rose-500/10 text-rose-400 border border-rose-500/20">🔴 የታገደ</span>';
            
            const btnText = t.status === 'active' ? 'እገድ' : 'ማገጃ አንሳ';
            const btnClass = t.status === 'active' 
                ? 'bg-amber-500/10 hover:bg-amber-500 text-amber-400 hover:text-black border border-amber-500/20' 
                : 'bg-emerald-500/10 hover:bg-emerald-500 text-emerald-400 hover:text-white border border-emerald-500/20';

            tbody.innerHTML += `
                <tr class="hover:bg-white/[0.02] transition-colors">
                    <td class="p-4"><b class="text-white text-base font-semibold">${t.shopName}</b></td>
                    <td class="p-4 text-slate-300 font-mono">${t.username}</td>
                    <td class="p-4 text-slate-300 font-mono">${t.password}</td>
                    <td class="p-4">${badge}</td>
                    <td class="p-4 text-right">
                        <div class="flex justify-end gap-2">
                            <button class="${btnClass} font-bold px-3 py-1.5 rounded-lg text-xs transition-all active:scale-95" onclick="toggleTenantStatus('${t.username}')">${btnText}</button>
                            <button class="bg-rose-500/10 hover:bg-rose-500 text-rose-400 hover:text-white border border-rose-500/20 font-bold px-3 py-1.5 rounded-lg text-xs transition-all active:scale-95" onclick="deleteTenant('${t.username}')">ሰርዝ</button>
                        </div>
                    </td>
                </tr>
            `;
        });
    };

    // ──────────────────────────────────────────────
    // TENANT MORNING SESSION MANAGEMENT
    // ──────────────────────────────────────────────
    function checkMorningSessionStatus() {
        const d = currentTenant.data;
        if (!d || !d.sessionData) {
            document.getElementById('sessionEmployee').value = '';
            document.getElementById('sessionFloat').value = '';
            
            openModalContainer();
            document.getElementById('morningSessionModal').classList.remove('hidden');
        } else {
            loadTenantApp();
        }
    }

    window.submitMorningSession = async function() {
        const employee = document.getElementById('sessionEmployee').value.trim();
        const floatMoney = parseFloat(document.getElementById('sessionFloat').value) || 0;

        if (!employee) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የሠራተኛ ስም ያስገቡ!");
            return;
        }

        const today = new Date();
        const dateStr = today.toLocaleDateString('am-ET');
        const timeStr = today.toLocaleTimeString('am-ET', { hour: '2-digit', minute: '2-digit' });

        const d = currentTenant.data;
        d.sessionData = {
            date: dateStr,
            loginTime: timeStr,
            employee: employee,
            initialFloat: floatMoney
        };

        // Reset daily dynamic calculations on new morning
        d.expenses = [];
        d.collectedCreditToday = 0;
        
        // Remove temporary data from previous day payoff
        d.inventory = d.inventory.filter(i => !i.isCreditPay);
        d.inventory.forEach(i => {
            i.sold = 0;
            i.profit = 0;
        });

        closeActiveModal();
        showCustomAlert("⏳ በመጫን ላይ...", "እባክዎ ለጥቂት ሰኮንዶች ይጠብቁ...");
        await updateTenantDocument(currentTenant.username, currentTenant);
        closeActiveModal();
        loadTenantApp();
    };

    function displaySessionBadge() {
        const sd = currentTenant.data.sessionData;
        if (sd) {
            document.getElementById('sessionDisplay').innerHTML = `
                <div class="flex items-center gap-1">📅 ቀን፦ <span class="text-white">${sd.date}</span></div>
                <span class="hidden md:inline text-slate-600">|</span>
                <div class="flex items-center gap-1">👤 ሠራተኛ፦ <span class="text-white">${sd.employee}</span></div>
                <span class="hidden md:inline text-slate-600">|</span>
                <div class="flex items-center gap-1">🕒 የገባበት ሰዓት፦ <span class="text-white">${sd.loginTime}</span></div>
                <span class="hidden md:inline text-slate-600">|</span>
                <div class="flex items-center gap-1">💰 መነሻ ብር፦ <span class="text-amber-300 font-bold">${sd.initialFloat.toLocaleString()} ETB</span></div>
            `;
        }
    }

    // ──────────────────────────────────────────────
    // TENANT APP CORE FLOWS
    // ──────────────────────────────────────────────
    function loadTenantApp() {
        const d = currentTenant.data;
        if (!d.inventory) d.inventory = [];
        if (!d.expenses) d.expenses = [];
        if (!d.debts) d.debts = [];
        if (!d.drawerLog) d.drawerLog = [];
        if (!d.history) d.history = [];
        if (d.collectedCreditToday === undefined) d.collectedCreditToday = 0;
        if (d.preferredBankApp === undefined) d.preferredBankApp = "telebirr://";

        collectedCreditToday = d.collectedCreditToday;
        preferredBankApp = d.preferredBankApp;

        displaySessionBadge();
        calculateDashboardMetrics();
        generateLiveReport();

        renderInventoryTable();
        renderDrawerTable();
        renderDebtTable();
        renderHistoryTable();
    }

    function calculateDashboardMetrics() {
        const d = currentTenant.data;
        const inv = d.inventory || [];
        const exp = d.expenses || [];
        const drawer = d.drawerLog || [];
        
        let totalAssetCost = 0;
        let todayProfit = 0;
        let todaySales = collectedCreditToday;

        inv.forEach(i => {
            const remaining = i.qty - i.sold;
            if (!i.isCreditPay) {
                totalAssetCost += (remaining * i.cost);
            }
            todaySales += (i.price * i.sold);
            todayProfit += (i.price - i.cost) * i.sold;
        });

        let totalExpenses = 0;
        exp.forEach(e => {
            totalExpenses += e.amt;
        });

        let totalDraws = 0;
        drawer.forEach(dw => {
            if (dw.status === 'ወጣ') {
                totalDraws += dw.amt;
            }
        });

        const netDailyProfit = todayProfit - totalExpenses;
        
        // Month total combines current day with history logs
        let netMonthlyProfit = netDailyProfit;
        if (d.history) {
            d.history.forEach(h => {
                netMonthlyProfit += h.profit;
            });
        }

        const currentCapital = totalAssetCost + netMonthlyProfit;
        const initFloat = d.sessionData ? d.sessionData.initialFloat : 0;
        const totalInCashBox = initFloat + todaySales - totalExpenses - totalDraws;

        // Display updates
        document.getElementById('dispCapital').innerText = currentCapital.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) + " ETB";
        document.getElementById('dispDailyProfit').innerText = netDailyProfit.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) + " ETB";
        document.getElementById('dispMonthlyProfit').innerText = netMonthlyProfit.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) + " ETB";
        document.getElementById('dispDrawer').innerText = totalDraws.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) + " ETB";
        document.getElementById('dispTotalInCash').innerText = totalInCashBox.toLocaleString(undefined, {minimumFractionDigits: 2, maximumFractionDigits: 2}) + " ETB";

        // Save calculated references inside object state
        d.capital = currentCapital;
        d.dailyProfit = netDailyProfit;
        d.monthlyProfit = netMonthlyProfit;
        d.drawerAmt = totalDraws;
    }

    window.openAddInventoryModal = function() {
        document.getElementById('itemName').value = '';
        document.getElementById('itemCost').value = '';
        document.getElementById('itemPrice').value = '';
        document.getElementById('itemQty').value = '';
        
        openModalContainer();
        document.getElementById('addInventoryModal').classList.remove('hidden');
    };

    window.addItem = async function() {
        const name = document.getElementById('itemName').value.trim();
        const cost = parseFloat(document.getElementById('itemCost').value) || 0;
        const price = parseFloat(document.getElementById('itemPrice').value) || 0;
        const qty = parseInt(document.getElementById('itemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0 || qty <= 0) { 
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም የዕቃ መረጃዎች በትክክል ያስገቡ!"); 
            return; 
        }

        const d = currentTenant.data;
        const existing = d.inventory.find(i => i.name.toLowerCase() === name.toLowerCase());
        
        if (existing) {
            // Add quantity while keeping existing sales record intact
            existing.qty = (existing.qty - existing.sold) + qty;
            existing.cost = cost; 
            existing.price = price;
            existing.sold = 0;
            existing.profit = 0;
            showCustomAlert("🔄 ክምችት ተሻሽሏል", `የ"${name}" ዋጋና ብዛት በተሳካ ሁኔታ ተሻሽሏል!`);
        } else {
            d.inventory.push({ name, cost, price, qty, sold: 0, profit: 0 });
            showCustomAlert("📦 ስኬት", `"${name}" የተባለው ዕቃ በተሳካ ሁኔታ ተመዝግቧል!`);
        }
        
        closeActiveModal();
        calculateDashboardMetrics();
        await updateTenantDocument(currentTenant.username, currentTenant);
    };

    window.renderInventoryTable = function() {
        const tbody = document.getElementById('inventoryTable');
        tbody.innerHTML = '';
        const inv = currentTenant.data.inventory || [];
        const searchTerm = document.getElementById('inventorySearch').value.toLowerCase().trim();

        inv.forEach((i, idx) => {
            if (i.isCreditPay) return; // Skip temporary credit items
            if (searchTerm && !i.name.toLowerCase().includes(searchTerm)) return;

            const remaining = i.qty - i.sold;
            const isLowStock = remaining <= 5;
            const bgClass = isLowStock ? 'bg-rose-500/10 hover:bg-rose-500/15' : 'hover:bg-white/[0.01]';
            
            const alertBadge = isLowStock 
                ? `<span onclick="triggerRestockFlow('${i.name}')" class="ml-2 cursor-pointer bg-rose-500 text-black text-[10px] font-extrabold px-1.5 py-0.5 rounded shadow">⚠️ አልቋል! ይግዙ</span>` 
                : '';

            tbody.innerHTML += `
                <tr class="${bgClass} border-b border-white/5 transition-colors">
                    <td class="p-4"><b class="text-white">${i.name}</b> ${alertBadge}</td>
                    <td class="p-4 font-mono">${i.cost.toLocaleString()}</td>
                    <td class="p-4 font-mono">${i.price.toLocaleString()}</td>
                    <td class="p-4 text-center font-mono">${i.qty}</td>
                    <td class="p-4">
                        <div class="flex items-center gap-2">
                            <button class="bg-emerald-500 hover:bg-emerald-600 text-black font-extrabold px-2 py-1 rounded-lg text-xs transition-all active:scale-95" onclick="sellItem(${idx})">➕ 1 ሽጥ</button> 
                            <b class="text-slate-200 font-mono">${i.sold}</b>
                        </div>
                    </td>
                    <td class="p-4 font-mono text-center font-bold ${isLowStock ? 'text-rose-400' : 'text-slate-300'}">${remaining}</td>
                    <td class="p-4 text-emerald-400 font-bold font-mono">${i.profit.toLocaleString()}</td>
                    <td class="p-4 text-right">
                        <div class="flex justify-end gap-2">
                            <button class="bg-blue-500/15 hover:bg-blue-500 text-blue-400 hover:text-white border border-blue-500/20 font-bold px-2.5 py-1.5 rounded-lg text-xs transition-all" onclick="openEditInventoryModal(${idx})">✏️ ማስተካከያ</button>
                            <button class="bg-rose-500/10 hover:bg-rose-500 text-rose-400 hover:text-white border border-rose-500/20 font-bold px-2.5 py-1.5 rounded-lg text-xs transition-all" onclick="deleteInventoryItem(${idx})">🗑️ ሰርዝ</button>
                        </div>
                    </td>
                </tr>
            `;
        });
    };

    window.sellItem = async function(idx) {
        const item = currentTenant.data.inventory[idx];
        if((item.qty - item.sold) <= 0) { 
            showCustomAlert("⚠️ ክምችት አልቋል", "ይህ ዕቃ በሱቁ ውስጥ ቀሪ ክምችት የለውም!"); 
            return; 
        }
        
        item.sold += 1;
        item.profit += (item.price - item.cost);
        
        calculateDashboardMetrics();
        await updateTenantDocument(currentTenant.username, currentTenant);
    };

    window.triggerRestockFlow = function(itemName) {
        showCustomConfirm(
            "🏦 የክምችት ማዘዣ ክፍያ", 
            `የ"${itemName}" ዕቃ ማዘዣ ክፍያ ለመፈጸም ወደ ተመረጠው የባንክ መተግበሪያ መሸጋገር ይፈልጋሉ?`,
            () => {
                window.location.href = preferredBankApp;
                // Timeout fallback if app url fails
                setTimeout(() => {
                    openAddInventoryModal();
                    document.getElementById('itemName').value = itemName;
                }, 1000);
            }
        );
    };

    window.deleteInventoryItem = function(idx) {
        showCustomConfirm(
            "⚠️ ዕቃ ለመሰረዝ", 
            `የመረጡትን "${currentTenant.data.inventory[idx].name}" ሙሉ በሙሉ መሰረዝ ይፈልጋሉ?`, 
            async () => {
                currentTenant.data.inventory.splice(idx, 1);
                calculateDashboardMetrics();
                await updateTenantDocument(currentTenant.username, currentTenant);
                showCustomAlert("🧹 ስኬት", "ዕቃው በተሳካ ሁኔታ ተሰርዟል!");
            }
        );
    };

    window.openEditInventoryModal = function(idx) {
        const item = currentTenant.data.inventory[idx];
        document.getElementById('editItemIndex').value = idx;
        document.getElementById('editItemName').value = item.name;
        document.getElementById('editItemCost').value = item.cost;
        document.getElementById('editItemPrice').value = item.price;
        document.getElementById('editItemQty').value = item.qty;
        document.getElementById('editItemSold').value = item.sold;

        openModalContainer();
        document.getElementById('editInventoryModal').classList.remove('hidden');
    };

    window.saveInventoryChanges = async function() {
        const idx = parseInt(document.getElementById('editItemIndex').value);
        const name = document.getElementById('editItemName').value.trim();
        const cost = parseFloat(document.getElementById('editItemCost').value) || 0;
        const price = parseFloat(document.getElementById('editItemPrice').value) || 0;
        const qty = parseInt(document.getElementById('editItemQty').value) || 0;

        if(!name || cost <= 0 || price <= 0 || qty < 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የተሻሻሉትን መረጃዎች በትክክል ያስገቡ!");
            return;
        }

        const item = currentTenant.data.inventory[idx];
        item.name = name;
        item.cost = cost;
        item.price = price;
        item.qty = qty;
        item.profit = (price - cost) * item.sold;

        calculateDashboardMetrics();
        closeActiveModal();
        await updateTenantDocument(currentTenant.username, currentTenant);
        showCustomAlert("✅ ስኬት", "የዕቃው መረጃ በተሳካ ሁኔታ ተስተካክሏል!");
    };

    // EXPENSES MANAGEMENT
    window.openExpenseModal = function() {
        document.getElementById('expenseReason').value = '';
        document.getElementById('expenseAmount').value = '';
        openModalContainer();
        document.getElementById('expenseModal').classList.remove('hidden');
    };

    window.saveExpense = async function() {
        const reason = document.getElementById('expenseReason').value.trim();
        const amt = parseFloat(document.getElementById('expenseAmount').value) || 0;

        if(!reason || amt <= 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ ትክክለኛ የወጪ ምክንያት እና የብር መጠን ያስገቡ!");
            return;
        }

        const d = currentTenant.data;
        if(!d.expenses) d.expenses = [];
        d.expenses.push({ reason, amt }); 
        
        calculateDashboardMetrics();
        closeActiveModal();
        await updateTenantDocument(currentTenant.username, currentTenant);
        showCustomAlert("📉 ወጪ ተመዝግቧል", "የተጣራ ትርፍና የሳጥን ካሽ በወጪው መጠን ቀንሷል።");
    };

    // DEBTS AND CREDITS
    window.openDebtModal = function() {
        document.getElementById('debtName').value = '';
        document.getElementById('debtDetail').value = '';
        document.getElementById('debtAmount').value = '';
        openModalContainer();
        document.getElementById('debtModal').classList.remove('hidden');
    };

    window.saveDebt = async function() {
        const name = document.getElementById('debtName').value.trim();
        const detail = document.getElementById('debtDetail').value.trim();
        const amt = parseFloat(document.getElementById('debtAmount').value) || 0;

        if(!name || amt <= 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የተበዳሪ ስም እና ትክክለኛ የብር መጠን ያስገቡ!");
            return;
        }

        const d = currentTenant.data;
        const item = d.inventory.find(i => i.name.toLowerCase() === detail.toLowerCase());
        let calculatedCost = amt * 0.8; // default fallback cost (80% of sell price)

        if (item) {
            const count = parseInt(prompt(`ደንበኛው የወሰደው የ"${item.name}" ብዛት ስንት ነው?`)) || 1;
            const remaining = item.qty - item.sold;
            if (count > remaining) {
                showCustomAlert("⚠️ ስህተት", "በክምችት ውስጥ በቂ ዕቃ የለም!");
                return;
            }
            item.qty -= count; // Decrease stock directly
            calculatedCost = item.cost * count;
        }

        d.debts.push({ name, detail, amt, cost: calculatedCost }); 
        
        calculateDashboardMetrics();
        closeActiveModal();
        await updateTenantDocument(currentTenant.username, currentTenant);
        showCustomAlert("💳 ስኬት", "የደንበኛ ዕዳ በተሳካ ሁኔታ ተመዝግቧል!");
    };

    window.renderDebtTable = function() {
        const tbody = document.getElementById('debtTable'); 
        tbody.innerHTML = '';
        const debts = currentTenant.data.debts || [];
        
        debts.forEach((d, idx) => { 
            tbody.innerHTML += `
                <tr class="hover:bg-white/[0.01] border-b border-white/5 transition-colors">
                    <td class="p-4"><b class="text-white">${d.name}</b></td>
                    <td class="p-4 text-slate-300 text-xs">${d.detail || '-'}</td>
                    <td class="p-4 text-rose-400 font-extrabold font-mono">${d.amt.toLocaleString()} ETB</td>
                    <td class="p-4 text-right">
                        <div class="flex justify-end gap-1">
                            <button class="bg-emerald-500/15 hover:bg-emerald-500 text-emerald-400 hover:text-black border border-emerald-500/20 font-bold px-2 py-1 rounded-lg text-[10px] transition-all" onclick="openSettleDebtModal(${idx})">💵 ክፈል</button>
                            <button class="bg-rose-500/10 hover:bg-rose-500 text-rose-400 hover:text-white border border-rose-500/20 font-bold px-2 py-1 rounded-lg text-[10px] transition-all" onclick="deleteDebtItem(${idx})">🗑️ ሰርዝ</button>
                        </div>
                    </td>
                </tr>
            `; 
        });
    };

    window.openSettleDebtModal = function(idx) {
        const debt = currentTenant.data.debts[idx];
        document.getElementById('settleCustName').innerText = debt.name;
        document.getElementById('settleCustAmt').innerText = debt.amt.toLocaleString() + " ETB";
        document.getElementById('settlePayAmount').value = '';

        openModalContainer();
        document.getElementById('settleDebtModal').classList.remove('hidden');

        document.getElementById('settleSubmitBtn').onclick = async function() {
            const payAmt = parseFloat(document.getElementById('settlePayAmount').value) || 0;
            if(payAmt <= 0 || payAmt > debt.amt) {
                showCustomAlert("⚠️ ስህተት", "እባክዎ በትክክል ካለባቸው ዕዳ የማይበልጥ የክፍያ መጠን ያስገቡ!");
                return;
            }

            debt.amt -= payAmt;
            currentTenant.data.collectedCreditToday += payAmt;

            // Generate temporary credit payoff inventory line to dynamically balance profits
            if (!currentTenant.data.inventory) currentTenant.data.inventory = [];
            currentTenant.data.inventory.push({
                name: `የተሰበሰበ ዕዳ (${debt.detail || 'ልዩ'})`,
                cost: debt.cost ? (debt.cost * (payAmt / (debt.amt + payAmt))) : (payAmt * 0.8),
                price: payAmt,
                qty: 1,
                sold: 1,
                profit: payAmt - (debt.cost ? (debt.cost * (payAmt / (debt.amt + payAmt))) : (payAmt * 0.8)),
                isCreditPay: true
            });

            if(debt.amt <= 0) {
                currentTenant.data.debts.splice(idx, 1);
                showCustomAlert("🎉 ሙሉ በሙሉ ተከፈለ", `${debt.name} ዕዳቸውን ሙሉ በሙሉ ከፍለው ጨርሰዋል!`);
            } else {
                showCustomAlert("✅ ዕዳ ተቀንሷል", `ደንበኛው ${payAmt.toLocaleString()} ETB ከፍለዋል። ቀሪ ዕዳ፦ ${debt.amt.toLocaleString()} ETB`);
            }

            calculateDashboardMetrics();
            closeActiveModal();
            await updateTenantDocument(currentTenant.username, currentTenant);
        };
    };

    window.deleteDebtItem = function(idx) {
        showCustomConfirm(
            "⚠️ ዕዳ ለመሰረዝ", 
            "የመረጡትን የተበዳሪ መዝገብ ያለክፍያ ሙሉ በሙሉ መሰረዝ ይፈልጋሉ?", 
            async () => {
                currentTenant.data.debts.splice(idx, 1);
                calculateDashboardMetrics();
                await updateTenantDocument(currentTenant.username, currentTenant);
                showCustomAlert("🧹 ስኬት", "የተበዳሪው መዝገብ ተሰርዟል!");
            }
        );
    };

    // DRAWER CASH OPERATIONS
    window.openDrawerModal = function() {
        document.getElementById('drawerReason').value = '';
        document.getElementById('drawerAmount').value = '';
        openModalContainer();
        document.getElementById('drawerModal').classList.remove('hidden');
    };

    window.saveDrawer = async function() {
        const reason = document.getElementById('drawerReason').value.trim();
        const amt = parseFloat(document.getElementById('drawerAmount').value) || 0;

        if(!reason || amt <= 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ የተወሰደበትን ምክንያት እና የብር መጠን ያስገቡ!");
            return;
        }

        if(!currentTenant.data.drawerLog) currentTenant.data.drawerLog = [];
        currentTenant.data.drawerLog.push({ reason, amt, status: 'ወጣ' }); 
        
        calculateDashboardMetrics();
        closeActiveModal();
        await updateTenantDocument(currentTenant.username, currentTenant);
        showCustomAlert("💸 ስኬት", "ከሳጥን የወጣው ገንዘብ በተሳካ ሁኔታ ተመዝግቧል!");
    };

    window.renderDrawerTable = function() {
        const tbody = document.getElementById('drawerTable'); 
        tbody.innerHTML = '';
        const logs = currentTenant.data.drawerLog || [];
        
        logs.forEach((l, idx) => { 
            const statusBtn = l.status === 'ወጣ' 
                ? `<button class="bg-sky-500/10 hover:bg-sky-500 text-sky-400 hover:text-black border border-sky-500/20 font-bold px-2 py-0.5 rounded text-[10px] transition-all" onclick="settleDrawerBack(${idx})">መልስ</button>`
                : `<span class="text-emerald-400 text-[10px] font-bold">✅ ገብቷል</span>`;

            tbody.innerHTML += `
                <tr class="hover:bg-white/[0.01] border-b border-white/5 transition-colors">
                    <td class="p-4 text-slate-300 text-xs">${l.reason}</td>
                    <td class="p-4 text-purple-400 font-extrabold font-mono">${l.amt.toLocaleString()} ETB</td>
                    <td class="p-4 text-right">${statusBtn}</td>
                </tr>
            `; 
        });
    };

    window.settleDrawerBack = async function(idx) {
        showCustomConfirm(
            "💸 ከሳጥን የወጣ ገንዘብ መመለሻ",
            "ይህ ገንዘብ ሙሉ በሙሉ ወደ ሳጥን ተመልሶ መግባቱን ያረጋግጣሉ?",
            async () => {
                currentTenant.data.drawerLog[idx].status = "ገብቷል (ተወራርዷል)";
                calculateDashboardMetrics();
                await updateTenantDocument(currentTenant.username, currentTenant);
                showCustomAlert("✅ ስኬት", "ገንዘቡ በተሳካ ሁኔታ ተመልሷል!");
            }
        );
    };

    // HISTORY TABLE RENDER
    function renderHistoryTable() {
        const tbody = document.getElementById('historyTable');
        tbody.innerHTML = '';
        const history = currentTenant.data.history || [];

        history.forEach(h => {
            tbody.innerHTML += `
                <tr class="hover:bg-white/[0.01] border-b border-white/5 transition-colors text-xs">
                    <td class="p-4"><b>${h.date}</b><br><span class="text-slate-500 text-[10px]">${h.employee}</span></td>
                    <td class="p-4 text-slate-400 font-mono">${h.hours}</td>
                    <td class="p-4 text-emerald-400 font-bold font-mono">${h.profit.toLocaleString()} ETB</td>
                </tr>
            `;
        });
    }

    // BANK AND CONFIG OPTIONS
    window.openBankConfigModal = function() {
        document.getElementById('bankAppSelect').value = preferredBankApp;
        openModalContainer();
        document.getElementById('bankConfigModal').classList.remove('hidden');
    };

    window.saveBankConfig = async function() {
        preferredBankApp = document.getElementById('bankAppSelect').value;
        currentTenant.data.preferredBankApp = preferredBankApp;
        
        closeActiveModal();
        await updateTenantDocument(currentTenant.username, currentTenant);
        showCustomAlert("🏦 ባንክ መተግበሪያ", "የባንክ መተግበሪያው በተሳካ ሁኔታ ተገናኝቷል!");
    };

    // NEXT DAY RESTART SESSION
    window.startNewDaySession = function() {
        const inv = currentTenant.data.inventory || [];
        if (inv.length === 0) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ መጀመሪያ ቢያንስ አንድ ዕቃ ያስገቡ!");
            return;
        }

        showCustomConfirm(
            "🔄 የዛሬን ሂሳብ መዝጊያ",
            "የዛሬውን የሥራ ሂሳብ ሙሉ በሙሉ ዘግተው ወደ ቀጣዩ ቀን ማስተላለፍ ይፈልጋሉ? (ይህ ድርጊት የዛሬውን ትርፍ ወደ ታሪክ ያሸጋግረዋል)",
            async () => {
                const d = currentTenant.data;
                const today = new Date();
                const logoutTime = today.toLocaleTimeString('am-ET', { hour: '2-digit', minute: '2-digit' });

                let todayProfit = d.dailyProfit || 0;
                let todaySales = collectedCreditToday;

                inv.forEach(item => {
                    todaySales += (item.price * item.sold);
                });

                if(!d.history) d.history = [];
                d.history.push({
                    date: d.sessionData.date,
                    employee: d.sessionData.employee,
                    hours: `ከ ${d.sessionData.loginTime} እስከ ${logoutTime}`,
                    sales: todaySales,
                    profit: todayProfit
                });

                // Clear temporary dynamic entries
                d.inventory = d.inventory.filter(item => !item.isCreditPay);
                d.inventory.forEach(item => {
                    item.qty = item.qty - item.sold;
                    item.sold = 0;
                    item.profit = 0;
                });

                d.sessionData = null;
                d.expenses = [];
                d.collectedCreditToday = 0;

                await updateTenantDocument(currentTenant.username, currentTenant);
                showCustomAlert("🎉 መልካም አዲስ ቀን", "የዕለቱ ሂሳብ በተሳካ ሁኔታ ወደ ታሪክ ተዛውሯል። አዲስ የዕለት ፎርም ይከፈታል።", () => {
                    checkMorningSessionStatus();
                });
            }
        );
    };

    // AUTOMATIC REPORT GENERATOR (FOR 4:00 PM/22:00)
    function generateLiveReport() {
        const d = currentTenant.data;
        if (!d.sessionData) return;

        const inv = d.inventory || [];
        const exp = d.expenses || [];
        const drawer = d.drawerLog || [];

        let todaySales = d.collectedCreditToday || 0;
        let todayProfit = 0;
        let details = "";

        inv.forEach(i => {
            if (i.sold > 0) {
                todaySales += (i.price * i.sold);
                todayProfit += (i.price - i.cost) * i.sold;
                if (!i.isCreditPay) {
                    details += ` - ${i.name}፦ ${i.sold} ፍሬ ተሸጠ\n`;
                }
            }
        });

        let totalExpenses = 0;
        let expDetails = "";
        exp.forEach(e => {
            totalExpenses += e.amt;
            expDetails += ` - ${e.reason}፦ ${e.amt.toLocaleString()} ETB\n`;
        });

        let totalDraws = 0;
        drawer.forEach(dw => {
            if (dw.status === 'ወጣ') {
                totalDraws += dw.amt;
            }
        });

        const initFloat = d.sessionData ? d.sessionData.initialFloat : 0;
        const finalInCash = initFloat + todaySales - totalExpenses - totalDraws;

        const reportStr = `=== ትርፌ እለታዊ የሂሳብ ሪፖርት ===\nቀን፦ ${d.sessionData.date}\nሠራተኛ፦ ${d.sessionData.employee}\n------------------------------\nመነሻ ካሽ (Float)፦ ${initFloat.toLocaleString()} ETB\nጠቅላላ የዛሬ ገቢ (ሽያጭ + የተሰበሰበ እዳ)፦ ${todaySales.toLocaleString()} ETB\n  * ከዕዳ የተሰበሰበ ገቢ፦ ${d.collectedCreditToday.toLocaleString()} ETB\n\nየዛሬ ሽያጭ ዝርዝር፦\n${details || " - ምንም ሽያጭ የለም\n"}\nየዛሬ ወጪዎች፦\n${expDetails || " - ወጪ የለም\n"}ጠቅላላ ወጪ፦ ${totalExpenses.toLocaleString()} ETB\n\nከሳጥን የተነሳ ብር፦ ${totalDraws.toLocaleString()} ETB\n------------------------------\n💰 ማታ በሳጥን መገኘት ያለበት ካሽ (Cash Box)፦\n👉 [ ${finalInCash.toLocaleString()} ETB ] 👈\n==============================`;
        
        document.getElementById('reportText').value = reportStr;
    }

    window.copyReport = function() {
        const copyText = document.getElementById("reportText");
        copyText.select();
        document.execCommand('copy');
        showCustomAlert("📋 ተገልብጧል", "ሪፖርቱ በተሳካ ሁኔታ ወደ ክሊፕቦርድ ተገልብጧል!");
    };

    // Profile Change Operations
    window.openEditProfileModal = function() {
        document.getElementById('editShopName').value = currentTenant.shopName;
        document.getElementById('editPassword').value = currentTenant.password;
        
        openModalContainer();
        document.getElementById('editProfileModal').classList.remove('hidden');
    };

    window.saveProfileChanges = async function() {
        const shop = document.getElementById('editShopName').value.trim();
        const pass = document.getElementById('editPassword').value.trim();

        if(!shop || !pass) {
            showCustomAlert("⚠️ ስህተት", "እባክዎ ሁሉንም መረጃዎች ይሙሉ!");
            return;
        }

        currentTenant.shopName = shop;
        currentTenant.password = pass;

        document.getElementById('shopTitle').innerText = shop + " - መቆጣጠሪያ";
        updateProfileSectionUI();

        closeActiveModal();
        await updateTenantDocument(currentTenant.username, currentTenant);
        showCustomAlert("✅ ስኬት", "የእርስዎ መገለጫ መረጃ በተሳካ ሁኔታ ተስተካክሏል!");
    };

    // LOGOUT
    window.logout = function() {
        currentTenant = null;
        resetFontSize();
        
        document.getElementById('loginUser').value = '';
        document.getElementById('loginPass').value = '';
        
        document.getElementById('adminPage').classList.add('hidden');
        document.getElementById('appPage').classList.add('hidden');
        document.getElementById('logoutBtn').classList.add('hidden');
        document.getElementById('loginPage').classList.remove('hidden');
    };
</script>
</body>
</html>

```
