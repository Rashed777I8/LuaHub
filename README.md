<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>Lua Script Hub</title>
    <link href="https://fonts.googleapis.com/css2?family=Fredoka+One&family=Fira+Code&display=swap" rel="stylesheet">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.0.0/css/all.min.css">
    
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.css">
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/theme/dracula.min.css">
    
    <style>
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/themes/tomorrow-night.min.css');
        @import url('https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/plugins/line-numbers/prism-line-numbers.min.css');
    </style>
    
    <style>
        body { font-family: 'Fredoka One', cursive; background-color: #121212; color: #ffffff; padding: 10px; margin: 0; font-size: 14px; }
        .container { max-width: 500px; margin: auto; }
        
        /* PROFESSIONAL HEADER */
        .header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            background: #1e1e1e;
            padding: 10px;
            border-radius: 12px;
            border: 2px solid #333;
            position: relative;
        }
        .logo { font-size: 22px; font-weight: bold; }
        .logo-hub { color: #007bff; }
        .header-actions { display: flex; align-items: center; gap: 10px; }
        
        /* Header PFP */
        .pfp-container { position: relative; }
        .pfp-header { 
            width: 35px; height: 35px; 
            border-radius: 50%; 
            object-fit: cover; 
            border: 2px solid #007bff; 
            cursor: pointer;
        }
        .ban-counter {
            position: absolute; top: -5px; right: -5px;
            background: #dc3545; color: white; border-radius: 50%;
            width: 18px; height: 18px; font-size: 10px;
            display: flex; justify-content: center; align-items: center;
            border: 2px solid #121212; font-family: sans-serif;
        }

        .btn { 
            padding: 8px 12px; border: none; border-radius: 8px; 
            cursor: pointer; font-family: 'Fredoka One', cursive; 
            font-size: 12px; transition: all 0.2s;
        }
        .btn-login { background: #007bff; color: white; }
        .btn-admin { background: #dc3545; color: white; border-radius: 50%; width: 35px; height: 35px; display:flex; align-items:center; justify-content:center;}
        .btn:hover { opacity: 0.9; }

        /* --- USER PROFILE PANEL (MODAL) --- */
        .profile-panel {
            display: none;
            position: absolute;
            top: calc(100% + 5px);
            right: 0;
            background: #1e1e1e;
            border: 2px solid #007bff;
            border-radius: 12px;
            padding: 15px;
            width: 220px;
            z-index: 1000;
            box-shadow: 0 5px 15px rgba(0,0,0,0.5);
            text-align: center;
        }
        .profile-panel.show { display: block; }
        
        .panel-pfp { width: 60px; height: 60px; border-radius: 50%; border: 3px solid #007bff; margin-bottom: 10px; }
        
        .panel-username { font-size: 16px; margin-bottom: 5px; cursor: pointer; color: white;}
        
        .owner-tag { color: #ffc107; font-size: 11px; font-family: sans-serif; display: block; font-weight: bold;}
        .warning-tag {
            color: #dc3545; font-size: 11px; font-family: sans-serif;
            display: inline-block; background: rgba(220,53,69,0.2);
            padding: 2px 4px; border-radius: 4px;
        }
        .beloved-title { font-size: 10px; color: #aaa; margin-top: 10px; }
        .beloved-script { background: #2a2a2a; padding: 8px; border-radius: 8px; margin-top: 5px; font-size: 12px; border: 1px solid #333; }
        .panel-logout { width: 100%; margin-top: 15px; background: #dc3545; color: white; }

        /* PROFESSIONAL SEARCH BAR */
        .search-container {
            display: flex;
            gap: 5px;
            margin-bottom: 15px;
            background: #1e1e1e;
            padding: 8px;
            border-radius: 12px;
            border: 2px solid #333;
            align-items: center;
        }
        .search-input { 
            flex-grow: 1; padding: 10px;
            background: #2a2a2a; border: 2px solid #444; 
            color: #fff; border-radius: 8px;
            font-family: 'Fredoka One', cursive; font-size: 13px;
        }
        
        /* Custom Dropdown Styling */
        .custom-select-wrapper { position: relative; min-width: 80px; }
        .custom-select {
            background: #2a2a2a; border: 2px solid #444; color: #fff;
            padding: 10px; border-radius: 8px; cursor: pointer;
            display: flex; justify-content: space-between; align-items: center;
            font-family: 'Fredoka One', cursive; font-size: 12px;
        }
        .custom-select-options {
            display: none; position: absolute; top: calc(100% + 5px); left: 0; right: 0;
            background: #2a2a2a; border: 2px solid #444; border-radius: 8px; z-index: 10;
        }
        .custom-select-options.open { display: block; }
        .custom-select-option { padding: 10px; cursor: pointer; font-size: 12px; }
        .custom-select-option:hover { background: #333; }

        /* Card Styles */
        .card { 
            background: #1e1e1e; 
            border: 2px solid #333; 
            border-radius: 12px; 
            padding: 15px; 
            margin-bottom: 15px;
            position: relative;
        }
        h3 { font-size: 16px; margin-top: 0; margin-bottom: 5px; }
        p.desc { color: #aaa; font-family: sans-serif; margin-bottom: 10px; font-size: 13px; }
        
        /* --- CORE FIX FOR DARK CODEBOX --- */
        pre[class*="language-"], 
        .CodeMirror {
            background: #1e1e1e !important;
            border-radius: 8px;
            overflow-x: auto;
            max-height: 250px;
            margin-top: 5px;
            border: 1px solid #333;
        }
        code[class*="language-"] {
            font-family: 'Fira Code', monospace;
            font-size: 12px;
            background: #1e1e1e !important; /* Forces dark background */
        }
        
        input, textarea { 
            width: 100%; padding: 10px; margin: 5px 0; 
            background: #2a2a2a; border: 2px solid #444; 
            color: #fff; border-radius: 8px; box-sizing: border-box;
            font-family: 'Fredoka One', cursive; font-size: 13px;
        }
        .CodeMirror { font-size: 12px !important; }

        /* CARD MENU (Three Dots) */
        .card-options { position: absolute; top: 15px; right: 15px; cursor: pointer; color: #888; font-size: 16px; }
        
        .dropdown-menu {
            display: none; position: absolute; top: 100%; right: 0;
            background: #333; border-radius: 8px; padding: 5px;
            z-index: 100; min-width: 120px; border: 1px solid #444;
        }
        .dropdown-menu button {
            display: block; width: 100%; background: none; border: none;
            color: white; text-align: left; padding: 8px;
            cursor: pointer; font-family: 'Fredoka One', cursive; font-size: 12px;
        }
        .dropdown-menu button.danger { color: #dc3545; }

        .btn-post { background: #007bff; color: white; width: 100%; }
        
        .btn-reaction { background: #333; color: #aaa; padding: 5px 8px; font-size: 11px; }
        .btn-reaction.active-like { background: #28a745; color: white; }
        .btn-reaction.active-dislike { background: #dc3545; color: white; }
        
        .rating-container {
            display: flex; align-items: center; gap: 5px; font-family: sans-serif;
            font-size: 12px; color: #aaa; background: #2a2a2a;
            padding: 5px; border-radius: 8px;
        }
        .rating-slider { -webkit-appearance: none; width: 100px; height: 6px; background: #444; border-radius: 5px; }

        .comment-section { margin-top: 15px; border-top: 1px solid #444; padding-top: 10px; }
        .comment-input { font-size: 12px; }
        .comment-item { background: #2a2a2a; padding: 8px; border-radius: 8px; margin-top: 8px; font-size: 12px; position: relative; }
        
        .author-tag { color: #007bff; font-weight: bold; font-family: 'Fredoka One', cursive; display: flex; align-items: center; gap: 5px;}
        .pfp-tiny { width: 14px; height: 14px; border-radius: 50%; }
        .comment-text { color: #eee; font-family: sans-serif; margin-top: 3px; font-size: 12px; word-wrap: break-word;}
        .comment-actions { display: flex; gap: 5px; margin-top: 5px; }

        .modal { display: none; position: fixed; top: 0; left: 0; width: 100%; height: 100%; background-color: rgba(0,0,0,0.85); justify-content: center; align-items: center; z-index: 2000; }
        .modal.show { display: flex; }
        .modal-content { background: #1e1e1e; padding: 20px; border-radius: 12px; border: 2px solid #007bff; width: 85%; max-width: 350px; text-align: center; }
        .modal-buttons { display: flex; justify-content: center; gap: 10px; margin-top: 15px; }
        
        .pfp-option { width: 40px; height: 40px; border-radius: 50%; cursor: pointer; border: 2px solid transparent; }
        .pfp-option.selected { border-color: #007bff; }
        
        #toast {
            position: fixed; bottom: 20px; left: 50%; transform: translateX(-50%);
            background: #28a745; color: white; padding: 10px 20px; border-radius: 8px;
            display: none; z-index: 3000; font-family: 'Fredoka One', cursive; font-size: 12px;
        }
        #toast.show { display: block; }
    </style>
</head>
<body>

<div id="toast"></div>

<div class="container">
    
    <div class="header">
        <div class="logo">Lua<span class="logo-hub">Hub</span></div>
        
        <div class="header-actions" id="authAreaContainer"></div>

        <div class="profile-panel" id="profilePanel">
            <img src="" id="panelPfp" class="panel-pfp">
            <div class="panel-username" id="panelUsername" onclick="promptChangeUsername()"></div>
            <span class="owner-tag" id="panelTag" style="display:none;">👑 OWNER</span>
            <span class="warning-tag" id="panelWarning" style="display:none;">Warning Level : 0</span>
            <div class="panel-logout btn" onclick="logout()">Logout</div>
        </div>
    </div>

    <div class="search-container">
        <input type="text" id="searchInput" class="search-input" placeholder="Search..." onkeyup="renderScripts()">
        
        <div class="custom-select-wrapper">
            <div class="custom-select" id="searchTypeDisplay">
                <span id="selectedTypeText">Name</span>
            </div>
            <div class="custom-select-options" id="searchTypeOptions">
                <div class="custom-select-option" data-value="name">Name</div>
                <div class="custom-select-option" data-value="creator">Creator</div>
            </div>
        </div>
        <input type="hidden" id="searchType" value="name">
    </div>

    <div class="card" id="createCard" style="display:none;">
        <h2 style="font-size:16px;">✍️ Create Script</h2>
        <input type="text" id="scriptTitle" placeholder="Script Title">
        <textarea id="scriptDesc" rows="1" placeholder="Description"></textarea>
        <textarea id="scriptCode"></textarea>
        <button class="btn btn-post" onclick="saveScript()">Post</button>
    </div>

    <div id="scriptFeed"></div>
</div>

<div id="authModal" class="modal">
    <div class="modal-content">
        <h3 id="authTitle" style="margin-top:0;">Login</h3>
        <input type="text" id="authUsername" placeholder="Username">
        <input type="password" id="authPassword" placeholder="Password">
        <div style="display:flex; justify-content:center; gap:5px; margin:10px 0;">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=1" class="pfp-option selected" onclick="selectPfp(this, 'seed=1')">
            <img src="https://api.dicebear.com/7.x/pixel-art/svg?seed=2" class="pfp-option" onclick="selectPfp(this, 'seed=2')">
        </div>
        <div class="modal-buttons">
            <button class="btn btn-post" id="authSubmitBtn" onclick="processAuth()">Login</button>
            <button class="btn btn-logout" style="background:#555;" onclick="closeAuthModal()">Cancel</button>
        </div>
    </div>
</div>

<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/codemirror.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.65.13/mode/lua/lua.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/prism.min.js"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/prism/1.29.0/components/prism-lua.min.js"></script>

<script>
    // --- APP LOGIC (Core functionality kept from original, updated for mobile) ---
    let scripts = JSON.parse(localStorage.getItem('lua_hub_data')) || [];
    let users = JSON.parse(localStorage.getItem('lua_hub_users')) || {};
    let currentUser = JSON.parse(localStorage.getItem('lua_hub_session')) || null;
    let selectedPfpSeed = 'seed=1';
    let authType = 'login';
    
    // Initialize CodeMirror on textarea
    const editor = CodeMirror.fromTextArea(document.getElementById("scriptCode"), {
        lineNumbers: false,
        theme: "dracula",
        mode: "lua",
        viewportMargin: Infinity
    });

    // --- Core Functions for Rendering & Auth ---
    function renderScripts() {
        const feed = document.getElementById('scriptFeed');
        const searchTerm = document.getElementById('searchInput').value.toLowerCase();
        const searchType = document.getElementById('searchType').value;
        feed.innerHTML = '';

        const filteredScripts = scripts.filter(s => {
            if(searchType === 'name') return s.title.toLowerCase().includes(searchTerm);
            if(searchType === 'creator') return s.author.toLowerCase().includes(searchTerm);
            return true;
        });

        filteredScripts.forEach((s) => {
            const card = document.createElement('div');
            card.className = 'card';
            
            // Re-using original reaction logic simplified
            card.innerHTML = `
                <h3>${escapeHTML(s.title)}</h3>
                <span class="author-tag"><img src="${s.authorPfp}" class="pfp-tiny"> ${escapeHTML(s.author)}</span>
                <p class="desc">${escapeHTML(s.desc)}</p>
                <pre><code class="language-lua">${escapeHTML(s.code)}</code></pre>
                
                <div class="card-options" onclick="toggleDropdown(event, '${s.id}')">
                    <i class="fas fa-ellipsis-v"></i>
                    <div class="dropdown-menu" id="dropdown-${s.id}">
                        <button onclick="copyScript('${s.id}')"><i class="fas fa-copy"></i> Copy</button>
                    </div>
                </div>
            `;
            feed.appendChild(card);
        });
        Prism.highlightAll();
    }

    function saveScript() {
        if(!currentUser) return showToast("Login first!");
        const title = document.getElementById('scriptTitle').value;
        const desc = document.getElementById('scriptDesc').value;
        const code = editor.getValue();
        if(!title || !code) return showToast("Title and Code required!");
        
        const newScript = { 
            id: 'id' + Date.now(), 
            author: currentUser.username, 
            authorPfp: currentUser.pfp, 
            title, desc, code 
        };
        scripts.unshift(newScript);
        localStorage.setItem('lua_hub_data', JSON.stringify(scripts));
        renderScripts();
        document.getElementById('scriptTitle').value = '';
        document.getElementById('scriptDesc').value = '';
        editor.setValue('');
        showToast("Published!");
    }

    // --- Helper Functions ---
    function copyScript(scriptId) {
        const script = scripts.find(s => s.id === scriptId);
        navigator.clipboard.writeText(script.code).then(() => showToast("Copied!"));
    }
    
    function showToast(message) {
        const toast = document.getElementById('toast');
        toast.innerText = message;
        toast.classList.add('show');
        setTimeout(() => toast.classList.remove('show'), 2000);
    }
    function escapeHTML(str) { return str.replace(/[&<>"']/g, m => ({'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[m])); }
    
    // --- Dropdown and Modal Logic ---
    document.getElementById('searchTypeDisplay').addEventListener('click', (e) => {
        e.stopPropagation();
        document.getElementById('searchTypeOptions').classList.toggle('open');
    });

    document.querySelectorAll('.custom-select-option').forEach(option => {
        option.addEventListener('click', (e) => {
            e.stopPropagation();
            document.getElementById('searchType').value = option.getAttribute('data-value');
            document.getElementById('selectedTypeText').innerText = option.innerText;
            document.getElementById('searchTypeOptions').classList.remove('open');
            renderScripts();
        });
    });

    function toggleDropdown(event, id) {
        event.stopPropagation();
        const menu = document.getElementById(`dropdown-${id}`);
        document.querySelectorAll('.dropdown-menu').forEach(m => m.style.display = 'none');
        menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
    }

    function openAuthModal(type) {
        authType = type;
        document.getElementById('authTitle').innerText = type === 'login' ? 'Login' : 'Sign Up';
        document.getElementById('authSubmitBtn').innerText = type === 'login' ? 'Login' : 'Register';
        document.getElementById('authModal').classList.add('show');
    }
    function closeAuthModal() { document.getElementById('authModal').classList.remove('show'); }

    function processAuth() {
        const username = document.getElementById('authUsername').value.trim();
        const password = document.getElementById('authPassword').value;
        const pfp = `https://api.dicebear.com/7.x/pixel-art/svg?${selectedPfpSeed}`;
        
        if(authType === 'register') {
            users[username] = { username, password, pfp };
            localStorage.setItem('lua_hub_users', JSON.stringify(users));
            openAuthModal('login');
        } else {
            if(users[username] && users[username].password === password) {
                currentUser = users[username];
                localStorage.setItem('lua_hub_session', JSON.stringify(currentUser));
                closeAuthModal();
                updateLoginUI();
            } else { showToast("Invalid credentials"); }
        }
    }

    function updateLoginUI() {
        const authArea = document.getElementById('authAreaContainer');
        const createCard = document.getElementById('createCard');
        if(currentUser) {
            authArea.innerHTML = `<img src="${currentUser.pfp}" class="pfp-header" onclick="toggleProfilePanel()">`;
            createCard.style.display = 'block';
        } else {
            authArea.innerHTML = `<button class="btn btn-login" onclick="openAuthModal('login')">Login</button>`;
            createCard.style.display = 'none';
        }
    }
    
    function toggleProfilePanel() { document.getElementById('profilePanel').classList.toggle('show'); }
    function logout() {
        currentUser = null;
        localStorage.removeItem('lua_hub_session');
        toggleProfilePanel();
        updateLoginUI();
    }
    function selectPfp(imgElement, seed) {
        document.querySelectorAll('.pfp-option').forEach(img => img.classList.remove('selected'));
        imgElement.classList.add('selected');
        selectedPfpSeed = seed;
    }

    // Initialize
    updateLoginUI();
    renderScripts();
</script>
</body>
</html>
