<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>LuaHub | Mobile</title>
    
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Fira+Code:wght@400;500&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/tomorrow-night.min.css">
    
    <style>
        :root {
            --primary: #007bff;
            --bg: #0f0f0f;
            --card-bg: #1a1a1a;
            --input-bg: #252525;
            --text: #ffffff;
            --text-dim: #b0b0b0;
            --danger: #ff4d4d;
            --success: #2ecc71;
        }

        * { box-sizing: border-box; -webkit-tap-highlight-color: transparent; }

        body { 
            font-family: 'Fredoka One', cursive; 
            background-color: var(--bg); 
            color: var(--text); 
            margin: 0; 
            padding-bottom: 50px; /* Space for mobile nav if needed */
        }

        .container { 
            width: 100%; 
            max-width: 800px; 
            margin: auto; 
            padding: 15px; 
        }

        /* --- MOBILE HEADER --- */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            background: var(--card-bg);
            padding: 12px 20px;
            border-bottom: 2px solid #333;
            position: sticky;
            top: 0;
            z-index: 1000;
        }

        .logo { font-size: 24px; letter-spacing: 1px; }
        .logo span { color: var(--primary); }

        .pfp-header {
            width: 40px; height: 40px;
            border-radius: 50%;
            border: 2px solid var(--primary);
            object-fit: cover;
        }

        /* --- SEARCH & FILTERS --- */
        .search-area {
            display: flex;
            flex-direction: column;
            gap: 10px;
            margin: 20px 0;
        }

        .search-input {
            width: 100%;
            padding: 14px;
            background: var(--input-bg);
            border: 2px solid #333;
            border-radius: 12px;
            color: white;
            font-size: 16px; /* Prevents iOS zoom on focus */
            font-family: inherit;
        }

        .filter-row {
            display: flex;
            gap: 10px;
        }

        .btn-filter {
            flex: 1;
            padding: 10px;
            background: var(--input-bg);
            border: 2px solid #333;
            border-radius: 10px;
            color: var(--text-dim);
            font-family: inherit;
        }

        .btn-filter.active {
            background: var(--primary);
            color: white;
            border-color: var(--primary);
        }

        /* --- SCRIPT CARDS --- */
        .card {
            background: var(--card-bg);
            border-radius: 16px;
            padding: 18px;
            margin-bottom: 20px;
            border: 1px solid #333;
            position: relative;
        }

        .card-title { font-size: 20px; margin-bottom: 5px; color: var(--primary); }
        .card-author { font-size: 13px; color: var(--text-dim); margin-bottom: 12px; display: flex; align-items: center; gap: 6px; }
        .card-author img { width: 18px; border-radius: 50%; }

        /* Code Block Mobile Optimization */
        pre[class*="language-"] {
            font-size: 13px !important;
            padding: 12px !important;
            border-radius: 10px !important;
            max-height: 250px;
            overflow: auto;
            background: #000 !important;
        }

        /* --- ACTIONS --- */
        .action-bar {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-top: 15px;
        }

        .btn {
            border: none;
            border-radius: 8px;
            padding: 10px 16px;
            font-family: inherit;
            font-size: 14px;
            cursor: pointer;
            transition: 0.2s;
            display: flex;
            align-items: center;
            gap: 8px;
        }

        .btn-primary { background: var(--primary); color: white; width: 100%; justify-content: center; padding: 14px; }
        .btn-outline { background: #333; color: white; }
        .btn-copy { background: var(--success); color: white; }

        /* --- MODALS --- */
        .modal {
            display: none;
            position: fixed;
            inset: 0;
            background: rgba(0,0,0,0.85);
            z-index: 2000;
            align-items: center;
            justify-content: center;
            padding: 20px;
        }

        .modal.active { display: flex; }

        .modal-content {
            background: var(--card-bg);
            width: 100%;
            max-width: 400px;
            border-radius: 20px;
            padding: 25px;
            border: 2px solid var(--primary);
        }

        /* --- CREATION FORM --- */
        .create-form {
            background: var(--card-bg);
            padding: 20px;
            border-radius: 16px;
            border: 2px dashed #444;
            margin-bottom: 30px;
        }

        .create-form textarea {
            width: 100%;
            background: var(--input-bg);
            border: 1px solid #444;
            border-radius: 8px;
            color: white;
            padding: 12px;
            margin: 10px 0;
            font-family: 'Fira Code', monospace;
            min-height: 100px;
        }

        /* Toast Notification */
        #toast {
            position: fixed;
            bottom: 20px;
            left: 50%;
            transform: translateX(-50%);
            background: var(--primary);
            padding: 12px 24px;
            border-radius: 50px;
            font-size: 14px;
            z-index: 9999;
            display: none;
            box-shadow: 0 4px 15px rgba(0,0,0,0.5);
        }

        /* Helper Utilities */
        .hidden { display: none !important; }
    </style>
</head>
<body>

    <div id="toast">Copied to Clipboard!</div>

    <header class="header">
        <div class="logo">Lua<span>Hub</span></div>
        <div id="user-nav">
            <button class="btn btn-outline" onclick="toggleModal('auth-modal')">Login</button>
        </div>
    </header>

    <div class="container">
        <div class="search-area">
            <input type="text" id="main-search" class="search-input" placeholder="Search scripts..." onkeyup="renderScripts()">
            <div class="filter-row">
                <button class="btn-filter active" id="filter-name" onclick="setFilter('name')">By Name</button>
                <button class="btn-filter" id="filter-creator" onclick="setFilter('creator')">By Creator</button>
            </div>
        </div>

        <div id="post-section" class="hidden">
            <div class="create-form">
                <h3 style="margin-top:0">✍️ Post New Script</h3>
                <input type="text" id="new-title" class="search-input" placeholder="Script Title">
                <textarea id="new-code" placeholder="-- Paste Lua code here..."></textarea>
                <button class="btn btn-primary" onclick="publishScript()">Post to Hub</button>
            </div>
        </div>

        <div id="script-feed">
            </div>
    </div>

    <div class="modal" id="auth-modal">
        <div class="modal-content">
            <h2 style="margin-top:0">Welcome</h2>
            <p style="color:var(--text-dim)">Enter a username to join the hub.</p>
            <input type="text" id="login-username" class="search-input" placeholder="Username...">
            <div style="display:flex; gap:10px; margin-top:20px;">
                <button class="btn btn-primary" onclick="handleLogin()">Join Now</button>
                <button class="btn btn-outline" onclick="toggleModal('auth-modal')">Back</button>
            </div>
        </div>
    </div>

    <script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-lua.min.js"></script>

    <script>
        // --- DATABASE EMULATION ---
        let scripts = JSON.parse(localStorage.getItem('lh_scripts')) || [
            { id: 1, title: "Speed Hack GUI", creator: "Admin", code: "-- Simple Speed\ngame.Players.LocalPlayer.Character.Humanoid.WalkSpeed = 100", date: Date.now() },
            { id: 2, title: "Infinite Jump", creator: "Tasin", code: "-- Jump Script\nprint('Jump enabled')", date: Date.now() }
        ];

        let currentUser = localStorage.getItem('lh_user') || null;
        let activeFilter = 'name';

        // --- APP LOGIC ---
        function init() {
            if (currentUser) {
                updateUIForUser();
            }
            renderScripts();
        }

        function toggleModal(id) {
            document.getElementById(id).classList.toggle('active');
        }

        function setFilter(type) {
            activeFilter = type;
            document.querySelectorAll('.btn-filter').forEach(b => b.classList.remove('active'));
            document.getElementById('filter-' + type).classList.add('active');
            renderScripts();
        }

        function handleLogin() {
            const user = document.getElementById('login-username').value.trim();
            if (user.length < 3) return showToast("Name too short!");
            
            currentUser = user;
            localStorage.setItem('lh_user', user);
            toggleModal('auth-modal');
            updateUIForUser();
            showToast("Welcome, " + user + "!");
        }

        function updateUIForUser() {
            document.getElementById('user-nav').innerHTML = `
                <div style="display:flex; align-items:center; gap:10px;">
                    <span style="font-size:14px; color:var(--primary)">@${currentUser}</span>
                    <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=${currentUser}" class="pfp-header" onclick="logout()">
                </div>
            `;
            document.getElementById('post-section').classList.remove('hidden');
        }

        function logout() {
            if(confirm("Logout?")) {
                localStorage.removeItem('lh_user');
                location.reload();
            }
        }

        function publishScript() {
            const title = document.getElementById('new-title').value.trim();
            const code = document.getElementById('new-code').value.trim();

            if(!title || !code) return showToast("Fill all fields!");

            const newObj = {
                id: Date.now(),
                title: title,
                creator: currentUser,
                code: code,
                date: Date.now()
            };

            scripts.unshift(newObj);
            localStorage.setItem('lh_scripts', JSON.stringify(scripts));
            
            // Reset
            document.getElementById('new-title').value = '';
            document.getElementById('new-code').value = '';
            renderScripts();
            showToast("Script Published!");
        }

        function renderScripts() {
            const feed = document.getElementById('script-feed');
            const search = document.getElementById('main-search').value.toLowerCase();
            
            feed.innerHTML = '';

            const filtered = scripts.filter(s => {
                const target = activeFilter === 'name' ? s.title : s.creator;
                return target.toLowerCase().includes(search);
            });

            filtered.forEach(s => {
                const card = document.createElement('div');
                card.className = 'card';
                card.innerHTML = `
                    <div class="card-title">${escape(s.title)}</div>
                    <div class="card-author">
                        <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=${s.creator}">
                        <span>by ${escape(s.creator)}</span>
                    </div>
                    <pre class="language-lua"><code>${escape(s.code)}</code></pre>
                    <div class="action-bar">
                        <button class="btn btn-copy" onclick="copyCode(this, \`${s.id}\`)">
                            <i class="fas fa-copy"></i> Copy Code
                        </button>
                        <span style="font-size:11px; color:#555">ID: ${s.id}</span>
                    </div>
                `;
                feed.appendChild(card);
            });

            Prism.highlightAll();
        }

        function copyCode(btn, id) {
            const script = scripts.find(s => s.id == id);
            navigator.clipboard.writeText(script.code).then(() => {
                const original = btn.innerHTML;
                btn.innerHTML = '<i class="fas fa-check"></i> Copied!';
                btn.style.background = "#2ecc71";
                setTimeout(() => {
                    btn.innerHTML = original;
                    btn.style.background = "";
                }, 2000);
            });
        }

        function showToast(msg) {
            const t = document.getElementById('toast');
            t.innerText = msg;
            t.style.display = 'block';
            setTimeout(() => t.style.display = 'none', 3000);
        }

        function escape(html) {
            const text = document.createTextNode(html);
            const p = document.createElement('p');
            p.appendChild(text);
            return p.innerHTML;
        }

        // Run
        init();
    </script>
</body>
</html>
